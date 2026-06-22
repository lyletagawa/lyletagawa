---
title: "Age-Based Live Event Caching"
date: 2026-05-11
publishdate: 2026-05-11
lastmod: 2026-06-17
summary: "LRU evicts the segments live viewers need when they rewind. Netflix's patent US 12,621,504 B2 fixes this with segment age and distributed ownership, without bloating every edge server."
tags: ["streaming", "caching", "cdn"]
image: /images/live-event-caching.svg
draft: false
---

![]()

## Age-Based Live Event Caching

Netflix's stream of the Tyson-Paul fight in November 2024 peaked at 65 million concurrent viewers {{< cite 1 "Rayburn, Dan (2023). A List of the Largest Live Streaming Events in History and How They Are Measured. Streaming Media Blog." >}}. When a viewer rewinds their stream by a few minutes, the request targets segments that receive relatively little traffic at the live edge. A standard LRU cache treats low request frequency as a signal to evict, adding latency for those viewers.

The gap between what viewers want and what edge servers preserve is the problem a recent Netflix patent addresses {{< cite 2 "Newton, Christopher Alan (2026). Techniques for Caching Media Content When Streaming Live Events. U.S. Patent 12,621,504 B2." >}}.

## What Is the Live Caching Problem?

Standard CDN edge caching works well for video-on-demand (VOD). Popular content accumulate requests. Niche content gradually drop out of cache. LRU aligns cache incentives with actual demand {{< cite 3 "Hasslinger, Gerhard, et al. (2023). An Overview of Analysis Methods and Evaluation Results for Caching Strategies. Computer Networks, 228: 109583." >}}.

Live events break this alignment. During a live stream, nearly all viewers watch the live edge. Two-minute-old segments receive a fraction of the traffic that two-second-old segments do. LRU interprets sparse access as low value and evicts accordingly. When a viewer scrubs backward (rewinds), the edge server has nothing to serve. It must fetch from origin or a mid-tier cache, adding latency and load to the upstream link {{< cite 4 "Liu, Xiaomei, Joseph Lynch, and Christopher Newton (2025). Netflix Live Origin. Netflix Technology Blog." >}}.

Another approach is to pin every segment on every edge server for the event's duration. That solves scrubbing but scales with the product of stream count, rendition count, and event duration. Memory becomes the new constraint.

Netflix's patent US 12,621,504 B2 describes a third approach {{< cite 2 "Newton, Christopher Alan (2026). Techniques for Caching Media Content When Streaming Live Events. U.S. Patent 12,621,504 B2." >}}.

## The Assignment Approach

Each live event downloadable (a specific encoded rendition at a given bitrate and resolution) is assigned to exactly one edge server per distribution center. The assignment uses a consistent hash on the downloadable's identifier. Every edge server independently computes the same assignment without runtime coordination.

The assigned edge server caches every downloaded segment of its assigned downloadable in a high-priority list. Nothing in that list gets evicted while the live event is active.

Edge servers not assigned to a given downloadable operate differently. They maintain a cutoff threshold, defaulting to five minutes. Segments younger than the threshold are near-live and go into a high-priority list. Segments older than the threshold move to a low-priority list that can be pruned when free cache falls below a threshold.

The result is a two-tier model. Recent segments are available at every edge server in the distribution center. The full event history lives on exactly one server.

## Segment Age as a Control Variable

Age is derived from the last-modified response header returned by the origin server when delivering the segment to the CDN {{< cite 2 "Newton, Christopher Alan (2026). Techniques for Caching Media Content When Streaming Live Events. U.S. Patent 12,621,504 B2." >}}.

When an unassigned edge server receives a segment, it checks the age against the cutoff threshold. Near-live segments go into the high-priority list. Older segments go into the low-priority list, pruned tail-first when free cache falls below a floor.

Segments of assigned downloadables are never classified by age and are never evicted while the event is active.

## Common Mistakes

**Using the same cache policy for live and VOD content.** LRU works for VOD. It fails for live events where access recency and retention value diverge sharply. Running both content types through the same eviction policy means one is served poorly. {{< cite 2 "Newton, Christopher Alan (2026). Techniques for Caching Media Content When Streaming Live Events. U.S. Patent 12,621,504 B2." >}}.

**Fixing the cutoff threshold globally.** The patent's default is five minutes. But, a short-form event and a six-hour broadcast have different scrubbing needs.

**Leaving the assigned server as a single point of failure.** If the assigned server becomes unavailable, full DVR rewind fails until the event ends and VOD segments are available from origin. That failure mode deserves explicit attention in reliability planning.

## Put It Into Practice

Find a storage tier or data pipeline in your system where retention is uniform but demand is not. Determine the age past which queries become rare. Identify which node handles the most historical reads for each series, by consistent hashing or load balancer affinity. Designate that node explicitly as the owner of full depth, extend its retention, and configure the others to drop data past the threshold. That is the assignment approach, applied outside the CDN.

Prometheus is one example. By default every instance keeps the same retention window, but recent samples dominate queries. Assign one instance per metric series to own full retention and configure the rest to serve a short window. Thanos and Mimir formalize this through store tier separation {{< cite 5 "Wilkie, Tom (2019). [PromCon Recap] Two Households, Both Alike in Dignity: Cortex and Thanos. Grafana Labs Blog." >}}{{< cite 6 "Pracucci, Marco and Dimitar Dimitrov (2023). Breaking the Memory Barrier: How Grafana Mimir's Store-Gateway Overcame Out-of-Memory Errors. Grafana Labs Blog." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Rayburn, Dan (2023). "A List of the Largest Live Streaming Events in History and How They Are Measured." <em>Streaming Media Blog</em>, October 16 (updated April 1, 2025). <a href="https://www.streamingmediablog.com/2023/10/largestlivestreaminghistory.html">https://www.streamingmediablog.com/2023/10/largestlivestreaminghistory.html</a></li>
  <li id="ref-2">Newton, Christopher Alan (2026). <em>Techniques for Caching Media Content When Streaming Live Events</em>. U.S. Patent 12,621,504 B2. Filed April 6, 2023. Issued May 5, 2026. <a href="https://patents.justia.com/patent/12621504">https://patents.justia.com/patent/12621504</a></li>
  <li id="ref-3">Hasslinger, Gerhard, Mahshid Okhovatzadeh, Konstantinos Ntougias, Frank Hasslinger, and Oliver Hohlfeld (2023). "An overview of analysis methods and evaluation results for caching strategies." <em>Computer Networks</em>, 228: 109583. <a href="https://arxiv.org/abs/2308.02875">https://arxiv.org/abs/2308.02875</a></li>
  <li id="ref-4">Liu, Xiaomei, Joseph Lynch, and Christopher Newton (2025). "Netflix Live Origin." <em>Netflix Technology Blog</em>, December. <a href="https://netflixtechblog.com/netflix-live-origin-41f1b0ad5371">https://netflixtechblog.com/netflix-live-origin-41f1b0ad5371</a></li>
  <li id="ref-5">Wilkie, Tom (2019). "[PromCon Recap] Two Households, Both Alike in Dignity: Cortex and Thanos." <em>Grafana Labs Blog</em>, November 21. <a href="https://grafana.com/blog/2019/11/21/promcon-recap-two-households-both-alike-in-dignity-cortex-and-thanos/">https://grafana.com/blog/2019/11/21/promcon-recap-two-households-both-alike-in-dignity-cortex-and-thanos/</a></li>
  <li id="ref-6">Pracucci, Marco and Dimitar Dimitrov (2023). "Breaking the memory barrier: How Grafana Mimir's store-gateway overcame out-of-memory errors." <em>Grafana Labs Blog</em>, July 6. <a href="https://grafana.com/blog/2023/07/06/breaking-the-memory-barrier-how-grafana-mimirs-store-gateway-overcame-out-of-memory-errors/">https://grafana.com/blog/2023/07/06/breaking-the-memory-barrier-how-grafana-mimirs-store-gateway-overcame-out-of-memory-errors/</a></li>
</ol>

---

## Outtakes

**The fight night failures.** Netflix peaked at 65 million concurrent streams and experienced widespread buffering. Industry experts pointed to CDN capacity and last-mile congestion at ISP peering points. Akamai's Will Law and YouTube's Sean McCarthy noted there is little that a streaming provider can do when congestion builds at ISP interconnects. ([Streaming Media, 2024](https://www.streamingmedia.com/Articles/Editorial/Short-Cuts/Why-Did-Netflixs-Tyson-Paul-Stream-Fail-at-Scale-Or-Did-it-Fail-at-All-168302.aspx))

**The flash crowd.** Larry Niven's 1973 story "Flash Crowd" imagined instantaneous teleportation creating mobs at newsworthy events the moment they were broadcast. Internet engineers borrowed the term for sudden traffic spikes. A live broadcast creates simultaneous demand that no single server was designed to absorb. (Niven, 1973)

**Twitch Stream Rewind.** In September 2025, Twitch launched Stream Rewind, letting subscribers scrub back through live broadcasts. The gating was not about caching but about monetization. Scrubbing lets viewers skip ads, so Twitch tied the feature to subscriptions before wider rollout. The caching and monetization problems are separate constraints. Netflix sidesteps the second because its model is subscription-based. ([Twitch Blog, 2025](https://blog.twitch.tv/en/2025/05/31/ten-years-of-twitchcon-here-s-what-we-announced-in-rotterdam/))

---

## Changelog

**2026-05-16** Added Outtakes section.  
**2026-05-11** Initial publication.
