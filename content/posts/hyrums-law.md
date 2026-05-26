---
title: "Hyrum's Law"
date: 2026-05-24
publishdate: 2026-05-24
lastmod: 2026-05-24
summary: "With enough users, every observable behavior becomes a depended-upon feature. Your implementation details become your API contract, regardless of documentation or intent."
tags: ["api-design", "software-architecture", "backwards-compatibility"]
image:
draft: true
---

## Hyrum's Law

Your API returns results in alphabetical order. The documentation says nothing about ordering. It is an implementation detail. Six months later, you optimize the query. Results now return in insertion order. Production breaks. Dozens of clients depended on alphabetical sorting. They never told you. They assumed it was guaranteed.

Every observable behavior becomes a contract. Documentation does not matter. Intent does not matter. If users can observe it, someone will depend on it.

## What Is Hyrum's Law?

Hyrum's Law states: "With a sufficient number of users of an API, it does not matter what you promise in the contract: all observable behaviors of your system will be depended on by somebody" (Wright, 2020). Hyrum Wright formulated this while working on Google's internal infrastructure.

The law emerged from managing APIs with thousands of internal clients. Changing any widely-used API requires understanding not just documented contracts but every observable behavior clients might depend on (Winters et al., 2020).

The principle applies beyond APIs. It governs any interface with multiple users: command-line tools, file formats, network protocols, database schemas, UI behaviors. With ten users, you might coordinate changes. With ten thousand, coordination becomes impossible.

## Why Implementation Details Become Contracts

Users optimize for their immediate needs. They observe behavior. They depend on it. They move on. The dependency becomes implicit knowledge, undocumented and forgotten (Parnas, 1972).

**Timing dependencies emerge.** Your API responds in 50 milliseconds. Clients set timeouts to 100 milliseconds. You optimize. Response time drops to 10 milliseconds. A client breaks. They were using the delay as a rate limiter. The timing was never specified. It was observable. Someone depended on it.

**Error messages become APIs.** Your function returns "Error: invalid input" for malformed data. A client parses that string to determine error type. You improve the message to "Error: field 'email' must be valid email address". The client breaks. They were doing string matching on "invalid input". The error message was never part of the contract. It was observable.

**Ordering becomes guaranteed.** Your database query returns results in primary key order. The documentation says "order is undefined". Clients assume the current order. They build pagination logic around it. You add an index. Query planner changes. Results return in different order. Pagination breaks across the entire system.

**Side effects become features.** Your cache implementation writes to disk. The documentation says nothing about persistence. Clients restart and expect cached data to survive. You switch to in-memory cache. Clients break. The persistence was never promised. It was observable (Lehman, 1980).

## The Scale Problem

Small systems can coordinate changes. Large systems cannot.

At ten users, you know them all. You can coordinate breaking changes. Communication overhead is manageable.

At one hundred users, you know most of them. Some dependencies remain hidden. Breaking changes require careful planning.

At one thousand users, you know few of them. Hidden dependencies multiply. Breaking changes require migration tools and long deprecation periods.

At ten thousand users, you know almost none of them. Hidden dependencies dominate. Breaking changes become nearly impossible. The observable behavior is the contract (Booch, 1994).

Google's experience demonstrates this at extreme scale. Changing a widely-used API requires automated tooling to find and update all call sites. Even with tooling, some dependencies hide in generated code, reflection, or dynamic dispatch. The cost of change grows superlinearly with user count (Winters et al., 2020).

## Real-World Examples

Python's dictionary iteration order illustrates the principle. Before Python 3.7, dictionaries had undefined iteration order. The CPython implementation preserved insertion order. Code depended on this. When PyPy used different ordering, code broke. Python 3.7 made insertion order official (Van Rossum, 2017).

Linux system calls demonstrate permanent contracts. The kernel maintains binary compatibility with userspace. Every system call behavior, including bugs, becomes permanent API. Linus Torvalds stated "we do not break userspace" (Torvalds, 2012).

AWS S3 eventual consistency created implicit contracts. Applications built retry logic around eventual consistency. When S3 added strong consistency, some applications broke. They had built timing assumptions into their code. The consistency model was documented. The timing was not (Brooker, 2020).

Java serialization shows how implementation leaks through abstraction. Serialization exposes private fields, class structure, and implementation details. Changing any of these breaks deserialization. The entire class implementation becomes part of the serialization contract. Refactoring becomes impossible without breaking compatibility (Bloch, 2008).

## Common Mistakes

**Assuming documentation defines the contract.** Documentation describes intent. Observable behavior defines reality. Users depend on what they observe, not what you document. If behavior contradicts documentation, behavior wins.

**Believing "undefined" means "free to change".** Undefined behavior is still observable. If it is observable, someone depends on it. Marking something as undefined does not prevent dependencies. It just makes them implicit and harder to find.

**Thinking you can deprecate implementation details.** You cannot deprecate what users never knew they were using. They did not read documentation saying "this is an implementation detail". They observed behavior and used it. Deprecation notices are invisible to them.

**Expecting users to report dependencies.** Users do not know they depend on implementation details. The dependency is implicit. They will discover it when you change the behavior and their code breaks. By then it is too late.

**Treating internal and external APIs differently.** Internal APIs with many users face the same constraints as external APIs. The boundary is user count, not organizational structure. A thousand internal users create the same dependency problems as a thousand external users (Winters et al., 2020).

## Put It Into Practice

Audit your APIs for observable behaviors beyond documented contracts. Response timing, error message formats, result ordering, resource cleanup timing, and side effects all create implicit contracts. Document them or eliminate them.

Build tooling to detect dependencies. Static analysis can find some. Runtime monitoring can find others. Google's Kythe project indexes code relationships to enable large-scale refactoring (Winters et al., 2020). Smaller organizations can use grep and call graph analysis.

When changing behavior, assume someone depends on it. Provide migration paths. Add new behavior alongside old. Deprecate gradually. Monitor for breakage. The cost of coordination grows with user count. Plan accordingly.

---

## References

Bloch, Joshua (2008). *Effective Java, Second Edition*. Addison-Wesley. [https://www.pearson.com/en-us/subject-catalog/p/effective-java/P200000000138](https://www.pearson.com/en-us/subject-catalog/p/effective-java/P200000000138)

Booch, Grady (1994). *Object-Oriented Analysis and Design with Applications, Second Edition*. Benjamin/Cummings. [https://www.pearson.com/en-us/subject-catalog/p/object-oriented-analysis-and-design-with-applications/P200000003448](https://www.pearson.com/en-us/subject-catalog/p/object-oriented-analysis-and-design-with-applications/P200000003448)

Brooker, Marc (2020). "Amazon S3 Update: Strong Read-After-Write Consistency." AWS News Blog. [https://aws.amazon.com/blogs/aws/amazon-s3-update-strong-read-after-write-consistency/](https://aws.amazon.com/blogs/aws/amazon-s3-update-strong-read-after-write-consistency/)

Lehman, Meir M. (1980). "Programs, Life Cycles, and Laws of Software Evolution." *Proceedings of the IEEE*, 68(9): 1060-1076. [https://ieeexplore.ieee.org/document/1456074](https://ieeexplore.ieee.org/document/1456074)

Parnas, David L. (1972). "On the Criteria To Be Used in Decomposing Systems into Modules." *Communications of the ACM*, 15(12): 1053-1058. [https://dl.acm.org/doi/10.1145/361598.361623](https://dl.acm.org/doi/10.1145/361598.361623)

Torvalds, Linus (2012). "Linux Kernel Mailing List: We do not break userspace." [https://lkml.org/lkml/2012/12/23/75](https://lkml.org/lkml/2012/12/23/75)

Van Rossum, Guido (2017). "PEP 520: Preserving Class Attribute Definition Order." Python Enhancement Proposals. [https://www.python.org/dev/peps/pep-0520/](https://www.python.org/dev/peps/pep-0520/)

Winters, Titus, Tom Manshreck, and Hyrum Wright (2020). *Software Engineering at Google: Lessons Learned from Programming Over Time*. O'Reilly Media. [https://abseil.io/resources/swe-book](https://abseil.io/resources/swe-book)

Wright, Hyrum (2020). "Hyrum's Law." Software Engineering at Google. [https://www.hyrumslaw.com/](https://www.hyrumslaw.com/)

---

## Outtakes

**The Excel date bug that became a feature.** Excel stores dates as numbers. January 1, 1900 is day 1. February 28, 1900 is day 59. March 1, 1900 is day 61. Day 60 does not exist. 1900 was not a leap year. Excel thinks it was. This is a bug copied from Lotus 1-2-3 for compatibility. Millions of spreadsheets now depend on the nonexistent day. Microsoft cannot fix it. February 29, 1900 is a permanent part of Excel's calendar.

**The Windows filename that crashes programs.** Windows reserves certain filenames: CON, PRN, AUX, NUL, COM1 through COM9, LPT1 through LPT9. These are device names from MS-DOS. You cannot create files with these names. Programs that try will fail. This has been true since 1981. In 2020, a developer discovered that Git on Windows would crash if a repository contained a file named "aux.c". The bug was in Windows, not Git. Microsoft could not fix it. Too many programs depend on the device name behavior. The workaround is to never name files after 1980s hardware ports.

**The browser that rendered blank pages correctly.** Internet Explorer had a quirk. If an HTML page was exactly 256 bytes, IE would not render it. The page would appear blank. This was a buffer size bug. Developers discovered it. They started padding pages to 257 bytes. They added invisible comments. They added extra whitespace. Thousands of sites implemented the workaround. When IE fixed the bug, those sites broke. The padding caused layout issues in the fixed browser. The bug had become the expected behavior.

**The database that sorted numbers alphabetically.** MySQL's default string comparison is case-insensitive. This seems reasonable. The implementation had a quirk. When comparing strings that look like numbers, MySQL would sometimes sort them numerically, sometimes alphabetically. "10" would sort after "2" (numeric) or before "2" (alphabetic) depending on context. Applications built logic around the observed behavior. When MySQL tried to make sorting consistent, applications broke. The inconsistency had become the contract.

**The API that returned errors in the wrong order.** A REST API validated input fields. It returned all validation errors at once. The errors appeared in the order fields were defined in the code. This was an implementation detail. The documentation said nothing about order. Clients started depending on the order. They displayed the first error to users. They assumed it was the most important. The team refactored the code. Field order changed. Error order changed. Client UIs broke. The "most important" error was now buried in the list. The team had to add explicit error ordering to maintain compatibility.

**The compiler optimization that broke video games.** Game developers write timing-sensitive code. They count CPU cycles. They depend on instruction timing. Compiler optimizations change timing. A new compiler version optimized a loop. The game ran faster. Too fast. Physics broke. Collision detection failed. The game was unplayable. The developers had depended on the unoptimized instruction timing. The compiler vendor could not change the optimization. Too many games would break. They added a flag to disable that specific optimization. The flag is still there.

---

## Changelog

**2026-05-24** Initial publication.