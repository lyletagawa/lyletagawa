---
title: "The Two Generals Problem"
date: 2026-07-30
publishdate: 2026-07-30
lastmod: 2026-07-30
summary: "Two generals can never confirm a coordinated attack succeeded, no matter how many messages cross between them. Jim Gray proved it in 1978, and the same impossibility still shapes how databases and networks handle failure."
tags: ["distributed", "consensus"]
image: /images/two-generals-problem.jpg
draft: true
---

![Two generals can never confirm a coordinated attack succeeded, no matter how many messages cross between them. Jim Gray proved it in 1978, and the same impossibility still shapes how databases and networks handle failure.](/images/two-generals-problem.jpg)
*Fog filling the valley between two ridgelines, hiding one side from the other. Photo: [Peter Evans (2010)](https://commons.wikimedia.org/wiki/File:Fog_and_mist_below_the_Clee_Hill_-_geograph.org.uk_-_2163671.jpg). CC BY-SA 2.0.*

## The Two Generals Problem

Two armies camp on separate hills, with the enemy dug in between them. Attack together, and they win. Attack alone, and that army is wiped out before the other arrives. Neither general can see the other's camp. The only way to coordinate a time is to send a runner through enemy territory, and every runner has a real chance of getting caught{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}.

So the first general sends a message. Attack at dawn. If the runner makes it through, the second general knows the plan, but the first doesn't know that yet. All they know is they sent something, and attacking on that assumption is a gamble they can't afford. So they wait for a runner to confirm receipt, but now the second general is in the same position the first one started in, unsure if their own confirmation arrived{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}.

There is no way to end the loop. Every message that closes the gap opens an identical one on the other side. Send one more runner to confirm the confirmation, and now you need one more runner after that. It never terminates, and both generals know it never terminates, which is what makes this a hard limit rather than an inconvenience{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}.

## What Is the Two Generals Problem?

Computer scientist Jim Gray gave this scenario its name and its proof in a 1978 set of lecture notes on database operating systems, using it to explain why guaranteeing coordination over an unreliable channel is fundamentally different from making it merely likely{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}. His proof is short. Assume the shortest possible protocol that works, call it P, and ask what happens to its last message. If that message gets lost, either it wasn't actually necessary, or the general who needed it fails to act. If it wasn't necessary, a shorter protocol would have worked, contradicting the claim that P was shortest. So the last message has to matter, which means losing it breaks the protocol, which means P never really guaranteed anything. No finite protocol can{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}.

## The Proof Behind The Paradox

People call this the Two Generals Paradox, and Gray called it that too, right up until the sentence where he corrected himself. "The generals paradox," he wrote, "which as you now see is not a paradox{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}." A paradox produces a contradiction out of reasonable premises. Nothing here conflicts with itself. This is a proof, with a clean conclusion. Certainty over an unreliable channel is impossible, no matter how clever the protocol.

That distinction carries real weight. A paradox invites the hope that something clever enough might dissolve it. A proof forecloses that hope entirely. The channel itself sets the limit, and no amount of engineering cleverness changes what a channel is.

## Not The Byzantine Generals Problem

A second, unrelated generals problem gets mixed in with this one constantly. In 1982, Leslie Lamport, Robert Shostak, and Marshall Pease published the Byzantine Generals Problem, where multiple generals have to agree on a battle plan despite some of the messengers, or some of the generals themselves, being traitors who send contradictory orders on purpose{{< cite 2 "Lamport, Leslie, Robert Shostak, and Marshall Pease (1982). The Byzantine Generals Problem. ACM Transactions on Programming Languages and Systems, 4(3)." >}}.

The two share a name and a military setting, but they describe different failures. The Two Generals Problem is an unreliable channel between two cooperating parties, where messages just sometimes vanish. The Byzantine Generals Problem is unreliable parties on a channel that works fine, where messages arrive but might be lies. Consensus algorithms built to tolerate malicious or arbitrarily broken nodes, the kind behind blockchains, solve the Byzantine version. A retry loop and a timeout solve the Two Generals version, and reaching for the wrong one hands you a fix built for a failure you don't have.

## The Two-Phase Commit Answer

Gray followed the proof with an answer. His notes introduce the Two-Phase Commit Protocol right after it, the mechanism that lets databases coordinate a commit across multiple machines despite the same failure mode{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}. It works by relaxing the constraint the proof depends on, a fixed, finite number of messages agreed on in advance. Instead, it promises to keep retrying, with a coordinator that remembers the outcome and asks again, and again, until every participant confirms{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}.

A coordinator asks every participant whether it can commit. Each votes yes or no, then the coordinator records the final decision and keeps broadcasting it until everyone acknowledges. Gray illustrated the stakes with a real version of the same problem, a computer in Tokyo and a cash machine in Fuessen, Germany, that both have to agree before the machine hands over a million marks, or either the bank loses money or a customer walks away without cash{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}.

The proof still holds. Two-Phase Commit can stall indefinitely if the coordinator crashes at the worst possible moment, leaving participants unable to safely commit or abort on their own. What changes is how often that happens. Rare enough to build a career on, instead of a guarantee anyone can make.

## Where People Get This Wrong

**They think TCP's handshake already solved this.** The three-way handshake, SYN, SYN-ACK, ACK, looks like it settles whether both sides are ready{{< cite 3 "Postel, Jon (1981). Transmission Control Protocol. RFC 793." >}}. It carries the identical last-message problem, since that final ACK can get lost the same way a general's runner can. TCP tolerates the rare failure instead of guaranteeing against it, a fine trade for a web request and a terrible one for launching an attack.

**They think enough retries add up to certainty.** Retrying lowers the odds of failure, but it never reaches zero. Each additional confirmation is just another message that can be lost, and no number of successful roundtrips proves the next one will land{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}.

**They treat every timeout as a bug to fix.** A team chasing five nines treats an unacknowledged request as an engineering failure rather than a property of the channel. Networks fail no matter how much engineering goes into them. The fix is a system that stays correct when they do.

## What To Do About It

Stop designing as if certainty is available, and design for its absence instead. Make operations idempotent, so a duplicate retry produces the same result as the original instead of double-charging a customer or double-shipping an order. Add a reconciliation step that checks actual state later instead of trusting the last message sent, the way a bank statement catches a transfer that never confirmed. Give every request an identifier so a retry reads as the same request.

Two-Phase Commit's own weak spot is instructive here. It buys reliability by adding a coordinator and giving up a fixed message count, and it still has a failure mode where a crashed coordinator leaves everyone else stuck. Nothing closes every gap. Something can only fail smaller, less often, and in ways already planned for.

Next time a request hangs and the instinct is to add a retry and call it handled, ask whether the acknowledgment might be the thing that got lost this time, instead of the request. That's the actual design problem, and no amount of retrying answers it.

## Dig Deeper

**Consensus with one faulty process.** Fischer, Lynch, and Paterson extended the same impossibility to general asynchronous networks, proving no protocol can guarantee agreement if even one process might crash, without any message ever going missing{{< cite 4 "Fischer, Michael J., Nancy A. Lynch, and Michael S. Paterson (1985). Impossibility of Distributed Consensus with One Faulty Process. Journal of the ACM, 32(2)." >}}.

**The CAP theorem's proof.** Gilbert and Lynch formalized Brewer's conjecture that a distributed system can't have consistency, availability, and partition tolerance all at once, the same impossibility result scaled up to an entire system{{< cite 5 "Gilbert, Seth, and Nancy Lynch (2002). Brewer's Conjecture and the Feasibility of Consistent, Available, Partition-Tolerant Web Services. ACM SIGACT News, 33(2)." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Gray, Jim N. (1978). "Notes on Data Base Operating Systems." In <em>Operating Systems: An Advanced Course</em>, Lecture Notes in Computer Science, Vol. 60. Berlin: Springer-Verlag. <a href="https://jimgray.azurewebsites.net/papers/dbos.pdf">https://jimgray.azurewebsites.net/papers/dbos.pdf</a></li>
  <li id="ref-2">Lamport, Leslie, Robert Shostak, and Marshall Pease (1982). "The Byzantine Generals Problem." <em>ACM Transactions on Programming Languages and Systems</em>, 4(3). <a href="https://lamport.azurewebsites.net/pubs/byz.pdf">https://lamport.azurewebsites.net/pubs/byz.pdf</a></li>
  <li id="ref-3">Postel, Jon (1981). "Transmission Control Protocol." <em>RFC 793</em>. <a href="https://www.rfc-editor.org/rfc/rfc793.html">https://www.rfc-editor.org/rfc/rfc793.html</a></li>
  <li id="ref-4">Fischer, Michael J., Nancy A. Lynch, and Michael S. Paterson (1985). "Impossibility of Distributed Consensus with One Faulty Process." <em>Journal of the ACM</em>, 32(2). <a href="https://groups.csail.mit.edu/tds/papers/Lynch/jacm85.pdf">https://groups.csail.mit.edu/tds/papers/Lynch/jacm85.pdf</a></li>
  <li id="ref-5">Gilbert, Seth, and Nancy Lynch (2002). "Brewer's Conjecture and the Feasibility of Consistent, Available, Partition-Tolerant Web Services." <em>ACM SIGACT News</em>, 33(2). <a href="https://www.cs.cornell.edu/courses/cs6464/2009sp/papers/brewer.pdf">https://www.cs.cornell.edu/courses/cs6464/2009sp/papers/brewer.pdf</a></li>
</ol>

---

## Outtakes

**Three years earlier, they were gangsters.** Before Gray gave the problem its name, Akkoyunlu, Ekanadham, and Huber proved the identical impossibility result in 1975, using two groups of gangsters trying to coordinate instead of two armies. The math didn't change. Only the costumes did ([Akkoyunlu et al., 1975](https://dl.acm.org/doi/10.1145/800213.806523)).

**A class handout became a landmark.** Gray's proof and the Two-Phase Commit Protocol both first appeared not in a journal submission but in his own lecture notes for a 1978 graduate course on database operating systems ([Gray, 1978](https://jimgray.azurewebsites.net/papers/dbos.pdf)).

---

## Changelog

**2026-07-30** Initial release.
