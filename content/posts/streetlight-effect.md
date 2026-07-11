---
title: "The Streetlight Effect"
date: 2026-05-25
publishdate: 2026-05-25
lastmod: 2026-06-06
summary: "People search where it's easy to look, not where answers hide. Engineers debug familiar systems while ignoring unfamiliar ones where problems actually live."
tags: ["debugging", "observability", "monitoring"]
image:
draft: true
---

## The Streetlight Effect

Your API returns 500 errors. You check application logs. Nothing unusual. Database queries look fast. Cache hit rates are normal. Two hours pass. The problem persists.

Finally, someone checks the load balancer. Connection pool exhausted. The issue was never in the application. You searched where the light was bright.

Engineers debug systems they understand and instrument code they wrote. Unfamiliar territory stays dark, and that's where problems hide.

## What Is The Streetlight Effect?

The Streetlight Effect describes a simple failure pattern. People search where it's easy to look, not where answers are likely to be (Kaplan, 1964).

The name comes from an old joke. A drunk is searching under a streetlight for lost keys when a police officer asks why he's looking there. The drunk lost them somewhere else. "Because this is where the light is," he says.

The joke lands because it's true. People search where searching is easy, not where finding is likely. Research, medicine, business, and engineering all share the pattern (Freedman, 2010).

## Why We Search Where It's Easy

Your brain gravitates toward easy tasks. Searching unfamiliar territory takes real effort, and familiar ground searches feel automatic.

**Cognitive ease drives behavior.** Checking familiar logs feels effortless while understanding unfamiliar systems feels painful. Your brain gravitates toward the path of least resistance, even when that path leads away from the problem. This is System 1 thinking taking over (Kahneman, 2011). Experienced engineers are especially susceptible. The longer you've worked in a domain, the more your instincts route investigations toward familiar territory.

**Tool availability shapes investigation.** You reach for tools you already know. Application profilers feel familiar, and network analyzers don't, so you profile the application. If the only tool you have is a hammer, everything looks like a nail (Maslow, 1966). The streetlight effect works the same way. Light in one spot means you only search that spot.

**Familiarity creates false confidence.** You understand the application layer because you wrote it. The network layer feels foreign, and investigating it can feel like admitting ignorance. So you stay where you feel competent. The confidence is genuine. The competence, in that unfamiliar territory, is not (Dunning, 2011).

**Confirmation bias reinforces the pattern.** You've found problems in familiar systems before, so you expect to find them there again. You search there first, you find something, and the pattern reinforces itself (Nickerson, 1998). The cycle self-selects for comfortable evidence and filters out uncomfortable surprises.

## Real-World Examples

An engineer investigates slow API responses. Application code looks fine, database queries are fast, and cache performance is normal. The problem is DNS. The DNS server is timing out, but the engineer never checked it. They don't usually work with DNS (Allspaw, 2015).

Teams instrument what they understand. They add metrics for application performance but skip network latency and connection pool exhaustion. When problems happen in uninstrumented areas, monitoring is blind. Teams search where they have metrics, not where problems live (Beyer et al., 2016).

Penetration testers scan for known vulnerabilities. SQL injection, cross-site scripting, and authentication bypass all have standard tools. Novel attacks need custom analysis, and zero-day vulnerabilities hide in the territory testers don't usually explore (Schneier, 2000).

Engineers profile code they wrote, optimizing hot loops and reducing allocations. Application profilers show application code, so that's what gets optimized. System calls, kernel time, and I/O wait often dominate actual performance, and those don't show up in application profilers. Optimization targets visible code, not impactful code (Gregg, 2013).

## Common Mistakes

**Confusing visibility with importance.** Measurable metrics feel important and unmeasurable ones feel unimportant. That's backwards. Importance should determine what you measure, not the other way around.

**Over-investing in familiar tools.** Teams buy expensive application monitoring and skip network monitoring. They know one and not the other, so money follows familiarity rather than need.

**Ignoring dark matter in systems.** Every system has parts nobody fully understands. Legacy code, third-party libraries, infrastructure layers. When problems happen there, investigation stalls and teams assume visible parts must be responsible.

**Mistaking measurement for understanding.** Adding metrics feels like progress and more dashboards feel like better observability. But metrics only show what you already suspected. They don't reveal unknown unknowns. The streetlight gets brighter while the dark areas stay dark.

## Put It Into Practice

Map your system's observability. List every component: application, database, cache, load balancer, network, DNS, storage, and operating system. Rate your team's understanding of each from one to five, and rate your instrumentation the same way. Low scores are your blind spots.

When debugging, check unfamiliar systems first. You always check application logs? Start with infrastructure logs. You always profile application code? Start with system calls. The discomfort you feel doing this is the point. You're searching where you don't usually look, and that friction is what tells you you're in the right area.

Build runbooks that include unfamiliar territory. Document how to check DNS, inspect network connections, analyze kernel metrics, and review load balancer logs. When incidents happen, the runbook forces investigation beyond comfortable boundaries. The streetlight expands.

## References

Allspaw, John (2015). "Trade-Offs Under Pressure: Heuristics and Observations Of Teams Resolving Internet Service Outages." *Lund University Cognitive Systems Engineering Laboratory*. [https://www.adaptivecapacitylabs.com/blog/2018/03/23/trade-offs-under-pressure/](https://www.adaptivecapacitylabs.com/blog/2018/03/23/trade-offs-under-pressure/)

Beyer, Betsy, Chris Jones, Jennifer Petoff, and Niall Richard Murphy (2016). *Site Reliability Engineering: How Google Runs Production Systems*. O'Reilly Media. [https://sre.google/books/](https://sre.google/books/)

Dunning, David (2011). "The Dunning-Kruger Effect: On Being Ignorant of One's Own Ignorance." *Advances in Experimental Social Psychology*, 44: 247-296. [https://doi.org/10.1016/B978-0-12-385522-0.00005-6](https://doi.org/10.1016/B978-0-12-385522-0.00005-6)

Freedman, David H. (2010). *Wrong: Why Experts Keep Failing Us*. Little, Brown and Company. [https://www.hachettebookgroup.com/titles/david-h-freedman/wrong/9780316023788/](https://www.hachettebookgroup.com/titles/david-h-freedman/wrong/9780316023788/)

Gregg, Brendan (2013). *Systems Performance: Enterprise and the Cloud*. Prentice Hall. [https://www.brendangregg.com/systems-performance-2nd-edition-book.html](https://www.brendangregg.com/systems-performance-2nd-edition-book.html)

Kahneman, Daniel (2011). *Thinking, Fast and Slow*. Farrar, Straus and Giroux. [https://us.macmillan.com/books/9780374533557/thinkingfastandslow](https://us.macmillan.com/books/9780374533557/thinkingfastandslow)

Kaplan, Abraham (1964). *The Conduct of Inquiry: Methodology for Behavioral Science*. Chandler Publishing Company. [https://www.worldcat.org/title/conduct-of-inquiry-methodology-for-behavioral-science/oclc/523321](https://www.worldcat.org/title/conduct-of-inquiry-methodology-for-behavioral-science/oclc/523321)

Maslow, Abraham H. (1966). *The Psychology of Science: A Reconnaissance*. Harper & Row. [https://www.worldcat.org/title/psychology-of-science-a-reconnaissance/oclc/219733](https://www.worldcat.org/title/psychology-of-science-a-reconnaissance/oclc/219733)

Nickerson, Raymond S. (1998). "Confirmation Bias: A Ubiquitous Phenomenon in Many Guises." *Review of General Psychology*, 2(2): 175-220. [https://doi.org/10.1037/1089-2680.2.2.175](https://doi.org/10.1037/1089-2680.2.2.175)

Schneier, Bruce (2000). *Secrets and Lies: Digital Security in a Networked World*. Wiley. [https://www.schneier.com/books/secrets-and-lies/](https://www.schneier.com/books/secrets-and-lies/)

## Outtakes

**The Mars Climate Orbiter crashed because nobody checked units.** NASA lost a $327 million spacecraft in 1999 when one team sent pound-force-seconds and another expected newton-seconds. Engineers searched trajectory calculations and thruster performance. The unit mismatch was in plain sight. Nobody looked there.

**The doctor who washed his hands got fired.** Ignaz Semmelweis cut maternal mortality from 18% to 2% by having doctors wash their hands between autopsies and deliveries. The medical establishment searched for causes in miasma, bad air, and cosmic influences. Hand washing seemed too simple. He died in an asylum before germ theory was accepted.

**The Therac-25 killed patients because programmers only tested the happy path.** Between 1985 and 1987, the machine delivered doses 100 times too high, killing six people. The software had a race condition triggered only by fast-typing experienced operators. Programmers tested normal usage patterns. The bug lived where experts worked.

**The Challenger disaster happened because everyone searched for reasons to launch.** Morton Thiokol engineers had data showing O-rings failed in cold weather and recommended delay. NASA managers asked for proof the O-rings would fail, not proof they would work. Engineers could show risk but not guarantee failure. The launch proceeded. Seven astronauts died.

**The Millennium Bug cost $300 billion because programmers saved two bytes.** Early systems stored years as two digits to save space, and the problem was identifiable as far back as 1960. Nobody looked ahead. The year 2000 seemed impossibly far away, and the future stayed dark.

**Fukushima flooded because planners only studied recent history.** The plant was built to withstand a 5.7-meter tsunami based on 400 years of written records. The 2011 tsunami reached 14 meters. Geological evidence showed tsunamis of that scale every 1,000 years. The data existed. Nobody looked in the rocks.

## Changelog

**2026-05-25** Initial publication.
