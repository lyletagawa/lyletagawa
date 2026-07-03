---
title: "Probably Not There"
date: 2026-06-27
publishdate: 2026-06-27
lastmod: 2026-06-27
summary: "A Bloom filter answers one question well. It can guarantee an item is absent but only guess whether it is present, and that one-sided error is the whole point."
tags: ["algorithms", "performance", "caching"]
image: /images/bloom-filter.png
draft: true
---

![A Bloom filter answers one question well. It can guarantee an item is absent but only guess whether it is present, and that one-sided error is the whole point.](/images/bloom-filter.png)
*Image: [Eppstein, David (2008)](https://en.wikipedia.org/wiki/File:Bloom_filter.svg). Public domain.*

## Probably Not There

A database reads a key from disk. The seek costs milliseconds, and the answer is usually "not found." Most lookups miss. Cassandra keeps dozens of immutable data files open at once, and a naive read would scan every one on every query. Before touching the disk, the engine asks a tiny in-memory structure one question. Is this key definitely not in this file? If the answer is no, the read skips the file. If the answer is maybe, the engine reads the file and finds nothing. Annoying. Acceptable. Rare.

That structure is a Bloom filter. It trades certainty for space, and bends in one direction only. It can tell you with absolute confidence that something is absent. It can only guess that something is present. Every useful application sits on top of that asymmetry.

## What a Bloom Filter Does

A Bloom filter tests set membership without storing the set. Burton Howard Bloom described it in 1970 as a way to keep hash lookups in core memory even for huge data sets{{< cite 1 "Bloom, Burton H. (1970). Space/Time Trade-offs in Hash Coding with Allowable Errors. Communications of the ACM, 13(7): 422-426." >}}. His insight was that a small, allowable error in the positive direction buys a drastic cut in memory.

The structure is a bit array, every position starting at zero. To add an item, run it through several hash functions, each mapping to a position, and set those bits to one. To test an item, hash it the same way and read those same bits. If any bit is still zero, the item was never added. Definitely not there. If every bit is one, the item is probably there. Probably.

The "probably" comes from collisions. Two items hash to overlapping positions all the time. A test can read all ones that a different item set, and report a false positive. False negatives are the forbidden case. Once an item's bits are set, a test for that item always finds them set. The filter never says no to something it added.

## Tune the False Positive Rate

Three knobs set the false positive rate. The bit array size, the number of hash functions, and the number of items added{{< cite 2 "Broder, Andrei and Michael Mitzenmacher (2004). Network Applications of Bloom Filters: A Survey. Internet Mathematics, 1(4): 485-509." >}}. The math is exact. With m bits, n items, and k hash functions, the false positive probability is (1 - e^(-kn/m))^k. About 10 bits per item buys a 1% rate. About 15 bits drops it below 0.1%. Every factor of ten in precision costs roughly 5 extra bits per item. The [hur.st calculator](https://hur.st/bloomfilter/) does the arithmetic.

The instinct is to crank the rate as low as possible. Resist it. The right rate is the one where a false positive is cheap enough to absorb. Cassandra defaults to 1% on most strategies and lets you tune per table{{< cite 3 "Apache Cassandra (2024). Bloom Filters. Apache Cassandra Documentation." >}}. A false positive means one extra SSTable (Sorted String Table) scan, which the engine was willing to do anyway. Driving it to 0.001% would triple the memory for a tiny gain. Size the bet to the cost of being wrong.

One trap hides in the math. The rate assumes you insert close to n items. Insert more and the array fills, and the error rate climbs past your target. Size for the lifetime load, not the current load.

## Where This Breaks Down

**You can't remove items.** Flipping a bit back to zero is unsafe because other items may share it. Clear a bit for a deleted key and you create a false negative for every other item that hashes there. Counting Bloom filters solve this by storing a small counter per position instead of a single bit, incrementing on add and decrementing on delete{{< cite 2 "Broder, Andrei and Michael Mitzenmacher (2004). Network Applications of Bloom Filters: A Survey. Internet Mathematics, 1(4): 485-509." >}}. A 4-bit counter per position means four times the space, and a counter can saturate, freezing that position.

**You can't enumerate members.** The filter stores bits, not items. There is no way to ask what was added, only whether a specific candidate was. Keep a real data structure next to the filter if you need the actual set. The filter is the bouncer who checks names against a list, not the list itself.

**A false positive needs a fallback.** A database reads the file anyway and moves on. A security decision cannot. Bitcoin light wallets ship a Bloom filter to full nodes describing which transactions they want. The false positives are intentional noise that hides which addresses the wallet owns{{< cite 4 "Hearn, Mike and Matt Corallo (2012). BIP 37: Connection Bloom Filtering. Bitcoin Improvement Proposals." >}}. The client chooses a high error rate on purpose. The privacy comes from the noise. Remove the fallback and a tolerable miss becomes a wrong answer.

**It degrades as it fills.** The filter has no notion of capacity. It accepts items forever, the bit array approaches all ones, and the error rate climbs toward certainty. A filter built for one million keys at 1% runs at roughly 5% when you load two million. There is no in-place fix. Cross the threshold and rebuild.

## Put It Into Practice

Reach for a Bloom filter when three things are true. The negative case is common and expensive to check. A false positive triggers a recoverable fallback instead of a final decision. And the full set is too large to hold in memory. All three, or find a different structure.

Databases hit all three on every read. Most lookups miss, a miss means a wasted seek, and the union of every key across every file is far larger than RAM. A filter turns a scan of dozens of files into a scan of one or two. Caches hit the same pattern. A Bloom filter holds the seen-set in a fraction of the space and answers "already handled?" fast enough to gate the real work.

The discipline is in the sizing. Pick the error rate from the cost of a false positive, not from a desire for precision. Size the bit array for the maximum number of items you will ever insert, with margin. Put a fallback behind every positive. Do that and the filter pays for itself in skipped work. Do it wrong and you ship a structure that confidently skips files it should have read.

## Go Further

**Counting and cuckoo filters.** When you need deletion, counting Bloom filters add per-position counters at a four-times memory cost. Cuckoo filters take a different approach, storing short fingerprints in a hash table with two candidate buckets per item, and they support deletion natively while using less memory than a standard Bloom filter at low error rates{{< cite 5 "Fan, Bin et al. (2014). Cuckoo Filter: Practically Better Than Bloom. CoNEXT '14." >}}.

**Ribbon filters.** For static sets known at build time, ribbon filters encode membership as a system of binary linear equations solved in batch. They approach the space floor of log2(1/e) bits per item, roughly 30% smaller than a Bloom filter, at the cost of no incremental insertion{{< cite 6 "Dillinger, Peter C. (2021). Ribbon Filter: Practically Smaller than Bloom and Xor. arXiv:2103.02515." >}}.

**The sketch family.** Bloom filters belong to a broader family of probabilistic structures that each trade certainty for space on a different question. HyperLogLog counts distinct items in a stream{{< cite 7 "Flajolet, Philippe et al. (2007). HyperLogLog: The Analysis of a Near-Optimal Cardinality Estimation Algorithm. AOFA '07." >}}. Count-min sketch estimates how often each item appears{{< cite 8 "Cormode, Graham and S. Muthukrishnan (2005). An Improved Data Stream Summary: The Count-Min Sketch and Its Applications. Journal of Algorithms, 55(1): 58-75." >}}. Pick the question, accept a bounded error, shrink the memory.

---

## References

<ol class="references">
  <li id="ref-1">Bloom, Burton H. (1970). "Space/Time Trade-offs in Hash Coding with Allowable Errors." <em>Communications of the ACM</em>, 13(7): 422-426. <a href="https://people.cs.umass.edu/~emery/classes/cmpsci691st/readings/Misc/p422-bloom.pdf">https://people.cs.umass.edu/~emery/classes/cmpsci691st/readings/Misc/p422-bloom.pdf</a></li>
  <li id="ref-2">Broder, Andrei and Michael Mitzenmacher (2004). "Network Applications of Bloom Filters: A Survey." <em>Internet Mathematics</em>, 1(4): 485-509. <a href="https://www.eecs.harvard.edu/~michaelm/postscripts/im2005b.pdf">https://www.eecs.harvard.edu/~michaelm/postscripts/im2005b.pdf</a></li>
  <li id="ref-3">Apache Cassandra (2024). "Bloom Filters." Apache Cassandra Documentation. <a href="https://cassandra.apache.org/doc/latest/cassandra/managing/operating/bloom_filters.html">https://cassandra.apache.org/doc/latest/cassandra/managing/operating/bloom_filters.html</a></li>
  <li id="ref-4">Hearn, Mike and Matt Corallo (2012). "BIP 37: Connection Bloom Filtering." Bitcoin Improvement Proposals. <a href="https://github.com/bitcoin/bips/blob/master/bip-0037.mediawiki">https://github.com/bitcoin/bips/blob/master/bip-0037.mediawiki</a></li>
  <li id="ref-5">Fan, Bin, Dave Andersen, Michael Kaminsky, and Michael Mitzenmacher (2014). "Cuckoo Filter: Practically Better Than Bloom." <em>CoNEXT '14</em>. <a href="https://www.cs.cmu.edu/~dga/papers/cuckoo-conext2014.pdf">https://www.cs.cmu.edu/~dga/papers/cuckoo-conext2014.pdf</a></li>
  <li id="ref-6">Dillinger, Peter C. (2021). "Ribbon Filter: Practically Smaller than Bloom and Xor." <em>arXiv:2103.02515</em>. <a href="https://arxiv.org/abs/2103.02515">https://arxiv.org/abs/2103.02515</a></li>
  <li id="ref-7">Flajolet, Philippe, Éric Fusy, Olivier Gandouet, and Frédéric Meunier (2007). "HyperLogLog: The Analysis of a Near-Optimal Cardinality Estimation Algorithm." <em>AOFA '07</em>. <a href="https://algo.inria.fr/flajolet/Publications/FlFuGaMe07.pdf">https://algo.inria.fr/flajolet/Publications/FlFuGaMe07.pdf</a></li>
  <li id="ref-8">Cormode, Graham and S. Muthukrishnan (2005). "An Improved Data Stream Summary: The Count-Min Sketch and Its Applications." <em>Journal of Algorithms</em>, 55(1): 58-75. <a href="https://doi.org/10.1016/j.jalgor.2003.12.001">https://doi.org/10.1016/j.jalgor.2003.12.001</a></li>
</ol>

---

## Outtakes

**The filter that leaks your wallet.** Bitcoin light wallets with fewer than 20 addresses can have nearly all of them recovered from a Bloom filter. The false positives were meant to hide ownership. With few real addresses, there is not enough noise to hide behind. ([Gervais et al., 2014](https://discovery.ucl.ac.uk/id/eprint/10182350/1/2014-763.pdf))

**The naming bug.** Bloom's 1970 paper never used the term "Bloom filter." Its title is "Space/Time Trade-offs in Hash Coding with Allowable Errors." Later researchers applied the name and it stuck. The structure now fills algorithms courses under a label absent from the original paper.

---

## Changelog

**2026-06-27** Initial release.  
