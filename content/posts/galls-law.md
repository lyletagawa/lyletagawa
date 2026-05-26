---
title: "Gall's Law"
date: 2026-05-25
publishdate: 2026-05-25
lastmod: 2026-05-25
summary: "Complex systems that work evolved from simple systems that worked. Designing complex systems from scratch rarely succeeds."
tags: ["systems-thinking", "complexity", "design", "architecture", "devops", "sre", "microservices"]
image:
draft: true
---

## Gall's Law

Your team designs a comprehensive microservices architecture. You map out service boundaries, define APIs, plan for failure modes. Six months later, services cannot talk to each other reliably. Data consistency is a nightmare. The system is too complex to debug.

Meanwhile, a competitor starts with a monolith. They split off services only when bottlenecks appear. Their system grows messier but keeps working. They ship features while you untangle dependencies.

## What Is Gall's Law?

Gall's Law states: "A complex system that works is invariably found to have evolved from a simple system that worked. A complex system designed from scratch never works and cannot be patched up to make it work. You have to start over with a working simple system" (Gall, 1975).

John Gall introduced this principle in *Systemantics: How Systems Work and Especially How They Fail*, a book examining why large systems fail despite careful planning. Gall was a pediatrician and systems theorist who observed that medical systems, government programs, and organizations all exhibited similar failure patterns. Complex designs looked good on paper but collapsed in practice.

The law challenges a core assumption in engineering and management. We believe that careful upfront design prevents problems. We create detailed specifications, model edge cases, plan for scale. Gall argues this approach fails because complexity cannot be designed. It must be grown.

## Why Complexity Cannot Be Designed

Complex systems have emergent properties. Interactions between components create behaviors unpredictable from components alone. You can model individual services perfectly and still have a system that deadlocks under load (Perrow, 1984).

**Interfaces multiply faster than components.** Three components create three connections. Four components create six. Ten create 45. Each connection is where assumptions mismatch, timing fails, or data corrupts. You cannot anticipate all interactions upfront.

**Requirements change during implementation.** You design for known needs. Building reveals unknown needs. Users request unplanned features. Bottlenecks appear in unexpected places. Security requirements tighten. The system you need is not the system you designed. Changing a complex design is harder than evolving a simple one (Brooks, 1975).

**Feedback loops are invisible until the system runs.** A service might work in isolation but create cascading failures when deployed. Reasonable retry logic can amplify load during outages. Performance-improving caches can serve stale data at critical moments. You discover these loops by running the system, not designing it.

**Simple systems provide learning.** A working simple system teaches you what matters. You see where load concentrates, which features users need, which failure modes occur in practice. This knowledge guides evolution. Without a working system, you are guessing.

## Evolution Versus Design

Evolution does not mean no planning. It means planning for change rather than planning for completeness.

Start with the simplest system that solves the core problem. For a web application, a single server with a database. For a data pipeline, a script on a schedule. For a platform team, a basic CI/CD pipeline with manual deployments. The system should work end to end, even if it handles only the happy path.

Add complexity only when the simple system breaks. Database bottleneck? Add read replicas. Server overloaded? Add a load balancer. Monolith too large? Extract one service. Manual deployments causing incidents? Add automated rollback. Each addition solves a real observed problem (Beyer et al., 2016).

Keep each change small. Extract one service, not ten. Add one cache, not a caching layer. Small changes are easier to test, roll back, and understand. They provide faster feedback. You learn if the change helped before investing in the next.

Maintain working state between changes. The system should work before and after each change. Teams violate this by adding half-finished features, deploying broken services, merging incomplete refactors. Each violation makes evolution harder because you lose the working baseline.

## Real-World Examples

Amazon started as a monolithic application. They extracted services gradually over years as specific bottlenecks appeared (Vogels, 2017). They did not design microservices upfront. They evolved toward them as the monolith became unmaintainable. Each service solved a specific scaling or organizational problem.

Netflix followed a similar path. They began with a monolithic DVD rental system, then evolved to streaming with a monolith, and only later extracted microservices as specific needs emerged (Cockcroft, 2016). Their chaos engineering practices evolved from simple failure injection to sophisticated tools like Chaos Monkey. Each capability was added when the previous system could not handle the next level of scale.

Google's Site Reliability Engineering practices emerged from operational necessity, not upfront design. They started with traditional operations, discovered what broke, and built tools to prevent those specific failures (Beyer et al., 2016). SLOs, error budgets, and toil reduction evolved as responses to real problems. The SRE model works because it grew from production experience.

Kubernetes itself demonstrates Gall's Law. It evolved from Google's internal Borg system, which evolved from even simpler cluster management tools (Burns et al., 2016). Google did not design Borg from scratch. They built simple schedulers, learned what failed, and added features incrementally over a decade. Kubernetes inherited this evolutionary wisdom rather than attempting to design container orchestration from first principles.

## Common Mistakes

**Confusing evolution with lack of planning.** Evolution requires discipline. You need clear goals, good testing, and careful design of each increment. The difference is timing. You design each piece as you need it, not all pieces upfront. This requires resisting the urge to build for imagined future needs.

**Adding complexity without removing it.** Evolution creates cruft. Old code paths remain when new ones are added. Temporary solutions become permanent. Successful evolution requires pruning. Remove old code when new code replaces it. Simplify interfaces as you learn. Refactor as you go.

**Evolving without measurement.** You need to know if changes help. Instrument the system. Track latency, error rates, resource usage. Measure before and after each change. Without data, you cannot tell if you are evolving toward a better system or just a different one.

**Mistaking incremental for minimal.** Incremental means small steps. Minimal means the smallest thing that works. You want both. Each step should be small enough to understand and test, complete enough to provide value. A half-built feature is incomplete. Build the smallest complete thing, then the next.

## Put It Into Practice

Identify the simplest version of your system that solves the core problem. Strip away nice-to-have features, optimizations, and edge cases. What remains should be buildable in days or weeks. Build that first.

Deploy it to real users, even if only internal. Real usage reveals what matters. You will learn more from a week of production traffic than a month of design meetings.

When you add complexity, add one thing at a time. Extract one service. Add one optimization. Introduce one new technology. Wait to see the impact before adding the next. This discipline keeps the system understandable and keeps you in control of its evolution.

## References

Beyer, Betsy, Chris Jones, Jennifer Petoff, and Niall Richard Murphy (2016). *Site Reliability Engineering: How Google Runs Production Systems*. O'Reilly Media. [https://sre.google/sre-book/table-of-contents/](https://sre.google/sre-book/table-of-contents/)

Brooks, Frederick P. (1975). *The Mythical Man-Month: Essays on Software Engineering*. Addison-Wesley. [https://www.worldcat.org/title/mythical-man-month-essays-on-software-engineering/oclc/1308797](https://www.worldcat.org/title/mythical-man-month-essays-on-software-engineering/oclc/1308797)

Burns, Brendan, Brian Grant, David Oppenheimer, Eric Brewer, and John Wilkes (2016). "Borg, Omega, and Kubernetes." *ACM Queue*, 14(1): 70-93. [https://queue.acm.org/detail.cfm?id=2898444](https://queue.acm.org/detail.cfm?id=2898444)

Cockcroft, Adrian (2016). "Microservices Workshop: Why, What, and How to Get There." *SlideShare*. [https://www.slideshare.net/adriancockcroft/microservices-workshop-all-topics-deck-2016](https://www.slideshare.net/adriancockcroft/microservices-workshop-all-topics-deck-2016)

Gall, John (1975). *Systemantics: How Systems Work and Especially How They Fail*. Quadrangle/The New York Times Book Company. [https://www.worldcat.org/title/systemantics-how-systems-work-and-especially-how-they-fail/oclc/1505845](https://www.worldcat.org/title/systemantics-how-systems-work-and-especially-how-they-fail/oclc/1505845)

Meadows, Donella H. (2008). *Thinking in Systems: A Primer*. Chelsea Green Publishing. [https://www.chelseagreen.com/product/thinking-in-systems/](https://www.chelseagreen.com/product/thinking-in-systems/)

Perrow, Charles (1984). *Normal Accidents: Living with High-Risk Technologies*. Basic Books. [https://www.worldcat.org/title/normal-accidents-living-with-high-risk-technologies/oclc/9697361](https://www.worldcat.org/title/normal-accidents-living-with-high-risk-technologies/oclc/9697361)

Vogels, Werner (2017). "A Decade of Dynamo." *All Things Distributed*. [https://www.allthingsdistributed.com/2017/10/a-decade-of-dynamo.html](https://www.allthingsdistributed.com/2017/10/a-decade-of-dynamo.html)

## Changelog

**2026-05-25** Added modern DevOps, SRE, and platform engineering references. Expanded Real-World Examples with Netflix, Google SRE, and Kubernetes evolution.

**2026-05-25** Initial publication.