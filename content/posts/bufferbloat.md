---
title: "Bufferbloat"
date: 2026-06-28
publishdate: 2026-06-28
lastmod: 2026-06-28
summary: "Your connection has plenty of bandwidth and still feels broken under load. Bufferbloat is the oversized buffer that hoards packets instead of dropping them, trading a little throughput for seconds of lag."
tags: ["networking", "latency", "queueing"]
image: /images/bufferbloat.png
draft: true
---

![Your connection has plenty of bandwidth and still feels broken under load. Bufferbloat is the oversized buffer that hoards packets instead of dropping them, trading a little throughput for seconds of lag.](/images/bufferbloat.png)
*Image: placeholder.*

## Bufferbloat

You kick off a backup to the cloud and join a video call at the same time. The upload barely dents your bandwidth, 8 megabits out of 20. Two minutes in, the call freezes, a webpage spins, and a ping that idles at 15 milliseconds is suddenly answering in 1,200.

Nothing is saturated. Your link has bandwidth to spare. The packets are just stuck in line, waiting behind a buffer that decided hoarding them was a favor.

## What Is Bufferbloat?

Jim Gettys, who had a hand in the X Window System and the One Laptop per Child project, watched his home connection start doing exactly this and refused to let it go. He traced the lag to the buffers inside his cable modem and router, which had grown large enough to hold whole seconds of traffic{{< cite 1 "Gettys, Jim, and Kathleen Nichols (2011). Bufferbloat: Dark Buffers in the Internet. ACM Queue 9(11)." >}}. He named the problem bufferbloat.

A buffer is supposed to absorb a brief burst, smoothing a momentary mismatch between how fast packets arrive and how fast the link can send them. Memory got cheap, so equipment makers kept making buffers bigger, on the reasonable-sounding theory that a dropped packet is a failure and more room means fewer drops{{< cite 1 "Gettys, Jim, and Kathleen Nichols (2011). Bufferbloat: Dark Buffers in the Internet. ACM Queue 9(11)." >}}. The bigger buffers didn't smooth bursts. They built a standing line that never emptied.

## Why Big Buffers Backfire

TCP, the protocol behind most of your traffic, has no direct way to know how fast it should send. It speeds up until a packet gets dropped, reads that drop as a sign of congestion, and backs off{{< cite 2 "Jacobson, Van (1988). Congestion Avoidance and Control. SIGCOMM '88." >}}. Loss is the signal. The whole machinery of congestion control runs on it.

A giant buffer mutes that signal. Instead of dropping a packet when the link is full, the buffer swallows it and a hundred more, so the sender never hears the bad news and keeps accelerating. By the time the buffer finally overflows it holds seconds of backlog, and every packet behind your video call waits out that entire queue{{< cite 1 "Gettys, Jim, and Kathleen Nichols (2011). Bufferbloat: Dark Buffers in the Internet. ACM Queue 9(11)." >}}. Bandwidth looks fine because data keeps flowing. Latency falls apart because everything is waiting in the same long line.

The failure has a pedigree. In 1986 the early internet suffered a string of congestion collapses, and throughput on one Berkeley link cratered from 32 kilobits per second to 40 bits, nearly a thousandfold drop, until Van Jacobson gave TCP the congestion control that rescued it{{< cite 2 "Jacobson, Van (1988). Congestion Avoidance and Control. SIGCOMM '88." >}}. That fix assumed the network would drop packets when it was full. Bufferbloat quietly broke the assumption.

## The Fix Is Smarter Dropping

Adding bandwidth doesn't help, because the problem is the queue, not the capacity. You fix bufferbloat by managing the queue{{< cite 3 "Nichols, Kathleen, and Van Jacobson (2012). Controlling Queue Delay. ACM Queue 10(5)." >}}.

Kathleen Nichols and Van Jacobson built the cleanest answer, an algorithm called CoDel, short for controlled delay. Instead of watching how full the buffer is, CoDel watches how long packets sit in it. When the shortest time any packet has spent waiting stays above five milliseconds for too long, CoDel drops a few, which tells the senders to slow down before the queue gets deep{{< cite 3 "Nichols, Kathleen, and Van Jacobson (2012). Controlling Queue Delay. ACM Queue 10(5)." >}}. No tuning knobs, no guessing the right buffer size for a link speed nobody knows in advance.

Pair CoDel with fair queuing so one heavy upload can't crowd out a latency-sensitive call, and you get fq_codel, written up as an internet standard and shipped as the default queue discipline in Linux and most open router firmware{{< cite 4 "Høiland-Jørgensen, Toke, et al. (2018). The FlowQueue-CoDel Packet Scheduler and Active Queue Management Algorithm. RFC 8290." >}}. The lag that plagued Gettys mostly disappears.

## Common Mistakes

**You measure bandwidth and ignore latency.** A speed test that reports a big download number says nothing about what happens to your ping while the link is busy. The bloat only shows up under load, which is exactly when a plain speed test isn't looking{{< cite 1 "Gettys, Jim, and Kathleen Nichols (2011). Bufferbloat: Dark Buffers in the Internet. ACM Queue 9(11)." >}}.

**You add buffer to stop drops.** The instinct that started all this is still around. Dropping a packet feels like data loss, so the fix looks like more memory. A packet held for two seconds is more useless than a packet dropped in two milliseconds, and the drop is what keeps the sender honest{{< cite 2 "Jacobson, Van (1988). Congestion Avoidance and Control. SIGCOMM '88." >}}.

**You throw bandwidth at it.** Upgrading the plan raises the ceiling without draining the queue. A fatter pipe fills its bloated buffer just as readily, and the standing line is shorter in time but it's still there{{< cite 3 "Nichols, Kathleen, and Van Jacobson (2012). Controlling Queue Delay. ACM Queue 10(5)." >}}.

**You manage a queue you don't own.** The worst buffer is often inside the ISP's modem or the carrier's gear, where you can't reach the settings. The practical move is to keep the bottleneck on hardware you control, so the queue forms where your router can manage it{{< cite 4 "Høiland-Jørgensen, Toke, et al. (2018). The FlowQueue-CoDel Packet Scheduler and Active Queue Management Algorithm. RFC 8290." >}}.

## Put It Into Practice

Run a test that loads your link in both directions and watches the ping the entire time, not just a throughput number. The gap between your idle latency and your loaded latency is your bufferbloat, measured in milliseconds you can feel.

If that gap is bad, turn on smart queue management. Most modern router firmware puts fq_codel a checkbox away, and the trick that makes it work is capping your upload a little below the line rate, around 90 percent, so the bottleneck and its queue sit on your hardware instead of the ISP's{{< cite 4 "Høiland-Jørgensen, Toke, et al. (2018). The FlowQueue-CoDel Packet Scheduler and Active Queue Management Algorithm. RFC 8290." >}}.

Test one thing this week. Start a big upload, ping your gateway, and watch the number. If it climbs from tens of milliseconds into the hundreds, you've found bufferbloat in your own house, and it's a checkbox away from fixed.

## Go Further

**The same trap in software.** Bufferbloat is what an unbounded queue does to a network. The general cure, letting a slow consumer push back on a fast producer instead of hoarding the overflow, is the subject of Backpressure{{< cite 5 "Nygard, Michael T. (2018). Release It! Design and Deploy Production-Ready Software, 2nd ed. Pragmatic Bookshelf." >}}.

**Where the standing queue began.** The deeper story of why a healthy network has to drop packets at all runs back through Van Jacobson's original congestion control, the work bufferbloat accidentally undid{{< cite 2 "Jacobson, Van (1988). Congestion Avoidance and Control. SIGCOMM '88." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Gettys, Jim, and Kathleen Nichols (2011). "Bufferbloat: Dark Buffers in the Internet." <em>ACM Queue</em>, 9(11). <a href="https://web.archive.org/web/2021/https://queue.acm.org/detail.cfm?id=2071893">https://queue.acm.org/detail.cfm?id=2071893</a></li>
  <li id="ref-2">Jacobson, Van (1988). "Congestion Avoidance and Control." <em>SIGCOMM '88</em>. <a href="https://ee.lbl.gov/papers/congavoid.pdf">https://ee.lbl.gov/papers/congavoid.pdf</a></li>
  <li id="ref-3">Nichols, Kathleen, and Van Jacobson (2012). "Controlling Queue Delay." <em>ACM Queue</em>, 10(5). <a href="https://web.archive.org/web/2021/https://queue.acm.org/detail.cfm?id=2209336">https://queue.acm.org/detail.cfm?id=2209336</a></li>
  <li id="ref-4">Høiland-Jørgensen, Toke, et al. (2018). "The FlowQueue-CoDel Packet Scheduler and Active Queue Management Algorithm." <em>RFC 8290</em>. <a href="https://www.rfc-editor.org/rfc/rfc8290.html">https://www.rfc-editor.org/rfc/rfc8290.html</a></li>
  <li id="ref-5">Nygard, Michael T. (2018). <em>Release It! Design and Deploy Production-Ready Software</em>, 2nd ed. Pragmatic Bookshelf. <a href="https://pragprog.com/titles/mnee2/release-it-second-edition/">https://pragprog.com/titles/mnee2/release-it-second-edition/</a></li>
</ol>

---

## Outtakes

**The 400-yard collapse.** The 1986 congestion collapse that forced TCP to grow up struck between two buildings about 400 yards apart at Berkeley. Throughput fell nearly a thousandfold over a hop you could walk in five minutes ([Jacobson, 1988](https://ee.lbl.gov/papers/congavoid.pdf)).

**Diagnosed in basements.** Bufferbloat got its name and a lot of its tooling outside the big standards bodies, in a volunteer project with its own test router firmware. Much of the internet's lag problem was chased down by people doing it on the side ([Gettys, 2011](https://web.archive.org/web/2021/https://queue.acm.org/detail.cfm?id=2071893)).

**Cheap memory, expensive lag.** The whole mess traces to a falling price. When buffer memory got cheap, more of it looked free. The cost just moved from dollars on a bill of materials to seconds on your ping ([Gettys, 2011](https://web.archive.org/web/2021/https://queue.acm.org/detail.cfm?id=2071893)).

---

## Changelog

**2026-06-28** Initial release.  
**2026-06-28** Edit pass: fixed an outtake that undersold the engineers behind the fix, minor tightening.  
