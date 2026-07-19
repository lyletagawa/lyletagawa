---
title: "How Redis Counts to 18 Quintillion in 12 KB"
date: 2026-07-18
publishdate: 2026-07-18
lastmod: 2026-07-18
summary: "HyperLogLog estimates how many unique items passed through a stream using a fixed, tiny amount of memory. Redis does it in 12 kilobytes, accurate to within 1 percent, whether you're counting thousands or quintillions."
tags: ["algorithms", "databases", "probability"]
image: /images/how-redis-counts-to-18-quintillion-in-12-kb.jpg
draft: true
---

![HyperLogLog estimates how many unique items passed through a stream using a fixed, tiny amount of memory. Redis does it in 12 kilobytes, accurate to within 1 percent, whether you're counting thousands or quintillions.](/images/how-redis-counts-to-18-quintillion-in-12-kb.jpg)
*A mechanical hand tally counter. Photo: [Alan Isherwood (2006)](https://commons.wikimedia.org/wiki/File:Tally_counter.jpg). CC BY 2.0.*

## How Redis Counts to 18 Quintillion in 12 KB

Google's PowerDrill system answers questions like how many distinct search queries hit google.com in a day. At scale, a meaningful share of those queries used to simply fail. Counting every distinct value takes memory proportional to how many distinct values exist, and at billions of rows that memory bill came due before the query finished{{< cite 1 "Heule, Stefan, Marc Nunkesser, and Alexander Hall (2013). HyperLogLog in Practice: Algorithmic Engineering of a State of the Art Cardinality Estimation Algorithm. Proceedings of the 16th International Conference on Extending Database Technology." >}}. Multiply that one query by every dashboard, every analytics job, every service that needs a rough distinct count, and the naive approach becomes untenable long before anyone reaches Google's scale.

The fix wasn't more memory. It was an algorithm willing to trade a small, known amount of accuracy for a memory footprint that stays fixed no matter how many billions of items pass through it. That algorithm is HyperLogLog.

## What Is HyperLogLog?

Philippe Flajolet and G. Nigel Martin published the core idea in 1985. Hash every item into a long, effectively random string of bits, then count the leading zeros before the first 1. A run of k zeros happens with probability 1/2^k, so longer runs get exponentially rarer{{< cite 2 "Flajolet, Philippe, and G. Nigel Martin (1985). Probabilistic Counting Algorithms for Data Base Applications. Journal of Computer and System Sciences, 31(2), 182-209." >}}.

Hash n distinct items and the longest run you'll see lands around log2(n). A run of 20 bits means n is probably near a million, since that's roughly how many hashes it takes before one gets lucky. Flip that max run R into 2^R, and you have Flajolet and Martin's original algorithm, one counter dominated entirely by whichever item got the luckiest hash. One freak run early in the stream throws off everything after it.

Durand and Flajolet fixed that in 2003 with LogLog. Route each hash into one of many buckets by its first few bits, track the longest run per bucket, then average across them{{< cite 3 "Durand, Marianne, and Philippe Flajolet (2003). Loglog Counting of Large Cardinalities. Algorithms - ESA 2003." >}}. More buckets, less noise from any one unlucky hash.

Flajolet, Fusy, Gandouet, and Meunier's 2007 paper fixed the averaging itself. Each bucket's R implies 2^R, and averaging those directly lets one unlucky bucket dominate. A harmonic mean, pulled toward the smallest values instead of the largest, fixes that. The result, HyperLogLog, holds standard error to about 1.04 divided by the square root of the bucket count, set by bucket count alone, and hits cardinalities past a billion, accurate to about 2 percent, in 1.5 kilobytes{{< cite 4 "Flajolet, Philippe, Éric Fusy, Olivier Gandouet, and Frédéric Meunier (2007). HyperLogLog: The Analysis of a Near-Optimal Cardinality Estimation Algorithm. Proceedings of the 2007 Conference on Analysis of Algorithms." >}}.

## Where This Shows Up Today

Google runs a refined version inside Sawzall, Dremel, and PowerDrill, adding an empirical bias correction for small and medium cardinalities and a sparse encoding that keeps memory low until a stream has enough distinct items to need the full structure{{< cite 1 "Heule, Stefan, Marc Nunkesser, and Alexander Hall (2013). HyperLogLog in Practice: Algorithmic Engineering of a State of the Art Cardinality Estimation Algorithm. Proceedings of the 16th International Conference on Extending Database Technology." >}}.

Redis ships the same idea as three commands. PFADD adds an item, PFCOUNT estimates cardinality, PFMERGE combines multiple HyperLogLogs into one. Each structure tops out at 12 kilobytes no matter how many items it has seen, with a standard error of 0.81 percent, enough to count unique visitors to a page or unique search terms without ever storing an identifier{{< cite 5 "HyperLogLog. Redis Documentation." >}}.

Elasticsearch's cardinality aggregation runs on the HyperLogLog++ algorithm from that same PowerDrill paper, and lets you trade a higher precision_threshold for tighter accuracy on large aggregations, at roughly 8 bytes of memory for every unit of threshold{{< cite 6 "Cardinality Aggregation. Elasticsearch Guide." >}}. ClickHouse ships a comparable function, uniqHLL12, though its own documentation is candid about where it breaks down. Past 100 million rows the estimate degrades, and ClickHouse recommends a different function, uniqCombined, for most production use{{< cite 7 "uniqHLL12. ClickHouse Documentation." >}}.

## Common Mistakes

**Expecting a precise count.** HyperLogLog gives you a statistical estimate with a known error bound. If a dashboard needs to reconcile to the true row count, this is the wrong tool{{< cite 5 "HyperLogLog. Redis Documentation." >}}.

**Merging when you meant to intersect.** PFMERGE gives you the cardinality of the union of two sets. It says nothing about how many items two HyperLogLogs have in common. Two HyperLogLogs that each estimate a million distinct items could share none of them or nearly all of them, and a merge alone can't tell you which. Estimating an intersection needs a different technique entirely.

**Assuming one estimator works at every scale.** The raw HyperLogLog estimator is biased at small and medium cardinalities. Google's production version switches to a different estimator, LinearCounting, below a measured threshold and only trusts the bias-corrected HyperLogLog estimate above it{{< cite 1 "Heule, Stefan, Marc Nunkesser, and Alexander Hall (2013). HyperLogLog in Practice: Algorithmic Engineering of a State of the Art Cardinality Estimation Algorithm. Proceedings of the 16th International Conference on Extending Database Technology." >}}. An implementation that skips this correction overestimates low counts.

**Assuming you can recover the original items.** A HyperLogLog never stores the elements it counted, only register state derived from their hashes. That's by design. Redis's own documentation points to it as a way to count unique visitors without storing personal identifiers, which some jurisdictions restrict outright{{< cite 5 "HyperLogLog. Redis Documentation." >}}.

## Put It Into Practice

HyperLogLog answers how many distinct items showed up. It never tells you which ones. Reach for it when that's the tradeoff you want, and the alternative is a memory bill that scales with your data instead of staying flat. Unique visitors, unique search terms, unique error signatures, anything where you'd otherwise keep a growing set just to answer one number.

Check what error bound you actually need before you pick it. A dashboard that can tolerate 1 percent error is a different decision than a billing system that has to bill for every unit of usage.

## Go Further

**The original conjecture.** Flajolet and Martin's 1985 paper is the root of this whole family of algorithms, worth reading for how far one clever observation about leading zeros can travel{{< cite 2 "Flajolet, Philippe, and G. Nigel Martin (1985). Probabilistic Counting Algorithms for Data Base Applications. Journal of Computer and System Sciences, 31(2), 182-209." >}}.

**Redis's own writeup.** Salvatore Sanfilippo's 2014 post announcing HyperLogLog in Redis explains the implementation choices, including the 6-bit register packing and the caching trick that keeps PFCOUNT fast{{< cite 8 "Sanfilippo, Salvatore (2014). Redis New Data Structure: The HyperLogLog. antirez weblog." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Heule, Stefan, Marc Nunkesser, and Alexander Hall (2013). "HyperLogLog in Practice: Algorithmic Engineering of a State of the Art Cardinality Estimation Algorithm." <em>Proceedings of the 16th International Conference on Extending Database Technology</em>: 683-692. <a href="https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/40671.pdf">https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/40671.pdf</a></li>
  <li id="ref-2">Flajolet, Philippe, and G. Nigel Martin (1985). "Probabilistic Counting Algorithms for Data Base Applications." <em>Journal of Computer and System Sciences</em>, 31(2): 182-209. <a href="https://algo.inria.fr/flajolet/Publications/FlMa85.pdf">https://algo.inria.fr/flajolet/Publications/FlMa85.pdf</a></li>
  <li id="ref-3">Durand, Marianne, and Philippe Flajolet (2003). "Loglog Counting of Large Cardinalities." <em>Algorithms - ESA 2003</em>. <a href="https://algo.inria.fr/flajolet/Publications/DuFl03-LNCS.pdf">https://algo.inria.fr/flajolet/Publications/DuFl03-LNCS.pdf</a></li>
  <li id="ref-4">Flajolet, Philippe, Éric Fusy, Olivier Gandouet, and Frédéric Meunier (2007). "HyperLogLog: The Analysis of a Near-Optimal Cardinality Estimation Algorithm." <em>Proceedings of the 2007 Conference on Analysis of Algorithms</em>. <a href="https://algo.inria.fr/flajolet/Publications/FlFuGaMe07.pdf">https://algo.inria.fr/flajolet/Publications/FlFuGaMe07.pdf</a></li>
  <li id="ref-5">"HyperLogLog." <em>Redis Documentation</em>. <a href="https://redis.io/docs/latest/develop/data-types/probabilistic/hyperloglogs/">https://redis.io/docs/latest/develop/data-types/probabilistic/hyperloglogs/</a></li>
  <li id="ref-6">"Cardinality Aggregation." <em>Elasticsearch Guide</em>. <a href="https://www.elastic.co/docs/reference/aggregations/search-aggregations-metrics-cardinality-aggregation">https://www.elastic.co/docs/reference/aggregations/search-aggregations-metrics-cardinality-aggregation</a></li>
  <li id="ref-7">"uniqHLL12." <em>ClickHouse Documentation</em>. <a href="https://clickhouse.com/docs/sql-reference/aggregate-functions/reference/uniqhll12">https://clickhouse.com/docs/sql-reference/aggregate-functions/reference/uniqhll12</a></li>
  <li id="ref-8">Sanfilippo, Salvatore (2014). "Redis New Data Structure: The HyperLogLog." <em>antirez weblog</em>. <a href="http://antirez.com/news/75">http://antirez.com/news/75</a></li>
</ol>

---

## Outtakes

**The names escalate.** Durand and Flajolet's 2003 paper introduced LogLog, then improved it by discarding the top 30 percent of bucket values before averaging and named that variant super-LogLog. HyperLogLog followed in 2007 ([Durand and Flajolet, 2003](https://algo.inria.fr/flajolet/Publications/DuFl03-LNCS.pdf)).

**One structure, 18 quintillion items.** Redis documents its HyperLogLog as able to estimate cardinalities up to 2^64, about 18.4 quintillion, while still capping out at 12 kilobytes ([Redis Documentation](https://redis.io/docs/latest/develop/data-types/probabilistic/hyperloglogs/)).

**Salvatore Sanfilippo called it "almost illogical."** Redis's creator described HyperLogLog as accomplishing something "almost illogical given how little it asks for in terms of time or space" when he announced its addition to Redis in 2014 ([Sanfilippo, 2014](http://antirez.com/news/75)).

---

## Changelog

**2026-07-18** Initial release.
