---
title: "The Streetlight Effect"
date: 2026-05-25
publishdate: 2026-05-25
lastmod: 2026-05-25
summary: "People search where it's easy to look, not where answers hide. Engineers debug familiar systems while ignoring unfamiliar ones where problems actually live."
tags: ["debugging", "observability", "cognitive-bias", "problem-solving", "monitoring"]
image:
draft: true
---

## The Streetlight Effect

Your API returns 500 errors. You check application logs. Nothing unusual. Database queries look fast. Cache hit rates are normal. Two hours pass. The problem persists.

Finally, someone checks the load balancer. Connection pool exhausted. The issue was never in the application. You searched where the light was bright.

Engineers debug systems they understand. They instrument code they wrote. The unfamiliar stays dark. Problems hide there.

## What Is The Streetlight Effect?

The Streetlight Effect is simple. People search where it's easy to look. Not where things are likely to be (Kaplan, 1964). Abraham Kaplan wrote about this in "The Conduct of Inquiry: Methodology for Behavioral Science." He was studying how researchers make mistakes.

The name comes from an old joke. A drunk searches under a streetlight for lost keys. A police officer asks why. The drunk lost them somewhere else. "Because this is where the light is," he says.

The joke is funny because it's true. People search where searching is easy. Not where finding is likely. This happens in research. In medicine. In business. In engineering. Anywhere humans investigate problems (Freedman, 2010).

## Why We Search Where It's Easy

Your brain likes easy tasks. Searching unfamiliar territory is hard. Searching familiar ground is easy. Your brain picks easy.

**Cognitive ease drives behavior.** Checking familiar logs feels effortless. Understanding unfamiliar systems feels painful. Your brain chooses the path of least resistance. Even when it's wrong. This is System 1 thinking taking over (Kahneman, 2011).

**Tool availability shapes investigation.** You use tools you know. You know application profilers. You don't know network analyzers. So you profile applications. Maslow said "if the only tool you have is a hammer, everything looks like a nail" (Maslow, 1966). The streetlight effect is the opposite. If you only have light in one spot, you only search that spot.

**Familiarity creates false confidence.** You understand the application layer. You wrote it. The network layer feels foreign. Investigating it feels like admitting ignorance. You avoid that feeling. You stay where you feel competent. The confidence is real. The competence is fake (Dunning, 2011).

**Confirmation bias reinforces the pattern.** You found problems in familiar systems before. So you expect problems there. You search there first. You find problems because you search there. The cycle feeds itself (Nickerson, 1998).

## Real-World Examples

An engineer investigates slow API responses. Application code looks fine. Database queries are fast. Cache performance is normal. The problem is DNS. The DNS server is timing out. The engineer never checked DNS. They don't usually work with DNS (Allspaw, 2015).

Teams instrument what they understand. They add metrics for application performance. They skip network latency. They skip connection pool exhaustion. When problems happen in uninstrumented areas, the monitoring is blind. The team searches where they have metrics. Not where problems live (Beyer et al., 2016).

Penetration testers scan for known vulnerabilities. SQL injection. Cross-site scripting. Authentication bypass. These have standard tools. Novel attacks need custom analysis. Testers find what they look for. Zero-day vulnerabilities hide in unexplored territory (Schneier, 2000).

Engineers profile code they wrote. They optimize hot loops. They reduce allocations. The profiler shows application code. So they optimize application code. System calls dominate performance. Kernel time dominates performance. I/O wait dominates performance. These don't show up in application profilers. The optimization focuses on visible code. Not impactful code (Gregg, 2013).

## Common Mistakes

**Confusing visibility with importance.** You can measure some metrics. Those feel important. You can't measure other metrics. Those feel unimportant. This is backwards. Importance should determine what you measure. Not the other way around.

**Over-investing in familiar tools.** Teams buy expensive application monitoring. They ignore network monitoring. They know application monitoring. Network monitoring is unfamiliar. Money follows familiarity. Not need.

**Ignoring dark matter in systems.** Every system has parts nobody understands. Legacy code. Third-party libraries. Infrastructure layers. When problems happen there, investigation stops. Teams assume problems must be in visible parts.

**Mistaking measurement for understanding.** Adding metrics feels like progress. More dashboards feel like better observability. But metrics only show what you already suspected. They don't reveal unknown unknowns. The streetlight gets brighter. The dark areas stay dark.

## Put It Into Practice

Map your system's observability. List every component. Application. Database. Cache. Load balancer. Network. DNS. Storage. Operating system. Rate your team's understanding from one to five. Rate your instrumentation the same way. Low scores are your blind spots.

When debugging, check unfamiliar systems first. You always check application logs? Start with infrastructure logs. You always profile application code? Start with system calls. The discomfort is the point. You're searching where you don't usually look.

Build runbooks that include unfamiliar territory. Document how to check DNS. How to inspect network connections. How to analyze kernel metrics. How to review load balancer logs. When incidents happen, the runbook forces investigation beyond comfortable boundaries. The streetlight expands.

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

**The Mars Climate Orbiter crashed because everyone looked at the wrong units.** NASA lost a $327 million spacecraft in 1999. One team used metric units. Another used imperial units. The navigation software expected newton-seconds. The ground software sent pound-force-seconds. Nobody checked the units. Everyone assumed their part was correct. They searched for problems in trajectory calculations. They searched for problems in thruster performance. The unit mismatch was obvious. Nobody looked. The orbiter burned up in Mars atmosphere.

**The doctor who washed his hands got fired.** Ignaz Semmelweis discovered something in 1847. Doctors washing hands between autopsies and deliveries reduced maternal mortality from 18% to 2%. Other doctors rejected this. They looked for causes in miasma. In bad air. In cosmic influences. Hand washing seemed too simple. Semmelweis was ridiculed. Eventually committed to an asylum. He died there. The medical establishment searched everywhere except the obvious place. Germ theory wasn't accepted until decades later.

**The Therac-25 radiation machine killed patients because programmers only tested the happy path.** Between 1985 and 1987, a radiation therapy machine gave patients doses 100 times too high. Six people died. The software had a race condition. It only happened when operators typed very quickly. Programmers tested normal usage. Experienced operators were fast. The bug only appeared in production with expert users. Testing focused on what developers expected. Not what users actually did.

**The Challenger disaster happened because nobody wanted to discuss O-rings.** Engineers at Morton Thiokol knew the O-rings failed in cold weather. They had data. They recommended delaying the launch. NASA managers asked for proof the O-rings would fail. Not proof they would work. The burden of proof was backwards. Engineers couldn't prove failure. Only show risk. The launch proceeded. The O-rings failed at 28°F. Seven astronauts died. Everyone searched for reasons to launch. Nobody searched hard enough for reasons not to.

**The Millennium Bug cost $300 billion to fix because programmers saved two bytes.** Early programmers stored years as two digits. 1970 became 70. This worked until 2000. Systems would interpret 00 as 1900. Banks spent years fixing code. Airlines spent years fixing code. Governments spent years fixing code. The problem was obvious in 1960. Nobody looked. The year 2000 seemed impossibly far away. The streetlight was on the present. The future was dark.

**The Fukushima nuclear plant flooded because planners only studied recent history.** The plant was built to withstand a 5.7-meter tsunami. Based on historical records. The 2011 tsunami was 14 meters. Historical records only went back 400 years. Geological evidence showed larger tsunamis every 1,000 years. Planners looked at written records. Not geological data. The data existed. Nobody looked in the rocks. Three reactors melted down.

## Changelog

**2026-05-25** Initial publication.