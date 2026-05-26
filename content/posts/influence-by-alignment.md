---
title: "Alignment Over Authority"
date: 2026-05-21
publishdate: 2026-05-21
lastmod: 2026-05-21
summary: "Authority centralizes decisions with whoever has the title. Alignment distributes them to whoever has the context, which is almost never the same person."
tags: ["leadership", "intent", "influence"]
image:
draft: false
---

## Alignment Over Authority

You've been promoted to Staff Engineer. No direct reports, no budget, no formal authority. Yet you're expected to drive technical direction and influence across the org.

Or you're a Director of Engineering. You have authority over your team, but the critical decisions require buy-in from Product, Infrastructure, Security, and three other engineering teams you don't control. Your org chart says you're in charge. Reality says otherwise.

The gap between formal authority and actual influence is where most technical leadership fails. The solution isn't more authority. It's alignment.

## What Is Alignment?

Alignment means everyone understands the goal, the constraints, and the reasoning. They don't need to agree with every decision, but they understand why it was made and how it fits the larger strategy.

David Marquet documented the transformation of the USS Santa Fe from the fleet's worst submarine to its best by replacing the "leader-follower" model with "leader-leader" (Marquet, 2012). Instead of issuing orders, he trained crew members to say "I intend to..." and explain their reasoning. This required alignment around goals and decision frameworks, not just commands.

Stanley McChrystal documented this in *Team of Teams*. Distributed teams made decisions the commander would have made and without asking permission, because they understood the intent (McChrystal et al., 2015). Simon Sinek documented this for business audiences. People follow leaders who articulate purpose, not those who wield positional power (Sinek, 2009).

Alignment is not consensus (everyone agrees), democracy (everyone votes), or compromise (everyone gets something). It's shared understanding that enables independent action.

## Why Alignment Scales

**Speed.** Authority requires escalation. Alignment enables local decision-making. Marquet's transformation of the Santa Fe reduced decision latency from hours to minutes by pushing decisions to crew members with the most context (Marquet, 2012).

**Quality.** Authority centralizes decisions with people who have less context. The engineer debugging the production incident knows more than the VP. The IC who built the system understands constraints better than the architect (Fournier, 2017). High-performing orgs distribute decisions to where context lives, enabling autonomous teams through shared goals rather than central control (Kim et al., 2016).

**Scale.** A director with authority is a bottleneck for 50 people. A director who creates alignment enables those same 50 people to make good decisions independently, in parallel. Authority doesn't scale. Alignment does.

**Durability.** When people feel decisions are imposed rather than collaborative, they resist with slow implementation, minimal effort, and malicious compliance. DORA research identified psychological safety as one of the strongest cultural predictors of software delivery performance (Forsgren et al., 2018).

## How to Build It

**Articulate the why.** Most technical communication focuses on what and how. Alignment requires why. Marquet trained crew members to state their intent and reasoning before acting (Marquet, 2012). In practice, don't announce "we're moving to Kubernetes." Explain that current deployments require three hours of manual coordination per release, and the goal is deployment independence even at the cost of initial complexity.

**Make decision frameworks explicit.** Strategy isn't goals. It's the framework for making trade-offs (Rumelt, 2011). Give people the criteria, not just the conclusion. "We prioritize maintainability over performance unless data shows it's a user-facing problem."

**Write for rooms you're not in.** Senior ICs build influence without authority by making their thinking legible at scale. A technical vision document or architecture proposal carries your reasoning to teams you'll never meet with directly (Larson, 2021). If people need you in the room to understand the direction, you haven't created alignment yet.

**Document decisions and reasoning.** Every significant decision should capture what was decided, why, what alternatives were considered, and what would make you revisit it. This creates institutional memory so new team members can make decisions the team would have made. Without it, organizational knowledge lives only in the heads of long-tenured employees, and alignment erodes as teams change (McChrystal et al., 2015).

**Delegate decisions, not tasks.** Marquet's "I intend to..." framework is the clearest example. Crew members state their intent, explain their reasoning, and act unless the captain sees a flaw in the logic. Task delegation creates dependency. Decision delegation creates alignment (Marquet, 2012).

**Test understanding.** Alignment isn't one-way communication. Marquet's model requires two conditions: clarity about the goal, and competence to act on it (Marquet, 2012). Ask "What would you do in this situation?" If people can't answer, you haven't created alignment.

## Common Mistakes

**Confusing alignment with consensus.** Alignment requires understanding the reasoning well enough to act independently, not agreement. Chasing consensus slows decisions and doesn't produce alignment.

**Reaching for authority when alignment fails.** Using authority instead of building alignment creates dependency and resentment. Marquet used authority sparingly, to create conditions for alignment, then stepped back (Marquet, 2012). Using authority as a shortcut trains people to wait for orders rather than exercise judgment. Authority is appropriate in emergencies and during onboarding. The goal is to reduce reliance on it as the default, not eliminate it entirely.

**Skipping the why.** Announcing decisions without explaining reasoning produces the appearance of alignment. Six months later, teams make choices that contradict the original intent because they understood what to do but not why.

**Treating alignment as a one-time event.** Alignment decays faster than people expect. Context changes, people turn over, strategy shifts. Teams that aligned around a decision two years ago may be operating on obsolete assumptions today. Re-alignment needs to be a recurring practice, not a kickoff meeting artifact.

## Put It Into Practice

Authority is a shortcut. It lets you make decisions quickly without building understanding. But shortcuts have costs. Slower execution over time, lower-quality decisions, resentment, dependency. Alignment is slower to build and faster to scale.

The test is simple. Could your team operate effectively without you present? If not, you've built dependency, not alignment. Marquet's crew went on to command positions at unprecedented rates because they learned to think like leaders, not followers (Marquet, 2012).

Pick one recurring decision your team escalates to you. Write down the why, the constraints, and the trade-off criteria. Share it. Then step back the next time the question comes up.

---

## References

Forsgren, Nicole, et al. (2018). *Accelerate: The Science of Lean Software and DevOps*. Portland, OR: IT Revolution Press. [https://itrevolution.com/product/accelerate/](https://itrevolution.com/product/accelerate/)

Fournier, Camille (2017). *The Manager's Path: A Guide for Tech Leaders Navigating Growth and Change*. Sebastopol, CA: O'Reilly Media. [https://www.oreilly.com/library/view/the-managers-path/9781491973882/](https://www.oreilly.com/library/view/the-managers-path/9781491973882/)

Kim, Gene, et al. (2016). *The DevOps Handbook*. Portland, OR: IT Revolution Press. [https://itrevolution.com/product/the-devops-handbook/](https://itrevolution.com/product/the-devops-handbook/)

Larson, Will (2021). *Staff Engineer: Leadership Beyond the Management Track*. Self-published. [https://staffeng.com/book](https://staffeng.com/book)

Marquet, L. David (2012). *Turn the Ship Around!: A True Story of Turning Followers into Leaders*. New York: Portfolio. [https://www.penguinrandomhouse.com/books/314163/turn-the-ship-around-by-l-david-marquet/](https://www.penguinrandomhouse.com/books/314163/turn-the-ship-around-by-l-david-marquet/)

McChrystal, Stanley, et al. (2015). *Team of Teams: New Rules of Engagement for a Complex World*. New York: Portfolio. [https://www.penguinrandomhouse.com/books/317066/team-of-teams-by-general-stanley-mcchrystal-tantum-collins-david-silverman-and-chris-fussell/](https://www.penguinrandomhouse.com/books/317066/team-of-teams-by-general-stanley-mcchrystal-tantum-collins-david-silverman-and-chris-fussell/)

Rumelt, Richard (2011). *Good Strategy Bad Strategy: The Difference and Why It Matters*. New York: Crown Business. [https://www.penguinrandomhouse.com/books/208668/good-strategy-bad-strategy-by-richard-rumelt/](https://www.penguinrandomhouse.com/books/208668/good-strategy-bad-strategy-by-richard-rumelt/)

Sinek, Simon (2009). *Start with Why: How Great Leaders Inspire Everyone to Take Action*. New York: Portfolio. [https://simonsinek.com/books/start-with-why/](https://simonsinek.com/books/start-with-why/)

---

## Outtakes

**No plan survives contact with the enemy.** Auftragstaktik (mission tactics) was a Prussian military doctrine developed by Helmuth von Moltke in the 1860s. No plan survives contact with the enemy, so officers must understand the goal well enough to improvise. (van Creveld, 1985).

**The Abilene Paradox.** In 1974, Jerry Harvey described a family that drove 53 miles to Abilene, Texas in 104-degree heat, even though nobody wanted to go. Each person assumed the others did, so nobody said otherwise. (Harvey, 1974).

**Disagree and commit.** Bezos detailed this principle in his 2016 shareholder letter. Amazon leadership is expected to argue their position before a decision is made, then execute completely once it is, whether or not their position won. (Bezos, 2016).

---

## Changelog

**2026-05-21** Added counterargument on appropriate use of authority.  
**2026-05-03** Removed "The Individual Contributor Perspective" section. Added Common Mistakes section.
