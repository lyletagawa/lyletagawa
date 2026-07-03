---
title: "Backpressure"
date: 2026-06-28
publishdate: 2026-06-28
lastmod: 2026-06-28
summary: "When a fast producer outruns a slow consumer, something has to give. Backpressure is the signal that slows the producer down instead of dropping data or running the queue out of memory."
tags: ["streaming", "concurrency", "reliability"]
image: /images/backpressure.png
draft: true
---

![When a fast producer outruns a slow consumer, something has to give. Backpressure is the signal that slows the producer down instead of dropping data or running the queue out of memory.](/images/backpressure.png)
*Image: placeholder.*

## Backpressure

The producer writes 50,000 events a second. The consumer reads them at 30,000. The queue between them has no limit set, because nobody thought it would need one. For two minutes the dashboards look healthy.

Then the consumer pod dies with an out-of-memory kill, and the two million events it was holding vanish with it. The queue was never the problem. The missing word back to the producer was.

## What Is Backpressure?

Backpressure is resistance to the flow of data through a system. When a consumer can't keep up, it pushes back on whatever feeds it, and the producer slows to match. The slowest stage sets the pace for everything upstream.

The Reactive Streams specification made the idea concrete for software. A subscriber asks for a specific number of items with a request call, and the publisher may send no more than that many{{< cite 1 "Reactive Streams (2015). Reactive Streams Specification 1.0. reactive-streams.org." >}}. Demand flows backward, data flows forward, and the buffer between them never has to guess how much to hold.

The pattern is older than the spec. TCP has carried it since 1981. Every receiver advertises a window, the amount of data it has room to accept, and the sender is not allowed to exceed it{{< cite 2 "Postel, Jon (1981). Transmission Control Protocol. RFC 793." >}}. Flow control by another name, built into the protocol that moves most of the internet.

## Something Has To Give

When the consumer falls behind, the system has three options, and physics picks the menu. Grow the buffer to hold the backlog. Drop whatever doesn't fit. Or slow the producer until the consumer catches up{{< cite 3 "Nygard, Michael T. (2018). Release It! Design and Deploy Production-Ready Software, 2nd ed. Pragmatic Bookshelf." >}}.

The first two happen by default. An unbounded queue grows until the process runs out of memory and the kernel kills it, taking the backlog with it. A fixed buffer that overflows drops data quietly, which you discover later, in a report that doesn't add up. Only the third option is backpressure, and it's the only one you have to build on purpose.

## Push the Signal Upstream

A bounded queue is the simplest form. Give the queue a limit, and when it fills, the producer either blocks until space frees up or gets a rejection it has to handle{{< cite 3 "Nygard, Michael T. (2018). Release It! Design and Deploy Production-Ready Software, 2nd ed. Pragmatic Bookshelf." >}}. Either way the producer feels the slowness instead of ignoring it.

The catch is that the signal has to travel all the way to the source. Bound one queue and the buffering just moves to the thread pool in front of it, or the socket buffer in front of that{{< cite 1 "Reactive Streams (2015). Reactive Streams Specification 1.0. reactive-streams.org." >}}. Backpressure works only when every stage between the consumer and the original producer can say slow down, and mean it.

## What Bufferbloat Taught Us

The internet ran a giant experiment in what happens when you swallow backpressure instead of passing it along. Router and modem makers added fat buffers to avoid dropping packets, on the theory that a held packet beats a lost one{{< cite 4 "Gettys, Jim, and Kathleen Nichols (2011). Bufferbloat: Dark Buffers in the Internet. ACM Queue 9(11)." >}}.

They were wrong. TCP reads a dropped packet as its signal to slow down. Hide the drops inside a bloated buffer and the sender never hears the bad news, so it keeps pushing while packets pile up and age. Latency climbed into whole seconds on links that should have felt instant, and the cause hid in hardware everyone trusted{{< cite 4 "Gettys, Jim, and Kathleen Nichols (2011). Bufferbloat: Dark Buffers in the Internet. ACM Queue 9(11)." >}}. The fix was smaller, smarter buffers that drop early, on purpose.

## Common Mistakes

**The queue with no limit.** The default queue in most libraries is unbounded, so it sails through testing and fails in production at the worst possible moment. A queue without a cap is a memory leak with a schedule{{< cite 3 "Nygard, Michael T. (2018). Release It! Design and Deploy Production-Ready Software, 2nd ed. Pragmatic Bookshelf." >}}.

**Backpressure that stops at one hop.** Bounding a single queue feels like a fix, but the backlog just relocates to the next unbounded buffer upstream. The signal has to reach the source, or you have only moved the crash{{< cite 1 "Reactive Streams (2015). Reactive Streams Specification 1.0. reactive-streams.org." >}}.

**Blocking the front door.** Push backpressure all the way to a user-facing entry point and it turns into requests that hang until they time out. At the edge you want to shed load with a fast rejection, not hold the caller hostage{{< cite 5 "Welsh, Matt, David Culler, and Eric Brewer (2001). SEDA: An Architecture for Well-Conditioned, Scalable Internet Services. SOSP." >}}.

**Retries that refill the queue.** Clients that retry on a slow or failed response re-inject the exact load you just pushed back, and a polite system melts down anyway. Your backpressure and your retry budget have to agree{{< cite 3 "Nygard, Michael T. (2018). Release It! Design and Deploy Production-Ready Software, 2nd ed. Pragmatic Bookshelf." >}}.

## Put It Into Practice

Walk your system and ask each queue what happens when it fills. If the answer is grows forever, you have found your next outage. Put a bound on it, and decide up front whether a full queue blocks the producer or rejects the work.

Then trace that bound back to the source. A limit the producer can ignore is decoration. Make sure the slowdown reaches whatever first accepts work, and at that outer edge, shed instead of block so a slow backend never becomes a frozen front end.

Pick your highest-throughput pipeline this week. Find the one queue in it with no limit and give it one. That single bound is the difference between a system that slows down and a system that falls over.

## Go Further

**Load shedding and admission control.** When backpressure reaches the front door, the move is to refuse work fast rather than queue it. Staged designs put explicit admission control between stages to keep each one well-conditioned under overload{{< cite 5 "Welsh, Matt, David Culler, and Eric Brewer (2001). SEDA: An Architecture for Well-Conditioned, Scalable Internet Services. SOSP." >}}.

**Backpressure and Little's Law.** Bounding a queue bounds the number of items in flight, and Little's Law turns that cap straight into a cap on latency. The arithmetic behind why a smaller queue means a shorter wait is the whole point of Little's Law{{< cite 6 "Little, John D. C., and Stephen C. Graves (2008). Little's Law. In Building Intuition. Springer." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Reactive Streams (2015). "Reactive Streams Specification 1.0." <a href="https://www.reactive-streams.org/">https://www.reactive-streams.org/</a></li>
  <li id="ref-2">Postel, Jon (1981). "Transmission Control Protocol." <em>RFC 793</em>. <a href="https://www.rfc-editor.org/rfc/rfc793.html">https://www.rfc-editor.org/rfc/rfc793.html</a></li>
  <li id="ref-3">Nygard, Michael T. (2018). <em>Release It! Design and Deploy Production-Ready Software</em>, 2nd ed. Pragmatic Bookshelf. <a href="https://pragprog.com/titles/mnee2/release-it-second-edition/">https://pragprog.com/titles/mnee2/release-it-second-edition/</a></li>
  <li id="ref-4">Gettys, Jim, and Kathleen Nichols (2011). "Bufferbloat: Dark Buffers in the Internet." <em>ACM Queue</em>, 9(11). <a href="https://web.archive.org/web/2021/https://queue.acm.org/detail.cfm?id=2071893">https://queue.acm.org/detail.cfm?id=2071893</a></li>
  <li id="ref-5">Welsh, Matt, David Culler, and Eric Brewer (2001). "SEDA: An Architecture for Well-Conditioned, Scalable Internet Services." <em>SOSP 2001</em>. <a href="https://people.eecs.berkeley.edu/~brewer/papers/SEDA-sosp.pdf">https://people.eecs.berkeley.edu/~brewer/papers/SEDA-sosp.pdf</a></li>
  <li id="ref-6">Little, John D. C., and Stephen C. Graves (2008). "Little's Law." In <em>Building Intuition: Insights from Basic Operations Management Models and Principles</em>. Springer. <a href="http://www.mit.edu/~sgraves/www/papers/Little%27s%20Law-Published.pdf">http://www.mit.edu/~sgraves/www/papers/Little's Law-Published.pdf</a></li>
</ol>

---

## Outtakes

**TCP had it first.** Backpressure sounds like a modern streaming concern, but the receive window has throttled senders since 1981. Every web page you load quietly negotiates how fast it's allowed to arrive, thousands of times, and you never notice ([Postel, 1981](https://www.rfc-editor.org/rfc/rfc793.html)).

**The grocery store version.** A cashier who stops scanning until you finish bagging is applying backpressure. The line backs up, a manager opens another lane, and the system finds its rate. Nothing hits the floor.

**Drop early, drop often.** The bufferbloat fix sounds backwards. To make the network faster, throw packets away sooner. A small buffer that drops beats a huge one that hides the truth until everything is seconds behind ([Gettys, 2011](https://web.archive.org/web/2021/https://queue.acm.org/detail.cfm?id=2071893)).

---

## Changelog

**2026-06-28** Initial release.  
**2026-06-28** Edit pass: fixed the intro backlog arithmetic and trimmed a repeated phrase.  
