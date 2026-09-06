---
title: "Byzantine Fault Tolerance"
date: 2026-08-13
publishdate: 2026-08-13
lastmod: 2026-08-18
summary: "Byzantine fault tolerance keeps a distributed system agreeing when nodes lie to one another. PBFT shows why that protection needs extra replicas, signatures, and quorum traffic."
tags: ["distributed", "consensus", "reliability"]
image: /images/byzantine-generals-problem.jpg
draft: true
---

![Byzantine fault tolerance keeps a distributed system agreeing when nodes lie to one another. PBFT shows why that protection needs extra replicas, signatures, and quorum traffic.](/images/byzantine-generals-problem.jpg)
*Mosaic of John V Palaiologos in Hagia Sophia. Public domain via [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Restored_mosaic_of_John_V_Palaiologos_(1).jpg).*

## Byzantine Fault Tolerance

Two coworkers get a restaurant address by text from a third, who is lying. One gets one address, the other gets a different address. Neither coworker can tell who got played, since it's one person's word against another's. Add a fourth coworker who isn't lying who confirms the first coworker's address, and the deadlock breaks. Two matching texts now outvote the liar's one.

A crash is easy to describe: a process stops, a health check fails, and the cluster moves on. A Byzantine failure is harder to pin down, since the faulty participant can behave arbitrarily, like the coworker above, and still look correct to whoever it's currently talking to{{< cite 1 "Lamport, Leslie, Robert Shostak, and Marshall Pease (1982). The Byzantine Generals Problem. ACM Transactions on Programming Languages and Systems, 4(3)." >}}.

Byzantine fault tolerance (BFT) keeps correct participants in agreement despite that behavior. It fits when the consensus group spans independently controlled organizations, where a participant might turn out malicious rather than just unavailable{{< cite 3 "Hyperledger Fabric (2026). The Ordering Service. Hyperledger Fabric Documentation." >}}.

## Count Your Traitors

Lamport, Robert Shostak, and Marshall Pease gave the problem its famous army story in 1982. Loyal generals need to follow the same order. When the commander is loyal, they must follow the commander's order. Traitors get to send different stories to different lieutenants{{< cite 1 "Lamport, Leslie, Robert Shostak, and Marshall Pease (1982). The Byzantine Generals Problem. ACM Transactions on Programming Languages and Systems, 4(3)." >}}.

Their ordinary, "oral message" model looks a lot like an authenticated computer network. Recipients know who sent a message, delivery errors are detectable, and a traitor still controls the message content{{< cite 1 "Lamport, Leslie, Robert Shostak, and Marshall Pease (1982). The Byzantine Generals Problem. ACM Transactions on Programming Languages and Systems, 4(3)." >}}. The result is brutal. To tolerate `f` Byzantine faults, the model needs at least `3f + 1` participants{{< cite 1 "Lamport, Leslie, Robert Shostak, and Marshall Pease (1982). The Byzantine Generals Problem. ACM Transactions on Programming Languages and Systems, 4(3)." >}}.

That's the same deadlock as the restaurant address, with generals in the coworkers' place. Three participants leave the two loyal ones deadlocked. A fourth gives them a vote the traitor loses{{< cite 1 "Lamport, Leslie, Robert Shostak, and Marshall Pease (1982). The Byzantine Generals Problem. ACM Transactions on Programming Languages and Systems, 4(3)." >}}.

Digital signatures help, but they do not turn a compromised machine into a reliable narrator. Lamport's signed-message model makes forgery and undetectable alteration visible. A traitor can still sign two contradictory statements. The signatures prove who caused the mess{{< cite 1 "Lamport, Leslie, Robert Shostak, and Marshall Pease (1982). The Byzantine Generals Problem. ACM Transactions on Programming Languages and Systems, 4(3)." >}}.

## Watch PBFT Work

Miguel Castro and Barbara Liskov's Practical Byzantine Fault Tolerance protocol, or PBFT, made the model concrete for replicated services. It runs `3f + 1` replicas with the same initial state and deterministic execution, then agrees on a total order before replicas execute requests{{< cite 2 "Castro, Miguel, and Barbara Liskov (1999). Practical Byzantine Fault Tolerance. Proceedings of the Third Symposium on Operating Systems Design and Implementation." >}}.

One client request takes three rounds.

**Pre-prepare.** The client sends the request to the primary, PBFT's current leader. The primary assigns a sequence number and sends every replica a `PRE-PREPARE` containing the request digest{{< cite 2 "Castro, Miguel, and Barbara Liskov (1999). Practical Byzantine Fault Tolerance. Proceedings of the Third Symposium on Operating Systems Design and Implementation." >}}.

**Prepare.** Each backup checks the primary's proposal, then sends a matching `PREPARE` to every replica. A replica reaches the prepared state after the request, the matching pre-prepare, and `2f` matching prepares from distinct backups{{< cite 2 "Castro, Miguel, and Barbara Liskov (1999). Practical Byzantine Fault Tolerance. Proceedings of the Third Symposium on Operating Systems Design and Implementation." >}}.

**Commit.** Prepared replicas broadcast `COMMIT`. A replica becomes committed locally after collecting `2f + 1` matching commits, then executes requests in sequence order{{< cite 2 "Castro, Miguel, and Barbara Liskov (1999). Practical Byzantine Fault Tolerance. Proceedings of the Third Symposium on Operating Systems Design and Implementation." >}}. The client waits for `f + 1` matching replies, enough to ensure that at least one reply came from a correct replica{{< cite 2 "Castro, Miguel, and Barbara Liskov (1999). Practical Byzantine Fault Tolerance. Proceedings of the Third Symposium on Operating Systems Design and Implementation." >}}.

The primary gets to propose an order. It does not get to make one true by saying it loudly. Prepare and commit turn the proposal into overlapping evidence held by the other replicas{{< cite 2 "Castro, Miguel, and Barbara Liskov (1999). Practical Byzantine Fault Tolerance. Proceedings of the Third Symposium on Operating Systems Design and Implementation." >}}.

## Replace The Leader

The hard part arrives when the primary slows down, disappears, or carefully stalls one group while flattering another. A backup that times out sends `VIEW-CHANGE` messages containing evidence of prepared requests and stable checkpoints. The next primary gathers `2f + 1` of those messages and sends `NEW-VIEW`, carrying forward requests that had already reached the prepared state{{< cite 2 "Castro, Miguel, and Barbara Liskov (1999). Practical Byzantine Fault Tolerance. Proceedings of the Third Symposium on Operating Systems Design and Implementation." >}}.

That bookkeeping keeps a replacement leader from quietly overwriting a prior decision. It also explains the bill. Normal operation includes authenticated messages, quorum tracking, and all-to-all prepare and commit traffic. A leadership change carries proof, since a shrug is not a recovery protocol{{< cite 2 "Castro, Miguel, and Barbara Liskov (1999). Practical Byzantine Fault Tolerance. Proceedings of the Third Symposium on Operating Systems Design and Implementation." >}}.

## Spend It Wisely

Hyperledger Fabric makes the choice explicit. Its Raft ordering service tolerates crash failures while a majority of orderers remain available. Its BFT ordering service, introduced in Fabric 3.0, uses SmartBFT for deployments where independently operated orderers may be malicious or compromised{{< cite 3 "Hyperledger Fabric (2026). The Ordering Service. Hyperledger Fabric Documentation." >}}. Four organizations can run four orderers and tolerate one faulty, crashed, or unreachable member. At one-third faulty or unavailable, the group cannot agree on new blocks{{< cite 3 "Hyperledger Fabric (2026). The Ordering Service. Hyperledger Fabric Documentation." >}}.

That is a useful boundary for ordinary system design. A team running three database replicas in one cloud account should fix access control, deployment safety, and backups before paying for a Byzantine protocol. A consortium that gives each member its own operator, keys, and incentive to disagree has already chosen a different problem.

## What To Do About It

Write down what a faulty member may do. "The node might crash" and "the node might equivocate while its operator denies it" lead to different quorum sizes, different protocols, and different operating costs. Your threat model picks the protocol long before a benchmark does.

Use BFT when the membership itself requires distrust. Otherwise, run crash-fault consensus well and spend the saved complexity on the failures your system is more likely to have. The network will provide enough drama without hiring traitors.

## Go Further

**Signed-message algorithms.** Lamport, Shostak, and Pease show how public, unforgeable signatures change the original problem's assumptions, while leaving malicious signed statements fully possible{{< cite 1 "Lamport, Leslie, Robert Shostak, and Marshall Pease (1982). The Byzantine Generals Problem. ACM Transactions on Programming Languages and Systems, 4(3)." >}}.

**State-machine replication.** PBFT starts from identical replica state and deterministic execution. The paper explains checkpointing, watermarks, and recovery, which keep the replicated log from becoming an attic full of old evidence{{< cite 2 "Castro, Miguel, and Barbara Liskov (1999). Practical Byzantine Fault Tolerance. Proceedings of the Third Symposium on Operating Systems Design and Implementation." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Lamport, Leslie, Robert Shostak, and Marshall Pease (1982). "The Byzantine Generals Problem." <em>ACM Transactions on Programming Languages and Systems</em>, 4(3), 382-401. <a href="https://lamport.azurewebsites.net/pubs/byz.pdf">https://lamport.azurewebsites.net/pubs/byz.pdf</a></li>
  <li id="ref-2">Castro, Miguel, and Barbara Liskov (1999). "Practical Byzantine Fault Tolerance." <em>Proceedings of the Third Symposium on Operating Systems Design and Implementation</em>, 173-186. <a href="https://www.usenix.org/conference/osdi-99/presentation/practical-byzantine-fault-tolerance">https://www.usenix.org/conference/osdi-99/presentation/practical-byzantine-fault-tolerance</a></li>
  <li id="ref-3">Hyperledger Fabric (2026). "The Ordering Service." <em>Hyperledger Fabric Documentation</em>. <a href="https://hyperledger-fabric.readthedocs.io/en/latest/orderer/ordering_service.html">https://hyperledger-fabric.readthedocs.io/en/latest/orderer/ordering_service.html</a></li>
</ol>

---

## Changelog

**2026-08-18** Cut the intro's closing paragraph, which restated the crash-vs-Byzantine decision rule already given at the end of the prior paragraph, and collapsed a second list of Byzantine failure behaviors that duplicated the three examples opening the post. Replaced the four-organization opening scenario with a simpler two-coworker illustration of the core 3-vs-4 mechanism, trimmed "Count Your Traitors" so it calls back to that scene instead of re-illustrating it from scratch, and rewrote the BFT applicability sentence to stop using "belongs" in two different senses in one sentence.  

**2026-08-14** Fixed a citation numbering error where a second Hyperledger Fabric fact was miscited to the same source as the first, and removed an unused reference entry that didn't actually support the claim.  

**2026-08-13** Initial draft.
