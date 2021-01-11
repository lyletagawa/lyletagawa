---
title: "Chaos Engineering"
date: 2020-06-25
summary: "Breaking things on purpose. Chaos Engineering is the facilitation of experiments to uncover systemic weakness. Chaos Engineering is the discipline of experimenting on a system in order to build confidence in the system’s capability to withstand turbulent conditions in production. Performance Improvement = Errors ↓ + Insights ↑"
tags: ["chaos", "learning", "incidents"]
---

## Chaos Engineering

Chaos Engineering was deliberately created as a proactive discipline to understand and navigate complex systems.

Chaos Engineering is the facilitation of experiements to uncover systemic weaknesses.

## Why?

Simple systems are often described as linear. A change to the input of a linear system produces a corresponding change to the output of the system.

Chaos Engineering specifically addresses the needs of operating a complex system, namely that these systems are nonlinear, which makes them unpredictable, and in turn leads to undesirable outcomes.

Complexity can be sorted into two buckets: accidental and essential.

Accidental complexity is always accruing. In order to make progress, essential complexity will increase.

In the Dynamic Safety Model, when the Safety property is unseen, engineers tend to drift away from it, and towards the Economic and Workload properties, which are more visible.

The Dynamic Safety Model tells us that once an engineer has an intuition for a boundary (e.g. Economic, Workload, Safety), they will implicitly change their behavior to optimize away from it. They will give themselves a margin to prevent that rubber band from snapping. No one has to tell them that an incident is to be avioded, or remind them to be careful, or encourage them to adopt best practices. They will implicitly modify their behavior in a way that leads to a more resilient system.

Optimizing for Reversibility is a foundational model for Chaos Engineering. The experiements in Chaos Engineering expose properties of a system that are counterproductive to Reversibility.

Efficiency can be a business goal, but efficiency is also brittle. Chaos experiments can surface brittleness, and can be optimized for Reversibility, so that the rest of the system can improvise and make different decisions if that brittle piece breaks.

## What Chaos Engineering Is

1. Define the "steady state" as some measurable output of a system that indicates normal behavior
1. Hypothesize that this steady state will continue in both the control group and the experimental group
1. Introduce variables that reflect real-world events like servers that crash, hard drives that malfunction, network connections that are severed, etc
1. Try to disprove the hypothesis by looking for a difference in steady state between the control group and the experiemental group

*Testing*, strictly speaking, does not create new knowledge. Tests make an assertion, based on existing knowledge. Tests are statements about known properties of the system.

*Experiementation*, on the other hand, creates new knowledge. Experimentation either builds confidence, or it teaches us new properties about our own system.

Chaos Engineering is strongly biased toward Verification over Validation. Chaos Engineering cares *whether* something works, not *how*.

*Verification* of a complex system is a process of analyzing output at a system boundary.

*Validation* of a complex system is a process of analyzing the parts of the system and building mental models that reflect the interaction of those parts.

## What Chaos Engineering Is Not

Chaos Engineering is not "breaking stuff." "Fixing stuff in production" does a much better job of capturing the value of Chaos Engineering. Chaos Engineering is the only major discipline in software engineering that focuses solely on *proactively* improving safety in complex systems.

Chaos Engineering is not "antifragility." Nassim Taleb invented the word "antifragile" as a way to refer to systems that get stronger when exposed to random stress. Chaos Engineering educates human operators about the chaos already inherent in the system, so that they can be a more resilient team. Antifragility, by contrast, adds chaos to a system in hopes that it will grow stronger in response.

Antifragility proposes that the first step in improving a system's robustness is to hunt for weaknesses and remove them. Resilience Engineering tells us that hunting for what goes *right* in safety is much more informative than what goes *wrong*.

Antifragility proposes that the second step is to add redundancy. Resilience Engineering tells us that redundancy actually contributes to safety failures.


## Chaos Engineering

> Chaos Engineering is the discipline of experimenting on a system in order to build confidence in the system’s capability to withstand turbulent conditions in production.
>
> -- <https://principlesofchaos.org/>

Chaos Engineering is a method of experimentation on infrastructure that brings systemic weaknesses to light. This empirical process of verification leads to more resilient systems, and builds confidence in the operational behavior of those systems.

## Why?

> A distributed system is one in which the failure of a computer you didn't even know existed can render your own computer unusable.
>
> -- [Leslie Lamport](https://www.microsoft.com/en-us/research/uploads/prod/2016/12/Distribution.pdf)

> multiple & unexpected interactions of failures are inevitable
>
> -- [Charles Perrow](https://en.wikipedia.org/wiki/Normal_Accidents)

>Chaos Engineering is an approach for learning about how your system behaves by applying a discipline of empirical exploration. Just as scientists conduct experiments to study physical and social phenomena, Chaos Engineering uses experiments to learn about a particular system.
>
>Applying Chaos Engineering improves the resilience of a system. By designing and executing Chaos Engineering experiments, you will learn about weaknesses in your system that could potentially lead to outages that cause customer harm. You can then address those weaknesses proactively, going beyond the reactive processes that currently dominate most incident response models.
>
> -- [Chaos Engineering (O'Reilly)](https://www.oreilly.com/library/view/chaos-engineering/9781491988459/)

> Even when all of the individual services in a distributed system are functioning properly, the interactions between those services can cause unpredictable outcomes. Unpredictable outcomes, compounded by rare but disruptive real-world events that affect production environments, make these distributed systems inherently chaotic.
>
> We need to identify weaknesses before they manifest in system-wide, aberrant behaviors. Systemic weaknesses could take the form of
> - improper fallback settings when a service is unavailable
> - retry storms from improperly tuned timeouts
> - outages when a downstream dependency receives too much traffic
> - cascading failures when a single point of failure crashes
>
> We must address the most significant weaknesses proactively, before they affect our customers in production. We need a way to manage the chaos inherent in these systems, take advantage of increasing flexibility and velocity, and have confidence in our production deployments despite the complexity that they represent.
>
> -- <https://principlesofchaos.org/>

> Performance Improvement = Errors ↓ + Insights ↑
>
> -- Gary Klein, Seeing What Others Don't

### Build a Hypothesis around Steady State Behavior
### Vary Real-world Events
### Run Experiments in Production
### FIT: Failure Injection Testing
### Automate Experiments to Run Continuously
### Minimize Blast Radius

## Continuous Verification

Like CI/CD, Continuous Verification is born out of a need to navigate increasingly complex systems. Modern organizations can’t validate that the internal machinations of the system work as intended, so instead they verify that the output of the system macthes expectations.

Approach:

- Proactive detection: discipline to prevent availability and security incidents
- Emperical experimentation: proactively discover vulnerabilities in complex systems
- Same experimental foundation as the Chaos Automation Platform (ChAP)

## Game Days

[GameDay: Creating Resiliency Through Destruction (Jesse Robbins)](https://www.youtube.com/watch?v=zoz0ZjfrQ9s)

> You don't choose the moment, the moment chooses you. You are choose how prepared you are when it does.
>
> `99.9%^3 = 99.7%`
>
> MTTR > MTBF
>
> You don't choose whether or not you have [latent defects]. You do get to choose when you discover some of them. You do get to choose about how much it's gonna cost you.

## Tools and Resources

- [Awesome Chaos Engineering](https://github.com/dastergon/awesome-chaos-engineering)

SaaS

- [Gremlin](https://gremlin.com/)
- [Verica](https://verica.io/)
- [Launch Darkly](http://launchdarkly.com/)

Open-source

- [Chaos Toolkit](https://chaostoolkit.org/)
- [Chaos Blade](https://chaosblade.io/) (Alibaba)
- [Pumba](https://github.com/alexei-led/pumba)
- [Litmus](https://litmuschaos.io/)
- [Chaos Mesh](https://github.com/chaos-mesh/chaos-mesh)

## References

- [Learning from Incidents](https://www.learningfromincidents.io/)
- [Chaos Community Broadcast](https://chaos.community/)
- [Resilience engineering papers](http://resiliencepapers.club/)
- [The Error of Counting "Errors"](https://www.researchgate.net/publication/5446061_The_Error_of_Counting_Errors)
- [The Verification of a Distributed System](https://queue.acm.org/detail.cfm?ref=rss&id=2889274)

