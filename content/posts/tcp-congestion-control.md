---
title: "From Tahoe to BBR"
date: 2026-07-03
publishdate: 2026-07-03
lastmod: 2026-09-12
summary: "In 1986, a network slowdown forced Van Jacobson to invent TCP congestion control. Tahoe/Reno, CUBIC, and BBR mark three different eras of solving that problem."
tags: ["networking", "protocols", "latency"]
image: /images/tcp-congestion-control.png
draft: false
---

![In 1986, a network slowdown forced Van Jacobson to invent TCP congestion control. Tahoe/Reno, CUBIC, and BBR mark three different eras of solving that problem.](/images/tcp-congestion-control.png)
*Image generated with Google Gemini (2026).*

## From Tahoe to BBR

In the fall of 1986, the network throughput between Van Jacobson's lab at Lawrence Berkeley Laboratory (then LBL, now LBNL) and UC Berkeley unexpectedly dropped from 32 Kbps to 40 bps{{< cite 1 "Jacobson, Van (1988). Congestion Avoidance and Control. SIGCOMM '88." >}}. It was the first of a series of "congestion collapses" on the Internet.

Jacobson's observations triggered an investigation. He realized the senders had no idea when to slow down, so as the link filled they kept pushing and strangled it. TCP needed a way to feel the congestion and back off. He and Michael Karels (UC Berkeley) developed and experimented with versions of slow start, congestion avoidance, and fast retransmit algorithms, which were included in the TCP/IP stack of 4.3BSD-Tahoe in 1988{{< cite 1 "Jacobson, Van (1988). Congestion Avoidance and Control. SIGCOMM '88." >}}.

## What Is Congestion Control?

`send window = min(cwnd, receiver window)` is the classic version that shipped with 4.3BSD-Tahoe.

The congestion window (`cwnd`) starts cautiously with one maximum-segment-sized (MSS) packet, followed by "slow start" which approximately doubles the `cwnd` once per round-trip time (RTT) until it reaches the slow-start threshold. The congestion avoidance phase follows, where `cwnd` grows linearly until it detects congestion. A retransmission timeout or three duplicate acknowledgements (ACKs) are used as evidence for congestion. At that point the algorithm backs-off by setting the slow-start threshold to roughly half the current flight size and restarting the `cwnd` back to its initial value.{{< cite 1 "Jacobson, Van (1988). Congestion Avoidance and Control. SIGCOMM '88." >}}.

The algorithm sends a little, ramps up quickly, and when it detects likely loss, it cuts back hard. The classic algorithm's two modes, slow start and congestion avoidance, creates a recognizable sawtooth pattern.

4.3BSD-Reno was released in 1990, which added TCP fast recovery. After three duplicate ACKs, Reno keeps `cwnd` near the reduced level rather than dropping it to one MSS.

## CUBIC Took Over

Reno's weakness shows up on long, high-bandwidth links. After a loss, Reno halves its `cwnd`, then increases it by about one MSS per RTT. A 10 Gbps transoceanic path with a 100ms RTT has about 86,000 MSS. After halving the `cwnd`, recovering 43,000 MSS would take about 70 minutes{{< cite 3 "Ha, Sangtae, Injong Rhee, and Lisong Xu (2008). CUBIC: A New TCP-Friendly High-Speed TCP Variant. ACM SIGOPS Operating Systems Review 42(5)." >}}.

CUBIC replaces Reno's primarily linear congestion-avoidance growth with a cubic function of the time since the last congestion event. After reducing its window, it grows cautiously, flattens as it approaches the previous maximum window, and then accelerates after passing that previous maximum.{{< cite 3 "Ha, Sangtae, Injong Rhee, and Lisong Xu (2008). CUBIC: A New TCP-Friendly High-Speed TCP Variant. ACM SIGOPS Operating Systems Review 42(5)." >}}.

CUBIC was selected as the linux 2.6.19 default TCP congestion-control algorithm in 2006. CUBIC later became the default in Apple and Microsoft TCP stacks.

CUBIC still uses the same loss-based congestion-control model as Reno though. It increases its window until it observes packet loss. In networks with oversized buffers, queues can fill before packet loss is observed, causing bufferbloat (excessive delay caused by packets stuck in a large network buffer).

## BBR Changed the Question

Instead of relying primarily on packet loss, BBR (Bottleneck Bandwidth and Round-trip propagation time) builds a small model of the path that estimates the bandwidth bottleneck from the fastest observed delivery rate and the path's minimum RTT when the queue is empty. It then tries to keep the pipe full without fully filling the queue{{< cite 4 "Cardwell, Neal, et al. (2016). BBR: Congestion-Based Congestion Control. ACM Queue 14(5)." >}}.

Around 2015, Google deployed BBR across their production networks, dogfooding it before publishing the algorithm in 2016. On Google’s B4 WAN, BBR achieved 2x-25x higher throughput than CUBIC, with a peak 133x improvement on one intercontinental path{{< cite 4 "Cardwell, Neal, et al. (2016). BBR: Congestion-Based Congestion Control. ACM Queue 14(5)." >}}.

## What Actually Ships Today

Tahoe included fast retransmit (in 1988) and Reno added fast recovery (in 1990). NewReno refined Reno to survive several losses at once{{< cite 5 "Henderson, Tom, et al. (2012). The NewReno Modification to TCP's Fast Recovery Algorithm. RFC 6582." >}}. Today, "Reno" typically means NewReno.

Linux defaulted to CUBIC since 2006{{< cite 6 "The Linux Kernel (2024). IP Sysctl: TCP Variables. Linux Kernel Documentation." >}}. BBR has been available as a loadable kernel module since 2016{{< cite 4 "Cardwell, Neal, et al. (2016). BBR: Congestion-Based Congestion Control. ACM Queue 14(5)." >}}.

FreeBSD defaulted to NewReno until it switched to CUBIC (FreeBSD 14, in 2023){{< cite 7 "The FreeBSD Project (2023). FreeBSD 14.0-RELEASE Release Notes." >}}. FreeBSD allows operators to swap them as pluggable congestion modules in a framework called `mod_cc`{{< cite 8 "The FreeBSD Project (2024). mod_cc: Modular Congestion Control. FreeBSD Kernel Interfaces Manual." >}}.

BBR and RACK are configured as alternate, pluggable TCP stacks on FreeBSD. RACK-TLP (Recent Acknowledgment and Tail Loss Probe) detects loss using probe packets to report gaps and timing measurements. Randall Stewart wrote FreeBSD's RACK implementation for Netflix, which runs it across its OpenConnect CDN{{< cite 9 "The FreeBSD Project (2024). tcp_rack: TCP RACK-TLP Loss Detection Algorithm. FreeBSD Kernel Interfaces Manual." >}}.

Windows and macOS still both default to CUBIC{{< cite 10 "Iyengar, Janardhan, et al. (2024). CUBIC for Fast and Long-Distance Networks. RFC 9438." >}}. Windows Server can be configured to use DCTCP (Data Center TCP) and ECN (explicit congestion notification){{< cite 11 "Microsoft (2024). Get-NetTCPSetting (NetTCPIP). PowerShell Documentation." >}}.

## Where This Breaks Down

Even on a strong wireless network, packets get lost to interference, which Reno and CUBIC detect as congestion{{< cite 4 "Cardwell, Neal, et al. (2016). BBR: Congestion-Based Congestion Control. ACM Queue 14(5)." >}}.

BBRv1 would crowd out loss-based flows such as CUBIC. BBRv2 (and v3) improved fairness in 2019 (and 2023){{< cite 4 "Cardwell, Neal, et al. (2016). BBR: Congestion-Based Congestion Control. ACM Queue 14(5)." >}}.

No single algorithm is best for every path. A datacenter with microsecond RTTs benefits from explicit signals, while a flaky cellular link is better served by BBR than a purely loss-based method{{< cite 12 "Alizadeh, Mohammad, et al. (2010). Data Center TCP (DCTCP). SIGCOMM '10." >}}.

## Put It Into Practice

Most people don't tweak their TCP congestion algorithm, and that's fine. The defaults keep getting better over time.

But if your servers handle high-throughput traffic over long or lossy paths, consider routing a fraction of it to a canary running BBR or RACK. Compare the numbers against your CUBIC baseline (retransmit rate and p99 latency), monitoring tail latency instead of just raw throughput.

---

## References

<ol class="references">
  <li id="ref-1">Jacobson, Van (1988). "Congestion Avoidance and Control." <em>SIGCOMM '88</em>. <a href="https://ee.lbl.gov/papers/congavoid.pdf">https://ee.lbl.gov/papers/congavoid.pdf</a></li>
  <li id="ref-3">Ha, Sangtae, Injong Rhee, and Lisong Xu (2008). "CUBIC: A New TCP-Friendly High-Speed TCP Variant." <em>ACM SIGOPS Operating Systems Review</em>, 42(5). <a href="https://www.cs.princeton.edu/courses/archive/fall16/cos561/papers/Cubic08.pdf">https://www.cs.princeton.edu/courses/archive/fall16/cos561/papers/Cubic08.pdf</a></li>
  <li id="ref-4">Cardwell, Neal, et al. (2016). "BBR: Congestion-Based Congestion Control." <em>ACM Queue</em>, 14(5). <a href="https://research.google/pubs/pub45646/">https://research.google/pubs/pub45646/</a></li>
  <li id="ref-5">Henderson, Tom, et al. (2012). "The NewReno Modification to TCP's Fast Recovery Algorithm." <em>RFC 6582</em>. <a href="https://www.rfc-editor.org/rfc/rfc6582.html">https://www.rfc-editor.org/rfc/rfc6582.html</a></li>
  <li id="ref-6">The Linux Kernel (2024). "IP Sysctl: TCP Variables." <em>Linux Kernel Documentation</em>. <a href="https://docs.kernel.org/networking/ip-sysctl.html">https://docs.kernel.org/networking/ip-sysctl.html</a></li>
  <li id="ref-7">The FreeBSD Project (2023). "FreeBSD 14.0-RELEASE Release Notes." <a href="https://www.freebsd.org/releases/14.0R/relnotes/">https://www.freebsd.org/releases/14.0R/relnotes/</a></li>
  <li id="ref-8">The FreeBSD Project (2024). "mod_cc: Modular Congestion Control." <em>FreeBSD Kernel Interfaces Manual</em>. <a href="https://man.freebsd.org/cgi/man.cgi?query=mod_cc&sektion=9">https://man.freebsd.org/cgi/man.cgi?query=mod_cc&sektion=9</a></li>
  <li id="ref-9">The FreeBSD Project (2024). "tcp_rack: TCP RACK-TLP Loss Detection Algorithm." <em>FreeBSD Kernel Interfaces Manual</em>. <a href="https://man.freebsd.org/cgi/man.cgi?query=tcp_rack&sektion=4">https://man.freebsd.org/cgi/man.cgi?query=tcp_rack&sektion=4</a></li>
  <li id="ref-10">Iyengar, Janardhan, et al. (2024). "CUBIC for Fast and Long-Distance Networks." <em>RFC 9438</em>. <a href="https://www.rfc-editor.org/rfc/rfc9438.html">https://www.rfc-editor.org/rfc/rfc9438.html</a></li>
  <li id="ref-11">Microsoft (2024). "Get-NetTCPSetting (NetTCPIP)." <em>PowerShell Documentation</em>. <a href="https://learn.microsoft.com/en-us/powershell/module/nettcpip/get-nettcpsetting">https://learn.microsoft.com/en-us/powershell/module/nettcpip/get-nettcpsetting</a></li>
  <li id="ref-12">Alizadeh, Mohammad, et al. (2010). "Data Center TCP (DCTCP)." <em>SIGCOMM '10</em>. <a href="https://people.csail.mit.edu/alizadeh/papers/dctcp-sigcomm10.pdf">https://people.csail.mit.edu/alizadeh/papers/dctcp-sigcomm10.pdf</a></li></ol>

---

## Changelog

**2026-09-12** Removed the Sawtooth and Go Further sections, tightened the Tahoe/Reno cwnd details.  
**2026-08-08** Corrected the RACK attribution, Randall Stewart wrote FreeBSD's implementation for Netflix.  
**2026-07-03** Initial release.  
