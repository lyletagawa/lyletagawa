---
title: "Bloom Filter"
date: 2026-06-26
publishdate: 2026-06-26
lastmod: 2026-08-02
summary: "A Bloom filter checks whether something belongs to a set without storing the set. Firefox's CRLite used one to compress revocation data for nearly a billion certificates into 6.7 megabytes."
tags: ["algorithms", "performance"]
image: /images/bloom-filter.png
draft: false
---

![A Bloom filter checks whether something belongs to a set without storing the set. Firefox's CRLite used one to compress revocation data for nearly a billion certificates into 6.7 megabytes.](/images/bloom-filter.png)
*Image: [Eppstein, David (2008)](https://en.wikipedia.org/wiki/File:Bloom_filter.svg). Public domain.*

## Bloom Filter

Every time a Firefox user visits a TLS-protected website, the browser checks whether that site's certificate has been revoked. A revoked certificate means that the site may be impersonated. The traditional approach queries the certificate authority (CA) or OCSP (Online Certificate Status Protocol) server every time, a wasteful roundtrip that also leaks your site visits.

CRLite (Certificate Revocation Lite){{< cite 1 "Mozilla (2025). CRLite: Fast, Private, and Comprehensive Certificate Revocation Checking in Firefox. Mozilla Hacks." >}} was developed by Mozilla to efficiently manage and compress revocation information, initially implemented as a cascade of Bloom filters with each layer handling the false positives of the previous one.

CRLite is shipped to Firefox, so on every HTTP/TLS connection, Firefox checks the certificate's revocation status against the local copy. A result of "definitely not revoked" lets the connection proceed, and anything flagged "might be revoked" triggers a live query instead.

## What a Bloom Filter does

A Bloom filter is a probabilistic data structure for testing set membership. Burton Howard Bloom invented it in 1970 to check membership without having to store the elements themselves{{< cite 2 "Bloom, Burton H. (1970). Space/Time Trade-offs in Hash Coding with Allowable Errors. Communications of the ACM, 13(7): 422-426." >}}.

Adding x sets A[hᵢ(x)] = 1 for each i from 1 to k, and querying x ANDs the bits at those same k positions.

The structure is a bit array, all initialized to zero. To add an item (x), run it through k hash functions, each producing an index into the array. Set those k bits to 1. When querying x, any unset bit means the item isn't present. If all k bits are set, the item is probably present.

When x's bits are all set, those bits might've been set by other items, which results in a false positive. But false negatives are impossible. If an item is present, its bits are set, and the query always returns positive.

The Bloom filter guarantees absence but only approximates presence.

Adding and querying run in constant time, O(k), since they both execute exactly k hash functions regardless of the set size.

A hash set of one million 50-byte (rough size of a CRLite certificate identifier) items is about 100 MB. A Bloom filter for the same set is 1.25 MB{{< cite 3 "Broder, Andrei and Michael Mitzenmacher (2004). Network Applications of Bloom Filters: A Survey. Internet Mathematics, 1(4): 485-509." >}}.

## Tune the false positive rate

The false positive rate depends on array size, hash function count, and items added{{< cite 3 "Broder, Andrei and Michael Mitzenmacher (2004). Network Applications of Bloom Filters: A Survey. Internet Mathematics, 1(4): 485-509." >}}.

At 10 bits per item, the false positive rate is around 1%. The rate rises to 10% at 5 bits and drops below 0.1% at 15 bits. The calculator at [hur.st/bloomfilter](https://hur.st/bloomfilter/) works from your target error rate and item count, and gives you the array size and hash function count you need.

Mozilla's CRLite cascade of Bloom filters covered the revocation status of nearly a billion certificates in a 14.6 MB snapshot, shipped to Firefox every 45 days{{< cite 1 "Mozilla (2025). CRLite: Fast, Private, and Comprehensive Certificate Revocation Checking in Firefox. Mozilla Hacks." >}}. Aside: in 2025, Mozilla replaced it with ribbon filters, further reducing the size to 6.7 MB{{< cite 4 "Larisch, James, et al. (2025). Clubcards for the WebPKI: Smaller Certificate Revocation Tests in Theory and Practice. IACR ePrint 2025/610." >}}.

## Where this breaks down

You can't remove items from a standard Bloom filter. Unsetting a bit could corrupt other items sharing that position. Counting Bloom filters fix this with a counter per position instead of just a single bit.

The false positive rate rises as you add more items. The filter is designed for an expected maximum, and exceeding it pushes the error rate above the design threshold. Set parameters for lifetime, not current load.

The filter discards the items themselves, so enumerating what was added is impossible. To retrieve members, keep a separate data structure alongside the filter.

False positives need a fallback mechanism. Firefox's CRLite works because a false positive triggers a live query to the CA (or OCSP), which is the original (slow) mechanism.

## Put it into practice

Using a Bloom filter needs three conditions: the full dataset is "large," a negative lookup is common and expensive to verify, and a fallback mechanism is present for false positives.

Databases benefit from a Bloom filter when most lookups come back empty. Apache Cassandra keeps a Bloom filter per SSTable (Sorted String Table){{< cite 5 "Apache Cassandra (2024). Bloom Filters. Apache Cassandra Documentation." >}}. If the filter says "definitely not here," Cassandra can skip the data file and avoid any disk I/O for that SSTable.

Deduplication pipelines use them to avoid reprocessing items already handled.

Recommendation systems skip content users have already seen. Medium's reading history per user is too large to query for every request, but a Bloom filter answers "definitely not read" in microseconds{{< cite 6 "Talbot, Josh (2015). What are Bloom filters? Medium Engineering Blog." >}}. A false positive occasionally hides an unread article, which was deemed an acceptable risk.

Capacity is fixed at creation and should be sized for the filter's maximum expected load. A filter built for one million items at 1% error climbs to 5% error at two million. When a filter overflows past its design threshold, it should be rebuilt.

## What we left out

The probability math. Broder and Mitzenmacher derive the false positive rate as a formula. With m bits, n items, and k hash functions, the rate is (1 - e^(-kn/m))^k, and the optimal k is (m/n) × ln(2){{< cite 3 "Broder, Andrei and Michael Mitzenmacher (2004). Network Applications of Bloom Filters: A Survey. Internet Mathematics, 1(4): 485-509." >}}. The numbers above come from that formula.

Estimating item count. You can estimate how many items were inserted just from how many bits are set to 1 (without maintaining a separate counter), useful for monitoring how close a filter is to its design threshold. With X bits currently set, n* ≈ -(m/k) × ln(1 - X/m){{< cite 3 "Broder, Andrei and Michael Mitzenmacher (2004). Network Applications of Bloom Filters: A Survey. Internet Mathematics, 1(4): 485-509." >}}. 

A cuckoo filter achieves a similar false positive rate while supporting deletion{{< cite 7 "Fan, Bin et al. (2014). Cuckoo Filter: Practically Better Than Bloom. CoNEXT '14." >}}. Instead of a bit array, it stores short fingerprints in a hash table with two candidate buckets per item. Deleting an item removes its fingerprint instead of unsetting shared bits.

Firefox replaced CRLite's Bloom filters with ribbon filters in 2025{{< cite 1 "Mozilla (2025). CRLite: Fast, Private, and Comprehensive Certificate Revocation Checking in Firefox. Mozilla Hacks." >}}. Ribbon filters approach the theoretical minimum of log₂(1/p) bits per item for a given false positive rate p, roughly 30% smaller than a Bloom filter at the same rate{{< cite 4 "Larisch, James, et al. (2025). Clubcards for the WebPKI: Smaller Certificate Revocation Tests in Theory and Practice. IACR ePrint 2025/610." >}}. The tradeoff is that ribbon filters are immutable, but CRLite was already static, rebuilt from Certificate Transparency logs and shipped to clients every 45 days.

---

## References

<ol class="references">
  <li id="ref-1">Mozilla (2025). "CRLite: Fast, Private, and Comprehensive Certificate Revocation Checking in Firefox." <em>Mozilla Hacks</em>. <a href="https://hacks.mozilla.org/2025/08/crlite-fast-private-and-comprehensive-certificate-revocation-checking-in-firefox/">https://hacks.mozilla.org/2025/08/crlite-fast-private-and-comprehensive-certificate-revocation-checking-in-firefox/</a></li>
  <li id="ref-2">Bloom, Burton H. (1970). "Space/Time Trade-offs in Hash Coding with Allowable Errors." <em>Communications of the ACM</em>, 13(7): 422-426. <a href="https://dl.acm.org/doi/10.1145/362686.362692">https://dl.acm.org/doi/10.1145/362686.362692</a></li>
  <li id="ref-3">Broder, Andrei and Michael Mitzenmacher (2004). "Network Applications of Bloom Filters: A Survey." <em>Internet Mathematics</em>, 1(4): 485-509. <a href="https://www.eecs.harvard.edu/~michaelm/postscripts/im2005b.pdf">https://www.eecs.harvard.edu/~michaelm/postscripts/im2005b.pdf</a></li>
  <li id="ref-4">Larisch, James, et al. (2025). "Clubcards for the WebPKI: Smaller Certificate Revocation Tests in Theory and Practice." <em>IACR ePrint 2025/610</em>. <a href="https://eprint.iacr.org/2025/610">https://eprint.iacr.org/2025/610</a></li>
  <li id="ref-5">Apache Cassandra (2024). "Bloom Filters." Apache Cassandra Documentation. <a href="https://cassandra.apache.org/doc/latest/cassandra/managing/operating/bloom_filters.html">https://cassandra.apache.org/doc/latest/cassandra/managing/operating/bloom_filters.html</a></li>
  <li id="ref-6">Talbot, Josh (2015). "What are Bloom filters?" Medium Engineering Blog. <a href="https://medium.com/blog/what-are-bloom-filters-1ec2a50c68ff">https://medium.com/blog/what-are-bloom-filters-1ec2a50c68ff</a></li>
  <li id="ref-7">Fan, Bin, Dave Andersen, Michael Kaminsky, and Michael Mitzenmacher (2014). "Cuckoo Filter: Practically Better Than Bloom." <em>CoNEXT '14</em>. <a href="https://www.cs.cmu.edu/~dga/papers/cuckoo-conext2014.pdf">https://www.cs.cmu.edu/~dga/papers/cuckoo-conext2014.pdf</a></li>
</ol>

---

## Outtakes

For web edge caches, resources requested once and only once are a waste of space. Akamai called them "one-hit wonders" and removed them by using a Bloom filter to track first-time requests and added them to the cache only after a second request. ([Maggs and Sitaraman, 2015](https://dl.acm.org/doi/10.1145/2805789.2805800))

Google Bigtable uses Bloom filters to skip disk reads for missing rows. ([Chang et al., 2006](https://research.google/pubs/pub27898/))

Since git 2.27, every `git log -- <path>` query runs against per-commit Bloom filters. Each commit stores a filter of which file paths it touched, so git skips commits that definitely didn't change. ([Singh, 2020](https://devblogs.microsoft.com/devops/updates-to-the-git-commit-graph-feature/))

A Bitcoin wallet querying for its transactions would reveal their addresses, a critical privacy failure. A Bloom filter hides that by sending a filter loose enough to match many addresses, so the full node can't tell which addresses are theirs. ([BIP 37, 2012](https://github.com/bitcoin/bips/blob/master/bip-0037.mediawiki))

---

## Changelog

**2026-09-12** Dropped the HyperLogLog/count-min-sketch aside.  
**2026-07-19** Fixed citation numbering and removed an uncited reference.  
**2026-06-26** Initial draft.  
