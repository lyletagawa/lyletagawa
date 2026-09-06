---
title: "Shuffle Sharding"
date: 2026-09-06
publishdate: 2026-09-06
lastmod: 2026-09-06
summary: "Shuffle sharding turns Route 53's 2,048 name servers into 730 billion possible four-server combinations instead of 512 fixed groups, without adding a single machine. The isolation comes from combinatorics alone."
tags: ["sharding", "reliability", "distributed"]
image: /images/shuffle-sharding.svg
draft: false
---

![Shuffle sharding turns Route 53's 2,048 name servers into 730 billion possible four-server combinations instead of 512 fixed groups, without adding a single machine. The isolation comes from combinatorics alone.](/images/shuffle-sharding.svg)

## Shuffle sharding

A volumetric DDoS attack floods one customer's DNS name servers with random labels, causing elevated latency. On a normal shared fleet of machines, that attack would cause delays for other customers too. At Route 53, only that single customer's name servers are impacted.

## What is shuffle sharding?

Shuffle sharding assigns each tenant a random K-resource subset of a shared pool of N, instead of splitting N into fixed, non-overlapping N/K groups. Colm MacCárthaigh found it in 2010 in one of Knuth's fascicles on generating combinations{{< cite 1 "Knuth, Donald E. (2005). The Art of Computer Programming, Volume 4, Fascicle 3: Generating All Combinations and Partitions. Addison-Wesley." >}}, while his six-person team was working out how Route 53 would withstand DDoS attacks. Assign each customer a unique combination of four virtual name servers from a pool of about 2,000, and the maths bound how much overlap any two customers could share{{< cite 2 "MacCárthaigh, Colm (2025). Comment on Donald Knuth's 2024 Christmas Lecture: Strong and Weak Components. Hacker News." >}}.

First, regular sharding. Picture eight workers handling requests for many customers. Split them into four regular shards of two workers each. Any single failure takes down that shard, with a blast radius of one in four customers.

Now shuffle it. With the same eight workers, the count of distinct pairs is (8 * 7) / 2! = 28, instead of just four groups{{< cite 3 "MacCárthaigh, Colm (2019). Workload Isolation Using Shuffle-Sharding. Amazon Builders' Library." >}}. Spread customers across all 28, and any single failure now impacts roughly one in 28 customers (instead of one in four).

## The maths scales combinatorially

Route 53 runs this at a scale where the effect gets absurd. The service uses 2,048 virtual name servers and assigns each hosted zone to a shard of four name servers{{< cite 3 "MacCárthaigh, Colm (2019). Workload Isolation Using Shuffle-Sharding. Amazon Builders' Library." >}}. The count of distinct four-server combinations is (2048 choose 4) = (2048 * 2047 * 2046 * 2045) / 4! = 730,862,190,080.

Regular sharding gives 512 (2048/4) groups. Shuffle sharding gives 730 billion, using the same hardware. Route 53 can assign a unique shard to each hosted zone with plenty of room to spare.

Having 730 billion possible shards doesn't guarantee two customers never collide though. Draw two four-server combinations at random from the same pool of 2,048, and across enough customers, some pair will eventually collide.

Route 53 doesn't leave that to chance. Assigning a new zone searches for and rejects any candidate that would share more than two of four virtual name servers with an existing hosted zone{{< cite 3 "MacCárthaigh, Colm (2019). Workload Isolation Using Shuffle-Sharding. Amazon Builders' Library." >}}. That number is two because testing showed domain resolution is reliable even while two of its four name servers are unreachable{{< cite 2 "MacCárthaigh, Colm (2025). Comment on Donald Knuth's 2024 Christmas Lecture: Strong and Weak Components. Hacker News." >}}.

Run the numbers (2,048 servers, four per customer) through MacCárthaigh's blast-radius calculator{{< cite 4 "MacCárthaigh, Colm. shardcalc.py. GitHub Gist." >}}: 99.22% of customer pairs share zero servers, 0.78% share one, and sharing two or more drops below two hundredths of a percent. The same cap that limits two-way overlap to two shared servers rules out three- and four-way overlap entirely. When one domain gets attacked, the overwhelming majority of other zones notice nothing at all.

## Beyond AWS

Grafana Labs built the same idea into Cortex and Mimir, the multi-tenant metrics systems behind Grafana Cloud, sharding tenants across ingesters, queriers, store-gateways, and compactors, each shard sized between 1 and the full instance count{{< cite 5 "Cortex. Shuffle Sharding. Cortex Documentation." >}}{{< cite 6 "Grafana Labs (2024). Configure Shuffle Sharding. Grafana Mimir Documentation." >}}.

Kubernetes API Priority and Fairness (APF) shuffle-shards each request across a subset of queues, sized by a handSize parameter, so one noisy client can't starve the rest{{< cite 7 "Kubernetes. API Priority and Fairness. Kubernetes Documentation." >}}. With a handSize of 10 across 64 queues, one noisy flow has about a 1 in 150 billion chance of colliding with a quiet one.

HashiCorp Terraform Cloud uses a cellular architecture to schedule customer workloads across a pool of Nomad clusters. Each customer is assigned a random but deterministic subset, "the odds that any two customers share the exact same virtual shard assignment is exceedingly small"{{< cite 8 "Ludden, Chris, and Anthony Davis (2023). Using Temporal at HashiCorp. Replay 2023." >}}.

## Common mistakes

**You draw random shards without bounding overlap.** Pure random assignment can still give two customers shards that overlap too much, unless the assignment scheme checks for that and rejects bad draws.

**You isolate the shard but miss the shared dependency.** Shuffle sharding protects against the failure of an individual worker. A config push or a shared database will still affect the whole fleet at once.

**You let retries route around the isolation.** Cortex and Mimir document a failure mode where a query-frontend retries a crashing query (e.g., oomkiller trigger, panic, segv) against a fresh querier, so the crash works its way through the whole fleet. Both projects mitigate it with a forget-delay setting that pins retries to the same querier{{< cite 5 "Cortex. Shuffle Sharding. Cortex Documentation." >}}{{< cite 6 "Grafana Labs (2024). Configure Shuffle Sharding. Grafana Mimir Documentation." >}}.

**You shrink a stateful shard without a rollout plan.** Route 53's name servers are stateless, so resizing a customer's shard costs nothing. Ingesters in Cortex and Mimir hold tenant data, so shrinking a shard too soon can silently return incomplete results. Both projects document a specific rollout order for changing shard size{{< cite 5 "Cortex. Shuffle Sharding. Cortex Documentation." >}}{{< cite 6 "Grafana Labs (2024). Configure Shuffle Sharding. Grafana Mimir Documentation." >}}.

## Put it into practice

Examine your system the way Route 53 examined its fleet of name servers. If one bad request or one bad customer can take down a shard that serves a meaningful slice of the whole, that's a candidate for shuffle sharding. It shrinks your blast radius combinatorially instead of linearly.

Ask what the maximum overlap is between any two tenants' assignments, rather than the theoretical number of combinations available. Route 53's open source Infima library implements that bound directly instead of leaving it to random draws{{< cite 3 "MacCárthaigh, Colm (2019). Workload Isolation Using Shuffle-Sharding. Amazon Builders' Library." >}}.

You'll reduce your blast radius for basically nothing. Route 53's original team proved it on a total infrastructure budget in the tens of thousands of dollars, versus the tens of millions for proper packet-scrubbing hardware{{< cite 2 "MacCárthaigh, Colm (2025). Comment on Donald Knuth's 2024 Christmas Lecture: Strong and Weak Components. Hacker News." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Knuth, Donald E. (2005). <em>The Art of Computer Programming, Volume 4, Fascicle 3: Generating All Combinations and Partitions</em>. Addison-Wesley. <a href="https://www.informit.com/store/art-of-computer-programming-volume-4-fascicle-3-generating-9780201853940">https://www.informit.com/store/art-of-computer-programming-volume-4-fascicle-3-generating-9780201853940</a></li>
  <li id="ref-2">MacCárthaigh, Colm (2025). Comment on "Donald Knuth's 2024 Christmas Lecture: Strong and Weak Components." Hacker News. <a href="https://news.ycombinator.com/item?id=42975315">https://news.ycombinator.com/item?id=42975315</a></li>
  <li id="ref-3">MacCárthaigh, Colm (2019). "Workload Isolation Using Shuffle-Sharding." <em>Amazon Builders' Library</em>. <a href="https://d1.awsstatic.com/builderslibrary/pdfs/workload-isolation-using-shuffle-sharding.pdf">https://d1.awsstatic.com/builderslibrary/pdfs/workload-isolation-using-shuffle-sharding.pdf</a></li>
  <li id="ref-4">MacCárthaigh, Colm. "shardcalc.py." GitHub Gist. <a href="https://gist.github.com/colmmacc/4a39a6416d2a58b6c70bc73027bea4dc/0cbe948c3b2e60289aed25f40e1a6a72dca2cec9">https://gist.github.com/colmmacc/4a39a6416d2a58b6c70bc73027bea4dc/0cbe948c3b2e60289aed25f40e1a6a72dca2cec9</a></li>
  <li id="ref-5">Cortex. "Shuffle Sharding." <em>Cortex Documentation</em>. <a href="https://cortexmetrics.io/docs/guides/shuffle-sharding/">https://cortexmetrics.io/docs/guides/shuffle-sharding/</a></li>
  <li id="ref-6">Grafana Labs (2024). "Configure Shuffle Sharding." <em>Grafana Mimir Documentation</em>. <a href="https://grafana.com/docs/mimir/latest/configure/configure-shuffle-sharding/">https://grafana.com/docs/mimir/latest/configure/configure-shuffle-sharding/</a></li>
  <li id="ref-7">Kubernetes. "API Priority and Fairness." <em>Kubernetes Documentation</em>. <a href="https://kubernetes.io/docs/concepts/cluster-administration/flow-control/">https://kubernetes.io/docs/concepts/cluster-administration/flow-control/</a></li>
  <li id="ref-8">Ludden, Chris, and Anthony Davis (2023). "Using Temporal at HashiCorp." <em>Replay 2023</em>. <a href="https://temporal.io/resources/on-demand/temporal-hashicorp">https://temporal.io/resources/on-demand/temporal-hashicorp</a></li>
</ol>

---

## Outtakes

MacCárthaigh describes "recursive" shuffle sharding. Shard a caller's caller too, so a misbehaving downstream client burns through only its own slice of the assigned shard ([MacCárthaigh, 2021](https://www.youtube.com/watch?v=xorjrw7Xu7w)).

MacCárthaigh noted that shuffle sharding applied to rate limits, resembles Stochastic Fair Blue, a fairness algorithm for stopping one flow from hogging a shared link ([Feng et al., 2001](https://www.thefengs.com/wuchang/blue/41_2.PDF)).

Infrastructure wasn't the only cost. Route 53 needed 2,000 anycast IP addresses for its virtual name servers, and had to register 512 domains to satisfy TLD glue-record requirements ([MacCárthaigh, 2025](https://news.ycombinator.com/item?id=42975315)).

Route 53 allocates each customer's four name servers from four independent stripes, one per TLD (co.uk (2LD actually), com, net, org). If one of those TLDs has a problem (e.g., TLD key-rollover mistake, DS/DNSKEY mismatch), only one of the four name servers is affected ([MacCárthaigh, 2025](https://news.ycombinator.com/item?id=42975315)).

---

## Changelog

**2026-09-06** Initial release.  
