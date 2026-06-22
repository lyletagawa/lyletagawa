---
title: "Trie"
date: 2026-06-21
publishdate: 2026-06-21
lastmod: 2026-06-21
summary: "A trie stores strings by their characters, one per node, so every lookup follows the string itself. Prefix search comes free with the structure."
tags: ["algorithms", "strings", "performance"]
image: /images/trie.png
draft: true
---

![A trie stores strings by their characters, one per node, so every lookup follows the string itself. Prefix search comes free with the structure.](/images/trie.png)
*Image: placeholder.*

## Trie

Every packet you send travels through a chain of routers. Each router has hundreds of thousands of route entries and matches the destination address against all of them on every hop, fast enough to keep up with millions of packets per second. The match isn't exact. Routers want the most specific prefix.

That same operation fills in your search box before you finish typing. The data structure is called a trie.

## What Is a Trie?

Edward Fredkin coined the term in 1960, taking it from the middle of "retrieval"{{< cite 1 "Fredkin, Edward (1960). Trie Memory. Communications of the ACM, 3(9): 490-499." >}}. He pronounced it "tree," which has caused confusion ever since. Most people now say "try" to distinguish it from a generic tree. René de la Briandais described the structure a year earlier, without a name{{< cite 2 "de la Briandais, René (1959). File searching using variable length keys. Proceedings of the Western Joint Computer Conference." >}}.

A trie is a tree where the path to a node spells out a key, one character per edge. The root is empty. Store "cat," "car," and "card" and you get one shared "c-a" path with two branches below. One ends at "t" for "cat." The other leads to "r" for "car," continuing to "d" for "card."

The structure can't hold a prefix without making it available to every word that shares it. Add a thousand words starting with "ca" and the "c-a" path still exists exactly once. Prefix sharing isn't incidental. It falls out of how the data is arranged.

## How Lookup Works

Lookup walks the trie one character at a time. At each node, it follows the edge for the next character. A missing edge means the string isn't there. A marked endpoint after the final character means it is.

Lookup takes O(m) time, where m is the length of the search string{{< cite 1 "Fredkin, Edward (1960). Trie Memory. Communications of the ACM, 3(9): 490-499." >}}. The number of strings stored doesn't matter. A trie with ten words and a trie with ten million words both look up "cat" in three steps.

A hash table also takes O(m) time to hash a string before comparing, but a trie handles prefix queries in the same O(m) time. To find every string starting with "ca," walk to the "ca" node and traverse everything below. A hash table has no equivalent operation.

## Where Tries Win

**Autocomplete.** Every keystroke narrows the search to a subtree. Walk to the prefix node, collect the words below it. Done. Google's search suggestions, code editor autocompletion, and phone keyboard prediction all work this way.

**IP routing.** Internet routers need the most specific matching prefix for a destination address, not an exact match. A packet to 192.168.1.50 might match both a /16 and a /24 route. The router wants the longer one. A trie of binary address bits walks the address bit by bit and returns the deepest match found{{< cite 3 "Ruiz-Sanchez, M.A., Biersack, E.W., and Dabbous, W. (2001). Survey and taxonomy of IP address lookup algorithms. IEEE Network, 15(2): 8-23." >}}. A hash table can't express "longest prefix" at all.

**Sorted output.** A depth-first traversal visits strings in alphabetical order. No extra sorting step. If you already have the trie, alphabetical order falls out.

**Spell checking.** Trie traversal lets you enumerate words within edit distance of a query. The trie bounds the search without scanning every word in the dictionary.

## Where This Breaks Down

**Memory.** Each node needs a way to reference its children. A naive implementation allocates an array of 26 pointers at every node, one per letter, most of them null. For even a modest dictionary, the empty pointers dwarf the actual data. Compact implementations use hash maps per node or encode children as linked lists, but the overhead is real.

**Large alphabets.** The 26-letter English alphabet is manageable. Unicode has over 140,000 code points. A trie over arbitrary Unicode strings either wastes enormous space on sparse arrays or pays a per-node lookup cost to find the right child.

**Random strings.** Tries pay off when strings share prefixes. UUIDs, hashes, and randomly generated identifiers share almost nothing. A trie of random keys degenerates into a collection of long paths with one node each. A hash table fits better.

**No standard implementation.** Java's standard library doesn't include a trie. Neither does Python's. You build it yourself or pull in a dependency, which adds maintenance, tests, and a learning curve for whoever reads the code next.

## When to Reach for a Trie

Prefix search is the signal. If queries start with "find everything beginning with X," a trie has a structural advantage no general-purpose data structure can match.

Reach for one when your keys are strings, many share prefixes, and you need fast prefix queries or autocomplete. Routing tables, search suggestions, and dictionary lookups with spell correction fit. Session tokens and UUIDs don't.

When memory is tight and your alphabet is small, consider a radix tree{{< cite 4 "Morrison, Donald R. (1968). PATRICIA -- Practical Algorithm To Retrieve Information Coded in Alphanumeric. Journal of the ACM, 15(4): 514-534." >}}, a compressed trie also known as a Patricia trie in networking and academic literature. It collapses single-child chains into single labeled edges, cutting node count dramatically without losing the prefix advantage.

## What This Doesn't Cover

**Compressed tries.** A radix tree collapses paths with only one child into a single labeled edge. Morrison's 1968 paper{{< cite 4 "Morrison, Donald R. (1968). PATRICIA -- Practical Algorithm To Retrieve Information Coded in Alphanumeric. Journal of the ACM, 15(4): 514-534." >}} introduced the structure under the name Patricia trie. It appears in every serious routing implementation and many database indexes.

**Suffix trees.** A suffix tree stores every suffix of a string, enabling substring search in O(m) time. It underlies much of computational biology, from genome assembly to sequence alignment{{< cite 5 "Gusfield, Dan (1997). Algorithms on Strings, Trees, and Sequences. Cambridge University Press." >}}.

**DAWGs.** A directed acyclic word graph (DAWG) merges shared suffixes as well as shared prefixes, producing the most compact word list possible. Spell checkers and word games use them when memory is the binding constraint{{< cite 6 "Daciuk, Jan, Stoyan Mihov, Bruce Watson, and Richard Watson (2000). Incremental Construction of Minimal Acyclic Finite-State Automata. Computational Linguistics, 26(1): 3-16." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Fredkin, Edward (1960). "Trie Memory." <em>Communications of the ACM</em>, 3(9): 490-499. <a href="https://doi.org/10.1145/367390.367400">https://doi.org/10.1145/367390.367400</a></li>
  <li id="ref-2">de la Briandais, René (1959). "File searching using variable length keys." <em>Proceedings of the Western Joint Computer Conference</em>. <a href="https://doi.org/10.1145/1457838.1457895">https://doi.org/10.1145/1457838.1457895</a></li>
  <li id="ref-3">Ruiz-Sanchez, M.A., Biersack, E.W., and Dabbous, W. (2001). "Survey and taxonomy of IP address lookup algorithms." <em>IEEE Network</em>, 15(2): 8-23. <a href="https://ieeexplore.ieee.org/document/912648">https://ieeexplore.ieee.org/document/912648</a></li>
  <li id="ref-4">Morrison, Donald R. (1968). "PATRICIA -- Practical Algorithm To Retrieve Information Coded in Alphanumeric." <em>Journal of the ACM</em>, 15(4): 514-534. <a href="https://doi.org/10.1145/321479.321481">https://doi.org/10.1145/321479.321481</a></li>
  <li id="ref-5">Gusfield, Dan (1997). <em>Algorithms on Strings, Trees, and Sequences</em>. Cambridge University Press. <a href="https://www.cambridge.org/9780521585194">https://www.cambridge.org/9780521585194</a></li>
  <li id="ref-6">Daciuk, Jan, Stoyan Mihov, Bruce Watson, and Richard Watson (2000). "Incremental Construction of Minimal Acyclic Finite-State Automata." <em>Computational Linguistics</em>, 26(1): 3-16. <a href="https://aclanthology.org/J00-1002">https://aclanthology.org/J00-1002</a></li>
</ol>

---

## Outtakes

**The pronunciation wars.** Fredkin spelled it "trie," pulling the letters from "retrieval," then pronounced it "tree." Most practitioners say "try" to distinguish it from a tree. Knuth endorsed "try." Papers today still use both. The field never agreed.

**T9.** Pre-smartphone phones used T9 predictive text. Pressing 4-6-6-3 could mean "good," "home," or "gone." T9 walked candidate paths through a trie in parallel and ranked by frequency. Billions of people used a trie every day without knowing it.

**Fredkin's original use case.** The motivating example in Fredkin's 1960 paper was telephone directory lookup, retrieving subscriber information by spelling a name. The problem is indistinguishable from autocomplete. He just didn't have a touchscreen. (Fredkin, 1960)

---

## Changelog

**2026-06-21** Initial draft.
