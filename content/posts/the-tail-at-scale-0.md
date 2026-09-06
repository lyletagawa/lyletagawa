---
title: "The Tail at Scale"
date: 2026-08-19
publishdate: 2026-08-19
lastmod: 2026-08-23
summary: "Tail latency compounds with fan-out. A metasearch engine querying a handful of backend providers already feels it, and Google's own systems show how much worse it gets at scale."
tags: ["latency", "reliability", "performance"]
image: /images/the-tail-at-scale.svg
draft: true
---

![Tail latency compounds with fan-out. A metasearch engine querying a handful of backend providers already feels it, and Google's own systems show how much worse it gets at scale.](/images/the-tail-at-scale.svg)

## The tail at scale

Type a query into DuckDuckGo and one search request turns into several backend requests. Algorithmic results come largely from Bing while ads and specialized answers come from several other partners{{< cite 1 "DuckDuckGo. Where do DuckDuckGo search results come from? DuckDuckGo Help Pages." >}}. It's the same idea Dogpile built a business on years earlier. Look at results from several engines at once, eliminate duplicates, and hand back one list{{< cite 2 "Dogpile. About Dogpile. Dogpile.com." >}}. The page can't render until the slowest backend reports back.

Each backend answers on its own schedule, and none of it is the metasearch engine's to control. Bing might be having a slow moment. A partner API might be rate-limiting. DuckDuckBot's own index might be mid-crawl on a busy shard. A single query has the bad luck to depend on whichever one stumbles that second.

## The maths is brutal

Jeff Dean and Luiz Barroso named this problem at Google. They called it the tail at scale, the tendency of rare, individually harmless slowdowns to become a near-certain property of the system once enough machines sit in one request's path, and worked out why it's so hard to avoid{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. The same math applies whether the slow party is your own shard or someone else's API.

Imagine a backend that answers in 10 milliseconds most of the time but has a 99th-percentile (p99) latency of 1 second. One request in a hundred is slow. Query just that one backend and you'll rarely notice.

Depend on five backends for a single results page instead, and the page waits on whichever one is slowest. The chance at least one is slow is (1 - 0.99^n), where n is how many backends the page touches. At n = 5, that's about 4.9%, already worse than any single source alone. Google's own internal systems push this further, since a single search query there fans out across far more than five machines. At n = 100 servers, the odds climb to 63.4%{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. At n = 1,000, it's 99.99%. A one-in-a-hundred event on a single machine becomes nearly certain once enough of them sit in the critical path.

## Where slowness hides

No single bug makes a backend slow, and the cause is rarely the same one twice{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. Inside a company's own fleet, that cause is at least visible, shared cores, a background job, a garbage collection pause. Behind a third-party API, a metasearch engine doesn't get to see the cause at all, only the timeout.

Bing has its own load, its own maintenance windows, its own bad day, and none of it coordinates with DuckDuckGo or with any other partner source. Something on the other side of one of these service boundaries is always having a bad moment, and the metasearch engine finds out only when its own response clock runs long.

## Route around the tail

You can't make someone else's API faster, so borrow Dean and Barroso's playbook for routing around slow replies inside a single company's fleet{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

**Hedged requests.** Inside Google's own fleet, a hedge is a second request to a second replica holding the same data, cancel whichever answers second. Across independent providers there's no second Bing to call, so the workable version is a deadline. Give each backend a budget based on its own expected latency, and render the page without any source that blows past it. The mechanism changes, but the principle survives the swap, bound the wait instead of trusting every source to finish{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

**Tied requests.** Some sources overlap. If both DuckDuckBot's own index and a partner source can plausibly answer the same general web query, use Dean and Barroso's tied-request mechanism. Fire both requests and let whichever finishes first cancel the other{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. Redundant coverage only pays off if you race it, not just double-check it.

**Micro-partitions.** This one belongs to the part of the stack a metasearch engine actually owns. DuckDuckBot's crawl index has to be sharded across many more machines than exist, say 20 slices per server{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}, so one slow shard scatters its load across many neighbors instead of stalling the whole crawl. It doesn't change how you call Bing. This is about not repeating the same mistake inside infrastructure you control.

**Canary requests.** Before trusting a new backend across all live traffic, send it a slice of real queries first{{< cite 3 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}. A provider that chokes on one category of query, local-language results, say, shows up in a canary slice long before it shows up as a fleet-wide slowdown.

## Common mistakes

**You treat the average as the experience.** The mean response time hides the tail completely. For a fanned-out page the p99 is the real contract, which is why Amazon's Dynamo wrote its service-level agreements at the p99 instead of the average{{< cite 4 "DeCandia, Giuseppe, et al. (2007). Dynamo: Amazon's Highly Available Key-value Store. SOSP 2007." >}}.

**You average percentiles across sources.** You cannot take each backend's p99 and average them into a single p99. Percentiles don't add. Keep mergeable summaries (e.g., t-digest{{< cite 5 "Dunning, Ted, and Otmar Ertl (2019). Computing Extremely Accurate Quantiles Using t-Digests. arXiv." >}}) per source and combine those instead.

**You hedge with no budget.** Fire duplicate requests too eagerly and you double your own load right when a backend is already struggling. In 2015 a brief network blip left Amazon's DynamoDB storage servers all retrying for their metadata at once, and the retries pinned the metadata service in overload for hours after the blip itself was gone. AWS got it back only by pausing traffic to shed the load{{< cite 6 "Amazon Web Services (2015). Summary of the Amazon DynamoDB Service Disruption and Related Impacts in the US-East Region." >}}.

**You measure it wrong.** If your load tester waits for a slow response before sending the next request, it never issues the requests that would have piled up behind the stall, so your measured p99 hides the worst of the tail{{< cite 7 "Tene, Gil (2015). How NOT to Measure Latency. InfoQ." >}}.

## Put it into practice

Start by looking at the right number. Pull up your results page and chart its p99 and p99.9 latency, not its mean, next to the same percentiles for whichever single backend you can measure, Bing's, your own crawler's, whatever you have visibility into. The gap between the page and that one backend is the tail, and it grows every time you add another source to the page.

Once you can see it, set a deadline instead of waiting on every source. Most slow responses come from one backend having a momentary stall, not a systemic failure, and a page that renders without the straggler beats a page that waits for everyone.

---

## References

<ol class="references">
  <li id="ref-1">DuckDuckGo. "Where do DuckDuckGo search results come from?" <em>DuckDuckGo Help Pages</em>. <a href="https://duckduckgo.com/duckduckgo-help-pages/results/sources">https://duckduckgo.com/duckduckgo-help-pages/results/sources</a></li>
  <li id="ref-2">Dogpile. "About Dogpile." <em>Dogpile.com</em>. <a href="https://www.dogpile.com/about">https://www.dogpile.com/about</a></li>
  <li id="ref-3">Dean, Jeffrey, and Luiz André Barroso (2013). "The Tail at Scale." <em>Communications of the ACM</em>, 56(2). <a href="https://www.barroso.org/publications/TheTailAtScale.pdf">https://www.barroso.org/publications/TheTailAtScale.pdf</a></li>
  <li id="ref-4">DeCandia, Giuseppe, et al. (2007). "Dynamo: Amazon's Highly Available Key-value Store." <em>SOSP 2007</em>. <a href="https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf">https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf</a></li>
  <li id="ref-5">Dunning, Ted, and Otmar Ertl (2019). "Computing Extremely Accurate Quantiles Using t-Digests." <em>arXiv</em>. <a href="https://arxiv.org/abs/1902.04023">https://arxiv.org/abs/1902.04023</a></li>
  <li id="ref-6">Amazon Web Services (2015). "Summary of the Amazon DynamoDB Service Disruption and Related Impacts in the US-East Region." <a href="https://aws.amazon.com/message/5467D2/">https://aws.amazon.com/message/5467D2/</a></li>
  <li id="ref-7">Tene, Gil (2015). "How NOT to Measure Latency." <em>InfoQ</em>. <a href="https://www.infoq.com/presentations/latency-response-time/">https://www.infoq.com/presentations/latency-response-time/</a></li>
</ol>

---

## Outtakes

Amazon ran A/B tests delaying pages in 100-millisecond increments and found that even small delays caused substantial, costly drops in revenue ([Linden, 2006](https://glinden.blogspot.com/2006/11/marissa-mayer-at-web-20.html)).

MapReduce coined "straggler" for a machine grinding through the last few tasks, fixed by scheduling a duplicate run near the end and keeping whichever finished first ([Dean and Ghemawat, 2004](https://www.usenix.org/conference/osdi-04/mapreduce-simplified-data-processing-large-clusters)). Duplicate execution cut one Google sort benchmark's runtime by about 31%.

---

## Changelog

**2026-08-23** Sentence-cased section headings to match site style. Named the term explicitly in "The maths is brutal," the post used "the tail at scale" throughout but never stated what Dean and Barroso called it. Tightened a few clunky sentences in "Route around the tail" for clarity. Matched the n = 100 figure to the hero image's 63.4% instead of a rounded 63%, converted "one second" to "1 second" for AP-style consistency with the rest of the post's units, and varied a phrase repeated three times in the mitigation section.  
**2026-08-21** Rewrote the post around a metasearch engine (DuckDuckGo, Dogpile) fanning queries out to independent backend providers, instead of a single company's internal server fleet, and adapted each mitigation technique to note which ones transfer to third-party APIs and which stay specific to infrastructure you own.  
**2026-08-19** Initial release.  
