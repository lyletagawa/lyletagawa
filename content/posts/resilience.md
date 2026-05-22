---
title: "Resilience"
date: 2019-10-30
publishdate: 2019-10-30
lastmod: 2026-05-01
summary: "Your system passed every test. Redundancy was in place. Then something unexpected happened anyway. Resilience Engineering is the framework for building systems that hold up when the unexpected is inevitable."
tags: ["resilience", "safety", "capacity"]
---

## Resilience

At some point, most engineers encounter an incident that shouldn't have been possible. Everything looked fine, until a dependency got slow rather than failing cleanly, a retry storm crossed a boundary nobody had mapped, and the system failed in a way nobody had anticipated.

That's the failure mode resilience engineering was built to understand.

## Resilience vs Robustness

**Robustness** is the capacity to handle known failure modes. **Resilience** is the capacity to handle failures that weren't foreseeable by the designer. Robustness deals with known unknowns; resilience deals with unknown unknowns.

The distinction matters because complex systems regularly produce failures that fall outside what their designers imagined.

## Why Software Systems Need Resilience Engineering

Rigorous testing, redundancy, and defensive programming all rest on the same assumption: we can anticipate and prevent every failure mode. For complex distributed systems, that assumption breaks down (Hollnagel, 2011).

**Complexity and emergent behavior** mean that distributed systems create interaction effects that can't be predicted from individual components. No amount of testing can explore all possible system states (Cook, 2000).

**Cascading failures** occur when localized problems propagate through system dependencies. A slow database query triggers connection pool exhaustion, causing application timeouts, leading to retry storms that overwhelm the database further.

**Human-computer interaction under pressure** means operators are making decisions with incomplete information during the exact moments when systems are most likely to fail them (Cook, 2000). Experienced operators improvising around system brittleness is not a workaround. It is what keeps complex systems running. Human expertise is the adaptive capacity.

**The myth of root cause** persists despite evidence that complex system failures rarely have single causes (Dekker, 2006). Incidents result from combinations of conditions, decisions, and circumstances; searching for a single root cause impedes learning.

Resilience Engineering grew from studying high-stakes industries like aviation, healthcare, and nuclear power. Pioneered by Hollnagel, Cook, Dekker, and Woods, it shifts focus from preventing all failures to building systems that hold up when something unexpected happens.

## Core Principles

Hollnagel identifies four capabilities that define a resilient system (2011):

**Anticipation:** knowing what to expect and preparing for potential disruptions. This extends beyond traditional risk assessment to include scenario planning, load testing, and chaos engineering.

**Monitoring:** knowing what to look for in system behavior. Effective monitoring observes leading indicators of system stress, not just lagging indicators of failure, using observability that reveals how systems actually behave under varying conditions.

**Response:** knowing what to do when anomalies occur. Response capability depends on technical mechanisms like circuit breakers and organizational practices like incident command structures.

**Learning:** knowing what has happened and understanding why. This goes beyond documenting incidents to understanding how system behavior, human decisions, and organizational factors shaped the outcome.

> Resilience is something a system does, not what a system has; creating and sustaining "adaptive capacity" within an organisation (while being unable to justify doing it
specifically) is resilient action; and learning about how people cope with surprise is the path to finding sources of resilience.
>
> John Allspaw ([Amplifying Sources of Resilience](https://qconlondon.com/london2019/presentation/amplifying-sources-resilience-what-research-says))

## Resilience and Safety

> When we think of safety it is usually by reference to its opposite, the absence of safety. The traditional view of safety, called Safety-I, has consequently been defined by the absence of accidents and incidents, or as the 'freedom from unacceptable risk.' As a result, the focus of safety research and safety management has usually been on unsafe system operation rather than on safe operation. In contrast to the traditional view, resilience engineering maintains that 'things go wrong' and 'things go right' for the same basic reasons. This corresponds to a view of safety, called Safety-II, which defines safety as the ability to succeed under varying conditions. The understanding of everyday functioning is therefore a necessary prerequisite for the understanding of the safety performance of an organisation.
>
> Erik Hollnagel ([Safety I and Safety II](https://erikhollnagel.com/ideas/safety-i%20and%20safety-ii.html))

Safety-I focuses on eliminating failure causes; Safety-II focuses on understanding and amplifying successful adaptations. Traditional software reliability is Safety-I thinking. Resilience Engineering insists on both, grounded in the observation that complex systems succeed because of continuous human adaptation, not despite it (Hollnagel, 2014). The implication: you learn more about how a system actually works by studying the thousands of times people adapted successfully than by studying the handful of times it failed.

## Practical Applications

These capabilities aren't abstract. Each one maps to a recognizable software engineering practice.

**Chaos engineering** operationalizes anticipation by deliberately injecting failures into production systems to verify they respond as expected. Netflix's Chaos Monkey, which randomly terminates instances, is the well-known example (Basiri et al., 2016).

> Vacations are planned outages
>
> – Dave Rensin ([Wheel of Staycation](https://www.gremlin.com/blog/dave-rensin-chaos-engineering-for-people-systems-chaos-conf-2019/))

**Observability** supports monitoring and anticipation. Good instrumentation reveals not just that something failed but how the system behaved in the lead-up (Beyer et al., 2016).

**Incident response** is the response capability made operational. Site Reliability Engineering practices like incident commanders and well-defined escalation paths provide structure during high-pressure situations (Beyer et al., 2016).

**Blameless post-mortems** surface how people adapted, improvised, and held things together under pressure. That knowledge is the richest source of information about how a system actually works. Allspaw's insight: you only get honest accounts if people aren't afraid to report them (Allspaw, 2012).

**Game days and disaster recovery testing** make incident response practiced rather than improvised. Teams that run realistic scenarios regularly respond better under pressure.

**Resilience patterns** like circuit breakers, bulkheads, and timeouts are how graceful degradation gets implemented. Nygard's *Release It!* catalogs them (Nygard, 2018).

## Your Role in Building Resilience

Your design decisions shape how much adaptive capacity a system has.

**Design for graceful degradation.** Think beyond happy paths. What happens when a dependency is slow, unavailable, or returning corrupt data? Build mechanisms that detect these conditions and respond through fallbacks, cached data, or reduced functionality.

**Build observability in from the start.** Instrument code to reveal system behavior for operators responding to incidents, not just during normal operations. What information would help someone understand this system at 3 AM during an outage?

**Participate actively in incident response.** Volunteer for on-call. Get involved in incidents outside your area. Observe how systems fail and how teams respond.

**Contribute to a learning culture.** Write post-mortems, share learnings, and push for systemic improvements over individual blame. Ask what system conditions made an outcome possible, not who caused it.

**Balance velocity with resilience investment.** Not every system needs chaos engineering or elaborate circuit breakers. Assess risk based on blast radius, recovery time objectives, and business impact.

**Protect the conditions for human resilience.** Psychological safety, shared mental models, and the freedom to speak up about risk are what allow adaptive capacity to surface. An experienced operator who won't flag a problem because of how it will be received is a system with a silenced warning light.

The work doesn't stop at the code. Model resilience thinking in design reviews, ask questions about failure modes, and share stories of how systems failed. That influence often has more leverage than any individual technical contribution.

## Common Misconceptions

**Resilience is not just redundancy.** Adding more servers, databases, or availability zones improves fault tolerance but doesn't guarantee resilience. Resilient systems adapt to unexpected conditions; they don't just tolerate anticipated failures (Woods, 2015).

**Over-engineering is not adaptive capacity.** Resilience doesn't require elaborate mechanisms for every possible failure. Sometimes the most resilient solution is simple, well-understood, and easy to reason about during an incident.

**Metrics can hide system health.** Aggregate metrics like average response time may look healthy while a subset of users experiences severe degradation. Percentile metrics, error budgets, and user-centric measurements give better visibility into what the system is actually doing (Beyer et al., 2016).

## Put It Into Practice

Pick one dependency your system relies on and ask what happens when it's slow, unavailable, or returning corrupt data. If the answer isn't "we degrade gracefully and we'd know within minutes," that's the work. Build the observability. Add the circuit breaker. Write the runbook. Run the game day.

Then talk to the people who operate this system under pressure. What do they know that isn't in any runbook? What are they quietly working around? That knowledge is where the real adaptive capacity lives.

The four capabilities (anticipation, monitoring, response, learning) make a useful regular check. Where is your system weakest? Where would your team struggle most, and what do they already know about it that you don't? Ask those questions in design reviews and post-mortems.

Resilience won't emerge from a single project or sprint. It's built by the people who ask those questions consistently, and who create the conditions for honest answers.

## References

Allspaw, J. (2012). [Blameless PostMortems and a Just Culture](https://web.archive.org/web/20230601000000/https://codeascraft.com/2012/05/22/blameless-postmortems/). *Etsy Code as Craft Blog*.

Basiri, A., Behnam, N., de Rooij, R., Hochstein, L., Kosewski, L., Reynolds, J., & Rosenthal, C. (2016). [Chaos engineering](https://arxiv.org/abs/1702.05843). *IEEE Software*, 33(3), 35-41.

Beyer, B., Jones, C., Petoff, J., & Murphy, N. R. (2016). [*Site Reliability Engineering: How Google Runs Production Systems*](https://sre.google/sre-book/). O'Reilly Media.

Cook, R. I. (2000). [How complex systems fail](https://how.complexsystems.fail/). *Cognitive Technologies Laboratory, University of Chicago*.

Dekker, S. (2006). [*The Field Guide to Understanding Human Error*](https://sidneydekker.com/the-field-guide-to-understanding-human-error). Ashgate Publishing.

Hollnagel, E. (2011). [*Resilience Engineering in Practice: A Guidebook*](https://erikhollnagel.com/books/resilience-engineering-in-practice.html). Ashgate Publishing.

Hollnagel, E. (2014). [*Safety-I and Safety-II: The Past and Future of Safety Management*](https://erikhollnagel.com/books/safety-i-safety-ii-2014). Ashgate Publishing.

Nygard, M. T. (2018). [*Release It! Design and Deploy Production-Ready Software*](https://pragprog.com/titles/mnee2/release-it-second-edition/) (2nd ed.). Pragmatic Bookshelf.

Woods, D. D. (2015). [Four concepts for resilience and the implications for the future of resilience engineering](https://maritimesafetyinnovationlab.org/wp-content/uploads/2021/06/4sensesofresiliencepublic.pdf). *Reliability Engineering & System Safety*, 141, 5-9.

## Changelog

**2026-05-01:** Expanded from a notes-and-quotes format. Removed the Goldratt, Bergström, and Dekker "Safety Differently" quotes.
**2019-10-30:** Initial publication.
