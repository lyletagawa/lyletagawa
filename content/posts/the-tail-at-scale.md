---
title: "The Tail at Scale"
date: 2026-08-28
publishdate: 2026-08-28
lastmod: 2026-08-28
summary: "Tail latency turns a one-in-a-hundred slow server into the median experience once a request fans out to a thousand of them. Hedging and tying route around it, but only if you tune the number right."
tags: ["latency", "reliability", "performance"]
image: /images/the-tail-at-scale.svg
draft: true
---

![Tail latency turns a one-in-a-hundred slow server into the median experience once a request fans out to a thousand of them. Hedging and tying route around it, but only if you tune the number right.](/images/the-tail-at-scale.svg)

## The tail at scale

Type a query into Google's search box and one request turns into thousands. The query fans out to leaf servers, each one searching its own slice of the web index. The page can't render until the slowest slice reports back.

Most of those servers answer in a blink. A handful don't. One is busy with garbage collection, another shares a CPU core with a noisy neighbor, a third just picked up a backup job. You never chose those servers to be slow. They took turns being slow on their own schedule, and this one request had the bad luck to touch all of them at once.

## The math is brutal

Jeff Dean and Luiz Barroso named this problem at Google and worked out why it's so hard to dodge{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. They called it the tail at scale, the way a rare, harmless slowdown on one machine turns into a near-certain property of the whole system once enough machines sit in a single request's path.

Picture a server that answers in 10 milliseconds most of the time but has a 99th-percentile (p99) latency of one second. One request in a hundred is slow. Run everything on that single server and 99 percent of users never notice.

Now fan a request out across many servers and wait for all of them. A request comes back fast only if every server it touches dodges its slow path. The chance at least one is slow is 1 - 0.99^n, where n is how many servers the request touches. At n = 10, that's 9.6 percent. At n = 100, it's 63.4 percent{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. At n = 1,000, it's 99.99 percent, and the fast path has effectively vanished. The rare event at a single server became the common case for the whole system.

## Where slowness hides

No single bug makes a server slow. The slowness comes from everywhere at once, and rarely the same place twice{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. A server shares its cores, caches, and memory bandwidth with other tenants. Background jobs wake up to compact logs and run health checks. A big request lands in the queue ahead of yours. The garbage collector stops the world for a few hundred milliseconds. Each event is brief and harmless on its own. Across thousands of servers, something is always having a bad moment.

## Set the hedge delay

You can't delete the variability, so you route around it. The simplest technique is a hedged request. Send the request to one replica. If it hasn't answered by the time it crosses a chosen percentile of that replica's expected latency, fire a copy at a second replica and take whichever finishes first, canceling the other{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

The delay is the whole technique. Dean and Barroso based theirs on the 95th percentile of the replica's own latency distribution, and in a benchmark reading 1,000 keys, a 10-millisecond hedge cut the 99.9th-percentile latency from 1,800 milliseconds to 74 while adding just 2 percent more requests{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

Set the delay too low, using the median instead of a real percentile, and half of all requests trip the hedge instead of one in twenty. Extra load jumps from 5 percent toward 50, on servers that were never struggling in the first place. Set it too high, past where the tail lives, and the hedge fires so late the original request usually finishes first anyway. You still pay for the extra code path without shortening the tail.

## Know when to race

Hedging waits before it duplicates. A tied request duplicates up front, sending the same request to two replicas at once and letting whichever starts work first cancel the other{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

Fire both copies the instant the request arrives and you waste the redundancy. With both queues empty, both replicas can start executing before either cancellation message arrives, so the client should wait roughly twice the average network delay, often under a millisecond, before sending the second copy{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. Skip that wait and you've paid for a duplicate that never had a chance to save time. Stretch it out and the tied request stops racing, becoming a second hedge that rarely beats the original.

## Two more levers

**Micro-partitions.** Cut the data into far more pieces than you have machines, dozens of slices per server. When one machine slows down or dies, its slices scatter across many neighbors instead of dumping onto a single unlucky one{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

**Canary requests.** Before fanning a request out to thousands of servers, send it to one or two first. If they choke on it, you found the poison query without taking down the whole fleet{{< cite 1 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

## Common mistakes

**You treat the average as the experience.** The mean response time hides the tail completely. For a fanned-out service, the p99 is the real contract, which is why Amazon's Dynamo wrote its service-level agreements at the 99.9th percentile instead of the average{{< cite 2 "DeCandia, Giuseppe, et al. (2007). Dynamo: Amazon's Highly Available Key-value Store. SOSP 2007." >}}.

**You average percentiles across servers.** You cannot take each server's p99 and average them into a system p99. Percentiles don't add, so a number built that way is fiction. Keep mergeable summaries per server, a t-digest works well{{< cite 3 "Dunning, Ted, and Otmar Ertl (2019). Computing Extremely Accurate Quantiles Using t-Digests. arXiv." >}}, and combine those instead.

**You hedge with no budget.** Fire duplicate requests too eagerly and you double your own load right when the system is already struggling. In 2015 a brief network blip left Amazon's DynamoDB storage servers all retrying for their metadata at once, and the retries pinned the metadata service in overload for hours after the blip itself was gone. AWS got it back only by pausing traffic to shed the load{{< cite 4 "Amazon Web Services (2015). Summary of the Amazon DynamoDB Service Disruption and Related Impacts in the US-East Region." >}}.

## Put it into practice

Start by looking at the right number. Pull up your highest-fan-out endpoint and chart its 99th and 99.9th percentile latency, not its mean. Then chart a single backend's latency next to it. The gap between the two is the tail, and the wider your fan-out, the wider that gap grows.

Once you can see it, add a hedge with a delay pulled from that chart, not a guess. Most slow responses come from a healthy server caught in a momentary stall, and a second copy of the request routes around it for a couple percent more load. Measure the overhead, watch it stay small, and only then reach for bigger machines.

---

## References

<ol class="references">
  <li id="ref-1">Dean, Jeffrey, and Luiz André Barroso (2013). "The Tail at Scale." <em>Communications of the ACM</em>, 56(2). <a href="https://www.barroso.org/publications/TheTailAtScale.pdf">https://www.barroso.org/publications/TheTailAtScale.pdf</a></li>
  <li id="ref-2">DeCandia, Giuseppe, et al. (2007). "Dynamo: Amazon's Highly Available Key-value Store." <em>SOSP 2007</em>. <a href="https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf">https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf</a></li>
  <li id="ref-3">Dunning, Ted, and Otmar Ertl (2019). "Computing Extremely Accurate Quantiles Using t-Digests." <em>arXiv</em>. <a href="https://arxiv.org/abs/1902.04023">https://arxiv.org/abs/1902.04023</a></li>
  <li id="ref-4">Amazon Web Services (2015). "Summary of the Amazon DynamoDB Service Disruption and Related Impacts in the US-East Region." <a href="https://aws.amazon.com/message/5467D2/">https://aws.amazon.com/message/5467D2/</a></li>
</ol>

---

## Outtakes

Amazon ran A/B tests delaying pages in 100-millisecond increments and found that even small delays caused substantial, costly drops in revenue ([Linden, 2006](https://glinden.blogspot.com/2006/11/marissa-mayer-at-web-20.html)).

MapReduce coined "straggler" for a machine grinding through the last few tasks, fixed by scheduling a duplicate run near the end and keeping whichever finished first ([Dean and Ghemawat, 2004](https://www.usenix.org/conference/osdi-04/mapreduce-simplified-data-processing-large-clusters)). Duplicate execution cut one Google sort benchmark's runtime by about 31%.

---

## Changelog

**2026-08-28** Initial release.
