---
title: "Hyrum's Law"
date: 2026-07-19
publishdate: 2026-07-19
lastmod: 2026-07-19
summary: "With enough users, every observable behavior becomes a depended-upon feature. Your implementation details become your API contract, regardless of documentation or intent."
tags: ["api", "architecture", "compatibility"]
image: /images/hyrums-law.jpg
draft: false
---

![With enough users, every observable behavior becomes a depended-upon feature. Your implementation details become your API contract, regardless of documentation or intent.](/images/hyrums-law.jpg)
*"Desire path, Belém, Lisbon." Photo: [Metro Centric (2015)](https://commons.wikimedia.org/wiki/File:Desire_path_(19811581366).jpg). CC BY 2.0.*

## Hyrum's Law

Your API returns results in alphabetical order. The documentation says nothing about ordering. It's an implementation detail, not a promise.

Six months later, you optimize the query, and results start coming back in insertion order instead. Production breaks. Dozens of clients had quietly built pagination and diffing logic on alphabetical sorting. They never told you. They just assumed it was guaranteed.

Every observable behavior becomes a contract, whether or not you documented it or intended it that way. If users can observe something, someone will depend on it.

## What Is Hyrum's Law?

Hyrum's Law states, "With a sufficient number of users of an API, it does not matter what you promise in the contract: all observable behaviors of your system will be depended on by somebody"{{< cite 1 "Wright, Hyrum (2020). Hyrum's Law. Software Engineering at Google." >}}.

Hyrum Wright formulated this while running large-scale API deprecation at Google, migrating thousands of callers off old internal APIs. Changing any of them meant understanding not just the documented contract but every behavior a caller might have come to depend on, since a contract promising nothing about some behavior never stopped someone from relying on it anyway{{< cite 2 "Winters, Titus, Tom Manshreck, and Hyrum Wright (2020). Software Engineering at Google: Lessons Learned from Programming Over Time. O'Reilly Media." >}}.

The principle applies beyond APIs. It governs any interface with multiple users, from command-line tools and file formats to network protocols and database schemas. With ten users, you might coordinate changes. With ten thousand, coordination becomes impossible.

## Why Implementation Details Become Contracts

Users optimize for their immediate needs, observing behavior and depending on it before moving on. The dependency becomes implicit knowledge, undocumented and forgotten{{< cite 3 "Parnas, David L. (1972). On the Criteria To Be Used in Decomposing Systems into Modules. Communications of the ACM, 15(12)." >}}.

**Timing dependencies emerge.** Your API responds in 50 milliseconds, and a client sets its timeout to 100 milliseconds, a comfortable margin based on observed behavior, since there's no documented SLA to go by. You add a validation step, and response time creeps up to 150 milliseconds. The client's timeout starts firing, and requests that used to succeed now fail. The timing was never specified, but it was observable, and someone built on it anyway.

**Error messages become APIs.** Your function returns "Error: invalid input" for malformed data, and a client starts parsing that string to detect the error type. You improve the message to "Error: field 'email' must be valid email address," and the client breaks. It was never part of the contract. It was just visible.

**Ordering becomes guaranteed.** Your database query happens to return results in primary key order, even though the documentation says order is undefined. Clients assume the current order and build pagination around it. Then you add an index, the query planner shifts, and results come back differently. Pagination breaks across the entire system.

**Side effects become features.** Your cache implementation writes to disk, though the documentation says nothing about persistence. Clients restart and expect the cached data to still be there. You switch to an in-memory cache and clients break, because the persistence was never promised, just observed{{< cite 4 "Lehman, Meir M. (1980). Programs, Life Cycles, and Laws of Software Evolution. Proceedings of the IEEE, 68(9)." >}}.

## The Scale Problem

Small systems can coordinate changes. Large systems cannot.

At ten users, you know them all, and you can coordinate breaking changes directly. At one thousand, you know few of them, hidden dependencies multiply, and breaking changes require migration tools and long deprecation windows. At a hundred thousand, you know almost none of them, and hidden dependencies dominate. The observable behavior is the contract{{< cite 5 "Booch, Grady (1994). Object-Oriented Analysis and Design with Applications, Second Edition. Benjamin/Cummings." >}}.

Google's experience shows this at extreme scale. Changing a widely-used internal API requires automated tooling just to find and update every call site, and even then some dependencies hide in generated code, reflection, or dynamic dispatch. The cost of a change grows faster than the number of users who make it{{< cite 2 "Winters, Titus, Tom Manshreck, and Hyrum Wright (2020). Software Engineering at Google: Lessons Learned from Programming Over Time. O'Reilly Media." >}}.

## Real-World Examples

Python's dictionary iteration order illustrates the principle. Before Python 3.7, dictionaries had no defined iteration order, but the CPython implementation happened to preserve insertion order, and code started depending on it. When PyPy used a different ordering, that code broke. Python 3.7 made insertion order official{{< cite 6 "Van Rossum, Guido (2017). PEP 520: Preserving Class Attribute Definition Order. Python Enhancement Proposals." >}}.

Linux system calls show how a contract can become permanent. The kernel maintains binary compatibility with userspace, and every syscall behavior, bugs included, becomes part of the permanent API. Linus Torvalds put it plainly: "we do not break userspace"{{< cite 7 "Torvalds, Linus (2012). We do not break userspace. Linux Kernel Mailing List." >}}.

AWS S3's eventual consistency created an implicit contract of its own. Applications built retry logic around the delay between a write and a read reflecting it. When S3 added strong consistency, some of those applications broke, because they'd built timing assumptions into code, and only the consistency model was ever documented, never the specific timing{{< cite 8 "Brooker, Marc (2020). Amazon S3 Update: Strong Read-After-Write Consistency. AWS News Blog." >}}.

Browser user-agent strings show how a workaround outlives its reason. Early sites only served frames to browsers claiming "Mozilla," so every browser claimed it too. WebKit claimed to be KHTML, KHTML claimed Gecko, and Chrome ships a string naming four engines it isn't running{{< cite 9 "Andersen, Aaron (2008). History of the Browser User-Agent String. WebAIM." >}}. Nobody can drop the claims without breaking sites sniffing for them.

## Common Mistakes

**Assuming documentation defines the contract.** Documentation describes intent, but observable behavior defines reality. Users depend on what they observe, and when that disagrees with documentation, behavior wins.

**Believing "undefined" means "free to change."** Undefined behavior is still observable, and if it's observable, someone depends on it. Marking something undefined doesn't prevent dependencies. It just makes them harder to find.

**Thinking you can deprecate implementation details.** You can't deprecate what users never knew they were using. They didn't read a note saying "this is an implementation detail." They just observed behavior and built on it, so the deprecation notice is invisible to the people who need to see it.

**Expecting users to report dependencies.** Users don't know they depend on implementation details until the behavior changes and their code breaks. By then, it's too late to ask.

**Treating internal and external APIs differently.** Internal APIs with many users face the same constraints as external ones. User count sets the boundary. Org chart position is irrelevant. A thousand internal users create the same dependency problems as a thousand external ones{{< cite 2 "Winters, Titus, Tom Manshreck, and Hyrum Wright (2020). Software Engineering at Google: Lessons Learned from Programming Over Time. O'Reilly Media." >}}.

## Put It Into Practice

Audit your APIs for observable behavior beyond the documented contract. Response timing, error message formats, result ordering, cleanup timing, and side effects all create implicit contracts whether you meant them to or not. Document them, or eliminate them.

Build tooling to find dependencies before your users do. Static analysis catches some dependencies, runtime monitoring catches others, and Google's Kythe project indexes code relationships at a scale that makes large refactors possible{{< cite 2 "Winters, Titus, Tom Manshreck, and Hyrum Wright (2020). Software Engineering at Google: Lessons Learned from Programming Over Time. O'Reilly Media." >}}. Smaller teams can get most of the way there with grep and a call graph.

When you change behavior, assume someone depends on it. Add the new behavior alongside the old, deprecate gradually, and watch for breakage. The cost of coordinating a change grows with the number of people who'd notice it. Plan for that cost before you need it.

---

## References

<ol class="references">
  <li id="ref-1">Wright, Hyrum (2020). "Hyrum's Law." Software Engineering at Google. <a href="https://www.hyrumslaw.com/">https://www.hyrumslaw.com/</a></li>
  <li id="ref-2">Winters, Titus, Tom Manshreck, and Hyrum Wright (2020). <em>Software Engineering at Google: Lessons Learned from Programming Over Time</em>. O'Reilly Media. <a href="https://abseil.io/resources/swe-book">https://abseil.io/resources/swe-book</a></li>
  <li id="ref-3">Parnas, David L. (1972). "On the Criteria To Be Used in Decomposing Systems into Modules." <em>Communications of the ACM</em>, 15(12): 1053-1058. <a href="https://dl.acm.org/doi/10.1145/361598.361623">https://dl.acm.org/doi/10.1145/361598.361623</a></li>
  <li id="ref-4">Lehman, Meir M. (1980). "Programs, Life Cycles, and Laws of Software Evolution." <em>Proceedings of the IEEE</em>, 68(9): 1060-1076. <a href="https://ieeexplore.ieee.org/document/1456074">https://ieeexplore.ieee.org/document/1456074</a></li>
  <li id="ref-5">Booch, Grady (1994). <em>Object-Oriented Analysis and Design with Applications, Second Edition</em>. Benjamin/Cummings. <a href="https://www.informit.com/store/object-oriented-analysis-and-design-with-applications-9780805353402">https://www.informit.com/store/object-oriented-analysis-and-design-with-applications-9780805353402</a></li>
  <li id="ref-6">Van Rossum, Guido (2017). "PEP 520: Preserving Class Attribute Definition Order." Python Enhancement Proposals. <a href="https://www.python.org/dev/peps/pep-0520/">https://www.python.org/dev/peps/pep-0520/</a></li>
  <li id="ref-7">Torvalds, Linus (2012). "We do not break userspace." Linux Kernel Mailing List. <a href="https://lkml.org/lkml/2012/12/23/75">https://lkml.org/lkml/2012/12/23/75</a></li>
  <li id="ref-8">Brooker, Marc (2020). "Amazon S3 Update: Strong Read-After-Write Consistency." AWS News Blog. <a href="https://aws.amazon.com/blogs/aws/amazon-s3-update-strong-read-after-write-consistency/">https://aws.amazon.com/blogs/aws/amazon-s3-update-strong-read-after-write-consistency/</a></li>
  <li id="ref-9">Andersen, Aaron (2008). "History of the Browser User-Agent String." WebAIM. <a href="https://webaim.org/blog/user-agent-string-history/">https://webaim.org/blog/user-agent-string-history/</a></li>
</ol>

---

## Outtakes

**The permanent leap day.** Excel treats 1900 as a leap year, a bug copied from Lotus 1-2-3 for spreadsheet compatibility. February 29, 1900, never existed, but millions of spreadsheets now depend on Excel counting it, and Microsoft won't remove a day that isn't real ([Microsoft, 2024](https://learn.microsoft.com/en-us/troubleshoot/microsoft-365-apps/excel/wrongly-assumes-1900-is-leap-year)).

**The filename Windows still can't allow.** CON, PRN, AUX, NUL, and COM1 through LPT9 are reserved device names left over from MS-DOS. In 2023, cloning the opnsense/tools repository on Windows failed outright because it contained a file named aux.conf ([GitHub, 2023](https://github.com/desktop/desktop/issues/17149)).

**Go named the law in its own source code.** MaxBytesReader's error string, "http: request body too large," had no structured type, so callers matched it by text. Go added a proper MaxBytesError type in 2022, but the string itself couldn't change. Too much code depended on it ([Go, n.d.](https://go.dev/src/net/http/request.go#L1199)).

**A security fix broke Steam, Discord, and MATLAB in one release.** glibc 2.41 stopped automatically making a program's stack executable, closing a real security gap. Nobody had promised that behavior would last, but enough software depended on it that glibc shipped an emergency compatibility flag weeks later ([Phoronix, 2025](https://www.phoronix.com/news/Glibc-WA-Steam-Exec-Stack)).

**The button that slowed your PC down.** Early PC games timed themselves off raw CPU cycles, so faster processors made them unplayably fast. The fix wasn't in software. 1980s and 1990s PCs shipped a "turbo" button that throttled the CPU back down to the original speed ([Wikipedia, n.d.](https://en.wikipedia.org/wiki/Turbo_button)).

---

## Changelog

**2026-07-19** Initial publication.
