---
title: "The Andon Cord"
date: 2026-05-22
publishdate: 2026-05-22
lastmod: 2026-05-22
summary: "Toyota's Andon Cord gives any worker the authority to stop the production line when they detect a defect. Stopping is cheaper than passing problems forward, and the principle translates directly to software."
tags: ["safety", "incidents", "management"]
image:
draft: true
---

## The Andon Cord

A worker on the Toyota assembly line sees a misaligned bracket. She has seconds to decide. The line moves at the pace of one car every fifty-seven seconds. If she pulls the cord, the line stops, a light board above her station illuminates, and a team leader arrives within moments. If she doesn't, the bracket passes to the next station, where it becomes someone else's problem, and then someone else's, until it reaches final inspection or a customer.

She pulls the cord.

This is not exceptional behavior at Toyota. It is expected behavior. In the early years of implementation, Toyota plants were stopped many times per shift. Each stop was a signal. Each signal was a candidate for improvement (Liker, 2004).

## What Is the Andon Cord?

The word "andon" refers to a paper lantern in Japanese. In the Toyota Production System, the Andon is a light board showing the status of each workstation on the assembly line. The cord, later a button at many plants, gives any worker the authority to stop the line when something is wrong.

Taiichi Ohno developed the system as part of jidoka, one of the two pillars of the Toyota Production System. Jidoka is often translated as "automation with a human touch." The principle is that machines and people should detect abnormalities and stop immediately rather than pass defects forward (Ohno, 1988).

The underlying logic is economic. A defect fixed at the point of origin takes minutes. The same defect passed ten stations downstream takes hours. Passed to the customer, it triggers recalls, repairs, and reputational damage. Toyota's insight was that stopping is cheaper than continuing, and the system should make stopping easy (Liker, 2004).

## The Mechanism

Three components make the system function.

**The cord.** It is accessible to every worker without escalation. No approval is required. No severity threshold must be met before pulling. Any abnormality justifies the pull.

**The Andon board.** It makes the stop visible to the entire line. The station number illuminates. Everyone sees where the problem is. This transparency removes the shame that would otherwise attach to stopping. The stop is a data point, not an accusation.

**The rapid response.** A team leader arrives within the takt time, the interval between units, to assess and help. If the problem is solvable within that interval, the line doesn't fully stop. If not, it stops until the problem is resolved (Liker, 2004).

Each component is necessary. A cord without visibility becomes an anonymous complaint mechanism. A visible stop without rapid response becomes a punishment system. Rapid response without the cord means only senior workers have the authority to surface problems.

## From Factory Floor to Software

Amazon adapted the mechanism for their marketplace. Any customer service representative can pull a metaphorical cord to pause selling of a product when they identify a defect, a safety concern, or a pattern of complaints. The authority doesn't require management approval. The pause triggers investigation. Selling resumes when the issue is resolved (Bryar and Carr, 2021).

The translation to software is direct. Kim et al. mapped the Andon Cord explicitly to software delivery. Any engineer should be able to stop the deployment pipeline when they detect a problem, without waiting for approval (Kim et al., 2016). The question is what stops the line in a software organization, and who has authority to stop it.

**Deployment halts.** Any engineer who sees something suspicious in a deploy, whether an unexpected metric, an anomalous alert, or an unresolved concern in the diff, should have explicit authority to halt the deploy without sign-off from the engineer who owns the release. The default in most teams is the opposite. You need a reason to stop, not a reason to continue.

**Feature kill switches.** Production feature flags are the software equivalent of the cord. Any on-call engineer can disable a feature without a deploy. The existence of the flag isn't enough. The team must know it exists and know they're authorized to use it.

**Incident declaration.** Any engineer should be able to declare an incident without needing a manager to confirm it. The cost of a false declaration is low. The cost of a delayed declaration is high. Most teams have this policy in writing. Far fewer have it in practice, because the social cost of declaring an incident that turned out to be minor is real and unaddressed.

## Common Mistakes

**Making stopping exceptional.** If pulling the cord requires a justification, a meeting, or an explanation afterward, the cord stops being pulled. The mechanism only works when stopping is the easy default, not the defended exception.

**Skipping the rapid response.** The Andon Cord at Toyota works because a team leader arrives immediately. Without the response, the cord is a delay mechanism, not a quality mechanism. The software equivalent is a deployment halt with no clear owner of the investigation and no time limit on the pause.

**Treating stops as failures.** Organizations that track cord pulls as a negative metric see fewer pulls, not fewer problems. Toyota tracked cord pulls as improvement signals. More pulls meant the system was working (Ohno, 1988). A team that never stops has either solved every problem or stopped reporting them.

**Restricting authority to senior engineers.** The value of the mechanism is that the person closest to the problem has the authority to act. A system where only senior engineers can halt deploys or declare incidents has replicated the hierarchy the cord was designed to bypass.

## Put It Into Practice

Identify one place in your delivery process where the default is to keep moving and stopping requires justification. For most teams, that is a deploy in progress. Write down who currently has the authority to halt it and what they need to demonstrate before halting. If the answer is anything other than "anyone who sees something wrong," the cord is missing.

Create the mechanism and make it visible. A kill switch that only the owning team knows exists is not a cord. A deployment halt that requires approval from the person running the deploy is not a cord. The authority must be explicit, documented, and held by the people most likely to see the problem first.

Track the first pull. When someone uses the mechanism and nothing turns out to be wrong, treat it as evidence the system is working. The cord's value is not that it catches every problem. It is that it makes stopping safe enough that problems get surfaced at all.

---

## References

Bryar, Colin and Bill Carr (2021). *Working Backwards: Insights, Stories, and Secrets from Inside Amazon*. St. Martin's Press. [https://books.google.com/books/about/Working_Backwards.html?id=b9LtDwAAQBAJ](https://books.google.com/books/about/Working_Backwards.html?id=b9LtDwAAQBAJ)

Kim, Gene, Jez Humble, Patrick Debois, and John Willis (2016). *The DevOps Handbook: How to Create World-Class Agility, Reliability, and Security in Technology Organizations*. IT Revolution Press. [https://itrevolution.com/product/the-devops-handbook/](https://itrevolution.com/product/the-devops-handbook/)

Liker, Jeffrey K. (2004). *The Toyota Way: 14 Management Principles from the World's Greatest Manufacturer*. McGraw-Hill. [https://books.google.com/books/about/The_Toyota_Way.html?id=eZutzPww02EC](https://books.google.com/books/about/The_Toyota_Way.html?id=eZutzPww02EC)

Ohno, Taiichi (1988). *Toyota Production System: Beyond Large-Scale Production*. Productivity Press. [https://www.routledge.com/Toyota-Production-System-Beyond-Large-Scale-Production/Ohno/p/book/9780915299140](https://www.routledge.com/Toyota-Production-System-Beyond-Large-Scale-Production/Ohno/p/book/9780915299140)

---

## Changelog

**2026-05-22** Initial publication.
