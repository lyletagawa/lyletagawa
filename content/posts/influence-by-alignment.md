---
title: "Influence by Alignment"
date: 2026-05-03
publishdate: 2026-05-03
lastmod: 2026-05-03
summary: "Why the gap between formal authority and actual influence is where most technical leadership fails, and how building shared understanding scales where authority cannot."
tags: ["leadership", "management", "influence"]
image:
draft: true
---

## Influence by Alignment

You've been promoted to Staff Engineer. You have no direct reports, no budget authority, and no formal power to make decisions. Yet you're expected to drive technical direction and influence across the organization.

Or you're a Director of Engineering. You have authority over your team, but the critical decisions require buy-in from Product, Infrastructure, Security, and three other engineering teams you don't control. Your org chart says you're in charge. Reality says otherwise.

The gap between formal authority and actual influence is where most technical leadership fails. The solution isn't more authority. It's alignment.

## What Is Alignment?

Alignment means everyone understands the goal, the constraints, and the reasoning. They don't need to agree with every decision, but they understand why it was made and how it fits the larger strategy.

The concept has roots in military leadership theory. David Marquet documented his transformation of the USS Santa Fe from the worst-performing submarine in the fleet to the best by replacing the traditional "leader-follower" model with "leader-leader" (Marquet, 2012). Instead of issuing orders, he trained crew members to say "I intend to..." and explain their reasoning. This required alignment around goals and decision frameworks, not just commands.

Stanley McChrystal extended this in *Team of Teams*: distributed teams making decisions the commander would make, without asking permission, because they understood the intent (McChrystal et al., 2015). Simon Sinek framed the same principle for business audiences: people follow leaders who articulate purpose, not those who wield positional power (Sinek, 2009).

Alignment is not consensus (everyone agrees), democracy (everyone votes), or compromise (everyone gets something). It's shared understanding that enables independent action.

## Why Alignment Scales

**Speed.** Authority requires escalation. Alignment enables local decision-making. Marquet's transformation of the Santa Fe reduced decision latency from hours to minutes by pushing decisions to crew members with the most context (Marquet, 2012).

**Quality.** Authority centralizes decisions with people who have less context. The engineer debugging the production incident knows more than the VP. The IC who built the system understands constraints better than the architect who designed it (Fournier, 2017). High-performing engineering organizations address this by distributing decisions to where context actually lives, enabling autonomous teams through shared goals rather than central control (Kim et al., 2016).

**Scale.** A director with authority is a bottleneck for 50 people. A director who creates alignment enables those same 50 people to make good decisions independently, in parallel. Authority doesn't scale. Alignment does.

**Durability.** When people feel decisions are imposed rather than collaborative, they resist: slow implementation, minimal effort, malicious compliance. DORA research identified psychological safety as one of the strongest cultural predictors of software delivery performance (Forsgren et al., 2018).

## How to Build It

**Articulate the why.** Most technical communication focuses on what and how. Alignment requires why. Marquet trained crew members to state their intent and reasoning before acting (Marquet, 2012). In practice: don't announce "we're moving to Kubernetes." Explain that current deployments require three hours of manual coordination per release, and the goal is deployment independence even at the cost of initial complexity.

**Make decision frameworks explicit.** Strategy isn't goals; it's the framework for making trade-offs (Rumelt, 2011). Give people the criteria, not just the conclusion. "When choosing between performance and maintainability, we prioritize maintainability unless we have data showing performance is a user-facing problem."

**Document decisions and reasoning.** Every significant decision should capture what was decided, why, what alternatives were considered, and what would make you revisit it. This creates institutional memory that enables new team members to make decisions the team would have made. Without it, organizational knowledge lives only in the heads of long-tenured employees, and alignment erodes as teams change (McChrystal et al., 2015).

**Delegate decisions, not tasks.** Marquet's "I intend to..." framework is the clearest example: crew members state their intent, explain their reasoning, and act unless the captain sees a flaw in the logic. Task delegation creates dependency. Decision delegation creates alignment (Marquet, 2012).

**Test understanding.** Alignment isn't one-way communication. Marquet's model requires two conditions before decisions can be pushed down: clarity about the goal, and competence to act on it (Marquet, 2012). Ask "What would you do in this situation?" If people can't answer, you haven't created alignment.

## Common Mistakes

**Confusing alignment with consensus.** Alignment doesn't require everyone to agree. It requires everyone to understand the reasoning well enough to act independently. Chasing consensus slows decisions and doesn't produce alignment.

**Reaching for authority when alignment fails.** Every time you use authority instead of building alignment, you create dependency and build resentment. Marquet used authority sparingly, to create conditions for alignment, then stepped back (Marquet, 2012). Using authority as a shortcut trains people to wait for orders rather than exercise judgment.

**Skipping the why.** Announcing decisions without explaining reasoning produces the appearance of alignment. Six months later, teams make choices that contradict the original intent because they understood what to do but not why.

**Treating alignment as a one-time event.** Alignment decays faster than people expect. Context changes, people turn over, strategy shifts. Teams that aligned around a decision two years ago may be operating on obsolete assumptions today. Re-alignment needs to be a recurring practice, not a kickoff meeting artifact.

**Assuming influence requires position.** For senior ICs, influence without authority is the entire job description. The fastest path to influence is making other people successful, not accumulating formal power (Larson, 2021). Leverage comes from context and coalition, not title.

## Put It Into Practice

Authority is a shortcut. It lets you make decisions quickly without building understanding. But shortcuts have costs: slower execution over time, lower-quality decisions, resentment, and dependency. Alignment is slower to build and faster to scale.

The test is simple: could your team operate effectively without you present? If not, you've built dependency, not alignment. Marquet's crew went on to command positions at unprecedented rates because they learned to think like leaders, not followers (Marquet, 2012).

Pick one recurring decision your team escalates to you. Write down the why, the constraints, and the trade-off criteria. Share it. Then step back the next time the question comes up.

## References

Forsgren, Nicole, et al. (2018). *Accelerate: The Science of Lean Software and DevOps*. Portland, OR: IT Revolution Press. [https://itrevolution.com/product/accelerate/](https://itrevolution.com/product/accelerate/)

Fournier, Camille (2017). *The Manager's Path: A Guide for Tech Leaders Navigating Growth and Change*. Sebastopol, CA: O'Reilly Media. [https://www.oreilly.com/library/view/the-managers-path/9781491973882/](https://www.oreilly.com/library/view/the-managers-path/9781491973882/)

Kim, Gene, et al. (2016). *The DevOps Handbook*. Portland, OR: IT Revolution Press. [https://itrevolution.com/product/the-devops-handbook/](https://itrevolution.com/product/the-devops-handbook/)

Larson, Will (2021). *Staff Engineer: Leadership Beyond the Management Track*. Self-published. [https://staffeng.com/book](https://staffeng.com/book)

Marquet, L. David (2012). *Turn the Ship Around!: A True Story of Turning Followers into Leaders*. New York: Portfolio. [https://www.penguinrandomhouse.com/books/314163/turn-the-ship-around-by-l-david-marquet/](https://www.penguinrandomhouse.com/books/314163/turn-the-ship-around-by-l-david-marquet/)

McChrystal, Stanley, et al. (2015). *Team of Teams: New Rules of Engagement for a Complex World*. New York: Portfolio. [https://www.penguinrandomhouse.com/books/317066/team-of-teams-by-general-stanley-mcchrystal-tantum-collins-david-silverman-and-chris-fussell/](https://www.penguinrandomhouse.com/books/317066/team-of-teams-by-general-stanley-mcchrystal-tantum-collins-david-silverman-and-chris-fussell/)

Rumelt, Richard (2011). *Good Strategy Bad Strategy: The Difference and Why It Matters*. New York: Crown Business. [https://www.penguinrandomhouse.com/books/208668/good-strategy-bad-strategy-by-richard-rumelt/](https://www.penguinrandomhouse.com/books/208668/good-strategy-bad-strategy-by-richard-rumelt/)

Sinek, Simon (2009). *Start with Why: How Great Leaders Inspire Everyone to Take Action*. New York: Portfolio. [https://simonsinek.com/books/start-with-why/](https://simonsinek.com/books/start-with-why/)

## Changelog

**2026-05-03** Removed "The Individual Contributor Perspective" and "Origins" sections; added Common Mistakes section.
