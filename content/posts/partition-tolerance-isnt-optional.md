---
title: "Partition Tolerance Isn't Optional"
date: 2026-07-11
publishdate: 2026-07-11
lastmod: 2026-07-11
summary: "In 2013, Jepsen's tests caught MongoDB losing writes it had already acknowledged, proof that the CAP theorem's real lesson isn't picking two of three. Partitions happen whether you plan for them or not."
tags: ["systems", "databases", "distributed"]
image: /images/partition-tolerance-isnt-optional.jpg
draft: true
---

![In 2013, Jepsen's tests caught MongoDB losing writes it had already acknowledged, proof that the CAP theorem's real lesson isn't picking two of three. Partitions happen whether you plan for them or not.](/images/partition-tolerance-isnt-optional.jpg)
*Cable-laying machinery on the SS Great Eastern, the ship that laid the first lasting transatlantic telegraph cable in 1866. Cables like this one still get cut today. Photo: [National Maritime Museum, Greenwich (1865-1872)](https://commons.wikimedia.org/wiki/File:Cable_laying_machinery_on_the_Great_Eastern_(5092775547).jpg). Public domain.*

## Partition Tolerance Isn't Optional

In May 2013, Kyle Kingsbury pointed his testing tool, Jepsen, at MongoDB and cut the network in half. The database had already told 5,700 clients their writes succeeded. When Kingsbury counted afterward, 2,381 of them were gone, a 42 percent loss rate{{< cite 1 "Kingsbury, Kyle (2013). Jepsen: MongoDB. Jepsen." >}}.

Stronger write settings helped some, but not by much. "Safe" writes still lost 37 percent, and even MAJORITY, MongoDB's strongest setting, should have been airtight. It dropped only 2 writes out of 5,701, but not because those writes were actually safe. A separate bug meant that when the network partitioned, the server checked the success box and sent it back to the client regardless of what had happened{{< cite 1 "Kingsbury, Kyle (2013). Jepsen: MongoDB. Jepsen." >}}. MongoDB called itself strongly consistent. Reality disagreed.

## What Is the CAP Theorem?

Kingsbury's tests are really about the CAP theorem, first stated by Eric Brewer at a 2000 keynote at the ACM Symposium on Principles of Distributed Computing{{< cite 2 "Brewer, Eric (2000). Towards Robust Distributed Systems. PODC 2000 Invited Talk." >}}. Drawing on his years running Inktomi's search infrastructure, Brewer claimed that a shared-data system can guarantee at most two of three properties, consistency, availability, and tolerance of network partitions. Two years later, Seth Gilbert and Nancy Lynch turned Brewer's conjecture into a formal proof{{< cite 3 "Gilbert, Seth, and Nancy Lynch (2002). Brewer's Conjecture and the Feasibility of Consistent, Available, Partition-Tolerant Web Services. ACM SIGACT News." >}}.

Their definitions matter more than the folk version most engineers repeat. Consistency means linearizability specifically. Every read returns the most recent write, as if the whole system were a single machine. Availability means every request to a live node gets a response, not an error, not a timeout. Partition tolerance means the system keeps working even when the network drops or delays messages between nodes.

You can't have all three at once. Most engineers stop right there and draw a triangle with three labeled corners. That's the version that gets people into trouble.

## The Rule Was Never Pick Two

Look at the triangle again. Partition tolerance isn't a feature you can decline. Networks drop packets, switches fail, someone in a data center trips over the wrong cable. If your system spans more than one machine, a partition will eventually happen whether you designed for it or not{{< cite 4 "Brewer, Eric (2012). CAP Twelve Years Later: How the 'Rules' Have Changed. Computer." >}}.

Brewer made this explicit twelve years after his own talk. The real choice was never "pick two of three." It's what your system does with consistency and availability during the specific window when a partition is happening. Outside that window, with the network intact, there's no CAP tradeoff to make at all. A well-built system can be consistent and available essentially all the time, then give up one of the two for the seconds or minutes a partition lasts.

That's a narrower claim than the "pick two" folklore suggests, and a far more useful one. It tells you exactly when the tradeoff bites, not at design time. At partition time.

## The Labels Oversimplify

The other half of the confusion is what people mean by "consistent." Database marketing slaps "CP" or "AP" on a product like a nutrition label, but real systems rarely sit at one clean point in that space{{< cite 5 "Kleppmann, Martin (2015). A Critique of the CAP Theorem. arXiv." >}}.

MongoDB's Jepsen results show why. A single database offered several write concern levels, each trading durability for speed differently, and even its strongest setting had a bug that undercut the label entirely. Calling that one system "CP" or "AP" flattens all of that into one label. Kleppmann's critique goes further. The C in CAP is linearizability, a stricter guarantee than what most engineers mean when they call their own system "consistent." A database can offer strong guarantees within a single row and weaker ones across rows, or change its behavior between versions, all while carrying the same two-letter label.

## Where This Shows Up Today

Google's Spanner is the system usually cited as proof CAP is obsolete. Spanner uses GPS receivers and atomic clocks in its data centers, a system called TrueTime, to bound clock uncertainty to about six milliseconds and offer strongly consistent transactions at global scale{{< cite 6 "Corbett, James C., et al. (2012). Spanner: Google's Globally-Distributed Database. OSDI." >}}. That's real and impressive, but it doesn't repeal the theorem. Brewer has said as much. During an actual partition, Spanner chooses consistency and forfeits availability, making it technically a CP system{{< cite 7 "Brewer, Eric (2017). Spanner, TrueTime and the CAP Theorem. Google Research." >}}. TrueTime just makes the ordinary, non-partitioned case fast enough that the tradeoff rarely feels like one.

Cassandra takes the opposite approach. Instead of hiding the tradeoff, it exposes it per query. Every read or write specifies a consistency level, ONE for speed, QUORUM for a majority of replicas, ALL for the strongest guarantee Cassandra offers{{< cite 8 "Dynamo. Apache Cassandra Documentation." >}}. At weaker levels like ONE, Cassandra doesn't promise a read sees the latest write. It promises eventual consistency instead. Replicas converge over time, reconciled by background anti-entropy repair{{< cite 8 "Dynamo. Apache Cassandra Documentation." >}}. Two queries against the same cluster can sit at different points on the CAP spectrum, which is exactly the kind of nuance a single "AP" label can't capture.

## Common Mistakes

**Slapping a CP or AP label on a whole system.** Real databases mix consistency levels by query or by write concern. A single label describes a marketing decision, not the system's actual behavior under partition{{< cite 5 "Kleppmann, Martin (2015). A Critique of the CAP Theorem. arXiv." >}}.

**Trying to build a "CA" system.** Some products still advertise consistency and availability with no partition tolerance tradeoff at all. That's not a third option. It just means nobody has tested what happens when the network splits, and CAP says an answer exists whether the vendor has found it yet or not{{< cite 9 "Hale, Coda (2010). You Can't Sacrifice Partition Tolerance." >}}.

**Assuming "consistency" means what you think it means.** In CAP, consistency is linearizability, not "the data is generally correct" or "the replicas will eventually agree." Confirm which one your database actually promises before you build around it.

**Assuming "availability" means uptime.** CAP's availability means every request to a live node gets a non-error response, immediately, even during a partition. A system with a 99.99 percent uptime SLA can still fail CAP's availability test in the specific seconds a partition is active.

## What To Do About It

Before you trust a "CP" or "AP" label, ask what happens to a specific write or read during an actual partition, not during normal operation. That's the only moment CAP has anything to say about.

If you're picking a database, read past the marketing page. Check the consistency level it defaults to, whether that default is configurable per query, and what its own test suite, or someone else's Jepsen report, says happens when nodes stop talking to each other.

The CAP theorem isn't a menu you check off once at design time. It's a description of what your system already does the next time a cable gets cut.

## Go Further

**PACELC.** Daniel Abadi's 2010 extension notes that even without a partition, there's still a tradeoff between latency and consistency during ordinary operation, something CAP says nothing about{{< cite 10 "Abadi, Daniel (2010). Problems with CAP, and Yahoo's Little Known NoSQL System. DBMS Musings." >}}.

**The Jepsen project.** Kingsbury's MongoDB report from 2013 was the first of dozens. The project keeps testing new databases against the same partition scenarios, and keeps finding gaps between what vendors claim and what their systems do{{< cite 11 "Kingsbury, Kyle. Jepsen." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Kingsbury, Kyle (2013). "Jepsen: MongoDB." <em>Jepsen</em>. <a href="https://aphyr.com/posts/284-call-me-maybe-mongodb">https://aphyr.com/posts/284-call-me-maybe-mongodb</a></li>
  <li id="ref-2">Brewer, Eric (2000). "Towards Robust Distributed Systems." <em>PODC 2000 Invited Talk</em>. <a href="https://www.podc.org/podc2000/brewer.html">https://www.podc.org/podc2000/brewer.html</a></li>
  <li id="ref-3">Gilbert, Seth, and Nancy Lynch (2002). "Brewer's Conjecture and the Feasibility of Consistent, Available, Partition-Tolerant Web Services." <em>ACM SIGACT News</em>, 33(2): 51-59. <a href="https://dl.acm.org/doi/10.1145/564585.564601">https://dl.acm.org/doi/10.1145/564585.564601</a></li>
  <li id="ref-4">Brewer, Eric (2012). "CAP Twelve Years Later: How the 'Rules' Have Changed." <em>Computer</em>, 45(2): 23-29. <a href="https://ieeexplore.ieee.org/document/6133253">https://ieeexplore.ieee.org/document/6133253</a></li>
  <li id="ref-5">Kleppmann, Martin (2015). "A Critique of the CAP Theorem." <em>arXiv</em>. <a href="https://arxiv.org/abs/1509.05393">https://arxiv.org/abs/1509.05393</a></li>
  <li id="ref-6">Corbett, James C., et al. (2012). "Spanner: Google's Globally-Distributed Database." <em>OSDI</em>. <a href="https://research.google/pubs/spanner-googles-globally-distributed-database-2/">https://research.google/pubs/spanner-googles-globally-distributed-database-2/</a></li>
  <li id="ref-7">Brewer, Eric (2017). "Spanner, TrueTime and the CAP Theorem." <em>Google Research</em>. <a href="https://research.google/pubs/spanner-truetime-and-the-cap-theorem/">https://research.google/pubs/spanner-truetime-and-the-cap-theorem/</a></li>
  <li id="ref-8">"Dynamo." <em>Apache Cassandra Documentation</em>. <a href="https://cassandra.apache.org/doc/latest/cassandra/architecture/dynamo.html">https://cassandra.apache.org/doc/latest/cassandra/architecture/dynamo.html</a></li>
  <li id="ref-9">Hale, Coda (2010). "You Can't Sacrifice Partition Tolerance." <a href="https://web.archive.org/web/20250905071809/https://codahale.com/you-cant-sacrifice-partition-tolerance/">https://web.archive.org/web/20250905071809/https://codahale.com/you-cant-sacrifice-partition-tolerance/</a></li>
  <li id="ref-10">Abadi, Daniel (2010). "Problems with CAP, and Yahoo's Little Known NoSQL System." <em>DBMS Musings</em>. <a href="http://dbmsmusings.blogspot.com/2010/04/problems-with-cap-and-yahoos-little.html">http://dbmsmusings.blogspot.com/2010/04/problems-with-cap-and-yahoos-little.html</a></li>
  <li id="ref-11">Kingsbury, Kyle. "Jepsen." <a href="https://jepsen.io/">https://jepsen.io/</a></li>
</ol>

---

## Outtakes

**Jepsen's name is a pop song pun.** Kingsbury's testing series is named after Carly Rae Jepsen's "Call Me Maybe." The original 2013 post's URL still carries the joke, literally titled "call-me-maybe-carly-rae-jepsen-and-the-perils-of-network-partitions" ([Kingsbury, 2013](https://aphyr.com/posts/281-call-me-maybe-carly-rae-jepsen-and-the-perils-of-network-partitions)).

**Brewer's talk was never a paper.** The original CAP theorem was delivered only as a PODC keynote in July 2000, never published as a paper ([PODC, 2000](https://www.podc.org/podc2000/brewer.html)). What people cite as "the CAP theorem" is technically Gilbert and Lynch's proof, published two years later.

**Spanner keeps atomic clocks in its data centers.** Google's Spanner runs GPS receivers and atomic clocks inside its own facilities, not just at the network edge, to shrink clock uncertainty to about six milliseconds ([Corbett et al., 2012](https://research.google/pubs/spanner-googles-globally-distributed-database-2/)).

---

## Changelog

**2026-07-11** Initial release. Swapped the hero image to a period photo of the Great Eastern's cable-laying machinery.  
