---
title: "Compare-And-Swap"
date: 2026-08-05
publishdate: 2026-08-05
lastmod: 2026-08-05
summary: "Compare-and-swap lets distributed systems reject conflicting writes without taking a lock. A 2015 Starbucks bug shows what happens when a system skips it."
tags: ["distributed", "concurrency", "algorithms"]
image: /images/compare-and-swap.jpg
draft: true
---

![Compare-and-swap lets distributed systems reject conflicting writes without taking a lock. A 2015 Starbucks bug shows what happens when a system skips it.](/images/compare-and-swap.jpg)
*Interlocking cogwheels in a water dam mechanism, Vantaa, Finland. Photo: [Boaz Yoffe (2014)](https://commons.wikimedia.org/wiki/File:Cogwheels_In_Vantaa_(137564257).jpeg). CC BY 3.0.*

## Compare-And-Swap

In March 2015, security researcher Egor Homakov bought three Starbucks gift cards and loaded five dollars onto each one. He opened two browser sessions and fired two transfer requests at the same instant, both moving the same five dollars out of one card and into two different cards{{< cite 1 "Homakov, Egor (2015). Hacking Starbucks for unlimited coffee. Sakurity Blog, May 21, 2015." >}}. Both requests read a balance of five dollars before either wrote anything back. Starbucks's servers wrote whatever the client sent, regardless of what had already changed in between.

Homakov walked away with twenty dollars from a fifteen dollar investment, and verified it was real by buying coffee with it. He reported the bug. Starbucks called it fraud{{< cite 1 "Homakov, Egor (2015). Hacking Starbucks for unlimited coffee. Sakurity Blog, May 21, 2015." >}}.

The fix is simple to state. Before writing a new balance, check that the balance still matches what you read, and only write if it does. Comparing the current value before swapping in the new one is compare-and-swap (CAS), and it's the mechanism a huge share of concurrent systems lean on to avoid exactly this kind of collision.

## What Is Compare-And-Swap?

Compare-and-swap works in one step: read the current value, compare it to what you expected, and write the new value only if the two match. When they don't match, the write fails, and the caller finds out immediately instead of overwriting someone else's change.

The idea predates distributed systems. CPUs have supported it in hardware since the 1970s{{< cite 2 "Herlihy, Maurice (1991). Wait-Free Synchronization. ACM Transactions on Programming Languages and Systems, 13(1): 124-149." >}}, a single instruction that reads, compares, and writes one word of memory before another core gets a chance to interleave. Maurice Herlihy proved in 1991 that compare-and-swap is powerful enough to implement any concurrent algorithm you could build with locks, without ever taking one{{< cite 2 "Herlihy, Maurice (1991). Wait-Free Synchronization. ACM Transactions on Programming Languages and Systems, 13(1): 124-149." >}}. Distributed systems borrowed the same idea and scaled it up. Instead of comparing one machine word, you compare a version number, a hash, or a ref, and the swap happens across a network instead of inside a chip.

Had Starbucks's balance write been a compare-and-swap instead of a blind write, Homakov's second concurrent request would have found the balance already changed and failed instead of succeeding.

## Everyone Reinvents It

**Git.** Every `git push --force-with-lease` is a compare-and-swap on a ref. Before overwriting the remote branch, Git checks that the remote's current commit still matches the commit you last fetched. If someone else pushed in between, the push fails instead of discarding their work{{< cite 3 "Git (2026). git-push Documentation. git-scm.com." >}}.

**Kubernetes.** Every object the API server stores carries a `resourceVersion` field. A `kubectl apply` sends back the version it read, and the write only lands if that version still matches what's in etcd. Two controllers racing to update the same object produce one winner and one `409 Conflict`, no lock required{{< cite 4 "Kubernetes (2026). Kubernetes API Concepts. kubernetes.io." >}}.

**DynamoDB.** A `ConditionExpression` attached to a write tells DynamoDB to check the item's current state before committing. Fail the condition, and DynamoDB returns a `ConditionalCheckFailedException` instead of applying the write{{< cite 5 "Amazon Web Services (2026). Condition Expressions. Amazon DynamoDB Developer Guide." >}}.

**S3.** Amazon added the same capability to plain object storage in November 2024. An `If-Match` header carries the object's expected ETag, and S3 rejects the write if the object changed since the last read, turning what used to require an external locking service into one HTTP header{{< cite 6 "Amazon Web Services (2024). Amazon S3 adds new functionality for conditional writes. aws.amazon.com." >}}.

**ZooKeeper.** Every znode carries a version number that increments on every write. A client submitting an update or delete must supply the version it expects, and the operation fails if that version is stale{{< cite 7 "Apache ZooKeeper (2026). ZooKeeper Programmer's Guide. zookeeper.apache.org." >}}.

None of these systems talk to each other. All five landed on the identical fix, because it's the only fix that avoids taking a lock across a network nobody fully controls.

## Common Mistakes

**Forgetting the retry loop.** A failed compare-and-swap means re-read the current value and try again. Code that gives up after one failed attempt turns a brief collision into a permanent failure.

**Assuming it covers more than one field.** A compare-and-swap on a single key or single machine word only guarantees safety for that one key. Wrapping two separate compare-and-swap calls in application code and calling the pair "atomic" still leaves a window where a third party sees one update land before the other. Real cross-record atomicity requires a transaction that covers both records at once.

**The ABA problem.** A value can change from A to B and back to A before your compare-and-swap runs. The comparison passes, since the value matches what you expected, but state moved underneath you anyway and the swap proceeds on a false assumption. Version numbers and counters that only increase close this hole. Anything that reuses old values reopens it.

**Livelock under heavy contention.** When many writers retry against the same hot key, every retry can still lose the race, and the whole set of clients spins forever without any of them making progress. Backoff between retries turns a livelock into a queue.

## Put It Into Practice

Find the place in your system where two writers can update the same record without either one knowing about the other. It's usually a read, some application logic, then a write, three separate steps with a gap between the first and the last. That gap is where the Starbucks bug lived, and it's where yours does too.

Close the gap with whatever compare-and-swap your storage already gives you. Postgres and MySQL support it through a version column and a `WHERE version = ?` clause. DynamoDB and S3 support it natively. Most key-value stores support it through the CAS or `PutIfAbsent` variant of their write API. You rarely need to build this yourself. You need to notice where you skipped using it.

Then handle the failure path. Treat a rejected compare-and-swap as useful information, the system just told you exactly when a collision would have happened. Retry it, and the collision never turns into a bug.

## Go Further

**Multi-version concurrency control.** Instead of rejecting a collision, MVCC gives every reader a private snapshot and lets writers create new versions rather than overwrite in place. It's how Postgres avoids blocking readers against writers at all{{< cite 8 "PostgreSQL Global Development Group (2026). 13.1. Introduction (Concurrency Control). PostgreSQL Documentation." >}}.

**Raft consensus.** A compare-and-swap against etcd only means something because Raft already got every node in the cluster to agree on what the current value is before you compare against it{{< cite 9 "Ongaro, Diego and John Ousterhout (2014). In Search of an Understandable Consensus Algorithm. USENIX Annual Technical Conference." >}}.

**CRDTs.** The opposite philosophy. Instead of preventing concurrent writes from colliding, design data types where concurrent writes merge automatically by construction{{< cite 10 "Shapiro, Marc, et al. (2011). Conflict-free Replicated Data Types. INRIA Research Report RR-7687." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Homakov, Egor (2015). "Hacking Starbucks for unlimited coffee." <em>Sakurity Blog</em>, May 21, 2015. <a href="https://sakurity.com/blog/2015/05/21/starbucks.html">https://sakurity.com/blog/2015/05/21/starbucks.html</a></li>
  <li id="ref-2">Herlihy, Maurice (1991). "Wait-Free Synchronization." <em>ACM Transactions on Programming Languages and Systems</em>, 13(1): 124-149. <a href="https://dl.acm.org/doi/10.1145/114005.102808">https://dl.acm.org/doi/10.1145/114005.102808</a></li>
  <li id="ref-3">Git (2026). "git-push Documentation." <a href="https://git-scm.com/docs/git-push">https://git-scm.com/docs/git-push</a></li>
  <li id="ref-4">Kubernetes (2026). "Kubernetes API Concepts." <a href="https://kubernetes.io/docs/reference/using-api/api-concepts/">https://kubernetes.io/docs/reference/using-api/api-concepts/</a></li>
  <li id="ref-5">Amazon Web Services (2026). "Condition Expressions." <em>Amazon DynamoDB Developer Guide</em>. <a href="https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Expressions.ConditionExpressions.html">https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Expressions.ConditionExpressions.html</a></li>
  <li id="ref-6">Amazon Web Services (2024). "Amazon S3 adds new functionality for conditional writes." <a href="https://aws.amazon.com/about-aws/whats-new/2024/11/amazon-s3-functionality-conditional-writes/">https://aws.amazon.com/about-aws/whats-new/2024/11/amazon-s3-functionality-conditional-writes/</a></li>
  <li id="ref-7">Apache ZooKeeper (2026). "ZooKeeper Programmer's Guide." <a href="https://zookeeper.apache.org/doc/current/zookeeperProgrammers.html">https://zookeeper.apache.org/doc/current/zookeeperProgrammers.html</a></li>
  <li id="ref-8">PostgreSQL Global Development Group (2026). "13.1. Introduction." <em>PostgreSQL Documentation</em>. <a href="https://www.postgresql.org/docs/current/mvcc-intro.html">https://www.postgresql.org/docs/current/mvcc-intro.html</a></li>
  <li id="ref-9">Ongaro, Diego, and John Ousterhout (2014). "In Search of an Understandable Consensus Algorithm." <em>USENIX Annual Technical Conference</em>. <a href="https://raft.github.io/raft.pdf">https://raft.github.io/raft.pdf</a></li>
  <li id="ref-10">Shapiro, Marc, Nuno Preguiça, Carlos Baquero, and Marek Zawirski (2011). "Conflict-free Replicated Data Types." <em>INRIA Research Report RR-7687</em>. <a href="https://inria.hal.science/inria-00609399v2">https://inria.hal.science/inria-00609399v2</a></li>
</ol>

---

## Changelog

**2026-08-05** Initial release. Replaced hero image with interlocking cogwheels.
