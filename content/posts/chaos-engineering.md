---
title: "Chaos Engineering"
date: 2020-06-25
summary: "Breaking things on purpose. Proactive resilience through controlled experimentation."
tags: ["chaos", "learning", "incidents"]
---

## Chaos Engineering

Modern distributed systems fail in ways we cannot predict.

Production incidents reveal unexpected interactions between services, infrastructure, and operators. Chaos Engineering takes a different approach: deliberately introducing controlled failures to discover systemic weaknesses before they cause customer-impacting outages.

Chaos Engineering is a proactive discipline for navigating complex systems, facilitating experiments to uncover weaknesses before they manifest as incidents (Rosenthal et al., 2020).

## Why Chaos Engineering Matters

Chaos Engineering addresses the needs of operating complex and nonlinear systems. As Leslie Lamport observed, "A distributed system is one in which the failure of a computer you didn't even know existed can render your own computer unusable."

Charles Perrow's research demonstrates that "multiple and unexpected interactions of failures are inevitable" in complex systems (Perrow, 1984).

Complexity can be sorted into two buckets: accidental and essential (Brooks, 1986). Accidental complexity is always accruing. In order to make progress, essential complexity will increase. Organizations cannot eliminate complexity; they must learn to navigate it effectively.

## The Dynamic Safety Model

In the Dynamic Safety Model, when the Safety property is unseen, engineers drift toward the Economic and Workload properties, which are more visible (Rasmussen, 1997).

Once an engineer has an intuition for a boundary, they will implicitly change their behavior to optimize away from it (Rasmussen, 1997). They will give themselves a margin to prevent that rubber band from snapping. Chaos Engineering makes the Safety boundary visible through experimentation, providing engineers with the intuition needed to maintain appropriate safety margins.

## Optimizing for Reversibility

Nygard's concept of optimizing for reversibility maps naturally to Chaos Engineering (Nygard, 2018). Chaos experiments expose properties of a system that are counterproductive to reversibility.

Efficiency is a valid business goal, but highly optimized systems lack the slack to absorb unexpected conditions. Chaos experiments surface this brittleness, enabling the rest of the system to improvise when a piece breaks (Nygard, 2018).

## What Chaos Engineering Is

Chaos Engineering is the discipline of experimenting on a system to build confidence in the system's capability to withstand turbulent conditions in production (Principles of Chaos Engineering, 2016).

The process follows a scientific method (Basiri et al., 2016):

1. **Define the "steady state"** as measurable output indicating normal behavior
2. **Hypothesize** that this steady state will continue in both control and experimental groups
3. **Introduce variables** that reflect real-world events like server crashes, network partitions, or resource exhaustion
4. **Try to disprove the hypothesis** by looking for differences in steady state

Testing makes assertions based on existing knowledge. Experimentation creates new knowledge, either building confidence or revealing new properties about our system. As Gary Klein observes, "Performance Improvement = Errors ↓ + Insights ↑" (Klein, 2013).

Chaos Engineering is strongly biased toward Verification over Validation (Rosenthal et al., 2020). In the chaos engineering context, which differs from standard software engineering usage, Verification analyzes output at system boundaries, while Validation analyzes internal parts and builds mental models. For complex distributed systems, verification proves more practical than validation.

## What Chaos Engineering Is Not

**Chaos Engineering is not "breaking stuff."** It focuses solely on proactively improving safety in complex systems (Rosenthal et al., 2020).

**Chaos Engineering is not "antifragility."** Nassim Taleb's "antifragile" refers to systems that get stronger when exposed to random stress (Taleb, 2012). Chaos Engineering educates human operators about chaos already inherent in the system. Antifragility adds chaos hoping the system will grow stronger in response.

Resilience Engineering shows that hunting for what goes right is more informative than what goes wrong, and that redundancy can contribute to safety failures by adding complexity (Hollnagel, 2014).

## Core Principles of Chaos Engineering

**Build a Hypothesis Around Steady State Behavior:** Focus on business metrics like throughput, error rates, or latency percentiles that matter to users (Principles of Chaos Engineering, 2016).

**Vary Real-World Events:** Introduce failures reflecting actual production conditions: server crashes, network partitions, resource exhaustion, dependency failures (Basiri et al., 2016).

**Run Experiments in Production:** Only production contains the true complexity, scale, and interactions that matter (Principles of Chaos Engineering, 2016).

**Minimize Blast Radius:** Start with experiments affecting a small percentage of traffic. Gradually expand scope as confidence grows (Rosenthal et al., 2020).

**Automate Experiments to Run Continuously:** Like continuous integration catches code regressions, continuous chaos engineering catches resilience regressions as systems evolve (Basiri et al., 2016).

## Game Days: Practicing for Incidents

As Jesse Robbins observed, you don't choose the moment, the moment chooses you. You do choose how prepared you are when it does (Robbins, 2011).

Game Days are scheduled exercises where teams deliberately cause failures to practice incident response, build muscle memory, reveal gaps in runbooks, and improve team coordination (Robbins, 2011).

Key insights:
- **99.9%³ = 99.7%**: Three nines across three dependencies yields only two nines overall
- **MTTR > MTBF**: Mean Time To Recovery matters more than Mean Time Between Failures
- **Latent defects exist**: You choose when you discover them and how much they cost you

## For Senior Technical Individual Contributors: Leading Chaos Engineering

Senior technical ICs play a crucial role in establishing and scaling Chaos Engineering practices (Rosenthal et al., 2020).

**Champion the discipline** by demonstrating value through small, successful experiments in low-risk scenarios.

**Design for chaos** from the beginning. Build systems with explicit failure modes, comprehensive observability, and graceful degradation (Nygard, 2018).

**Establish safety mechanisms** including automatic abort conditions, blast radius limits, and rollback procedures.

**Mentor teams** in hypothesis formation, experiment design, and scientific thinking about system behavior.

**Build tooling and automation** that makes chaos experiments accessible to all engineers while maintaining appropriate safeguards (Basiri et al., 2016).

**Document learnings** from experiments to improve organizational knowledge, connecting findings to architectural decisions and operational procedures.

## Practical Implementation

**Start Small:** Begin with Failure Injection Testing on non-critical services (Rosenthal et al., 2020). Terminate instances, introduce latency, or exhaust resources.

**Establish Steady State Metrics:** Define "normal" using business metrics: order completion rate, search success rate, video start time (Principles of Chaos Engineering, 2016).

**Create Hypotheses:** "We believe that terminating 10% of our API servers will not impact order completion rate because our load balancers will route traffic to healthy instances within 30 seconds."

**Run Controlled Experiments:** Execute during business hours when teams can respond (Rosenthal et al., 2020). Start with small blast radius. Monitor closely. Abort if steady state degrades beyond acceptable thresholds.

**Learn and Iterate:** Document findings, address weaknesses, expand scope, and automate successful experiments (Basiri et al., 2016).

## Put It Into Practice

Chaos Engineering shifts organizations from reactive incident response to proactive resilience building, discovering systemic weaknesses before they cause outages (Rosenthal et al., 2020).

The discipline requires cultural change: teams must embrace experimentation and accept that failures will occur, while leadership supports time invested in activities that don't directly deliver features.

Organizations that adopt Chaos Engineering build more resilient systems, respond to incidents more effectively, and operate with greater confidence (Basiri et al., 2016). They transform the question from "will our system fail?" to "when our system fails, how will it behave, and are we prepared?"

Start small. Run your first experiment this week. Discover one weakness. Fix it. Repeat. Over time, these small experiments compound into significantly more resilient systems.

## References

Basiri, A., Behnam, N., de Rooij, R., Hochstein, L., Kosewski, L., Reynolds, J., & Rosenthal, C. (2016). [Chaos engineering](https://doi.org/10.1109/MS.2016.60). *IEEE Software*, 33(3), 35-41.

Brooks, F. P. (1986). [No Silver Bullet: Essence and Accidents of Software Engineering](https://www.cs.unc.edu/techreports/86-020.pdf). *IEEE Computer*, 20(4), 10-19.

Hollnagel, E. (2014). [*Safety-I and Safety-II: The Past and Future of Safety Management*](https://erikhollnagel.com/books/safety-i-safety-ii-2014). Ashgate Publishing.

Klein, G. (2013). [*Seeing What Others Don't: The Remarkable Ways We Gain Insights*](https://www.hachettebookgroup.com/titles/gary-klein/seeing-what-others-dont/9781610392754/?lens=publicaffairs). PublicAffairs.

Nygard, M. T. (2018). [*Release It! Design and Deploy Production-Ready Software*](https://pragprog.com/titles/mnee2/release-it-second-edition/) (2nd ed.). Pragmatic Bookshelf.

Perrow, C. (1984). [*Normal Accidents: Living with High-Risk Technologies*](https://press.princeton.edu/books/paperback/9780691004129/normal-accidents). Basic Books.

[Principles of Chaos Engineering](https://principlesofchaos.org/). (2016).

Rasmussen, J. (1997). [Risk management in a dynamic society: A modelling problem](https://doi.org/10.1016/S0925-7535(97)00052-0). *Safety Science*, 27(2-3), 183-213.

Robbins, J. (2011). [GameDay: Creating Resiliency Through Destruction](https://www.usenix.org/blog/gameday-creating-resiliency-through-destruction). *USENIX LISA11*.

Rosenthal, C., Hochstein, L., Blohm, A., Jones, N., & Basiri, A. (2020). [*Chaos Engineering: System Resiliency in Practice*](https://www.oreilly.com/library/view/chaos-engineering/9781492043850/). O'Reilly Media.

Taleb, N. N. (2012). [*Antifragile: Things That Gain from Disorder*](https://www.penguinrandomhouse.com/books/176227/antifragile-by-nassim-nicholas-taleb/). Random House.

## Changelog

**2026-05-01** Full rewrite. Restructured from rough notes into a complete article with sections on the Dynamic Safety Model, Optimizing for Reversibility, Core Principles, Game Days, and Practical Implementation. Added formal academic citations throughout; added Brooks (1986) for accidental/essential complexity; corrected Robbins citation venue from Velocity Conference to USENIX LISA11.
