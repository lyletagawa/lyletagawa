---
title: "Parkinson's Law"
date: 2026-05-24
publishdate: 2026-05-24
lastmod: 2026-05-24
summary: "Organizations spend more time debating trivial decisions than critical ones. The bike-shed effect explains why meetings focus on minor details while major issues get rubber-stamped."
tags: ["decision-making", "code-review", "architecture", "sre", "engineering-culture"]
image:
draft: true
---

## Parkinson's Law

The executive committee reviews a $50 million data center proposal in twelve minutes. Everyone nods. Approved. Next item: the bike shed color. The debate runs forty-five minutes. Three people have strong opinions about paint finishes. Two others want to revisit the location. Someone suggests forming a subcommittee.

This pattern appears everywhere. Boards spend hours on catering budgets and minutes on strategic acquisitions. Engineering teams argue about code formatting while architectural decisions slide through unchallenged. The trivial consumes time that the critical does not.

## What Is Parkinson's Law of Triviality?

Parkinson's Law of Triviality states that organizations give disproportionate weight to trivial issues (Parkinson, 1957). C. Northcote Parkinson introduced the concept in "Parkinson's Law: The Pursuit of Progress," using a committee reviewing plans for a nuclear reactor and a bike shed. The reactor passes quickly. The bike shed generates extensive debate.

Committee members felt unqualified to challenge the reactor design. Technical complexity created a barrier. Nobody wanted to appear ignorant. The bike shed was different. Everyone understood paint and wood. The accessibility invited participation.

Also called the bike-shed effect, Parkinson chose this example deliberately. A bike shed is simple, cheap, and familiar. The nuclear reactor is complex, expensive, and specialized.

## Why Trivial Issues Dominate

The phenomenon stems from how humans assess their ability to contribute. People speak up when they feel competent. They stay quiet when they feel lost.

**Cognitive accessibility determines engagement.** People understand bike sheds. They have opinions about colors. The topic requires no specialized knowledge. A nuclear reactor requires physics and engineering expertise. Contributing feels risky. Better to stay quiet than reveal ignorance (Kahneman, 2011).

**Trivial decisions have lower perceived stakes.** Getting the bike shed color wrong seems fixable. Getting the reactor design wrong seems catastrophic. Lower stakes invite more debate. People relax their filters and explore alternatives. High stakes create pressure to defer to experts (Tetlock & Gardner, 2015).

**Participation signals competence.** Meetings reward visible contribution. Staying silent looks passive. Speaking up looks engaged. When the topic is accessible, everyone can participate. When the topic is specialized, only experts can contribute without risk. The result is that simple topics generate more discussion because more people can safely demonstrate engagement (Surowiecki, 2004).

**Time expands to fill perceived importance.** Groups unconsciously allocate time based on how much they feel qualified to discuss, not on actual importance. The bike shed feels discussable, so discussion happens. The reactor feels beyond most people's expertise, so discussion is brief. The allocation inverts the actual stakes (Parkinson, 1957).

## Real-World Examples

Software engineering teams demonstrate the pattern daily. An architecture review for migrating to microservices passes in twenty minutes with minimal questions. The team then spends an hour debating whether to use tabs or spaces for indentation. Everyone has opinions about formatting. Few feel qualified to challenge distributed systems design (Forsgren et al., 2018).

SRE teams face this during incident reviews. A postmortem analyzing a cascading failure in the load balancing tier concludes quickly. The team agrees on the technical root cause and moves on. The discussion then shifts to the incident notification format. Thirty minutes of debate about Slack message templates follows. Everyone understands notifications. Few want to challenge the load balancer analysis (Beyer et al., 2016).

Design systems exhibit the pattern in component libraries. A proposal to refactor the entire state management architecture receives cursory review. A proposal to adjust button border radius by two pixels generates extensive discussion. Designers feel comfortable debating visual details. They feel less comfortable evaluating state management patterns (Frost, 2016).

## Common Mistakes

**Confusing discussion length with decision quality.** More debate does not mean better outcomes. The bike shed might get forty-five minutes and still be wrong. The reactor might get twelve minutes and be perfect. Discussion length reflects participant comfort, not decision importance.

**Failing to recognize expertise barriers.** When nobody challenges a complex proposal, the problem might not be that the proposal is good. The problem might be that nobody feels qualified to challenge it. Silence can indicate intimidation rather than agreement. Leaders who interpret quiet approval as consensus miss opportunities to surface concerns.

**Allowing trivial debates to crowd out critical ones.** Meeting time is finite. Organizations that let trivial issues dominate systematically under-invest attention in high-stakes decisions. The urgent and accessible displaces the important and complex.

**Using the law to dismiss legitimate concerns.** Calling something a bike-shed debate can shut down valid discussion. Not every detailed conversation is trivial. Sometimes the details matter. The law describes a bias, not a rule for silencing dissent. Leaders who weaponize the concept to avoid scrutiny create different problems.

## Put It Into Practice

Before your next architecture review or design meeting, list agenda items by technical risk and impact. Allocate time proportional to complexity and blast radius, not to how comfortable the team feels discussing each topic. A database migration strategy deserves more time than variable naming conventions, regardless of which generates more opinions.

When you notice a trivial debate consuming time, name it. Say "This feels like a bike-shed discussion." The label creates awareness without judgment. The team can then decide whether to continue or redirect. Sometimes formatting debates matter for team cohesion. Often they displace critical technical decisions. Naming the pattern lets the team choose consciously.

For complex technical proposals, explicitly invite questions from junior engineers and non-specialists. Say "This is complex, but I want to hear concerns even if they seem basic." Permission to ask simple questions reduces the intimidation barrier. Teams surface assumptions and edge cases that experts miss. The distributed system design gets the scrutiny it deserves.

## References

Beyer, Betsy, Chris Jones, Jennifer Petoff, and Niall Richard Murphy (2016). *Site Reliability Engineering: How Google Runs Production Systems*. O'Reilly Media. [https://sre.google/books/](https://sre.google/books/)

Forsgren, Nicole, Jez Humble, and Gene Kim (2018). *Accelerate: The Science of Lean Software and DevOps*. IT Revolution Press. [https://itrevolution.com/product/accelerate/](https://itrevolution.com/product/accelerate/)

Frost, Brad (2016). *Atomic Design*. Brad Frost. [https://atomicdesign.bradfrost.com/](https://atomicdesign.bradfrost.com/)

Kahneman, Daniel (2011). *Thinking, Fast and Slow*. Farrar, Straus and Giroux. [https://us.macmillan.com/books/9780374533557/thinkingfastandslow](https://us.macmillan.com/books/9780374533557/thinkingfastandslow)

Parkinson, C. Northcote (1957). *Parkinson's Law: The Pursuit of Progress*. John Murray. [https://www.worldcat.org/title/parkinsons-law-the-pursuit-of-progress/oclc/2054428](https://www.worldcat.org/title/parkinsons-law-the-pursuit-of-progress/oclc/2054428)

Surowiecki, James (2004). *The Wisdom of Crowds*. Doubleday. [https://www.penguinrandomhouse.com/books/175380/the-wisdom-of-crowds-by-james-surowiecki/](https://www.penguinrandomhouse.com/books/175380/the-wisdom-of-crowds-by-james-surowiecki/)

Tetlock, Philip E. and Dan Gardner (2015). *Superforecasting: The Art and Science of Prediction*. Crown. [https://www.penguinrandomhouse.com/books/227815/superforecasting-by-philip-e-tetlock-and-dan-gardner/](https://www.penguinrandomhouse.com/books/227815/superforecasting-by-philip-e-tetlock-and-dan-gardner/)

## Changelog

**2026-05-24** Initial publication with DevOps, SRE, and software engineering examples.