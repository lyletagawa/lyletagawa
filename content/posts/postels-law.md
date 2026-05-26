---
title: "Postel's Law"
date: 2026-05-24
publishdate: 2026-05-24
lastmod: 2026-05-24
summary: "Be conservative in what you send, liberal in what you accept. This principle built the early internet but now creates security vulnerabilities and interoperability nightmares in modern distributed systems."
tags: ["protocols", "api-design", "security"]
image:
draft: true
---

## Postel's Law

Your API accepts malformed JSON. It guesses what the client meant. It fills in missing fields. It coerces types. It works. The client never fixes their bug because your tolerance masked it. Six months later, a new service integrates with the same client. It rejects the malformed input. The client breaks. Production goes down.

Services that tolerate bad input enable bad clients. Those clients fail when they encounter stricter implementations. The principle that built the early internet now undermines reliability in modern systems.

## What Is Postel's Law?

Postel's Law states: "Be conservative in what you send, liberal in what you accept" (Postel, 1980). Jon Postel introduced this in RFC 760, the Internet Protocol specification. The context was building interoperable protocols when implementations varied and specifications were incomplete.

Different vendors implemented TCP/IP with different interpretations. Strict implementations would reject packets from lenient ones. The network would fragment. Postel's principle solved this by encouraging implementations to accept variations while producing conformant output.

Also called the Robustness Principle, it became foundational to internet protocol design. HTTP, SMTP, and DNS all embody this philosophy. Browsers render malformed HTML. Email servers accept messages with minor header violations. The principle enabled the internet to grow despite inconsistent implementations.

## Why It Worked Then

The early internet operated under different constraints. Network reliability was poor. Packet loss was common. Retransmission was expensive. Accepting malformed input beat dropping packets (Cerf & Kahn, 1974).

Implementations were few. A dozen organizations implemented TCP/IP. They could coordinate fixes. When one produced non-conformant output, others adapted. The ecosystem was small enough to manage informally.

Specifications were incomplete. RFCs left details unspecified. Implementers made different choices. Strict conformance was impossible because conformance itself was undefined.

Security threats were minimal. The early internet connected trusted institutions. Malicious input was not a concern. The threat model assumed good faith actors (Saltzer & Schroeder, 1975).

## How It Breaks Down Now

Modern distributed systems operate under inverted constraints. The principle that enabled early internet growth now creates systemic problems.

**Security vulnerabilities multiply.** Accepting unexpected input creates attack surface. Parsers that tolerate malformed data must handle edge cases. Each is a potential vulnerability. Heartbleed exploited OpenSSL's lenient handling of malformed heartbeat messages. The parser accepted invalid length fields and leaked memory (Durumeric et al., 2014).

**Interoperability degrades.** When implementations accept bad input, clients never fix bugs. Those bugs become de facto requirements. New implementations must replicate the same tolerance or break existing clients. What started as flexibility becomes rigid dependency on undefined behavior (Thomson, 2015).

**Complexity explodes.** Liberal acceptance requires complex parsing logic. The parser must handle conformant input plus all variations clients produce. Each variation adds code paths and maintenance burden. Strict parsers are simpler.

**Testing becomes impossible.** You cannot test what you accept because you accept anything. You cannot enumerate all possible malformed inputs. The attack surface is unbounded. Strict validation creates a finite test space (Miller et al., 1990).

**Version migration fails.** When you deprecate old behavior, you cannot identify which clients depend on it. Your liberal acceptance hid their non-conformance. The technical debt becomes permanent.

## Real-World Failures

HTML parsing demonstrates the problem at scale. Browsers accept malformed HTML. This enabled the web to grow despite poor authoring tools. It also created a nightmare. Each browser implements different error recovery. The same malformed HTML renders differently across browsers (Hickson, 2011).

The HTML5 specification attempted to standardize error handling. It defined exactly how to parse malformed input. The specification is 1,281 pages. Much of that complexity comes from specifying error recovery for every possible malformation.

JSON parsing shows the alternative. The JSON specification is strict. Parsers reject malformed input. Early implementations varied in handling edge cases. The community converged on strict conformance. Modern JSON parsers are interoperable because they all reject the same malformed inputs (Crockford, 2006).

Kubernetes API versioning illustrates the operational cost. The API accepts multiple versions of the same resource and converts between them internally. The conversion logic must handle every version ever supported. Bugs cause data corruption. The API server carries permanent technical debt (Burns et al., 2016).

## Common Mistakes

**Applying the principle to application protocols.** Postel's Law was designed for network protocols operating over unreliable links. Application protocols operate over reliable transports like TCP. The original justification does not apply. Strict validation is feasible and preferable.

**Confusing tolerance with backward compatibility.** Backward compatibility means supporting old versions of your own protocol. Tolerance means accepting malformed input from broken clients. These are different. You can maintain backward compatibility with strict validation by versioning your API properly.

**Treating all input as potentially valid.** Some input is unambiguously wrong. A negative content length is wrong. A timestamp in the year 3000 is wrong. Accepting obviously invalid input does not improve interoperability. It hides bugs and creates security vulnerabilities.

**Assuming tolerance is free.** Liberal acceptance requires complex parsing logic. That complexity has costs in development time, maintenance burden, attack surface, and performance. The costs often exceed the benefits, especially in modern systems where strict validation is feasible.

## Put It Into Practice

Review your API input validation. Identify where you accept malformed input "to be helpful." Ask whether the tolerance serves a purpose or hides client bugs. If it hides bugs, make validation strict.

When designing new protocols or APIs, default to strict validation. Accept only well-formed input. Reject everything else with specific error messages. Clients fix their bugs instead of depending on your tolerance.

For existing systems, add metrics that track malformed input. Identify which clients need fixing. Work with their teams. Then tighten validation without breaking production.

---

## References

Burns, Brendan, Brian Grant, David Oppenheimer, Eric Brewer, and John Wilkes (2016). "Borg, Omega, and Kubernetes." *Communications of the ACM*, 59(5): 50-57. [https://dl.acm.org/doi/10.1145/2890784](https://dl.acm.org/doi/10.1145/2890784)

Cerf, Vinton and Robert Kahn (1974). "A Protocol for Packet Network Intercommunication." *IEEE Transactions on Communications*, 22(5): 637-648. [https://ieeexplore.ieee.org/document/1092259](https://ieeexplore.ieee.org/document/1092259)

Crockford, Douglas (2006). "The application/json Media Type for JavaScript Object Notation (JSON)." RFC 4627. [https://www.rfc-editor.org/rfc/rfc4627](https://www.rfc-editor.org/rfc/rfc4627)

Durumeric, Zakir, Frank Li, James Kasten, Johanna Amann, Jethro Beekman, Mathias Payer, Nicolas Weaver, David Adrian, Vern Paxson, Michael Bailey, and J. Alex Halderman (2014). "The Matter of Heartbleed." *Proceedings of the 2014 Conference on Internet Measurement Conference*: 475-488. [https://dl.acm.org/doi/10.1145/2663716.2663755](https://dl.acm.org/doi/10.1145/2663716.2663755)

Hickson, Ian (2011). "HTML5: A vocabulary and associated APIs for HTML and XHTML." W3C Working Draft. [https://www.w3.org/TR/2011/WD-html5-20110525/](https://www.w3.org/TR/2011/WD-html5-20110525/)

Miller, Barton P., Lars Fredriksen, and Bryan So (1990). "An Empirical Study of the Reliability of UNIX Utilities." *Communications of the ACM*, 33(12): 32-44. [https://dl.acm.org/doi/10.1145/96267.96279](https://dl.acm.org/doi/10.1145/96267.96279)

Postel, Jon (1980). "DoD Standard Internet Protocol." RFC 760. [https://www.rfc-editor.org/rfc/rfc760](https://www.rfc-editor.org/rfc/rfc760)

Saltzer, Jerome H. and Michael D. Schroeder (1975). "The Protection of Information in Computer Systems." *Proceedings of the IEEE*, 63(9): 1278-1308. [https://ieeexplore.ieee.org/document/1451869](https://ieeexplore.ieee.org/document/1451869)

Thomson, Martin (2015). "The Harmful Consequences of the Robustness Principle." RFC 9413. [https://www.rfc-editor.org/rfc/rfc9413](https://www.rfc-editor.org/rfc/rfc9413)

---

## Outtakes

**The email that broke the internet.** In 1982, a researcher at MIT sent an email with a malformed header that crashed every mail server on the ARPANET. The servers were being liberal in what they accepted. They just had not anticipated that particular flavor of malformed. The network was down for hours. Jon Postel himself had to coordinate the fix across a dozen institutions. The irony was not lost on anyone.

**Browser wars as parser wars.** Internet Explorer 6 would render `<table><div>` by moving the div outside the table. Firefox would keep it inside. Safari would sometimes do one, sometimes the other, depending on what came after. Web developers called this "the div lottery." You wrote HTML and gambled on where your content would appear. The HTML5 spec eventually standardized this to 247 pages of parsing rules for tables alone.

**The Y2K of JSON.** When JavaScript added BigInt support, some developers started serializing 64-bit integers in JSON. The JSON spec says numbers are IEEE 754 doubles, which cannot represent all 64-bit integers. Parsers disagreed on what to do. Some truncated. Some threw errors. Some converted to strings. Some invented a new number type. The "fix" was to tell developers to quote their big integers as strings, which meant JSON was no longer self-describing. You needed out-of-band knowledge to parse it correctly.

**The protocol that could not die.** FTP has a mode called "active mode" where the server connects back to the client. This made sense in 1971. It makes no sense behind NAT. Every FTP client implements a workaround called "passive mode." But they still support active mode because some server somewhere might need it. That server is probably running on a PDP-11 in a basement. Nobody knows where. Nobody dares turn it off. The protocol specification is 50 years old and still maintained because we cannot prove nothing depends on it.

**The Unicode normalization nightmare.** Unicode has multiple ways to represent the same character. "é" can be one codepoint or two (e + combining accent). File systems on macOS normalize to one form. Linux uses another. Windows does not normalize at all. This means a filename that works on Mac fails on Linux. The "solution" was to make every application normalize Unicode itself. Now every application has its own Unicode bugs. The liberal acceptance of multiple representations created a permanent interoperability problem.

---

## Changelog

**2026-05-24** Initial publication.