---
title: "The Tail at Scale"
date: 2026-08-19
publishdate: 2026-08-19
lastmod: 2026-08-23
summary: "Fan a metasearch query out to independent backend providers and tail latency compounds fast. Each fix, hedging, tying, partitioning, canarying, comes with a number, and getting it wrong can hurt more than doing nothing."
tags: ["latency", "reliability", "performance"]
image: /images/the-tail-at-scale.svg
draft: true
---

![Fan a metasearch query out to independent backend providers and tail latency compounds fast. Each fix, hedging, tying, partitioning, canarying, comes with a number, and getting it wrong can hurt more than doing nothing.](/images/the-tail-at-scale.svg)

## The Tail at Scale

At a previous company, a search box turned a user's request into several sub-requests behind the scenes. Algorithmic results came from one partner index, ads and specialized answers came from others, as well as its own index of results for backfill. The page couldn't render until the slowest backend reported back.

It's a common pattern. DuckDuckGo runs a version of it today with web links largely from Bing and other partners{{< cite 1 "DuckDuckGo. Where do DuckDuckGo search results come from? DuckDuckGo Help Pages." >}}. Dogpile built a whole business on the same idea years earlier. Look at results from several engines at once, eliminate duplicates, and hand back one list{{< cite 2 "Dogpile. About Dogpile. Dogpile.com." >}}.

Each backend answers on its own schedule. One partner might be having a slow moment. Another might be rate-limiting. The operator's own index might be mid-crawl on a busy shard. A single query has the bad luck to depend on whichever one stumbles that second.

Jeff Dean and Luiz Barroso named this problem at Google. They called it the tail at scale, the tendency of rare, individually harmless slowdowns to become a near-certain property of the system once enough machines sit in one request's path, and worked out why it's so hard to avoid{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

## The maths is brutal

Imagine a backend that answers in 100 milliseconds most of the time but has a 99th-percentile (p99) latency of one second. One request in a hundred is slow. Query just that one backend and you'll rarely notice.

Depend on five backends for a single results page instead, and the page waits on whichever one is slowest. The chance at least one is slow is (1 - 0.99^n), where n is how many backends the page touches. At n = 5, that's about 4.9%, already worse than any single source alone. Google's own internal systems push this further, since a single search query there fans out across far more than five machines. At n = 100 servers, the odds climb to 63.4%{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. At n = 1000, it's 99.99%. A one-in-a-hundred event on a single machine becomes nearly certain once enough of them sit in the critical path.

## Where slowness hides

No single bug makes a backend slow, and the cause is rarely the same one twice{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. Inside a company's own fleet, that cause is at least visible, shared cores, a background job, a garbage collection pause. Behind a third-party API, a metasearch engine doesn't get to see the cause at all, only the timeout.

A partner provider has its own load, its own maintenance windows, its own bad day, and none of it coordinates with the metasearch operator or with any other partner source. Something on the other side of one of these service boundaries is always having a bad moment, and the metasearch engine finds out only when its own response-time runs long.

## Every fix has a dial

Picture a one-second network blip that cuts the operator off from one of those providers. Requests start timing out, and its hedge logic retries anything that doesn't answer within 150 milliseconds. When the network heals, every retry lands on that provider at once, on top of its normal traffic.

That's not just a momentary annoyance. Bronson, Aghayev, Charapko, and Zhu describe this same shape of failure in a paper about a database-backed web app with its own fixed one-second retry timeout. Once retries push load past what the database can absorb, latency climbs, more requests cross that mark, and more retries pile on. The system "has no goodput because every database query times out," and stays that way, not for the ten seconds their outage lasted, but until someone cuts the load or changes the policy by hand{{< cite 4 "Bronson, Nathan, Abutalib Aghayev, Aleksey Charapko, and Timothy Zhu (2021). Metastable Failures in Distributed Systems. HotOS 2021." >}}.

Nothing about the retry logic was wrong. Retrying a timed-out request reads as an obviously good idea in a code review. What a review rarely catches is the constant attached to it, 150 milliseconds, applied to every request the instant the network came back. A slower retry, or a cap on in-flight requests to that provider, would have given the database room to drain the backlog instead of drowning in it. The number decides whether the system recovers on its own.

Hedged requests, tied requests, micro-partitions, and canary requests all show up in Dean and Barroso's paper on tail latency, and all four work the same way underneath. Sample repeatedly, at some threshold you pick{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. That threshold is never free.

A postmortem rarely blames the number. It blames "the hedging," or "the retry logic," or "the partitioning scheme," as if the technique itself were the defect. The technique was fine. Somebody picked 1 second instead of 100 milliseconds, or 2 partitions instead of 20, and that choice shipped. The sections below cover each technique, what it does, how to set its number from something measurable, and what breaks at each end of getting it wrong.

## Set the hedge delay

Inside Google's own fleet, a hedge is a second request to a second replica holding the same data, cancel whichever answers second. Across independent providers there's no second copy of the same provider to call, so the workable version is a deadline. Give each backend a budget based on its own expected latency, and render the page without any source that blows past it. The mechanism changes, but the principle survives the swap, bound the wait instead of trusting every source to finish{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

Base that deadline on the backend's measured latency distribution, its 95th percentile is a reasonable start, and fire the duplicate only once a request crosses it. Dean and Barroso picked that threshold because it limits the extra load to roughly 5 percent while cutting off nearly all of the tail{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}, the whole reason the technique is cheap.

Set it too low, using the median instead of a real percentile, and half of all requests trip the hedge instead of one in twenty. Extra load jumps from 5 percent toward 50, on a backend that was never struggling. During a real slowdown, that's what turns a blip into the metastable spiral from the retry-storm scene above, retries sustaining the failure past the event that triggered it{{< cite 4 "Bronson, Nathan, Abutalib Aghayev, Aleksey Charapko, and Timothy Zhu (2021). Metastable Failures in Distributed Systems. HotOS 2021." >}}.

Set the delay too high, past where the backend's tail lives, and the hedge fires so late the original request usually finishes first anyway. You still pay for it, in code complexity and the rare cases it triggers, without shortening the tail.

## Know when to race

Some sources overlap. If both the operator's own index and a partner source can plausibly answer the same general web query, tied requests are Dean and Barroso's mechanism, fire both, and let whichever finishes first cancel the other{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. Redundant coverage only pays off if you race it, not just double-check it. The number here isn't a delay or a percentile, it's the overlap rate itself, worth measuring instead of assuming any two web-results sources are close enough to bother racing. Race two sources that rarely agree, a general web index against a specialized local-language provider, and every query doubles cost for a race it was never going to win.

Even between real overlaps, firing both requests at once wastes the redundancy. Dean and Barroso found that with both queues empty, both destinations can start executing before either cancellation arrives, so the client should introduce a small delay, about twice the average network message delay, often under 1 millisecond, before sending the second request{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. Skip it and you've paid for a duplicate that never had a chance to save time, both copies were already running before either side knew to stop.

The delay only works because it's short enough to be invisible to the person waiting. Set it too long and the tied request stops being a race, becoming a second hedge that rarely beats the original. You'd have been better off skipping the delay entirely.

## Choose the partition count

This one belongs to the part of the stack a metasearch operator owns outright. The operator's own index has to be sharded across many more machines than exist. Dean and Barroso's own example uses 20 partitions per machine, enough that losing one sheds its load in roughly 5-percent increments across many neighbors, instead of dumping it all on whichever server is left holding it{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. Calling an outside provider doesn't change here. This is about not repeating the same mistake inside infrastructure you control.

Go too coarse, one partition per machine, and a slow shard is indistinguishable from a slow machine. The technique buys nothing.

Go too fine, and every partition carries its own bookkeeping, its own place in whatever system tracks where it lives. Twenty per machine is a rounding error against that overhead. Two thousand isn't, every rebalance means updating thousands of tiny assignments instead of dozens of larger ones, and the system spends more time reasoning about where data lives than serving it. That overhead eventually grows faster than the load-shedding benefit, and you're paying for smoothness nobody asked for.

## Size the canary slice

Before trusting a new backend across all live traffic, send it a slice of real queries first{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. A canary must be big enough to catch a real problem and small enough that a bad one doesn't hurt more people than skipping it would. Send a new backend one query in a thousand, and a provider that mishandles a whole category, local-language results, say, might not show up in the canary before the same bug ships to everyone else. Send it one query in ten instead, and a broken backend now breaks a tenth of your traffic before anyone notices the canary itself was the warning.

There's no universal answer, only a tradeoff between catching a bad backend fast and limiting how many users a bad canary touches doing it. A narrow category, say one language among dozens, needs a large enough slice that a query in that category shows up before you'd otherwise notice the failure. A backend serving every query equally can use a much thinner slice, since any problem it has shows up in every sample instead of hiding in one.

## Common mistakes

**You treat the average as the experience.** The mean response time hides the tail completely. For a fanned-out page the p99 is the real contract, which is why Amazon's Dynamo wrote its service-level agreements at the p99 instead of the average{{< cite 5 "DeCandia, Giuseppe, et al. (2007). Dynamo: Amazon's Highly Available Key-value Store. SOSP 2007." >}}.

**You average percentiles across sources.** You cannot take each backend's p99 and average them into a single p99. Percentiles don't add. Keep mergeable summaries (e.g., t-digest{{< cite 6 "Dunning, Ted, and Otmar Ertl (2019). Computing Extremely Accurate Quantiles Using t-Digests. arXiv." >}}) per source and combine those instead.

**You hedge with no budget.** Fire duplicate requests too eagerly and you double your own load right when a backend is already struggling. This is a different mistake than picking the wrong percentile. Even a well-chosen threshold can still add up to more duplicate traffic than the system can absorb if nothing caps the total. In 2015 a brief network blip left Amazon's DynamoDB storage servers all retrying for their metadata at once, and the retries pinned the metadata service in overload for hours after the blip itself was gone. AWS got it back only by pausing traffic to shed the load{{< cite 7 "Amazon Web Services (2015). Summary of the Amazon DynamoDB Service Disruption and Related Impacts in the US-East Region." >}}.

**You measure it wrong.** If your load tester waits for a slow response before sending the next request, it never issues the requests that would have piled up behind the stall, so your measured p99 hides the worst of the tail{{< cite 8 "Tene, Gil (2015). How NOT to Measure Latency. InfoQ." >}}.

## Put it into practice

Start by looking at the right number. Pull up your results page and chart its p99 and p99.9 latency, not its mean, next to the same percentiles for whichever single backend you can measure, a partner's, your own crawler's, whatever you have visibility into. The gap between the page and that one backend is the tail, and it grows every time you add another source to the page.

None of the four numbers behind hedging, tying, partitioning, and canarying should come from a guess either. Pull the latency distribution for the backend you're hedging, the overlap rate between sources you're tying, the failure history for a partition scheme, before you commit to a count.

Start with the one most likely to bite you first. If you already hedge or retry automatically, check what happens to that backend's load if every in-flight request trips at once, the way a real blip triggers them. If that number is one the backend can't survive, you don't have a hedge, you have a metastable failure waiting for a trigger.

Then write the number down where a future engineer will see it, next to the reasoning that produced it, not just the value. "95th percentile, measured against the last 30 days of this backend's latency" survives a system changing underneath it. "150 milliseconds" doesn't, it's the kind of constant that outlives whoever chose it, still firing years after the backend it was tuned for got replaced.

---

## References

<ol class="references">
  <li id="ref-1">DuckDuckGo. "Where do DuckDuckGo search results come from?" <em>DuckDuckGo Help Pages</em>. <a href="https://duckduckgo.com/duckduckgo-help-pages/results/sources">https://duckduckgo.com/duckduckgo-help-pages/results/sources</a></li>
  <li id="ref-2">Dogpile. "About Dogpile." <em>Dogpile.com</em>. <a href="https://www.dogpile.com/about">https://www.dogpile.com/about</a></li>
  <li id="ref-3">Dean, Jeffrey, and Luiz André Barroso (2013). "The Tail at Scale." <em>Communications of the ACM</em>, 56(2). <a href="https://www.barroso.org/publications/TheTailAtScale.pdf">https://www.barroso.org/publications/TheTailAtScale.pdf</a></li>
  <li id="ref-4">Bronson, Nathan, Abutalib Aghayev, Aleksey Charapko, and Timothy Zhu (2021). "Metastable Failures in Distributed Systems." <em>HotOS 2021</em>. <a href="https://sigops.org/s/conferences/hotos/2021/papers/hotos21-s11-bronson.pdf">https://sigops.org/s/conferences/hotos/2021/papers/hotos21-s11-bronson.pdf</a></li>
  <li id="ref-5">DeCandia, Giuseppe, et al. (2007). "Dynamo: Amazon's Highly Available Key-value Store." <em>SOSP 2007</em>. <a href="https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf">https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf</a></li>
  <li id="ref-6">Dunning, Ted, and Otmar Ertl (2019). "Computing Extremely Accurate Quantiles Using t-Digests." <em>arXiv</em>. <a href="https://arxiv.org/abs/1902.04023">https://arxiv.org/abs/1902.04023</a></li>
  <li id="ref-7">Amazon Web Services (2015). "Summary of the Amazon DynamoDB Service Disruption and Related Impacts in the US-East Region." <a href="https://aws.amazon.com/message/5467D2/">https://aws.amazon.com/message/5467D2/</a></li>
  <li id="ref-8">Tene, Gil (2015). "How NOT to Measure Latency." <em>InfoQ</em>. <a href="https://www.infoq.com/presentations/latency-response-time/">https://www.infoq.com/presentations/latency-response-time/</a></li>
</ol>

---

## Outtakes

Amazon ran A/B tests delaying pages in 100-millisecond increments and found that even small delays caused substantial, costly drops in revenue ([Linden, 2006](https://glinden.blogspot.com/2006/11/marissa-mayer-at-web-20.html)).

MapReduce coined "straggler" for a machine grinding through the last few tasks, fixed by scheduling a duplicate run near the end and keeping whichever finished first ([Dean and Ghemawat, 2004](https://www.usenix.org/conference/osdi-04/mapreduce-simplified-data-processing-large-clusters)). Duplicate execution cut one Google sort benchmark's runtime by about 31%.

---

## Changelog

**2026-08-23** Moved the two-second blip and retry-storm scene out of the intro and into "Every fix has a dial," where it's actually used, so the section titled after the post no longer implies it's explaining the tail-latency mechanism before "The math is brutal" actually does. Also named the term itself in "The math is brutal," the post's title and headings used "the tail at scale" throughout but the prose never actually said that's what Dean and Barroso called it.  
**2026-08-22** Merged `the-tail-at-scale-0.md`'s concept-level explainer (what a metasearch engine does, the compounding math, where slowness hides, Common Mistakes) into this post's tuning-focused deep dive, so each technique section now explains what it is and how to tune it in one place instead of two separate articles. References renumbered and consolidated to 8 sources. `the-tail-at-scale-0.md` left in place, unmerged content there is now duplicated here. Moved the two-second blip and 500-millisecond retry example into the intro, and turned "Every fix has a dial" into a callback to it instead of retelling the same scene. Reframed the intro's opening scene as an unnamed former employer rather than DuckDuckGo itself, kept DuckDuckGo and Dogpile as a separate, cited real-world grounding paragraph, and genericized the "Bing"/"DuckDuckBot" stand-ins used throughout the rest of the post so they no longer imply the anonymized operator is literally DuckDuckGo.  
**2026-08-21** Rewrote the post around a metasearch engine (DuckDuckGo, Dogpile) fanning queries out to independent backend providers, changed the opening scene's blip and retry numbers, ran an unslop pass, converted headings to sentence case, and tightened prose across two editing passes.  
**2026-08-19** Initial release.  
