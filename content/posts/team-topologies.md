---
title: "Team Topologies"
date: 2020-05-14
publishdate: 2020-05-14
lastmod: 2020-05-14
summary: "Team Topologies provides four fundamental team types -- stream-aligned, platform, enabling, and complicated-subsystem -- and three core team interaction modes -- collaboration, X-as-a-Service, and facilitating. Together with awareness of Conway's law, team cognitive load, and how to become a sensing organization, Team Topologies results in an effective and humanistic approach to building and running software systems."
image: https://images.squarespace-cdn.com/content/5b3296b78ab7229ecafcf4ed/1549553080029-RBNC0NOWOUE55VMFK8HB/main-logo-blue-text.png?format=1500w&content-type=image%2Fpng
tags: ["team", "cognitive", "capacity"]
---
## [Team Topologies](https://teamtopologies.com/book) by Matthew Skelton and Manuel Pais

## Teams as the means of delivery

### The problem with org charts

people don’t restrict their communications only to those connected lines on the [org] chart. We reach out to whomever we depend on to get work done. We bend the rules when required to achieve our goals. org charts don’t help us understand the actual patterns of communication in our organization

Systems thinking focuses on optimizing for the whole, looking at the overall flow of work, identifying what the largest bottleneck is today, and eliminating it. Then repeat

Niels Pflaeging suggests that the key to successful knowledge work organizations is in the interactions between the informal structure and the value creation structure (that is, the interactions between people and teams):

1. Formal structure (the org chart)—facilitates compliance
1. Informal structure—the “realm of influence” between individuals
1. Value creation structure—how work actually gets done based on interpersonal and interteam reputation

Conway’s law: “Organizations which design systems… are constrained to produce designs which are copies of the communication structures of these organizations.”

- “If you have four groups working on a compiler, you’ll get a 4-pass compiler.” – Eric S. Raymond
- James Lewis (Thoughtworks) “inverse Conway maneuver”: an organization focuses on organizing team structures to match the architecture they want the system to exhibit rather than expecting teams to follow a mandated architecture design.

cognitive load: limit on how much information one can hold in their brain at any given moment

Daniel Pink’s three elements of intrinsic motivation:

1. autonomy (quashed by constant juggling of requests and priorities from multiple teams)
1. mastery (“jack of all trades, master of none”)
1. purpose (too many domains of responsibility)

Historically, most organizations have seen software development as a kind of manufacturing to be completed by separate individuals arranged into functional specialties, with large projects planned up front and with little consideration for sociotechnical dynamics

Agile, Lean IT, and DevOps movements helped demonstrate the enormous value of smaller, more autonomous teams that were aligned to the flow of business, developing and releasing in small, iterative cycles, and course correcting based on feedback from users

### Conway’s law and why it matters

Organizations are constrained to produce designs that reflect communication paths

The design of the organization constrains the “solution search space,” limiting possible software designs

Requiring everyone to communicate with everyone else is a recipe for a mess

- One key implication of Conway’s law is that not all communication and collaboration is good. Thus it is important to define “team interfaces” to set expectations around what kind of work requires strong collaboration and what doesn’t. Many organizations assume that more communication is always better, but this is not really the case
- If the organization has an expectation that “everyone should see every message in the chat” or “everyone needs to attend the massive standup meetings” or “everyone needs to be present in meetings” to approve decisions, then we have an organization design problem. Conway’s law suggests that this kind of many-to-many communication will tend to produce monolithic, tangled, highly coupled, interdependent systems that do not support fast flow. More communication is not necessarily a good thing

## Team-first thinking

the best performing teams “accomplish remarkable feats not simply because of the individual qualifications of their members but because those members coalesce into a single organism.” – Stanley McChrystal

The team is the most effective means of software delivery, not individuals

- “team” has a very specific meaning. By team, we mean a stable group of five to nine people who work toward a shared goal as a unit. We consider the team to be the smallest entity of delivery within the organization. Therefore, an organization should never assign work to individuals; only to teams

(extended) Dunbar’s numbers:

- ~ 5 people – limit of people with whom we can hold close personal relationships
- ~ 15 people – limit of people with whom we can experience deep trust
- ~ 50 people – limit of people with whom we can have mutual trust
- ~ 150 people – limit of people whose capabilities we can remember
- ~ 500 people – limit to effective social relationships

Dunbar-compatible groupings:

- A single team: 5-8 people (based on industry experience)
- Families (“tribes”): groupings of teams of no more than 50 people
- Divisions/streams/profit & loss (P&L) lines: groupings of no more than 150 people

Typically, a team can take from two weeks to three months or more to become a cohesive unit. When (or if) a team reaches that special state, it can be many times more effective than individuals alone

- Brooks’s law: adding new people to a team doesn’t immediately increase its capacity

Beyond the Tuckman Performance Model

- The Tuckman model describes how teams perform in four stages:
  1. Forming: assembling for the first time
  1. Storming: working through initial differences in personality and ways of working
  1. Norming: evolving standard ways of working together
  1. Performing: reaching a state of high effectiveness
- However, in recent years, research by people like Pamela Knight has found that this model is not quite accurate, and that storming actually takes places continually throughout the life of the team.

With small, long-lived teams in place, we can begin to improve the ownership of software. Team ownership helps to provide the vital “continuity of care” that modern systems need in order to retain their operability and stay fit for purpose. Team ownership also enables a team to think in multiple “horizons”—from exploration stages to exploitation and execution—to better care for software and its viability. As Jez Humble, Joanne Molesky, and Barry O’Reilly put it in their book Lean Enterprise

1. Horizon 1 covers the immediate future with products and services that will deliver results the same year
1. Horizon 2 covers the next few periods, with an expanding reach of the products and services
1. Horizon 3 covers many months ahead, where experimentation is needed to assess market fit and suitability of new services, products, and features

The danger of allowing multiple teams to change the same system or subsystem is that no one owns either the changes made or the resulting mess.

Every part of the software system needs to be owned by exactly one team. This means there should be no shared ownership of components, libraries, or code. Teams may use shared services at runtime, but every running service, application, or subsystem is owned by only one team. Outside teams may submit pull requests or suggestions for change to the owning team, but they cannot make changes themselves.

Good boundaries minimize cognitive load

John Sweller defined cognitive load as “the total amount of mental effort being used in the working memory.”

- Intrinsic cognitive load – relates to aspects of the task fundamental to the problem space (e.g., “What is the structure of a Java class?” “How do I create a new method?”)
- Extraneous cognitive load – relates to the environment in which the task is being done (e.g., “How do I deploy this component again?” “How do I configure this service?”)
- Germane cognitive load – relates to aspects of the task that need special attention for learning or high performance (e.g., “How should this service interact with the ABC service?”)

effective delivery and operations of modern software systems, organizations should attempt to minimize intrinsic cognitive load (through training, good choice of technologies, hiring, pair programming, etc.) and eliminate extraneous cognitive load altogether (boring or superfluous tasks or commands that add little value to retain in the working memory and can often be automated away), leaving more space for germane cognitive load (which is where the “value add” thinking lies).

Match software boundary size to team cognitive load

- maximize the cognitive capacity of the team (by reducing the intrinsic and extraneous types of load):
  - Provide a team-first working environment (physical or virtual)
  - Minimize team distractions during the workweek by limiting meetings, reducing emails, assigning a dedicated team or person to support queries, and so forth
  - Change the management style by communicating goals and outcomes rather than obsessing over the “how,” what McChrystal calls “Eyes On, Hands Off ” in Team of Teams
  - Increase the quality of developer experience for other teams using your team’s code and APIs through good documentation, consistency, good UX
  - Use a platform that is explicitly designed to reduce cognitive load for teams building software on top of it

Team API (code, versioning, documentation, practices, communciation)

- The team API should explicitly consider usability by other teams
- Amazon CEO Jeff Bezos insisted on almost paranoid levels of separation between teams. For example, each team at AWS must assume that “every [other team] becomes a potential DOS [denial of service] attacker requiring service levels, quotas, and throttling.”

## Team topologies that work for flow

### Static team topologies

“Instead of structuring teams according to technical know-how or activities, organize teams according to business areas” – Jutta Eckstein

“Static team topologies” – team structures and interactions that fit a specific organization’s context at a given point in time

Team anti-patterns

- Ad-hoc team. Examples:
  - teams that have grown too large and were broken up as the communication overhead takes its toll)
  - teams created to take care of all COTS software, or all middleware, etc
  - DBA team created after a production outage
- Shuffling team members
  - volatile team assembled on a project basis (and disassembled immediately aing team design slows down software delivery
- Ad hoc or constantly changing team design slows down software delivery

Given our skills, constraints, cultural and engineering maturity, desired software architecture, and business goals, which team topology will help us deliver results faster and safer?

Old organizational model provide less speed, and less safety:

- functional silos between different departments
- heavy use of outsourcing
- repeated hand-offs between teams

Old organizational model’s significant flaw:

- assumption that software delivery is a one-way process
  - specification → design → dev → test → release → business-as-usual
- such a linear, step-wise sequence (usually with separate functional silos for each stage) is incompatible with the speed of change and complexity of modern software systems

Organizations that value information feedback from live (production) systems can not only improve their software more rapidly, but also develop a heightened
responsiveness to their customers and users

The key contribution of DevOps was to raise awareness of the problems lingering in how teams interacted (or not) across the delivery chain, causing delays, rework,
failures, and a lack of understanding and empathy toward other teams

[DevOps Topologies](https://web.devopstopologies.com/)

1. There is no one-size-fits-all approach to structuring teams for DevOps success
1. There are several topologies known to be detrimental (anti-patterns) to DevOps success

The “DevOps Team” anti-pattern is a quintessential example. On paper, it makes sense to bring automation and tooling experts in house to accelerate the delivery and operations of our software. However, this team can quickly become a hard dependency for application teams if the DevOps team is being asked to execute steps on the delivery path of every application, rather than helping to design and build self-service capabilities that application teams can rely on autonomously.

Microsoft has been using product teams since the 1980s. With the availability of Azure as an IaaS and PaaS solution for Microsoft products and services, teams within Microsoft are able to consume infrastructure and platform features “as a service.” This allows teams to significantly increase delivery speed. In particular, the teams building the Visual Studio product have undergone a radical transformation from a destop-first, multi-month delivery cycle, to a cloud-first, daily/weekly delivery cycle.

Product teams need autonomy to provision their own environments and resources in the cloud, creating new images and templates where necessary. The cloud team might still own the provisiong process – ensuring that necessary controls, policies, and auditing are in place (especially in highly regulated industries) – but their focus should be in providing high-quality self-services that match both the needs of product teams and the need for adequate risk and compliance management.

![](https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcSEBeRs3d2A5KRfAL7c-WaLUp7REgUpX-XtWw&usqp=CAU)

Technical and cultural maturity, org scale, and engineering discipline are critical aspects when considering which topology to adopt

In particular, the feature-team/product-team pattern is powerful but only works with a supportive surrounding environment

Splitting a team’s responsibilities can break down silos and empower other teams

### Four fundamental team topologies

The only four team topologies needed to build and run modern software systems:

- Stream-aligned team
- Enabling team
- Complicated-subsystem team
- Platform team

![](https://user-images.githubusercontent.com/1256183/73894183-0222a600-48bf-11ea-9c74-849f0332dca3.jpeg)

Reduced ambiguity around organizational roles is a key part of success

The “stream-aligned” team is the primary team type

The purpose of the other team topologies is to reduce the burden on the stream-aligned teams

Multiple stream-aligned teams are the starting point

- but, an organization may also have several platform, enabling, and subsystem teams
- “Ops” and “Support” teams are largely aligned to streams. There is no “handoff” to a separate Ops or Support team

A “stream” is the continous flow of work aligned to a business domain

A “stream-aligned” team is aligned to a single, valuable stream of work (e.g. a single product or service, a single set of features, a single user journey, or a single user persona)

A “stream-aligned” team is empowered to build and deliver customer or user value as quickly, safely, and independently as possible, without requiring hand-offs to other teams to perform parts of the work

“In product development, we can change direction more quickly when we have a small team of highly skilled people instead of a large team” – Don Reinertsen

> you build it, you run it
>
> – Werner Vogels

SRE are a type of stream-aligned team in the sense that they are responsible for the reliability of large-scale applications running in production, primarilly interacting with one or more stream-aligned teams responsible for developing applications

The four fundamental team topologies simplify modern software team interactions

Mapping common industry team types to the fundamental topologies sets up organizations for success, removing gray areas of ownership and overloaded/underloaded teams

The main topology is (business) stream-aligned; all other topologies support this type

The other topologies are enabling, complicated-subsystems, and platform

The topologies are often “fractal” (self-similar) at large scale: teams of teams

### Choose team-first boundaries

Choose software boundaries using a team-first approach

Beware of hidden monoliths and coupling in the software-delivery chain

Use software boundaries defined by business-domain bounded contexts

Consider alternative software boundaries when necessary and suitable

## Evolving team interactions for innovation and rapid delivery

### Team interaction modes

### Evolve team structures with organizational sensing

### Next-generation digital operating model
