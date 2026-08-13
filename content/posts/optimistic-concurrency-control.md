---
title: "Optimistic Concurrency Control"
date: 2026-08-05
publishdate: 2026-08-05
lastmod: 2026-08-08
summary: "Optimistic concurrency control assumes conflicts are rare and checks for them after the fact instead of locking first. Figma, Modern Treasury, and Postgres show when that bet pays off, and when the safer lock still wins."
tags: ["concurrency", "databases", "systems"]
image: /images/optimistic-concurrency-control.jpg
draft: true
---

![Optimistic concurrency control assumes conflicts are rare and checks for them after the fact instead of locking first. Figma, Modern Treasury, and Postgres show when that bet pays off, and when the safer lock still wins.](/images/optimistic-concurrency-control.jpg)
*Traffic at Place Charles de Gaulle, seen from atop the Arc de Triomphe, Paris. Photo: [BrokenSphere (2005)](https://commons.wikimedia.org/wiki/File:Traffic_seen_from_top_of_Arc_de_Triomphe.JPG). CC BY-SA 3.0.*

## Optimistic Concurrency Control

In 2019, Figma's engineers explained a decision that looks backward for a tool where dozens of people edit the same design at once. They rejected Operational Transformation, the conflict-resolution approach Google Docs uses, calling it unnecessarily complex for their problem{{< cite 1 "Wallace, Evan (2019). How Figma's Multiplayer Technology Works. Figma Blog, October 16, 2019." >}}. In its place, every client applies edits immediately, without waiting for permission from the server or from anyone else editing the same file. When two people touch the same property at the same moment, the server keeps whichever edit arrived last and discards the other.

Figma co-founder Evan Wallace was direct about the tradeoff. Two colliding edits produce "either AB or BC but never ABC. That's ok with us because Figma is a design tool, not a text editor{{< cite 1 "Wallace, Evan (2019). How Figma's Multiplayer Technology Works. Figma Blog, October 16, 2019." >}}." Figma bet that real conflicts, two people editing the same property of the same object in the same instant, would be rare enough that losing one occasionally beats coordinating on every single edit.

That bet has a name. Optimistic concurrency control assumes collisions are the exception, lets every operation proceed immediately, and only checks for a conflict when it's time to commit. Pessimistic concurrency control assumes collisions are common enough to prevent up front, and makes every other writer wait for a lock before touching the same data. Figma picked optimism. Whether that's the right call depends entirely on what you're building.

## What Is Optimistic Concurrency Control?

H.T. Kung and John Robinson gave the strategy its name in a 1979 conference paper, expanded two years later into the more widely cited 1981 journal version, formalizing what database systems had been groping toward. Instead of locking a record for the duration of a transaction, let transactions run against a private view of the data, and validate at commit time that nothing conflicting happened in between{{< cite 2 "Kung, H.T., and John Robinson (1981). On Optimistic Methods for Concurrency Control. ACM Transactions on Database Systems, 6(2): 213-226." >}}. If validation passes, the transaction commits. If it fails, the transaction restarts.

The strategy only pays off under one condition. Conflicts have to be rare enough that most transactions validate cleanly on the first try. Kung and Robinson's own analysis showed optimistic methods beating locking under low contention, and losing to locking as contention rose, since a busy system wastes more and more work on transactions that run to completion and then get discarded{{< cite 2 "Kung, H.T., and John Robinson (1981). On Optimistic Methods for Concurrency Control. ACM Transactions on Database Systems, 6(2): 213-226." >}}.

Pessimistic concurrency control is the older, more intuitive idea. Acquire a lock before you touch anything, and hold it until you're done, guaranteeing no one else can create a conflict in the first place. It trades throughput for certainty. A lock costs something on every operation, whether or not a conflict would have happened, while optimism only pays a cost when a conflict actually occurs.

## Where Optimism Wins

Figma is one answer to when optimism wins. Modern Treasury's answer is less obvious.

In 2021, the team building Modern Treasury's ledger API, infrastructure other financial companies build their own ledgers on top of, chose optimistic locking for a domain most engineers assume needs the opposite. Their reasoning was straightforward. The large majority of ledger operations are reads, and a pessimistic lock would force every read to wait behind every write, and every write to wait behind every read{{< cite 3 "Qin, Andy (2021). Designing the Ledgers API with Optimistic Locking. Modern Treasury Journal, August 15, 2021." >}}. Optimism let reads and writes proceed independently, with a version check catching the rare case where two writes actually collided.

Neither team picked optimism because conflicts never happen. They picked it because they measured how often conflicts happen in their specific workload, and the number was low enough that occasionally re-running a check beats permanently paying a locking cost. That check, comparing a version or a value before committing, is the [compare-and-swap](/posts/compare-and-swap/) mechanism. This post is about the decision to reach for it in the first place.

## Where It Loses

Optimism stops paying off the moment a single record becomes a real hot spot, hundreds of writers racing to update the same counter, the same inventory row, the same account balance, all at once. Under that kind of contention, most optimistic writes fail validation and have to retry, and the retries themselves add to the contention, so throughput can fall as load rises instead of scaling with it.

Postgres's own documentation recommends the opposite strategy for this kind of contention. Acquire an explicit row lock with `SELECT FOR UPDATE` before reading a row you're about to update, so competing transactions queue up instead of racing toward a conflict{{< cite 4 "PostgreSQL Global Development Group (2026). 13.3. Explicit Locking. PostgreSQL Documentation." >}}. A queue is slower per operation than an uncontested optimistic write, but its throughput stays predictable as contention rises, where optimism's throughput collapses.

Banking and inventory systems reach for locks more often than web APIs do. Modern Treasury shows the reason has nothing to do with money being special. It's that a specific account or a specific item can become a real hot spot, something an average API request rarely does.

## Counterarguments

**Assuming optimism instead of measuring it.** Figma and Modern Treasury measured their actual conflict rates before committing to a strategy. Copying their conclusion without checking whether your own workload shares their conflict-rarity or their read-heavy shape means copying an answer to a question you never asked.

**Applying one strategy uniformly across a whole system.** Most systems have contention concentrated in a handful of rows, a popular item, a shared counter, sitting inside an otherwise low-contention workload. The right call is usually optimistic almost everywhere, with pessimistic locking reserved for the few known hot spots rather than applied as a single global policy across every table.

**Ignoring the user-facing cost of a rejected optimistic write.** A rejected write is invisible to a background job, which just retries. To a person, it's a discarded form or a "someone else already changed this" error interrupting their work. Choosing optimism without a plan for that moment pushes the cost of the collision onto the user instead of absorbing it in the system.

## Put It Into Practice

Before picking a strategy, measure how often two writers actually touch the same record. Pull recent write logs for the resource in question and count actual collisions rather than raw request volume. A system that looks busy in aggregate can still have near-zero contention on any single row.

Default to optimistic almost everywhere, since it costs nothing when workloads are uncontended, which most of them are. Reserve pessimistic locking for the specific rows or resources where measurement shows real contention, a popular auction item, a shared balance, a single hot counter, and lock only those.

Whichever way you land, plan for the failure path before you ship it. An optimistic write that gets rejected needs a retry strategy if it's a background process, or a clear message if it's a person waiting on the other end. A pessimistic lock that's held too long needs a timeout. Neither strategy is free. The right one is whichever costs less for what your system actually does.

## Go Further

**Serializable Snapshot Isolation.** Postgres and other modern databases automate the validation step instead of leaving it to application code, detecting the same read-write conflicts optimistic concurrency control checks for, but doing it inside the database engine itself{{< cite 5 "Cahill, Michael J., Uwe Röhm, and Alan Fekete (2008). Serializable Isolation for Snapshot Databases. Proceedings of the 2008 ACM SIGMOD International Conference on Management of Data: 729-738." >}}.

**Sagas.** A single compare-and-swap or lock covers one record. Coordinating a transaction that spans multiple services or resources, moving money between two separate ledgers, for instance, needs a different pattern entirely{{< cite 6 "Garcia-Molina, Hector, and Kenneth Salem (1987). Sagas. ACM SIGMOD Record, 16(3): 249-259." >}}.

**Deadlock detection.** Pessimistic locking has a failure mode optimism doesn't share. Two transactions each hold a lock the other one needs, and neither can proceed. Postgres detects these automatically and aborts one of the transactions to break the cycle{{< cite 4 "PostgreSQL Global Development Group (2026). 13.3. Explicit Locking. PostgreSQL Documentation." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Wallace, Evan (2019). "How Figma's Multiplayer Technology Works." <em>Figma Blog</em>, October 16, 2019. <a href="https://www.figma.com/blog/how-figmas-multiplayer-technology-works/">https://www.figma.com/blog/how-figmas-multiplayer-technology-works/</a></li>
  <li id="ref-2">Kung, H.T., and John Robinson (1981). "On Optimistic Methods for Concurrency Control." <em>ACM Transactions on Database Systems</em>, 6(2): 213-226. <a href="https://dl.acm.org/doi/10.1145/319566.319567">https://dl.acm.org/doi/10.1145/319566.319567</a></li>
  <li id="ref-3">Qin, Andy (2021). "Designing the Ledgers API with Optimistic Locking." <em>Modern Treasury Journal</em>, August 15, 2021. <a href="https://www.moderntreasury.com/journal/designing-ledgers-with-optimistic-locking">https://www.moderntreasury.com/journal/designing-ledgers-with-optimistic-locking</a></li>
  <li id="ref-4">PostgreSQL Global Development Group (2026). "13.3. Explicit Locking." <em>PostgreSQL Documentation</em>. <a href="https://www.postgresql.org/docs/current/explicit-locking.html">https://www.postgresql.org/docs/current/explicit-locking.html</a></li>
  <li id="ref-5">Cahill, Michael J., Uwe Röhm, and Alan Fekete (2008). "Serializable Isolation for Snapshot Databases." <em>Proceedings of the 2008 ACM SIGMOD International Conference on Management of Data</em>: 729-738. <a href="https://dl.acm.org/doi/10.1145/1376616.1376690">https://dl.acm.org/doi/10.1145/1376616.1376690</a></li>
  <li id="ref-6">Garcia-Molina, Hector, and Kenneth Salem (1987). "Sagas." <em>ACM SIGMOD Record</em>, 16(3): 249-259. <a href="https://dl.acm.org/doi/10.1145/38713.38742">https://dl.acm.org/doi/10.1145/38713.38742</a></li>
</ol>

---

## Changelog

**2026-08-08** Fixed the Kung and Robinson date; the strategy got its name in a 1979 VLDB conference paper, not the 1981 journal version, which was an expansion of it.  
**2026-08-05** Initial release.  
