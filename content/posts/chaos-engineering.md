---
title: "Chaos Engineering"
date: 2020-06-25
publishdate: 2020-06-25
lastmod: 2026-06-14
summary: "Chaos Engineering builds confidence in distributed systems by deliberately introducing failures before they occur on their own. The discipline shifts teams from reactive incident response to proactive resilience building."
tags: ["chaos", "learning", "incidents"]
image: /images/chaos-engineering.jpg
draft: false
---

![](/images/chaos-engineering.jpg)
*"Chaos Monkey" by BUNKA Artoyz. Photo: BFLV (2008). CC BY-NC 2.0.*

## Chaos Engineering

Modern distributed systems fail in ways we cannot predict.

Production incidents reveal unexpected interactions between services, infrastructure, and operators. Chaos Engineering takes a different approach, deliberately introducing controlled failures to discover systemic weaknesses before they cause customer-impacting outages.

## Why Chaos Engineering Matters

In distributed systems, the failure of a component your team never directly touches can cascade into failures your users experience. Multiple and unexpected interactions of failures are inevitable in complex systems (Perrow, 1999).

Complexity sorts into two kinds, accidental and essential (Brooks, 1986). Accidental complexity accrues continuously, while essential complexity grows with the scope of what the system must do. Organizations can't eliminate complexity, only learn to navigate it.

## The Dynamic Safety Model

Rasmussen's Dynamic Safety Model describes three competing pressures on engineering work (Rasmussen, 1997). The Safety boundary marks where failures become likely, the Economic boundary where work becomes unaffordable, and the Workload boundary where teams become overwhelmed.

When the Safety boundary is not visible, engineers drift toward the other two, optimizing for cost and speed without knowing how close they are to failure. Chaos Engineering makes the Safety boundary visible through experimentation, giving teams the intuition to maintain appropriate margins.

## Optimizing for Reversibility

Nygard's concept of optimizing for reversibility maps naturally to Chaos Engineering (Nygard, 2018). Chaos experiments expose properties that work against reversibility.

Efficiency is a valid business goal, but highly optimized systems lack the slack to absorb unexpected conditions. Chaos experiments surface this brittleness, enabling the rest of the system to adapt when a piece breaks (Nygard, 2018).

## What Chaos Engineering Is

Chaos Engineering is the discipline of experimenting on a system to build confidence in the system's capability to withstand turbulent conditions in production (Principles of Chaos Engineering, 2016).

The process follows a scientific method (Basiri et al., 2016):

1. **Define the steady state** as measurable output indicating normal behavior
2. **Hypothesize** that this steady state will continue in both control and experimental groups
3. **Introduce variables** that reflect real-world events like server crashes, network partitions, or resource exhaustion
4. **Try to disprove the hypothesis** by looking for differences in steady state

Testing makes assertions based on existing knowledge. Experimentation creates new knowledge, either building confidence or revealing new properties about the system. Fewer errors, more insights (Klein, 2013).

Chaos Engineering favors observing behavior at system boundaries over inspecting internal components (Rosenthal et al., 2020). Checking whether the system serves users correctly under failure tells you more than checking whether individual parts behave as designed.

## What Chaos Engineering Is Not

**Chaos Engineering is not "breaking stuff."** It focuses solely on proactively improving safety in complex systems (Rosenthal et al., 2020).

**Chaos Engineering is not "antifragility."** Antifragile systems get stronger when exposed to random stress (Taleb, 2012). Chaos Engineering educates operators about chaos already inherent in the system. Antifragility adds chaos hoping the system will grow stronger in response.

Hunting for what goes right is more informative than hunting for what goes wrong, and redundancy can contribute to safety failures by adding complexity (Hollnagel, 2014).

## Run Experiments

**Define steady state and form a hypothesis.** Define normal behavior using business metrics such as order completion rate, search success rate, and video start time (Principles of Chaos Engineering, 2016). Then form a specific prediction: "We believe that terminating 10% of our API servers will not impact order completion rate because our load balancers will route traffic to healthy instances within 30 seconds."

**Vary real-world events.** Introduce failures that reflect actual production conditions. Server crashes, network partitions, resource exhaustion, and dependency failures are all valid variables (Basiri et al., 2016).

**Run in production.** Only production contains the true complexity, scale, and interactions that matter (Principles of Chaos Engineering, 2016).

**Start small.** Begin with failure injection testing on non-critical services (Rosenthal et al., 2020). Target a small percentage of traffic and expand scope as confidence grows.

**Monitor and abort if needed.** Execute during business hours when teams can respond. Abort if steady state degrades beyond acceptable thresholds (Rosenthal et al., 2020).

**Learn, iterate, and automate.** Document findings, address weaknesses, expand scope, and automate successful experiments. Like continuous integration catches code regressions, continuous chaos engineering catches resilience regressions as systems evolve (Basiri et al., 2016).

## Game Days

You don't choose the moment, the moment chooses you. You do choose how prepared you are when it does (Robbins, 2011).

Game Days are scheduled exercises where teams deliberately cause failures to practice incident response, build muscle memory, reveal gaps in runbooks, and improve team coordination (Robbins, 2011).

Three useful benchmarks:

- **99.9%³ = 99.7%.** Three nines across three dependencies yields only two nines overall.
- **MTTR over MTBF.** Mean Time To Recovery matters more than Mean Time Between Failures.
- **Latent defects exist.** You choose when you discover them and how much they cost you.

## Put It Into Practice

Chaos Engineering shifts organizations from reactive incident response to proactive resilience building, discovering systemic weaknesses before they cause outages (Rosenthal et al., 2020).

The discipline requires cultural change. Teams must embrace experimentation and accept that failures will occur, while leadership supports time invested in activities that don't directly deliver features (Rosenthal et al., 2020).

Organizations that adopt Chaos Engineering build more resilient systems, respond to incidents more effectively, and operate with greater confidence (Basiri et al., 2016). They transform the question from "will our system fail?" to "when our system fails, how will it behave, and are we prepared?"

Start small. Run your first experiment this week, discover one weakness, fix it, and repeat.

---

## References

Basiri, Ali, et al. (2016). "Chaos Engineering." *IEEE Software*, 33(3): 35-41. [https://doi.org/10.1109/MS.2016.60](https://doi.org/10.1109/MS.2016.60)

BFLV (2008). "Expos CHAOS MONKEY by BUNKA Artoyz." Flickr. CC BY-NC 2.0. [https://www.flickr.com/photos/bflv/2849375040/](https://www.flickr.com/photos/bflv/2849375040/)

Brooks, Frederick P. (1986). "No Silver Bullet: Essence and Accidents of Software Engineering." *IEEE Computer*, 20(4): 10-19. [https://www.cs.unc.edu/techreports/86-020.pdf](https://www.cs.unc.edu/techreports/86-020.pdf)

Hollnagel, Erik (2014). *Safety-I and Safety-II: The Past and Future of Safety Management*. Farnham: Ashgate. [https://erikhollnagel.com/books/safety-i-safety-ii-2014](https://erikhollnagel.com/books/safety-i-safety-ii-2014)

Klein, Gary (2013). *Seeing What Others Don't: The Remarkable Ways We Gain Insights*. New York: PublicAffairs. [https://www.hachettebookgroup.com/titles/gary-klein/seeing-what-others-dont/9781610392754/](https://www.hachettebookgroup.com/titles/gary-klein/seeing-what-others-dont/9781610392754/)

Nygard, Michael T. (2018). *Release It! Design and Deploy Production-Ready Software*, 2nd ed. Raleigh: Pragmatic Bookshelf. [https://pragprog.com/titles/mnee2/release-it-second-edition/](https://pragprog.com/titles/mnee2/release-it-second-edition/)

Perrow, Charles (1999). *Normal Accidents: Living with High-Risk Technologies*. Princeton: Princeton University Press. [https://press.princeton.edu/books/paperback/9780691004129/normal-accidents](https://press.princeton.edu/books/paperback/9780691004129/normal-accidents)

Principles of Chaos Engineering (2016). [https://principlesofchaos.org/](https://principlesofchaos.org/)

Rasmussen, Jens (1997). "Risk Management in a Dynamic Society: A Modelling Problem." *Safety Science*, 27(2-3): 183-213. [https://doi.org/10.1016/S0925-7535(97)00052-0](https://doi.org/10.1016/S0925-7535(97)00052-0)

Robbins, Jesse (2011). "GameDay: Creating Resiliency Through Destruction." *USENIX LISA11*. [https://www.usenix.org/blog/gameday-creating-resiliency-through-destruction](https://www.usenix.org/blog/gameday-creating-resiliency-through-destruction)

Rosenthal, Casey, et al. (2020). *Chaos Engineering: System Resiliency in Practice*. Sebastopol: O'Reilly Media. [https://www.oreilly.com/library/view/chaos-engineering/9781492043850/](https://www.oreilly.com/library/view/chaos-engineering/9781492043850/)

Taleb, Nassim Nicholas (2012). *Antifragile: Things That Gain from Disorder*. New York: Random House. [https://www.penguinrandomhouse.com/books/176227/antifragile-by-nassim-nicholas-taleb/](https://www.penguinrandomhouse.com/books/176227/antifragile-by-nassim-nicholas-taleb/)

---

## Changelog

**2026-06-07** Added hero image.  
**2026-05-17** Removed IC section. Simplified Dynamic Safety Model.  
**2026-05-01** Restructured from rough notes. Corrected Robbins citation venue from Velocity to USENIX LISA11.  
**2020-06-25** Initial publication.
