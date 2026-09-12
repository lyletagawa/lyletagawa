---
title: "Hyrum's Law"
date: 2026-07-19
publishdate: 2026-07-19
lastmod: 2026-08-08
summary: "Hyrum's Law says that with enough users, every observable behavior becomes a depended-upon feature. Your implementation details become your API contract, regardless of documentation or intent."
tags: ["api", "architecture", "compatibility"]
image: /images/hyrums-law.jpg
draft: true
---

![Hyrum's Law says that with enough users, every observable behavior becomes a depended-upon feature. Your implementation details become your API contract, regardless of documentation or intent.](/images/hyrums-law.jpg)
*"Desire path, Belém, Lisbon." Photo: [Metro Centric (2015)](https://commons.wikimedia.org/wiki/File:Desire_path_(19811581366).jpg). CC BY 2.0.*

## Hyrum's Law

Your API happens to return results in alphabetical order, not intentionally. The documentation says nothing about ordering, it's just an implementation detail.

Six months later, you optimize the query, and return the results in insertion order instead. Production breaks. Dozens of clients built pagination and diffing logic based on alphabetical sorting. They just assumed it was guaranteed.

Every observable behavior becomes a contract, whether or not you documented it or intended it that way. If users can observe a pattern, somebody will build a dependency on it.

## What Is Hyrum's Law?

Hyrum's Law states, "With a sufficient number of users of an API, it does not matter what you promise in the contract: all observable behaviors of your system will be depended on by somebody"{{< cite 1 "Wright, Hyrum (2020). Hyrum's Law. Software Engineering at Google." >}}.

Hyrum Wright ran API deprecation at Google, migrating thousands of callers off old internal APIs. Changing those callers meant tracing the documented contract and every behavior they'd come to rely on{{< cite 2 "Winters, Titus, Tom Manshreck, and Hyrum Wright (2020). Software Engineering at Google: Lessons Learned from Programming Over Time. O'Reilly Media." >}}.

The principle also applies beyond APIs, to command-line tools, file formats, network protocols and database schemas.

## Why Implementation Details Become Contracts

Users optimize for their immediate needs.

A malformed request gets back "Error: invalid input" from your function, and a client starts parsing that string to detect the error. You improve the message to "Error: field 'email' must be valid email address," and the client breaks.

Your database query happens to return results in primary key order, even though the documentation says order is undefined. Clients assume the current order and build pagination around it. Add an index, and the query planner shifts. Results come back differently, and pagination breaks across the entire system.

## The Scale Problem

Small systems can coordinate changes. Large systems cannot.

At ten users, you know them all, and you can coordinate breaking changes directly. Once you reach a thousand, you know only a few of them, hidden dependencies multiply, and breaking changes require migration tools and long deprecation windows. By a hundred thousand users, you know almost none of them. The observable behavior is the contract{{< cite 5 "Booch, Grady (1994). Object-Oriented Analysis and Design with Applications, Second Edition. Benjamin/Cummings." >}}.

Google's experience shows this at extreme scale. Changing a widely-used internal API requires automated tooling to find and update every call site, and even then some dependencies hide in generated code, reflection, or dynamic dispatch{{< cite 2 "Winters, Titus, Tom Manshreck, and Hyrum Wright (2020). Software Engineering at Google: Lessons Learned from Programming Over Time. O'Reilly Media." >}}.

## Real-World Examples

Before Python 3.7, dictionaries had no defined iteration order, but the CPython implementation happened to preserve insertion order, and code started depending on it. When PyPy used a different ordering, that code broke. Python 3.7 made insertion order official{{< cite 6 "Python Software Foundation (2018). What's New In Python 3.7. Python Documentation." >}}.

The Linux kernel maintains binary compatibility with userspace, and every syscall behavior, bugs included, becomes part of the permanent API. Linus Torvalds put it plainly, "We do not break userspace"{{< cite 7 "Torvalds, Linus (2012). We do not break userspace. Linux Kernel Mailing List." >}}.

Early websites checked the user agent string for "Mozilla" before serving the best (richest) version of the page, so every new browser claimed to be Mozilla. WebKit followed by calling itself KHTML, which in turn identified as Gecko, and Chrome now ships a string naming four engines other than the one it actually runs{{< cite 9 "Andersen, Aaron (2008). History of the Browser User-Agent String. WebAIM." >}}.

## Common Mistakes

**Assuming documentation defines the contract.** Documentation describes intent, but observable behavior defines reality. Users depend on what they observe, and when that disagrees with documentation, behavior wins.

**Believing "undefined" means "free to change."** Undefined behavior is still observable, and whatever can be observed will eventually be depended on. Marking something undefined still allows dependencies to form. It just makes them harder to find.

**Expecting users to report dependencies.** Users discover they depend on implementation details only when the behavior changes and their code breaks. By then, it's too late to ask.

**Treating internal and external APIs differently.** Internal APIs with many users face the same constraints as external ones. User count sets the boundary, rather than org chart position. A thousand internal users create the same dependency problems as a thousand external ones{{< cite 2 "Winters, Titus, Tom Manshreck, and Hyrum Wright (2020). Software Engineering at Google: Lessons Learned from Programming Over Time. O'Reilly Media." >}}.

## Put It Into Practice

Audit your APIs for observable behavior beyond the documented contract. Response timing, error message formats, result ordering, cleanup timing, and side effects all create implicit contracts whether you meant them to or not. Document them, or eliminate them.

Spotting dependencies before your users do means investing in tooling built for exactly that. Static analysis catches some dependencies, runtime monitoring surfaces others, and Google's Kythe project indexes code relationships at a scale that makes large refactors possible{{< cite 2 "Winters, Titus, Tom Manshreck, and Hyrum Wright (2020). Software Engineering at Google: Lessons Learned from Programming Over Time. O'Reilly Media." >}}. Smaller teams can get most of the way there with grep and a call graph.

When you change behavior, assume someone depends on it. Add the new behavior alongside the old, deprecate gradually, and watch for breakage. The cost of coordinating a change grows with the number of people who'd notice it. Plan for that cost before you need it.

---

## References

<ol class="references">
  <li id="ref-1">Wright, Hyrum (2020). "Hyrum's Law." Software Engineering at Google. <a href="https://www.hyrumslaw.com/">https://www.hyrumslaw.com/</a></li>
  <li id="ref-2">Winters, Titus, Tom Manshreck, and Hyrum Wright (2020). <em>Software Engineering at Google: Lessons Learned from Programming Over Time</em>. O'Reilly Media. <a href="https://abseil.io/resources/swe-book">https://abseil.io/resources/swe-book</a></li>
  <li id="ref-3">Parnas, David L. (1972). "On the Criteria To Be Used in Decomposing Systems into Modules." <em>Communications of the ACM</em>, 15(12): 1053-1058. <a href="https://dl.acm.org/doi/10.1145/361598.361623">https://dl.acm.org/doi/10.1145/361598.361623</a></li>
  <li id="ref-4">Lehman, Meir M. (1980). "Programs, Life Cycles, and Laws of Software Evolution." <em>Proceedings of the IEEE</em>, 68(9): 1060-1076. <a href="https://ieeexplore.ieee.org/document/1456074">https://ieeexplore.ieee.org/document/1456074</a></li>
  <li id="ref-5">Booch, Grady (1994). <em>Object-Oriented Analysis and Design with Applications, Second Edition</em>. Benjamin/Cummings. <a href="https://www.informit.com/store/object-oriented-analysis-and-design-with-applications-9780805353402">https://www.informit.com/store/object-oriented-analysis-and-design-with-applications-9780805353402</a></li>
  <li id="ref-6">Python Software Foundation (2018). "What's New In Python 3.7." <em>Python Documentation</em>. <a href="https://docs.python.org/3/whatsnew/3.7.html">https://docs.python.org/3/whatsnew/3.7.html</a></li>
  <li id="ref-7">Torvalds, Linus (2012). "We do not break userspace." Linux Kernel Mailing List. <a href="https://lkml.org/lkml/2012/12/23/75">https://lkml.org/lkml/2012/12/23/75</a></li>
  <li id="ref-8">Brooker, Marc (2020). "Amazon S3 Update: Strong Read-After-Write Consistency." AWS News Blog. <a href="https://aws.amazon.com/blogs/aws/amazon-s3-update-strong-read-after-write-consistency/">https://aws.amazon.com/blogs/aws/amazon-s3-update-strong-read-after-write-consistency/</a></li>
  <li id="ref-9">Andersen, Aaron (2008). "History of the Browser User-Agent String." WebAIM. <a href="https://webaim.org/blog/user-agent-string-history/">https://webaim.org/blog/user-agent-string-history/</a></li>
</ol>

---

## Outtakes

**The permanent leap day.** Excel treats 1900 as a leap year, a bug copied from Lotus 1-2-3 for spreadsheet compatibility. February 29, 1900, never existed, but millions of spreadsheets now depend on Excel counting it, and Microsoft keeps a day that was invented by mistake ([Microsoft, 2024](https://learn.microsoft.com/en-us/troubleshoot/microsoft-365-apps/excel/wrongly-assumes-1900-is-leap-year)).

**The filename Windows still forbids.** CON, PRN, AUX, NUL, and COM1 through LPT9 are reserved device names left over from MS-DOS. In 2023, cloning the opnsense/tools repository on Windows failed outright because it contained a file named aux.conf ([GitHub, 2023](https://github.com/desktop/desktop/issues/17149)).

**Go named the law in its own source code.** MaxBytesReader's error string, "http: request body too large," had no structured type, so callers matched it by text. Go added a proper MaxBytesError type in 2022, but the string itself stayed frozen. Too much code depended on it ([Go, n.d.](https://go.dev/src/net/http/request.go#L1199)).

**A security fix broke Steam, Discord, and MATLAB in one release.** glibc 2.41 stopped automatically making a program's stack executable, closing a real security gap. Nobody had promised that behavior would last, but enough software relied on it that glibc shipped an emergency compatibility flag weeks later ([Phoronix, 2025](https://www.phoronix.com/news/Glibc-WA-Steam-Exec-Stack)).

**The button that slowed your PC down.** Early PC games timed themselves off raw CPU cycles, so faster processors made them unplayably fast. The fix wasn't in software. 1980s and 1990s PCs shipped a "turbo" button that throttled the CPU back down to the original speed ([Wikipedia, n.d.](https://en.wikipedia.org/wiki/Turbo_button)).

---

## Changelog

**2026-08-08** Corrected the Python dict-ordering citation, which incorrectly pointed to PEP 520.  
**2026-07-19** Initial publication.
