---
title: "Head-of-Line Blocking"
date: 2026-06-29
publishdate: 2026-06-29
lastmod: 2026-06-29
summary: "When one stuck item sits at the front of a shared queue, everything behind it waits too. Head-of-line blocking is the failure mode that keeps returning at every layer, from packet switches to HTTP."
tags: ["networking", "queueing", "latency"]
image: /images/head-of-line-blocking.png
draft: true
---

![When one stuck item sits at the front of a shared queue, everything behind it waits too. Head-of-line blocking is the failure mode that keeps returning at every layer, from packet switches to HTTP.](/images/head-of-line-blocking.png)
*Image: placeholder.*

## Head-of-Line Blocking

You load a page and the browser fires a dozen requests on one connection. Eleven come back fast. The twelfth loses a single packet over the wire, and the whole page freezes. The data for the other eleven is already in your kernel, ready to render. It waits anyway.

One lost packet, one stuck request, and a queue that won't let anything past it. That is head-of-line blocking, and it has been ruining throughput since long before the web existed.

## What Is Head-of-Line Blocking?

Head-of-line (HOL) blocking happens when a shared queue serves many destinations one item at a time. The front item is stuck, so everything behind it waits, even items that could go now.

The concept was nailed down in 1987, in the design of telephone switches. Mark Karol, Mike Hluchyj, and Sam Morgan modeled an input-queued switch, where each incoming port keeps a single first in first out (FIFO) queue and sends packets across a fabric to output ports{{< cite 1 "Karol, Mark, Mike Hluchyj, and Sam Morgan (1987). Input Versus Output Queueing on a Space-Division Packet Switch. IEEE Transactions on Communications 35(12)." >}}. They hit a wall.

## The Switch That Hit a Ceiling

The problem is the shared queue. A packet at the head wants output A, but A is busy, so it sits. Behind it waits a packet for output B, which is idle and ready. It can't move, because the head of a FIFO goes first.

Karol and his coauthors proved the ceiling. As the switch grows, a single FIFO per input can never push more than 2 - sqrt(2) of its traffic through, about 58.6 percent, no matter how fast the fabric is{{< cite 1 "Karol, Mark, Mike Hluchyj, and Sam Morgan (1987). Input Versus Output Queueing on a Space-Division Packet Switch. IEEE Transactions on Communications 35(12)." >}}. Add ports or bandwidth and you stall at the same number. The queue's shape was the ceiling.

The fix is to stop sharing. Give each input one queue per output, a scheme called virtual output queueing (VOQ). A packet stuck for output A no longer blocks a packet headed for output B, since they sit in different lines. With a scheduler to match inputs to outputs each cycle, the switch reaches 100 percent throughput{{< cite 2 "McKeown, Nick (1999). The iSLIP Scheduling Algorithm for Input-Queued Switches. IEEE/ACM Transactions on Networking 7(2)." >}}.

## The Web Inherited the Same Shape

The internet's switches fixed this in the 1990s. The web rediscovered it twenty years later, one layer up.

HTTP/1.1 let a browser pipeline requests on one connection, but responses had to come back in order. A slow first response blocked every response behind it, application-layer HOL blocking in pure form. Browsers gave up on pipelining and opened six connections instead.

HTTP/2 fixed that. It multiplexes many streams over a single connection and lets responses arrive in any order, so one slow response no longer holds up the others{{< cite 3 "Thomson, M., and C. Benfield, Eds. (2022). HTTP/2. RFC 9113." >}}. The application layer was clean.

Then the same wound reopened one layer down. HTTP/2 runs over TCP, and TCP is one ordered byte stream. Lose one packet anywhere and TCP holds back everything after it until the loss is retransmitted, including bytes for streams whose data already arrived fine{{< cite 3 "Thomson, M., and C. Benfield, Eds. (2022). HTTP/2. RFC 9113." >}}. Multiplexing many streams onto one ordered stream put the shared queue right back.

## QUIC Cuts the Line

HTTP/3 runs over QUIC, a transport that gives each stream its own independent reliability. A lost packet on one stream stalls only that stream. The others keep flowing, never having shared one ordered line{{< cite 4 "Bishop, M., Ed. (2022). HTTP/3. RFC 9114." >}}.

The pattern repeats. Every generation finds the same blocking at a new layer, and the cure is always the move the switch designers made in 1987. Stop sharing one ordered queue across work headed somewhere else.

## Common Mistakes

**You multiplex onto one ordered stream.** The HTTP/2 trap. Bundling many logical streams onto one TCP connection saves handshake overhead and reintroduces transport-layer HOL blocking. More streams on the same wire will not help. The move is per-stream reliability, which is what QUIC provides{{< cite 4 "Bishop, M., Ed. (2022). HTTP/3. RFC 9114." >}}.

**You blame the bottleneck instead of the head.** Throughput looks fine until one stuck item gums up the line. A queue can report high utilization and still bleed latency, because the work behind the head is ready and waiting. Measure how long items sit, not just how many move{{< cite 1 "Karol, Mark, Mike Hluchyj, and Sam Morgan (1987). Input Versus Output Queueing on a Space-Division Packet Switch. IEEE Transactions on Communications 35(12)." >}}.

**You fix one layer and declare victory.** HTTP/2 killed the application-layer stall and reopened the transport-layer one in the same breath. Remove HOL blocking at one layer and check the layer below. The shared ordered queue moves. It does not vanish{{< cite 3 "Thomson, M., and C. Benfield, Eds. (2022). HTTP/2. RFC 9113." >}}.

**You share one FIFO across destinations.** The original switch mistake, alive in software. One thread pool serving many backends, one database connection serving many request types. A slow request for backend A blocks a fast request for backend B. The cure is the same as in 1987, split the queue by destination{{< cite 2 "McKeown, Nick (1999). The iSLIP Scheduling Algorithm for Input-Queued Switches. IEEE/ACM Transactions on Networking 7(2)." >}}.

## Put It Into Practice

Walk your stack for one ordered queue that serves many destinations. A thread pool behind many endpoints. One connection carrying many streams. One FIFO feeding many consumers. Each is a candidate for the 58.6 percent ceiling.

When you find one, ask whether the head can block work headed somewhere else. If it can, split the queue by destination, move to per-stream reliability, or shed the stuck item so the line can advance.

Find one shared queue this week and ask what happens when its head gets stuck. If the answer is everything behind it waits, you have found the bug the switch designers found in 1987. They fixed it with a second queue per destination. So can you.

## Go Further

**The scheduling problem underneath VOQ.** Splitting the queues is half the fix. Once every input holds a queue per output, something has to decide which inputs send to which outputs each cycle without collisions, and the round-robin iSLIP algorithm is the cleanest answer{{< cite 2 "McKeown, Nick (1999). The iSLIP Scheduling Algorithm for Input-Queued Switches. IEEE/ACM Transactions on Networking 7(2)." >}}.

**Why the transport layer matters at all.** Why TCP is one ordered byte stream, the choice that made congestion control possible and HOL blocking inevitable, runs through Van Jacobson's work in [TCP Congestion Control](/posts/tcp-congestion-control/){{< cite 5 "Jacobson, Van (1988). Congestion Avoidance and Control. SIGCOMM '88." >}}.

**One stuck request, multiplied by a thousand servers.** A single HOL stall at one queue is bad. Fan it across a distributed system where one slow fan-out call governs a whole request, and it becomes the tail-latency problem in The Tail at Scale{{< cite 6 "Dean, Jeffrey, and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM 56(2)." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Karol, Mark, Mike Hluchyj, and Sam Morgan (1987). "Input Versus Output Queueing on a Space-Division Packet Switch." <em>IEEE Transactions on Communications</em>, 35(12). <a href="https://web.mit.edu/modiano/www/6.263/Karol_1.pdf">https://web.mit.edu/modiano/www/6.263/Karol_1.pdf</a></li>
  <li id="ref-2">McKeown, Nick (1999). "The iSLIP Scheduling Algorithm for Input-Queued Switches." <em>IEEE/ACM Transactions on Networking</em>, 7(2). <a href="https://ieeexplore.ieee.org/document/769767">https://ieeexplore.ieee.org/document/769767</a></li>
  <li id="ref-3">Thomson, M., and C. Benfield, Eds. (2022). "HTTP/2." <em>RFC 9113</em>. <a href="https://www.rfc-editor.org/rfc/rfc9113.html">https://www.rfc-editor.org/rfc/rfc9113.html</a></li>
  <li id="ref-4">Bishop, M., Ed. (2022). "HTTP/3." <em>RFC 9114</em>. <a href="https://www.rfc-editor.org/rfc/rfc9114.html">https://www.rfc-editor.org/rfc/rfc9114.html</a></li>
  <li id="ref-5">Jacobson, Van (1988). "Congestion Avoidance and Control." <em>SIGCOMM '88</em>. <a href="https://ee.lbl.gov/papers/congavoid.pdf">https://ee.lbl.gov/papers/congavoid.pdf</a></li>
  <li id="ref-6">Dean, Jeffrey, and Luiz André Barroso (2013). "The Tail at Scale." <em>Communications of the ACM</em>, 56(2). <a href="https://www.barroso.org/publications/TheTailAtScale.pdf">https://www.barroso.org/publications/TheTailAtScale.pdf</a></li>
</ol>

---

## Outtakes

**The 58.6 percent that won't die.** The 2 - sqrt(2) ceiling is one of those numbers that sticks. Switch designers hit it in 1987, and the same fraction surfaces any time one FIFO serves many destinations, in hardware or in software ([Karol, 1987](https://web.mit.edu/modiano/www/6.263/Karol_1.pdf)).

**Pipelining that nobody used.** HTTP/1.1 pipelining was meant to fix head-of-line blocking and mostly made it worse, because a slow first response still blocked the rest. Browsers shipped it and promptly turned it off by default.

**The switch that almost didn't ship.** Early input-queued switches were cheap to build and capped at 58.6 percent. The fix, VOQ plus a scheduler, needed real silicon and a decade to win. The obvious design is not always the first one to ship.

---

## Changelog

**2026-06-29** Initial draft.
