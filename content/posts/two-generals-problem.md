---
title: "The Two Generals Problem"
date: 2026-08-13
publishdate: 2026-08-13
lastmod: 2026-08-13
summary: "The Two Generals Problem: neither general can ever confirm the other is ready to attack. Jim Gray proved it in 1978, and the same limit shapes how databases and networks handle failure."
tags: ["distributed", "networking"]
image: /images/two-generals-problem.jpg
draft: true
---

![The Two Generals Problem: neither general can ever confirm the other is ready to attack. Jim Gray proved it in 1978, and the same limit shapes how databases and networks handle failure.](/images/two-generals-problem.jpg)
*Fog filling the valley between two ridgelines, hiding one side from the other. Photo: [Peter Evans (2010)](https://commons.wikimedia.org/wiki/File:Fog_and_mist_below_the_Clee_Hill_-_geograph.org.uk_-_2163671.jpg). CC BY-SA 2.0.*

## The Two Generals Problem

Two generals camp a short distance apart, each with an eye on the same hill. Attack it together, and they win. Attack it alone, and they lose. The only way to coordinate is to send a runner, and with every trip, there's some chance one goes missing{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}.

So the first general sends a message, "Attack at dawn." If the runner makes it through, the second general knows the plan, but the first general has no way to know it arrived. Now the second general is in the same situation as the first, unsure if their own confirmation arrived{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}.

There is no way to end the loop. Send one more runner to confirm the confirmation, and now you need one more runner after that.

## Gray's Proof

Jim Gray named this scenario and proved it in a set of database lecture notes in 1978{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}. Guaranteeing coordination over an unreliable channel is impossible.

His proof is short. Assume the shortest possible protocol that works, call it P, and ask what happens to its last message. If that message gets lost, either it wasn't necessary, or the general who needed it fails to act. Had it not been necessary, a shorter protocol would have worked, contradicting the claim that P was shortest. The last message has to matter. Losing it breaks the protocol, so P never guaranteed anything{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}.

Gray called it "the generals paradox, which as you now see is not a paradox"{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}. A paradox contradicts itself; this doesn't, no matter how clever the protocol gets.

## Not The Byzantine Generals Problem

A second, unrelated generals problem gets confused with this one. In 1982, Leslie Lamport, Robert Shostak, and Marshall Pease published the Byzantine Generals Problem, where multiple generals have to agree on a battle plan despite some of the messengers, or some of the generals themselves, being traitors who send contradictory orders on purpose{{< cite 2 "Lamport, Leslie, Robert Shostak, and Marshall Pease (1982). The Byzantine Generals Problem. ACM Transactions on Programming Languages and Systems, 4(3)." >}}.

The Two Generals Problem is an unreliable channel between two cooperating parties, where messages just sometimes vanish. Byzantine generals face the opposite problem, a channel that works fine but carries messages from parties who might be lying. Consensus algorithms built to tolerate malicious or arbitrarily broken nodes, the kind behind blockchains, solve the Byzantine version. The Two Generals version doesn't need anything that heavy. A retry loop and a timeout are enough.

## The Two-Phase Commit Answer

Gray followed the proof with an answer. He called it the Two-Phase Commit Protocol, the mechanism that lets databases coordinate a commit across multiple machines despite the same failure mode{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}. It works by dropping the proof's core assumption, a fixed number of messages agreed on in advance, and retrying instead until every participant confirms{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}.

A coordinator asks every participant whether it can commit, and each votes yes or no. The coordinator then records the final decision and keeps broadcasting it until everyone acknowledges. Gray illustrated the stakes using a computer in Tokyo and a cash machine in Fuessen, Germany, where both sides have to agree before the machine hands over a million marks, or either the bank loses money or a customer walks away without cash{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}.

Two-Phase Commit can stall indefinitely if the coordinator crashes at the worst moment, leaving participants unable to safely commit or abort on their own. That moment is rare enough for teams to build reliable systems around, but without an actual guarantee.

## Where People Get This Wrong

TCP's three-way handshake, SYN, SYN-ACK, ACK, looks like it settles whether both sides are ready{{< cite 3 "Postel, Jon (1981). Transmission Control Protocol. RFC 793." >}}. It carries the identical last-message problem, since that final ACK can get lost the same way a general's runner can, and TCP just tolerates that rare failure rather than guaranteeing against it.

Enough retries do not add up to certainty, because the odds of failure only shrink and never reach zero, and each additional confirmation is itself another message that can be lost{{< cite 1 "Gray, Jim N. (1978). Notes on Data Base Operating Systems. In Operating Systems: An Advanced Course. Springer-Verlag." >}}.

A team chasing five nines treats an unacknowledged request as an engineering failure rather than a property of the channel, even though networks fail no matter how much engineering goes into them. A system that stays correct anyway is the only real fix.

## What To Do About It

Stop designing as if certainty is possible; design for its absence. Make operations idempotent, so a duplicate retry produces the same result as the original and never double-charges a customer or double-ships an order. A reconciliation step that checks actual state later catches what trusting the last message sent would miss, the way a bank statement catches a transfer that never confirmed. Every request needs an identifier, so a retry reads as the same request.

Two-Phase Commit buys reliability by adding a coordinator and giving up a fixed message count, and it still has a failure mode where a crashed coordinator leaves everyone else stuck. Nothing closes every gap. Aim to fail smaller, less often, and in ways already planned for.

Next time a request hangs and the instinct is to add a retry and call it handled, ask whether the acknowledgment, rather than the request, is the thing that actually got lost. That's the actual design problem, and no amount of retrying resolves it.

---

## References

<ol class="references">
  <li id="ref-1">Gray, Jim N. (1978). "Notes on Data Base Operating Systems." In <em>Operating Systems: An Advanced Course</em>, Lecture Notes in Computer Science, Vol. 60. Berlin: Springer-Verlag. <a href="https://jimgray.azurewebsites.net/papers/dbos.pdf">https://jimgray.azurewebsites.net/papers/dbos.pdf</a></li>
  <li id="ref-2">Lamport, Leslie, Robert Shostak, and Marshall Pease (1982). "The Byzantine Generals Problem." <em>ACM Transactions on Programming Languages and Systems</em>, 4(3). <a href="https://lamport.azurewebsites.net/pubs/byz.pdf">https://lamport.azurewebsites.net/pubs/byz.pdf</a></li>
  <li id="ref-3">Postel, Jon (1981). "Transmission Control Protocol." <em>RFC 793</em>. <a href="https://www.rfc-editor.org/rfc/rfc793.html">https://www.rfc-editor.org/rfc/rfc793.html</a></li>
</ol>

---

## Outtakes

Three years before Gray gave the problem its name, Akkoyunlu, Ekanadham, and Huber proved the identical impossibility result in 1975, using two groups of gangsters instead of two armies ([Akkoyunlu et al., 1975](https://dl.acm.org/doi/10.1145/800213.806523)).

In January 2007, Gray disappeared while sailing alone near San Francisco. Neither he nor his boat was ever found, and no message ever confirmed what happened ([NPR, 2007](https://www.npr.org/2007/02/04/7152514/computer-scientist-lost-at-sea-has-powerful-legacy)).

Zeno's paradox looks structurally similar but resolves in the opposite direction. Both involve an infinite regress, one more runner, one more half-distance. Zeno's dissolves under calculus, the infinite steps sum to a finite distance, so the runner crosses the line. Gray's proof never resolves that way ([Huggett, 2024](https://plato.stanford.edu/entries/paradox-zeno/)).

---

## Changelog

**2026-08-13** Initial release.  
