---
title: "Conway's Law in the Age of Agentic AI"
date: 2026-05-06
publishdate: 2026-05-06
lastmod: 2026-05-06
summary: "Conway's Law says your system architecture mirrors your organization's communication structure. Agentic AI doesn't change the law. It changes what counts as communication."
tags: ["team", "leadership", "management", "distributed"]
image: /images/conways-law.jpg
draft: true
---

## Conway's Law

A platform team grants their coding agent access to all five service repositories. By the end of the sprint, it has refactored shared utilities into a common library that no human engineer had proposed, and that two teams had agreed was out of scope. The architecture changed. Nobody called a meeting. Nobody approved the new dependency. The system evolved to reflect what the agent could touch.

Conway's Law didn't fail here. It worked exactly as described. The agent's access permissions were a communication structure. The system design followed.

## What Is Conway's Law?

In 1968, Melvin Conway published a deceptively simple observation: "Any organization that designs a system will inevitably produce a design whose structure is a copy of the organization's communication structure" (Conway, 1968). Systems mirror the communication paths of the people who build them. A team split across time zones builds loosely coupled services. A tightly integrated team builds a monolith.

This matters because communication structure constrains the solution space. Teams can only design clean interfaces where they communicate. Where that communication breaks down, so do the boundaries (Fowler, 2015). Architecture and org chart co-evolve, usually without anyone noticing.

The inverse Conway maneuver, attributed to James Lewis and popularized by Skelton and Pais, turns this observation into a tool (Skelton and Pais, 2019). If systems mirror org structure, deliberately design the org structure to get the architecture you want. Loosely coupled microservices require small, autonomous teams with narrow ownership and minimal cross-team dependencies. The architecture is emergent. It grows from the org design, not from a separate technical specification.

## How Agents Change the Mechanism

The inverse Conway maneuver relies on a specific mechanism: communication friction shapes design. Teams with high coordination overhead produce tightly coupled systems. Teams with clean interface boundaries produce loosely coupled ones. The friction is the engine.

Agents don't have that friction. They don't experience Dunbar's Number, which limits human cognitive capacity for stable relationships to roughly 150 people (Skelton and Pais, 2019). They don't attend standups. They don't file tickets to call another team's API. As Skelton and Pais have observed, "AI systems can be set up to communicate perfectly without the social limits humans have" (Skelton and Pais, 2025).

Remove the friction and the old model breaks. A single agent with access to five repositories doesn't behave like five teams coordinating through service APIs. It behaves like one engineer with full context across all of them. That produces different architecture, whether or not anyone intended it.

## Access Is Architecture

Conway's Law re-enters here, not as a limitation but as a lens.

If communication structure determines system structure, then for agentic systems, access topology is the communication structure. What an agent can read, write, call, and spawn defines its effective communication graph. That graph shapes the systems it produces. This is Conway's Law in its most direct form. The new communication structure is not a social network. It is a permissions model, and it is explicit, auditable, and designable in ways that human communication never fully is.

An agent with write access to one repository will centralize decisions there even when it reads broadly. An agent with symmetric write access across five repositories will distribute decisions more evenly. Neither outcome is accidental. Both follow directly from the access model.

This means access decisions are architecture decisions. Granting a coding agent repository access is not only a security choice. It is a structural choice about what kinds of systems that agent can produce. Organizations that treat it purely as a security matter will make architecture decisions without knowing it.

## The Inverse Agent Maneuver

The inverse Conway maneuver translates directly to agentic systems. To get the architecture you want, design the access topology to match it.

If the goal is loosely coupled services with clear ownership, scope agents to service boundaries. Give each agent write access to its owning service and read access to the APIs it depends on. A single agent with write access to the entire platform will produce platform-wide entanglement, regardless of how carefully the service contracts are documented elsewhere.

If the goal is cross-cutting improvement, give agents the access to make it. A refactoring agent needs broad read access. A dependency-patching agent may need broad write access. In both cases, the access topology is the design intent made explicit.

Multi-agent systems add another layer. When agents spawn sub-agents or hand off to specialists, the communication structure between agents shapes the architecture of what they build. A hub-and-spoke orchestration model produces different outcomes than a peer-to-peer one. Conway's Law applies recursively to the agent network itself. Designing the agent topology is designing the system.

## Common Mistakes

**Granting access without designing for it.** Broad access granted for convenience produces architectures nobody designed. Treat every access expansion as an architectural decision.

**Mirroring existing silos in agent scope.** Scoping agents to match current team boundaries reproduces the architecture already in place. The access topology is an opportunity to redesign, not replicate.

**Treating permissions as purely a security concern.** Security teams are not architects. Access decisions made without architectural input produce systems shaped by security policy rather than business domain.

**Ignoring inter-agent communication structure.** In multi-agent systems, the topology of handoffs, escalations, and tool sharing shapes the resulting architecture as directly as any human org chart does. Design it explicitly.

**Assuming agents produce neutral architecture.** Every agent reflects its training, its prompting, and its access constraints. There is no neutral agent. The design decisions are simply less visible.

## Put It Into Practice

Conway's Law has always been most dangerous when invisible. Teams unaware of it still produce systems that mirror their communication structure. Agentic AI makes the same dynamic available at machine speed, at scale, and with access patterns that cut across the organizational boundaries that previously held.

Before expanding an agent's permissions, draw its access map as if it were an org chart. Ask what architecture that org chart would naturally produce, and whether that is the architecture you want.

Map every agent or agent network your team currently operates or plans to deploy. Document its read and write access boundaries. Treat those boundaries as a first draft of the architecture. Then decide deliberately whether to keep them.

## References

Conway, Melvin E. (1968). "How Do Committees Invent?" *Datamation*, 14(4): 28-31. [https://www.melconway.com/Home/Committees_Paper.html](https://www.melconway.com/Home/Committees_Paper.html)

Fowler, Martin (2015). "Conway's Law." *martinfowler.com*. [https://martinfowler.com/bliki/ConwaysLaw.html](https://martinfowler.com/bliki/ConwaysLaw.html)

Skelton, Matthew and Pais, Manuel (2019). *Team Topologies: Organizing Business and Technology Teams for Fast Flow*. IT Revolution Press. [https://teamtopologies.com/book](https://teamtopologies.com/book)

Skelton, Matthew and Pais, Manuel (2025). "The Future of Team Topologies: When AI Agents Dominate." *Team Topologies Blog*, January 14. [https://teamtopologies.com/news-blogs-newsletters/2025/1/14/the-future-of-team-topologies-when-ai-agents-dominate](https://teamtopologies.com/news-blogs-newsletters/2025/1/14/the-future-of-team-topologies-when-ai-agents-dominate)

## Changelog

**2026-05-06** Initial publication.
