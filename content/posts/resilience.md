---
title: "Resilience"
date: 2019-10-30
publishdate: 2019-10-30
lastmod: 2026-06-14
summary: "Your system passed every test. Redundancy was in place. Then something unexpected happened anyway. Resilience Engineering is the framework for building systems that hold up when the unexpected is inevitable."
tags: ["resilience", "safety", "capacity"]
image: /images/resilience.jpg
draft: false
---

![](/images/resilience.jpg)
*"Windswept Hawthorn tree in Ireland." Photo: Eoin Gardiner (2008). CC BY 2.0.*

## Resilience

At some point, most engineers encounter an incident that shouldn't have been possible. Everything looked fine. Circuit breakers were in place. Retries were configured. The system failed anyway.

That's the failure mode resilience engineering was built to understand.

## Resilience vs Robustness

Robustness is the capacity to handle known failure modes. Resilience is the capacity to handle failures that weren't foreseeable by the designer. Robustness is designed in advance. Resilience is something a system does when conditions change (Allspaw, 2012).

The distinction matters because complex systems regularly produce failures that fall outside what their designers imagined.

## Why Testing Isn't Enough

Rigorous testing, redundancy, and defensive programming all rest on the assumption that every failure mode can be anticipated and prevented. For complex distributed systems, that assumption breaks down (Hollnagel, 2011).

Complexity and emergent behavior mean that distributed systems create interactions that can't be predicted from individual components. No amount of testing can explore all possible system states (Cook, 2000).

Cascading failures occur when localized problems propagate through system dependencies. A slow database query triggers connection pool exhaustion, causing application timeouts, leading to retry storms that overwhelm the database further.

Human-computer interaction under pressure means operators are making decisions with incomplete information during the exact moments when systems are most likely to fail them (Cook, 2000). Experienced operators improvising around system brittleness is what keeps complex systems running. Human expertise is the adaptive capacity.

Resilience Engineering grew from studying safety-critical industries like aviation, healthcare, and nuclear power. Pioneered by Hollnagel, Cook, Dekker, and Woods, it shifts focus from preventing all failures to building systems that hold up when something unexpected happens.

## Core Principles

Four capabilities define a resilient system (Hollnagel, 2011):

**Anticipation:** knowing what to expect and preparing for potential disruptions. This extends beyond traditional risk assessment to include scenario planning, load testing, and chaos engineering.

**Monitoring:** knowing what to look for in system behavior. Effective monitoring observes leading indicators of system stress, not just lagging indicators of failure, using observability that reveals how systems actually behave under varying conditions.

**Response:** knowing what to do when anomalies occur. Response capability depends on technical mechanisms like circuit breakers and organizational practices like incident command structures.

**Learning:** knowing what has happened and understanding why. This goes beyond documenting incidents to understanding how system behavior, human decisions, and organizational factors shaped the outcome.


## Resilience and Safety

Safety-I focuses on eliminating failure causes. Safety-II focuses on understanding successful adaptations, grounded in the observation that things go wrong and things go right for the same basic reasons (Hollnagel, 2014). Traditional software reliability is Safety-I thinking. You learn more about how a system actually works by studying the thousands of times people adapted successfully than by studying the handful of times it failed.

## Your Role in Building Resilience

Your design decisions shape how much adaptive capacity a system has.

**Design for graceful degradation.** Circuit breakers, bulkheads, and timeouts are the implementation (Nygard, 2018). Think beyond happy paths. What happens when a dependency is slow, unavailable, or returning corrupt data? Build mechanisms that respond through fallbacks, cached data, or reduced functionality.

**Build observability in from the start.** Good instrumentation reveals how the system behaved in the lead-up to a failure, not just that it failed (Beyer et al., 2016). Instrument for the operator who will be debugging this system at 3 AM, not just for normal operations.

**Participate in incident response.** Volunteer for on-call. Get involved in incidents outside your area. Incident commanders and escalation paths provide structure under pressure (Beyer et al., 2016). Game days make that structure practiced rather than improvised.

**Invest in learning.** Blameless postmortems surface how people adapted and held things together under pressure. That's the richest source of information about how a system actually works. Honest accounts only come when people aren't afraid to report them (Allspaw, 2012).

**Balance velocity with resilience.** Not every system needs chaos engineering or elaborate circuit breakers (Basiri et al., 2016). Assess risk based on blast radius, recovery time objectives, and business impact.

**Protect the conditions for human resilience.** Psychological safety, shared mental models, and the freedom to speak up about risk are what allow adaptive capacity to surface. An experienced operator who won't flag a problem because of how it will be received is a system with a silenced warning light.

The work doesn't stop at the code. Model resilience thinking in design reviews, ask questions about failure modes, and share stories of how systems failed.

## How Teams Get This Wrong

Resilience is more than just redundancy. Adding more servers, databases, or availability zones improves fault tolerance but doesn't guarantee resilience. Resilient systems adapt to unexpected conditions; they don't just tolerate anticipated failures (Woods, 2015).

Over-engineering is not adaptive capacity. Resilience doesn't require elaborate mechanisms for every possible failure. Sometimes the most resilient solution is simple, well-understood, and easy to reason about during an incident.

Root causes are rarely singular. Complex system failures result from combinations of conditions, decisions, and circumstances (Dekker, 2006). Searching for a single cause impedes learning.

Metrics can hide system health. Aggregate metrics like average response time may look healthy while a subset of users experiences severe degradation. Percentile metrics, error budgets, and user-centric measurements give better visibility into what the system is actually doing (Beyer et al., 2016).

## Put It Into Practice

Pick one dependency your system relies on and ask what happens when it's slow, unavailable, or returning corrupt data. If the answer isn't "we degrade gracefully and we'd know within minutes," that's the work. Build the observability, add the circuit breaker, write the runbook, and run the game day.

Then talk to the people who operate this system under pressure. What do they know that isn't in any runbook? What are they quietly working around? Learning how people cope with surprise is the path to finding sources of resilience.

Where is your system weakest? Where would your team struggle most, and what do they already know about it that you don't? Ask those questions in design reviews and postmortems. Resilience won't emerge from a single project or sprint. It's built by the people who ask consistently, and who create the conditions for honest answers.

## References

Allspaw, John (2012). "Blameless PostMortems and a Just Culture." *Etsy Code as Craft Blog*. [https://web.archive.org/web/20230601000000/https://codeascraft.com/2012/05/22/blameless-postmortems/](https://web.archive.org/web/20230601000000/https://codeascraft.com/2012/05/22/blameless-postmortems/)

Basiri, Ali, et al. (2016). "Chaos Engineering." *IEEE Software*, 33(3): 35-41. [https://arxiv.org/abs/1702.05843](https://arxiv.org/abs/1702.05843)

Beyer, Betsy, Chris Jones, Jennifer Petoff, and Niall Richard Murphy (2016). *Site Reliability Engineering: How Google Runs Production Systems*. O'Reilly Media. [https://sre.google/sre-book/](https://sre.google/sre-book/)

Cook, Richard I. (2000). "How Complex Systems Fail." *Cognitive Technologies Laboratory, University of Chicago*. [https://how.complexsystems.fail/](https://how.complexsystems.fail/)

Dekker, Sidney (2006). *The Field Guide to Understanding Human Error*. Ashgate Publishing. [https://sidneydekker.com/the-field-guide-to-understanding-human-error](https://sidneydekker.com/the-field-guide-to-understanding-human-error)

Gardiner, Eoin (2008). "Windswept tree in Ireland." Wikimedia Commons. CC BY 2.0. [https://commons.wikimedia.org/wiki/File:Windswept_tree_in_Ireland.jpg](https://commons.wikimedia.org/wiki/File:Windswept_tree_in_Ireland.jpg)

Hollnagel, Erik (2011). *Resilience Engineering in Practice: A Guidebook*. Ashgate Publishing. [https://erikhollnagel.com/books/resilience-engineering-in-practice.html](https://erikhollnagel.com/books/resilience-engineering-in-practice.html)

Hollnagel, Erik (2014). *Safety-I and Safety-II: The Past and Future of Safety Management*. Ashgate Publishing. [https://erikhollnagel.com/books/safety-i-safety-ii-2014](https://erikhollnagel.com/books/safety-i-safety-ii-2014)

Nygard, Michael T. (2018). *Release It! Design and Deploy Production-Ready Software*, 2nd ed. Pragmatic Bookshelf. [https://pragprog.com/titles/mnee2/release-it-second-edition/](https://pragprog.com/titles/mnee2/release-it-second-edition/)

Woods, David D. (2015). "Four Concepts for Resilience and the Implications for the Future of Resilience Engineering." *Reliability Engineering & System Safety*, 141: 5-9. [https://maritimesafetyinnovationlab.org/wp-content/uploads/2021/06/4sensesofresiliencepublic.pdf](https://maritimesafetyinnovationlab.org/wp-content/uploads/2021/06/4sensesofresiliencepublic.pdf)

## Changelog

**2026-06-07** Added hero image.  
**2026-05-01** Expanded from a notes-and-quotes format. Removed the Goldratt, Bergström, and Dekker quotes.  
**2019-10-30** Initial publication.
