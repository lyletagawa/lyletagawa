---
title: "The Tail at Scale"
date: 2026-06-27
publishdate: 2026-06-27
lastmod: 2026-06-27
summary: "A single slow server is a rounding error. Fan a request out to a hundred of them and that rounding error becomes the median user experience."
tags: ["latency", "reliability", "performance"]
image: /images/the-tail-at-scale.png
draft: true
---

![A single slow server is a rounding error. Fan a request out to a hundred of them and that rounding error becomes the median user experience.](/images/the-tail-at-scale.png)
*Image: placeholder.*

## The Tail at Scale

Your search box feels instant. Type a query, get results in 200 milliseconds, every time. Behind that box a single request fans out to a few thousand servers, each one searching its own slice of the index. The page can't render until the slowest slice reports back.

Most of those servers answer in a blink. A handful don't. One is busy with garbage collection, another is sharing a CPU core with a noisy neighbor, a third just got handed a background backup job. You never chose those servers to be slow. They took turns being slow on their own schedule, and your one request had the bad luck to touch all of them at once.

## The Math Is Brutal

Jeff Dean and Luiz Barroso named this problem at Google and worked out why it's so hard to dodge. Picture a server that answers in 10 milliseconds most of the time but has a 99th-percentile latency of one second. One request in a hundred is slow. Run everything on that single server and 99 percent of users never notice{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

Now fan a request out to 100 of those servers and wait for all of them. A request comes back fast only if every server it touches dodges its slow path. The odds of that are 0.99 to the 100th power, about 37 percent, so 63 percent of requests take more than a second{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

Watch how fast it compounds. The chance a request is slow is 1 - 0.99^n, where n is the number of servers it touches. At n = 10, that's 1 - 0.99^10 = 9.6 percent. At n = 100, it climbs to 1 - 0.99^100 = 63 percent. At n = 1,000, it's 1 - 0.99^1000 = 99.99 percent, and the fast path has effectively vanished. The rare event at a single server became the common case for the whole system.

## Where Slowness Hides

No single bug makes a server slow. The slowness comes from everywhere at once, and never the same place twice{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

A server shares its cores, caches, and memory bandwidth with other tenants. Background daemons wake up to compact logs and run health checks. A large request lands in the queue ahead of yours. The garbage collector stops the world for a few hundred milliseconds. The chip throttles itself to stay inside a power budget. Each event is brief and harmless on its own{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. Across thousands of servers, something is always having a bad moment.

## Route Around the Tail

You can't delete the variability, so you work around it the way you already work around hardware faults{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

**Hedged requests.** Send the request to one server. If it hasn't answered by the time it crosses the 95th percentile of expected latency, fire a copy at a second server and take whichever finishes first. The duplicate work stays small because most requests never trip the threshold. In a Google benchmark reading 1,000 keys, a 10-millisecond hedge cut the 99.9th-percentile latency from 1,800 milliseconds to 74 while adding just 2 percent more requests{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

**Tied requests.** Hedging waits before it duplicates. Tied requests duplicate up front and let the two servers cancel each other the instant one of them starts the work. You pay for the race, but you start it immediately, which trims the tail further when even the short wait hurts{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

**Micro-partitions.** Cut the data into far more pieces than you have machines, dozens of slices per server. When one machine slows down or dies, its slices scatter across many neighbors instead of dumping onto a single unlucky one{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

**Canary requests.** Before fanning a request out to thousands of servers, send it to one or two first. If they choke on it, you found the poison query without taking down the whole fleet{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

## Common Mistakes

**You treat the average as the experience.** The mean response time hides the tail completely. For a fanned-out service the 99.9th percentile is the real contract, which is why Amazon's Dynamo wrote its service-level agreements at the 99.9th percentile instead of the average{{< cite 2 "DeCandia, Giuseppe, et al. (2007). Dynamo: Amazon's Highly Available Key-value Store. SOSP 2007." >}}.

**You average percentiles across servers.** You cannot take each server's p99 and average them into a system p99. Percentiles don't add, so a number assembled that way is fiction. Keep mergeable summaries per server and combine those instead{{< cite 3 "Dunning, Ted, and Otmar Ertl (2019). Computing Extremely Accurate Quantiles Using t-Digests. arXiv." >}}.

**You hedge with no budget.** Fire duplicate requests too eagerly and you double your own load right when the system is already struggling. The 95th-percentile delay before the secondary request exists for a reason, holding the extra traffic near 5 percent{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. The unbudgeted version is a known killer. In 2015 a brief network blip left Amazon's DynamoDB storage servers all retrying for their metadata at once, and the retries pinned the metadata service in overload for hours after the blip itself was gone. AWS got it back only by pausing traffic to shed the load{{< cite 4 "Amazon Web Services (2015). Summary of the Amazon DynamoDB Service Disruption and Related Impacts in the US-East Region." >}}.

## Put It Into Practice

Start by looking at the right number. Pull up your highest-fan-out endpoint and chart its 99th and 99.9th percentile latency, not its mean. Then chart a single backend's latency next to it. The gap between the two is the tail, and the wider your fan-out, the wider that gap grows.

Once you can see it, you can fight it. Add a hedge with a sane delay before you reach for bigger machines. Most slow responses come from a healthy server caught in a momentary stall, and a second copy of the request just routes around it. Measure the overhead and keep it small.

Pick one endpoint this week. Find out how many servers a single request touches, then check whether your slowest 1 in 1,000 backend responses are quietly setting your p99. They probably are.

## Go Further

**Coordinated omission.** If your load tester waits for a slow response before sending the next request, it never issues the requests that would have piled up behind the stall, so your measured p99 hides the worst of the tail{{< cite 5 "Tene, Gil (2015). How NOT to Measure Latency. InfoQ." >}}.

**When retries make it worse.** Retries and hedges that fire too aggressively can add enough load to keep a system stuck in overload even after the original trigger is gone, a trap worth understanding before you turn up the duplication{{< cite 6 "Bronson, Nathan, et al. (2021). Metastable Failures in Distributed Systems. HotOS 2021." >}}.

**Measuring the tail honestly.** Why percentiles refuse to average, and the sketch structures that let you merge them across a fleet, is the whole subject of [Percentiles Don't Add Up](/posts/t-digest/){{< cite 3 "Dunning, Ted, and Otmar Ertl (2019). Computing Extremely Accurate Quantiles Using t-Digests. arXiv." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Dean, Jeffrey, and Luiz André Barroso (2013). "The Tail at Scale." <em>Communications of the ACM</em>, 56(2). <a href="https://www.barroso.org/publications/TheTailAtScale.pdf">https://www.barroso.org/publications/TheTailAtScale.pdf</a></li>
  <li id="ref-2">DeCandia, Giuseppe, et al. (2007). "Dynamo: Amazon's Highly Available Key-value Store." <em>SOSP 2007</em>. <a href="https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf">https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf</a></li>
  <li id="ref-3">Dunning, Ted, and Otmar Ertl (2019). "Computing Extremely Accurate Quantiles Using t-Digests." <em>arXiv</em>. <a href="https://arxiv.org/abs/1902.04023">https://arxiv.org/abs/1902.04023</a></li>
  <li id="ref-4">Amazon Web Services (2015). "Summary of the Amazon DynamoDB Service Disruption and Related Impacts in the US-East Region." <a href="https://aws.amazon.com/message/5467D2/">https://aws.amazon.com/message/5467D2/</a></li>
  <li id="ref-5">Tene, Gil (2015). "How NOT to Measure Latency." <em>InfoQ</em>. <a href="https://www.infoq.com/presentations/latency-response-time/">https://www.infoq.com/presentations/latency-response-time/</a></li>
  <li id="ref-6">Bronson, Nathan, et al. (2021). "Metastable Failures in Distributed Systems." <em>HotOS 2021</em>. <a href="https://sigops.org/s/conferences/hotos/2021/papers/hotos21-s11-bronson.pdf">https://sigops.org/s/conferences/hotos/2021/papers/hotos21-s11-bronson.pdf</a></li>
</ol>

---

## Outtakes

**The number that ships the feature.** Amazon found that every 100 milliseconds of added latency cost about 1 percent in sales, which is why its teams fought over the 99.9th percentile and not the average ([Linden, 2006](https://glinden.blogspot.com/2006/11/marissa-mayer-at-web-20.html)). The average was already fine. The average was never the problem.

**The straggler you scheduled.** A lot of tail latency is self-inflicted ([Dean and Barroso, 2013](https://www.barroso.org/publications/TheTailAtScale.pdf)). The backup job, the log rotation, the metrics flush. Your own infrastructure takes turns slowing down your own requests, politely, on a cron schedule.

**Fault tolerance grew up first.** Dean and Barroso framed tail tolerance as the sequel to fault tolerance. One builds a reliable whole from unreliable parts. The other builds a predictable whole from unpredictable ones ([Dean and Barroso, 2013](https://www.barroso.org/publications/TheTailAtScale.pdf)).

---

## Changelog

**2026-06-27** Initial release.  
