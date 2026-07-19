---
title: "Consistent Hashing"
date: 2026-07-11
publishdate: 2026-07-11
lastmod: 2026-07-11
summary: "In 1997, MIT researchers solved a web caching problem so well two of them built a company on it. Adding or removing a server shouldn't force you to remap almost every key."
tags: ["systems", "scaling", "distributed"]
image: /images/consistent-hashing.jpg
draft: true
---

![In 1997, MIT researchers solved a web caching problem so well two of them built a company on it. Adding or removing a server shouldn't force you to remap almost every key.](/images/consistent-hashing.jpg)
*MIT's Building 10 and Great Dome, where the consistent hashing paper originated. Photo: [John Phelan (2011)](https://commons.wikimedia.org/wiki/File:MIT_Building_10_and_the_Great_Dome,_Cambridge_MA.jpg). CC BY 3.0.*

## Consistent Hashing

In the mid-1990s, popular websites kept falling over. A link from a big enough source would send a wall of traffic at one web server while a hundred others sat idle, because nothing was spreading the load intelligently. Six researchers at MIT set out to fix that, and the fix they published in 1997 turned out to matter far beyond web caching{{< cite 1 "Karger, David, et al. (1997). Consistent Hashing and Random Trees: Distributed Caching Protocols for Relieving Hot Spots on the World Wide Web. Proceedings of the 29th Annual ACM Symposium on Theory of Computing." >}}.

Two of the six, Tom Leighton and Daniel Lewin, liked the idea enough to start a company on it. That company was Akamai, and the algorithm is the reason a website can add or remove a server without reshuffling almost everything it owns.

## What Is Consistent Hashing?

The obvious way to spread keys across servers is modulo hashing: hash the key, divide by the number of servers, and the remainder tells you which one owns it. It works fine until the number of servers changes. Add one server and the divisor changes, which changes the remainder for nearly every key in the system. A cache that was 95 percent warm goes cold in one deploy.

Consistent hashing fixes this by hashing servers and keys onto the same ring instead of into buckets. Each server gets a position on the ring from hashing its identity. Each key also gets a position from hashing its own value. A key belongs to whichever server is next going clockwise{{< cite 1 "Karger, David, et al. (1997). Consistent Hashing and Random Trees: Distributed Caching Protocols for Relieving Hot Spots on the World Wide Web. Proceedings of the 29th Annual ACM Symposium on Theory of Computing." >}}. Add a server and it only steals keys from its immediate clockwise neighbor. Remove one and only its neighbor absorbs its keys. Everyone else on the ring is untouched.

## Virtual Nodes

A ring with one position per server has an obvious problem. With only a handful of servers, the gaps between them on the ring are wildly uneven by chance, one server might own 40 percent of the keyspace while another owns 10 percent.

The fix is to give each physical server many positions on the ring instead of one, called virtual nodes. Apache Cassandra defaults to 256 virtual nodes per physical machine{{< cite 2 "Dynamo. Apache Cassandra Documentation." >}}. More positions per server means the law of large numbers does the work. The keyspace ends up close to evenly split, and when a server does fail, its load spreads across many neighbors instead of dumping onto just one.

## What Hashing Doesn't Fix

Consistent hashing solves the rebalancing kind of hot spot, the one caused by nodes joining or leaving. It does nothing for a single, wildly popular key. A viral post, a trending item, a celebrity's account still routes to exactly one node no matter how good the hash function is, because routing a key to one place is the entire point of hashing it{{< cite 3 "Kleppmann, Martin (2017). Designing Data-Intensive Applications: The Big Ideas Behind Reliable, Scalable, and Maintainable Systems. O'Reilly Media." >}}.

The usual fix is to salt the hot key with a random prefix, splitting its writes across several nodes instead of one. Reads pay for it, now they have to check every salted variant instead of a single key, but a write-heavy hot spot is often worth that trade.

## Where This Shows Up Today

Cassandra and DynamoDB both use a version of this ring directly. Cassandra hashes each key with Murmur3 onto a ring spanning -2^63 to 2^63 - 1, assigns every node a set of token ranges on that ring (256 of them by default, thanks to virtual nodes), and resolves ownership by walking the ring clockwise from the key's position{{< cite 2 "Dynamo. Apache Cassandra Documentation." >}}. Adding a node to a running cluster means it claims a slice of ring space from its neighbors and nothing else moves.

## Common Mistakes

**Confusing "consistent hashing" with "consistency."** The word is doing two unrelated jobs. Consistent hashing is about which server owns a key staying stable as the cluster changes. Consistency, in the CAP theorem sense, is about whether every replica agrees on a value at a given moment. A system can use consistent hashing and still be eventually consistent, or strongly consistent, or anything in between. The hashing scheme says nothing about the consistency model.

**Skipping virtual nodes.** A ring with one position per physical server looks like it works in testing with a handful of nodes and then produces visibly uneven load the moment it's under real traffic.

**Assuming the ring solves replication too.** Consistent hashing tells you who owns a key. It doesn't by itself tell you how many replicas to keep or how to keep them in sync. Real systems layer a replication strategy on top of the ring. They don't get it for free from the hashing scheme.

**Rehashing everything on any topology change.** The whole point of the ring is that most keys don't move. A migration script that redistributes every key whenever a node joins has quietly reinvented modulo hashing on top of a ring, throwing away the property that made the algorithm worth using.

**Assuming hash-based partitioning has no cost.** Consistent hashing destroys key ordering. Keys that were adjacent before hashing land on different nodes, so a range query can't be routed to one partition and has to fan out to all of them{{< cite 3 "Kleppmann, Martin (2017). Designing Data-Intensive Applications: The Big Ideas Behind Reliable, Scalable, and Maintainable Systems. O'Reilly Media." >}}. Cassandra's own fix is a compound key, hashing the first column for distribution while sorting the remaining columns within that partition.

**Assuming virtual nodes bound your worst case.** Virtual nodes smooth load out statistically, but plain consistent hashing's theoretical worst case still lets one node end up overloaded by a factor of roughly log n over log log n. Consistent hashing with bounded loads caps every node at close to its fair share, overflowing excess keys to the next node clockwise, which guarantees worst-case load stays within about twice the average{{< cite 4 "Mirrokni, Vahab, Mikkel Thorup, and Morteza Zadimoghaddam (2016). Consistent Hashing with Bounded Loads. arXiv." >}}.

## Put It Into Practice

Before choosing a partitioning scheme for anything you expect to scale, ask what happens to existing data when the node count changes. If the honest answer is "most of it moves," you're one hot spot away from a bad night.

If you're already running something ring-based, check whether it's actually using virtual nodes and how many. A default that was fine at ten nodes may be uneven at three, or the reverse.

Separate your partitioning question from your consistency question. They're solved by different parts of the system, and treating them as one decision is how the naming confusion becomes a design confusion.

## Go Further

**The original paper.** Karger et al.'s STOC 1997 paper is more mathematically dense than this post, including formal bounds on load balance that are worth reading if you want the actual proof, not just the intuition{{< cite 1 "Karger, David, et al. (1997). Consistent Hashing and Random Trees: Distributed Caching Protocols for Relieving Hot Spots on the World Wide Web. Proceedings of the 29th Annual ACM Symposium on Theory of Computing." >}}.

**Alternatives to the ring.** Rendezvous hashing skips the ring entirely, computing a weight for each key-node pair and picking the highest one, simple and evenly distributed but slow to look up once a cluster gets large. Google's Jump Consistent Hash trades that flexibility for near-zero memory overhead, at the cost of only supporting append-only growth rather than arbitrary node removal{{< cite 5 "Lamping, John, and Eric Veach (2014). A Fast, Minimal Memory, Consistent Hash Algorithm. arXiv." >}}. Maglev precomputes a lookup table for O(1) lookups inside Google's own network load balancer.

---

## References

<ol class="references">
  <li id="ref-1">Karger, David, Eric Lehman, Tom Leighton, Rina Panigrahy, Matthew Levine, and Daniel Lewin (1997). "Consistent Hashing and Random Trees: Distributed Caching Protocols for Relieving Hot Spots on the World Wide Web." <em>Proceedings of the 29th Annual ACM Symposium on Theory of Computing</em>: 654-663. <a href="https://dl.acm.org/doi/10.1145/258533.258660">https://dl.acm.org/doi/10.1145/258533.258660</a></li>
  <li id="ref-2">"Dynamo." <em>Apache Cassandra Documentation</em>. <a href="https://cassandra.apache.org/doc/3.11/cassandra/architecture/dynamo.html">https://cassandra.apache.org/doc/3.11/cassandra/architecture/dynamo.html</a></li>
  <li id="ref-3">Kleppmann, Martin (2017). <em>Designing Data-Intensive Applications: The Big Ideas Behind Reliable, Scalable, and Maintainable Systems</em>. O'Reilly Media. <a href="https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/">https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/</a></li>
  <li id="ref-4">Mirrokni, Vahab, Mikkel Thorup, and Morteza Zadimoghaddam (2016). "Consistent Hashing with Bounded Loads." arXiv. <a href="https://arxiv.org/abs/1608.01350">https://arxiv.org/abs/1608.01350</a></li>
  <li id="ref-5">Lamping, John, and Eric Veach (2014). "A Fast, Minimal Memory, Consistent Hash Algorithm." arXiv. <a href="https://arxiv.org/abs/1406.2294">https://arxiv.org/abs/1406.2294</a></li>
</ol>

---

## Outtakes

**One of the paper's co-authors didn't live to see how far it went.** Daniel Lewin, Akamai's co-founder, was killed aboard American Airlines Flight 11 on September 11, 2001, believed to be the first victim of the attacks after apparently trying to stop the hijackers ([Wikipedia, n.d.](https://en.wikipedia.org/wiki/Daniel_Lewin)).

**Akamai still runs on the idea.** The company Leighton and Lewin founded in 1998 on this exact algorithm now delivers a large share of the world's web traffic, built from a paper originally aimed at stopping popular sites from collapsing under sudden traffic spikes ([ACM DL, 1997](https://dl.acm.org/doi/10.1145/258533.258660)).

---

## Changelog

**2026-07-11** Initial release.  
