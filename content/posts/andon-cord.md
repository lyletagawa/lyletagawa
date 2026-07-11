---
title: "The Andon Cord"
date: 2026-07-04
publishdate: 2026-07-04
lastmod: 2026-07-04
summary: "Toyota's Andon Cord gives any worker the authority to stop the production line when they detect a defect. Stopping is cheaper than passing problems forward, and the principle translates directly to software."
tags: ["safety", "incidents", "management"]
image: /images/andon-cord.png
draft: true
---

![Toyota's Andon Cord gives any worker the authority to stop the production line when they detect a defect. Stopping is cheaper than passing problems forward, and the principle translates directly to software.](/images/andon-cord.png)
*An Andon board showing station status across an assembly line. Photo: [Toyota Motor Corporation (n.d.)](https://global.toyota/en/company/vision-and-philosophy/production-system/).*

## The Andon Cord

In most production systems, the default is to keep moving, stopping requires a reason and continuing doesn't. Toyota inverted this with the Andon Cord.

At a Toyota plant, any worker on the assembly line can pull the cord the moment something looks wrong, with no approval required and no severity threshold to clear. When they do, an alarm sounds and a light illuminates above the station, giving a team leader 5 to 30 seconds to reach the spot before that segment stops{{< cite 2 "Graban, Mark (2026). No, One Toyota Worker Can't Stop the Whole Factory. Lean Blog." >}}. In the early years, plants were halted many times per shift, and Toyota counted each stop as evidence the system was working, not failing{{< cite 1 "Liker, Jeffrey K. (2004). The Toyota Way: 14 Management Principles from the World's Greatest Manufacturer. McGraw-Hill." >}}.

Toyota proved what that authority is worth at NUMMI, the joint venture with General Motors that took over GM's worst-performing plant in Fremont, California in 1984. Workers got explicit authority to stop production the moment they spotted a defect, and within about a year, using largely the same workforce, the plant was producing some of the best quality in the entire GM system{{< cite 3 "Glass, Ira, and Frank Langfitt (2010). NUMMI. This American Life, Episode 403." >}}.

## What Is the Andon Cord?

The word "andon" refers to a paper lantern in Japanese. In the Toyota Production System, the Andon is a light board showing the status of each workstation on the assembly line. The cord (later a button) gives any worker the authority to stop the line when something is wrong.

Taiichi Ohno developed the system as part of jidoka, one of the two pillars of the Toyota Production System. Jidoka is often translated as "automation with a human touch" ("ji" (self), "dō" (work), "ka" (becoming)). The principle is that machines and people should detect abnormalities and stop immediately rather than pass defects forward{{< cite 4 "Ohno, Taiichi (1988). Toyota Production System: Beyond Large-Scale Production. Productivity Press." >}}.

The underlying logic is economic. A defect fixed at the point of origin takes minutes, the same defect passed ten stations downstream takes hours, and passed all the way to the customer, it triggers recalls, repairs, and reputational damage. Toyota's insight was that stopping is cheaper than continuing, and the system should make stopping easy{{< cite 1 "Liker, Jeffrey K. (2004). The Toyota Way: 14 Management Principles from the World's Greatest Manufacturer. McGraw-Hill." >}}.

## The Mechanism

Three components make the system function.

**The cord.** It is accessible to every worker without escalation, no approval required and no severity threshold to clear before pulling. Any abnormality justifies the pull.

**The Andon board.** It makes the stop visible to the entire line. The station number illuminates and everyone sees where the problem is. This transparency removes the shame that would otherwise attach to stopping. The stop is a data point, not an accusation.

**The rapid response.** A team leader arrives within the takt time, the interval between units, to assess and help{{< cite 1 "Liker, Jeffrey K. (2004). The Toyota Way: 14 Management Principles from the World's Greatest Manufacturer. McGraw-Hill." >}}. If the problem is solvable within that interval, the line doesn't fully stop. If not, only that segment stops, since buffers between segments keep the interruption from spreading further down the line{{< cite 2 "Graban, Mark (2026). No, One Toyota Worker Can't Stop the Whole Factory. Lean Blog." >}}.

Each component is necessary. A cord without visibility becomes an anonymous complaint mechanism. A visible stop without rapid response becomes a punishment system. Rapid response without the cord means only senior workers have the authority to surface problems.

## From Factory Floor to Software

Amazon adapted the mechanism for their marketplace. Any customer service representative can pull a metaphorical cord to pause selling of a product when they identify a defect, a safety concern, or a pattern of complaints. No management approval is required. The pause triggers an investigation, and selling resumes when the issue is resolved{{< cite 5 "Bryar, Colin, and Bill Carr (2021). Working Backwards: Insights, Stories, and Secrets from Inside Amazon. St. Martin's Press." >}}.

The translation to software is direct. Any engineer should be able to stop the deployment pipeline when they detect a problem, without waiting for approval{{< cite 6 "Kim, Gene, Jez Humble, Patrick Debois, and John Willis (2016). The DevOps Handbook: How to Create World-Class Agility, Reliability, and Security in Technology Organizations. IT Revolution Press." >}}. The question is what stops the line in a software organization, and who has authority to stop it.

**Deployment halts.** Any engineer who sees something suspicious in a deploy, whether an unexpected metric, an anomalous alert, or an unresolved concern in the diff, should have explicit authority to halt the deploy without sign-off from the engineer who owns the release. The default in most teams is the opposite. You need a reason to stop, not a reason to continue.

**Feature kill switches.** Production feature flags are the software equivalent of the cord. Any on-call engineer can disable a feature without a deploy. The existence of the flag isn't enough. The team must know it exists and know they're authorized to use it.

**Incident declaration.** Any engineer should be able to declare an incident without needing a manager to confirm it. The cost of a false declaration is low. The cost of a delayed declaration is high. Most teams have this policy in writing. Far fewer have it in practice, because the social cost of declaring an incident that turned out to be minor is real and unaddressed.

## Common Mistakes

**Making stopping exceptional.** If pulling the cord requires a justification, a meeting, or an explanation afterward, the cord stops being pulled. The mechanism only works when stopping is the easy default, not the defended exception.

**Skipping the rapid response.** The Andon Cord at Toyota works because a team leader arrives immediately. Without the response, the cord is a delay mechanism, not a quality mechanism. The software equivalent is a deployment halt with no clear owner of the investigation and no time limit on the pause.

**Treating stops as failures.** Organizations that track cord pulls as a negative metric see fewer pulls, not fewer problems. Toyota tracked cord pulls as improvement signals, with more pulls meaning the system was working{{< cite 4 "Ohno, Taiichi (1988). Toyota Production System: Beyond Large-Scale Production. Productivity Press." >}}. A team that never stops has either solved every problem or stopped reporting them.

**Restricting authority to senior engineers.** The value of the mechanism is that the person closest to the problem has the authority to act. A system where only senior engineers can halt deploys or declare incidents has replicated the hierarchy the cord was designed to bypass.

## Put It Into Practice

Identify one place in your delivery process where the default is to keep moving and stopping requires justification. For most teams, that is a deploy in progress. Write down who currently has the authority to halt it and what they need to demonstrate before halting. If the answer is anything other than "anyone who sees something wrong," the cord is missing.

Create the mechanism and make it visible. A kill switch that only the owning team knows exists is not a cord. A deployment halt that requires approval from the person running the deploy is not a cord. The authority must be explicit, documented, and held by the people most likely to see the problem first.

Track the first pull. When someone uses the mechanism and nothing turns out to be wrong, treat it as evidence the system is working. The cord's value is not that it catches every problem. It is that it makes stopping safe enough that problems get surfaced at all.

## Go Further

**Poka-yoke, the sibling concept.** Shigeo Shingo's mistake-proofing approach designs a process so an error is physically impossible or immediately obvious, catching problems before a worker ever needs to reach for the cord{{< cite 7 "Shingo, Shigeo (1986). Zero Quality Control: Source Inspection and the Poka-Yoke System. Productivity Press." >}}.

**Circuit breakers, the automated cord.** Michael Nygard's stability patterns include the circuit breaker, which stops calls to a failing service automatically instead of waiting for a person to notice, the same stop-is-cheaper-than-continuing logic running with nobody in the loop{{< cite 8 "Nygard, Michael T. (2018). Release It! Second Edition: Design and Deploy Production-Ready Software. Pragmatic Bookshelf." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Liker, Jeffrey K. (2004). <em>The Toyota Way: 14 Management Principles from the World's Greatest Manufacturer</em>. McGraw-Hill. <a href="https://books.google.com/books/about/The_Toyota_Way.html?id=eZutzPww02EC">https://books.google.com/books/about/The_Toyota_Way.html?id=eZutzPww02EC</a></li>
  <li id="ref-2">Graban, Mark (2026). "No, One Toyota Worker Can't Stop the Whole Factory." Lean Blog. <a href="https://www.leanblog.org/2026/06/andon-cord-stop-the-line-myth/">https://www.leanblog.org/2026/06/andon-cord-stop-the-line-myth/</a></li>
  <li id="ref-3">Glass, Ira, and Frank Langfitt (2010). "NUMMI." <em>This American Life</em>, Episode 403. <a href="https://www.thisamericanlife.org/403/nummi-2010">https://www.thisamericanlife.org/403/nummi-2010</a></li>
  <li id="ref-4">Ohno, Taiichi (1988). <em>Toyota Production System: Beyond Large-Scale Production</em>. Productivity Press. <a href="https://www.routledge.com/Toyota-Production-System-Beyond-Large-Scale-Production/Ohno/p/book/9780915299140">https://www.routledge.com/Toyota-Production-System-Beyond-Large-Scale-Production/Ohno/p/book/9780915299140</a></li>
  <li id="ref-5">Bryar, Colin, and Bill Carr (2021). <em>Working Backwards: Insights, Stories, and Secrets from Inside Amazon</em>. St. Martin's Press. <a href="https://books.google.com/books/about/Working_Backwards.html?id=b9LtDwAAQBAJ">https://books.google.com/books/about/Working_Backwards.html?id=b9LtDwAAQBAJ</a></li>
  <li id="ref-6">Kim, Gene, Jez Humble, Patrick Debois, and John Willis (2016). <em>The DevOps Handbook: How to Create World-Class Agility, Reliability, and Security in Technology Organizations</em>. IT Revolution Press. <a href="https://itrevolution.com/product/the-devops-handbook/">https://itrevolution.com/product/the-devops-handbook/</a></li>
  <li id="ref-7">Shingo, Shigeo (1986). <em>Zero Quality Control: Source Inspection and the Poka-Yoke System</em>. Productivity Press. <a href="https://www.routledge.com/Zero-Quality-Control-Source-Inspection-and-the-Poka-Yoke-System/Shingo/p/book/9780915299072">https://www.routledge.com/Zero-Quality-Control-Source-Inspection-and-the-Poka-Yoke-System/Shingo/p/book/9780915299072</a></li>
  <li id="ref-8">Nygard, Michael T. (2018). <em>Release It! Second Edition: Design and Deploy Production-Ready Software</em>. Pragmatic Bookshelf. <a href="https://pragprog.com/titles/mnee2/release-it-second-edition/">https://pragprog.com/titles/mnee2/release-it-second-edition/</a></li>
</ol>

---

## Outtakes

**Toyota pulled the cord a thousand times more often than Ford.** At Toyota's Georgetown, Kentucky plant, workers pulled the andon cord about 2,000 times a week. At Ford's newly built Dearborn, Michigan truck plant, workers pulled it twice a week, same equipment, opposite cultures ([Graban, 2007](https://www.leanblog.org/2007/02/toyota-ford-andon-cord-culture-difference/)).

**Toyota swapped the cord for a button.** In 2014, some Toyota plants started replacing the hanging pull-cord with a waist-high call button, mainly for ergonomics and to clear overhead clutter. The trust-based system behind it, and the name, stayed the same ([Graban, 2014](https://www.leanblog.org/2014/08/why-toyota-is-eliminating-the-andon-cord-from-its-factories/)).

---

## Changelog

**2026-07-04** Initial publication.
