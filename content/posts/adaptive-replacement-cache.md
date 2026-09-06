---
title: "Adaptive Replacement Cache"
date: 2026-08-31
publishdate: 2026-08-31
lastmod: 2026-08-31
summary: "Adaptive replacement cache doubled LRU's hit rate in published benchmarks while adding under 1 percent memory overhead, no tuning required. Here's the maths behind its self-adjusting parameter, and what it saves in bytes and misses."
tags: ["caching", "algorithms", "performance"]
draft: true
---

## Adaptive replacement cache

A nightly backup job starts scanning every table in the database, once, in order. Each page it touches gets marked most-recently-used. By the time the scan finishes, the cache is full of pages nobody will ask for again, and the pages your live traffic needs are gone.

That's the failure mode a plain least-recently-used (LRU) cache can't defend against on its own. Recency alone can't tell a page your users hit every minute from a page a batch job happened to touch once.

## What is ARC?

Nimrod Megiddo and Dharmendra Modha, researchers at IBM Almaden, built the adaptive replacement cache (ARC) to fix this, without asking anyone to tune a parameter{{< cite 1 "Megiddo, Nimrod, and Dharmendra S. Modha (2004). Outperforming LRU with an Adaptive Replacement Cache Algorithm. IEEE Computer, 37(4): 58-65." >}}.

ARC keeps two real lists instead of one. T1 holds pages requested once, recently, the recency signal LRU already tracks. T2 holds pages requested at least twice, recently, the frequency signal LRU throws away. Together the two lists hold no more than the cache's full capacity, c pages{{< cite 1 "Megiddo, Nimrod, and Dharmendra S. Modha (2004). Outperforming LRU with an Adaptive Replacement Cache Algorithm. IEEE Computer, 37(4): 58-65." >}}.

Behind those two visible lists sit two more that hold no data at all. B1 and B2 are ghost lists. They remember which pages got evicted from T1 and T2 by identifier only, no page data kept{{< cite 1 "Megiddo, Nimrod, and Dharmendra S. Modha (2004). Outperforming LRU with an Adaptive Replacement Cache Algorithm. IEEE Computer, 37(4): 58-65." >}}. A ghost entry costs a few bytes of bookkeeping instead of a full page. When a page reappears in B1 or B2, that's a signal the eviction was wrong, and it's the signal the algorithm learns from.

## The adaptation has no dial

ARC's self-tuning behavior comes down to one number, p, the target size for T1. Every request can move it{{< cite 1 "Megiddo, Nimrod, and Dharmendra S. Modha (2004). Outperforming LRU with an Adaptive Replacement Cache Algorithm. IEEE Computer, 37(4): 58-65." >}}.

A hit in B1 means a page ARC just evicted from the recency list got requested again, so T1 should have been bigger. p grows by max(|B2| / |B1|, 1), capped at the cache's full size{{< cite 1 "Megiddo, Nimrod, and Dharmendra S. Modha (2004). Outperforming LRU with an Adaptive Replacement Cache Algorithm. IEEE Computer, 37(4): 58-65." >}}. A hit in B2 means the opposite went wrong, a frequency page got evicted too early, so p shrinks by max(|B1| / |B2|, 1), floored at zero{{< cite 1 "Megiddo, Nimrod, and Dharmendra S. Modha (2004). Outperforming LRU with an Adaptive Replacement Cache Algorithm. IEEE Computer, 37(4): 58-65." >}}.

Neither move needs a tuning parameter or an offline pass over the workload first. p walks toward whichever list is currently getting it wrong, and inside a single workload it can walk all the way to either extreme, favoring pure recency one moment and pure frequency the next{{< cite 1 "Megiddo, Nimrod, and Dharmendra S. Modha (2004). Outperforming LRU with an Adaptive Replacement Cache Algorithm. IEEE Computer, 37(4): 58-65." >}}.

## Count the bytes saved

ARC's own bookkeeping is cheap. A real implementation measured the four-list overhead at 0.75 percent of the cache's size{{< cite 1 "Megiddo, Nimrod, and Dharmendra S. Modha (2004). Outperforming LRU with an Adaptive Replacement Cache Algorithm. IEEE Computer, 37(4): 58-65." >}}. What that overhead buys back is the number worth tracking.

On one of the paper's disk-access traces, ARC hit 16.48 percent of requests using an 8 MB cache. LRU needed 16 MB, double the memory, to reach a comparable 17.18 percent{{< cite 1 "Megiddo, Nimrod, and Dharmendra S. Modha (2004). Outperforming LRU with an Adaptive Replacement Cache Algorithm. IEEE Computer, 37(4): 58-65." >}}. Same hit rate, half the cache.

On a synthetic storage benchmark with a fixed 4 GB cache, the gap shows up differently. LRU hit 9.19 percent of requests. ARC hit 20.00 percent, more than double{{< cite 1 "Megiddo, Nimrod, and Dharmendra S. Modha (2004). Outperforming LRU with an Adaptive Replacement Cache Algorithm. IEEE Computer, 37(4): 58-65." >}}. That doesn't mean the savings doubled too. Miss rate went from 90.81 percent to 80 percent, an 11.9 percent relative drop in the requests that fell through to disk. Doubling the hit rate is real. Doubling the savings from it isn't guaranteed, since most requests were still misses to begin with.

## Common mistakes

**You store real pages in the ghost lists.** B1 and B2 are supposed to remember which pages got evicted by identifier alone. Keep the actual contents around just in case, and the cache's real footprint is double what anyone budgeted for. The low-overhead property that makes ARC worth using is gone.

**You ship it without checking who owns it.** IBM patented ARC in 2006{{< cite 2 "Megiddo, Nimrod, and Dharmendra S. Modha. System and Method for Implementing an Adaptive Replacement Cache Policy. U.S. Patent 6,996,676 B2, granted February 7, 2006." >}}. PostgreSQL put it into version 8.0.0's buffer manager, then pulled it within weeks once the patent risk was clear, replacing it with the patent-free 2Q algorithm instead{{< cite 3 "LWN.net (2005). PostgreSQL 8.0.2 Released With Patent Fix." >}}. Public and well-documented isn't the same as free to ship.

**You pin p to stop it from moving.** Locking the target size at a fixed split between T1 and T2 turns ARC back into the fixed policy it was built to replace. The adaptation is the entire point. A p that can't move can't respond when the access pattern changes underneath it.

## Put it into practice

Before reaching for ARC, check whether your workload mixes recency and frequency signals the way LRU struggles with it, a nightly scan next to hot live traffic, a cold start next to a repeat customer. If every request looks the same, the adaptation has nothing to learn from.

If it does fit, measure two numbers before rolling it out further. The hit-rate gap against your current LRU cache at the same size. The real memory overhead of the ghost lists in your own implementation, measured directly instead of assumed from the paper's 0.75 percent. Then decide whether that gap is worth building a self-tuning cache instead of just buying more of the cheap kind.

Check who owns the implementation before you ship it. PostgreSQL learned that lesson in production. You don't have to.

---

## References

<ol class="references">
  <li id="ref-1">Megiddo, Nimrod, and Dharmendra S. Modha (2004). "Outperforming LRU with an Adaptive Replacement Cache Algorithm." <em>IEEE Computer</em>, 37(4): 58-65. <a href="https://theory.stanford.edu/~megiddo/pdf/IEEE_COMPUTER_0404.pdf">https://theory.stanford.edu/~megiddo/pdf/IEEE_COMPUTER_0404.pdf</a></li>
  <li id="ref-2">Megiddo, Nimrod, and Dharmendra S. Modha. "System and Method for Implementing an Adaptive Replacement Cache Policy." U.S. Patent 6,996,676 B2, granted February 7, 2006. <a href="https://patents.google.com/patent/US6996676B2/en">https://patents.google.com/patent/US6996676B2/en</a></li>
  <li id="ref-3">LWN.net (2005). "PostgreSQL 8.0.2 Released With Patent Fix." <a href="https://lwn.net/Articles/131554/">https://lwn.net/Articles/131554/</a></li>
</ol>

---

## Changelog

**2026-08-31** Initial release.
