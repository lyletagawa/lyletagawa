# Post Ideas

Organized by theme. Each table uses the same format: `~~struck through~~` means a post exists for it, with status and filename in the last column. Plain rows are still just ideas.

---

## Systems, Networking & Queueing
*The systems track: how packets, queues, and probability behave under load. Built around primary sources, not management analogies. Posts cross-link each other heavily.*

| Concept | Source | Post Angle | Status / Closest Existing |
|---|---|---|---|
| ~~Bloom Filters~~ | ~~Bloom, 1970~~ | ~~A probabilistic structure that trades exactness for space~~ | **published** — `bloom-filter.md` |
| ~~Little's Law~~ | ~~Little, 1961~~ | ~~L = λW. Three numbers run every queue, you pick two~~ | **draft** — `littles-law.md` |
| ~~The Tail at Scale~~ | ~~Dean & Barroso, 2013~~ | ~~Latency percentiles fan out under load; p99 is a system property~~ | **draft** — `the-tail-at-scale.md` |
| ~~Backpressure~~ | ~~Reactive Streams, 2015~~ | ~~The signal that slows a producer instead of dropping data~~ | **draft** — `backpressure.md` |
| ~~Bufferbloat~~ | ~~Gettys & Nichols, 2011~~ | ~~Oversized buffers hoard packets and trade throughput for seconds of lag~~ | **draft** — `bufferbloat.md` |
| ~~TCP Congestion Control~~ | ~~Jacobson, 1988~~ | ~~AIMD, the sawtooth, and what Linux and FreeBSD actually ship~~ | **published** — `tcp-congestion-control.md` (titled "From Reno to BBR") |
| ~~Head-of-Line Blocking~~ | ~~Karol et al., 1987~~ | ~~One stuck item at the front of a shared queue stalls everything behind it~~ | **draft** — `head-of-line-blocking.md` |
| ~~Retrieval-Augmented Generation~~ | ~~Lewis et al., 2020~~ | ~~Grounding an LLM by retrieving evidence at query time~~ | **draft** — `retrieval-augmented-generation.md` |
| Coordinated Omission | Tene, 2014 | The load test reports a false p99 because the slow requests never got sent | The Tail at Scale |
| ~~The CAP Theorem & PACELC~~ | ~~Brewer, 2000; Hale, 2010~~ | ~~CAP is a trick question you only face during a partition; PACELC asks the trade you face every day~~ | **draft** — `partition-tolerance-isnt-optional.md` (titled "Partition Tolerance Isn't Optional") |
| ~~HyperLogLog~~ | ~~Flajolet et al., 2007~~ | ~~Count a billion uniques in ~1.5 KB by watching hashed coincidences~~ | **draft** — `how-redis-counts-to-18-quintillion-in-12-kb.md` (titled "How Redis Counts to 18 Quintillion in 12 KB") |
| ~~Consistent Hashing~~ | ~~Karger et al., 1997~~ | ~~Add a node without reshuffling every key. The ring behind every shard map~~ | **draft** — `consistent-hashing.md` |
| The Universal Scalability Law | Gunther, 2007 | Amdahl plus a coordination term that makes throughput go down past a point | Little's Law |
| Idempotency | — | The one property that makes retries, queues, and at-least-once delivery survivable | Backpressure |
| ~~t-Digest / Quantile Sketches~~ | ~~Dunning & Ertl, 2019~~ | ~~Averaging percentiles across replicas produces a number, not a statistic~~ | **published** — `t-digest.md` (titled "Percentiles Don't Add Up") |
| ~~Change Data Capture~~ | ~~Kleppmann, 2015~~ | ~~Reading a database's write-ahead log beats polling for changes, and most things called "CDC" aren't~~ | **draft** — `bottling-a-stream-of-changes.md` (titled "Bottling a Stream of Changes") |
| ~~Materialized Views~~ | ~~Gupta & Mumick, 1995~~ | ~~Not every "materialized view" behaves like a stored snapshot, and assuming so can empty a table~~ | **draft** — `materialized-views-arent-snapshots.md` |
| ~~Design for Growth~~ | ~~Dean, 2009~~ | ~~Design so a system survives 10x or 20x growth. Chasing 100x readiness before you need it usually costs more than it saves~~ | **draft** — `design-for-10x.md` (titled "Design for 10x") |
| ~~The Elevator Algorithm~~ | ~~Denning, 1967~~ | ~~Disk schedulers exist to minimize how far a mechanical arm has to travel. Flash storage removed that constraint, and the simplest scheduler often wins now~~ | **draft** — `elevator-algorithm.md` |
| ~~Numbers Everyone Should Know~~ | ~~Dean, 2009~~ | ~~A back-of-envelope calculation gets you within 10x of the truth in five minutes, using a handful of memorized numbers~~ | **draft** — `napkin-maths.md` (titled "Napkin Maths") |
| ~~The Critical Path~~ | ~~—~~ | ~~The critical path determines how fast a system can run. Moving work off it is often the fastest optimization available~~ | **draft** — `off-the-critical-path.md` (titled "Off the Critical Path") |
| ~~Tries~~ | ~~Fredkin, 1960~~ | ~~A trie stores strings by character, one per node, so prefix search comes free with the structure~~ | **draft** — `trie.md` (titled "Trie") |
| ~~The Two Generals Problem~~ | ~~Gray, 1978~~ | ~~Two armies can never confirm a coordinated attack succeeded over an unreliable channel. The same impossibility still shapes how databases and networks handle failure~~ | **draft** — `two-generals-problem.md` (titled "The Two Generals Problem") |
| ~~Live Event Caching~~ | ~~Netflix patent, 2026~~ | ~~LRU evicts the segments live viewers need when they rewind. Segment-age caching and distributed ownership fix it without bloating every edge server~~ | **published** — `live-event-caching.md` (titled "Age-Based Live Event Caching") |
| ~~Wildlife and Physical Outages~~ | ~~—~~ | ~~Squirrels have shut down Nasdaq trading twice, but misconfiguration, sabotage, and copper theft cause far more network damage than any animal ever has~~ | **published** — `squirrel-threat-model.md` (titled "The Squirrel Threat Model") |

---

## Systems Thinking
*The classic systems-thinking canon: Meadows, Forrester, Ashby, Senge. How feedback, structure, and intervention points shape a system's behavior, distinct from the networking/queueing track above.*

| Concept | Source | Post Angle | Status / Closest Existing |
|---|---|---|---|
| Ashby's Law of Requisite Variety | Ashby, 1956 | A controller needs at least as much variety as the system it regulates. Why a fixed runbook can't handle a system with more failure modes than the runbook has branches | — |
| Leverage Points | Meadows, 1999 | Meadows ranked 12 places to intervene in a system, from tweaking a parameter (weak) to changing the paradigm (strong). Most orgs spend all their effort on the weakest ones | — |
| Shifting the Burden | Senge, 1990 (systems archetype, traced to Forrester's industrial dynamics) | A symptomatic quick fix erodes the capacity to apply the real fix, and the system gets hooked on the quick fix. Firefighting culture and feature-flag-as-permanent-workaround | — |
| The Bullwhip Effect | Forrester, 1961; formalized by Lee, Padmanabhan & Whang, 1997 | Small fluctuations at the edge amplify into wild swings upstream. Originally supply chains, but it's the same mechanism behind retry storms and cascading failures | — |
| The Cynefin Framework | Snowden & Boone, 2007 | Categorizes problems as simple, complicated, complex, or chaotic, each needing a different response. Why "best practices" fail when applied to genuinely complex incidents | — |

---

## AI & Machine Learning
*Primary-source explainers on how the current wave of AI systems actually work, not hype pieces.*

| Concept | Source | Post Angle | Status / Closest Existing |
|---|---|---|---|
| ~~Knowledge Distillation~~ | ~~Hinton, Vinyals & Dean, 2015~~ | ~~A reviewer rejected Dean's 2014 paper as unlikely to matter. Every small model shipped today, including Gemini Flash, runs on the technique it described~~ | **draft** — `knowledge-distillation.md` |
| ~~Context Engineering~~ | ~~Lütke, 2025~~ | ~~Tobi Lütke coined the term in June 2025, and Karpathy's endorsement made it stick. A model's context window is scarce, and what you leave out matters as much as what you put in~~ | **draft** — `context-engineering.md` |
| ~~How LLMs Actually Work~~ | ~~—~~ | ~~Working notes on how LLMs train and generate text~~ | **published** — `how-llms-work.md` |
| ~~Stochastic Parrots~~ | ~~Bender et al., 2021~~ | ~~A language model that generates fluent text without understanding it, the term that cost its coiner her job at Google~~ | **draft** — `stochastic-parrot.md` |

---

## Named Laws & Effects
*Like Goodhart's Law and Jevons Paradox: named concepts from outside tech with direct, non-obvious application to engineering leadership.*

| Concept | Source | Post Angle | Closest Existing |
|---|---|---|---|
| ~~Goodhart's Law~~ | ~~Goodhart, 1975~~ | ~~When a measure becomes a target it ceases to be a good measure~~ | **published** — `goodharts-law.md` |
| Campbell's Law | Campbell, 1976 | Direct companion to Goodhart's: social indicators corrupt under decision-making pressure | Jevons Paradox |
| The Planning Fallacy | Kahneman & Tversky, 1979 | Systematic underestimation of time and cost even when historical data says otherwise | Vantasner Danger Meridian |
| ~~Hofstadter's Law~~ | ~~Hofstadter, 1979~~ | ~~Always takes longer than expected, even accounting for Hofstadter's Law~~ | **draft** — `hofstadters-law.md` |
| ~~Little's Law~~ | ~~Little, 1961~~ | ~~Queuing theory applied to WIP limits and engineering throughput~~ | **draft** — `littles-law.md` |
| ~~Parkinson's Law of Triviality~~ | ~~Parkinson, 1957~~ | ~~Committees fixate on trivial decisions — long debates about variable names, silence on architecture~~ | **draft** — `bikeshedding.md` (titled "Bikeshedding"). The older duplicate `parkinsons-law-of-triviality.md` was merged in and removed. |
| ~~Brooks' Law~~ | ~~Brooks, 1975~~ | ~~Adding people to a late project makes it later — the math behind communication overhead~~ | **draft** — `brooks-law.md` |
| ~~Gall's Law~~ | ~~Gall, 1977~~ | ~~Complex systems that work evolved from simple systems that worked~~ | **draft** — `galls-law.md` |
| The Second System Effect | Brooks, 1975 | The second project is always over-engineered — how engineers overcorrect from the first | Gall's Law |
| The Law of Leaky Abstractions | Spolsky, 2002 | All non-trivial abstractions leak — the gap between interface promise and implementation reality | Hyrum's Law |
| ~~Hyrum's Law~~ | ~~Wright, 2020~~ | ~~All observable behaviors of a system will be depended on — the contract you ship, not the one you promise~~ | **published** — `hyrums-law.md` |
| Zawinski's Law | Zawinski, 1990s | Every program expands until it can read email — feature creep as organizational gravity | Jevons Paradox |
| Amdahl's Law | Amdahl, 1967 | Theoretical speedup from parallelization is limited by the serial portion — applied to team scaling | Brooks' Law |
| Amara's Law | Amara | We overestimate technology short-term, underestimate long-term — calibrating AI expectations | LLM Context Biases |
| The Lindy Effect | Taleb | Things that have survived will survive proportionally longer — the case for boring technology | The Dip |
| The Red Queen Effect | Van Valen, 1973 | Must keep running just to maintain position — security patching, dependency updates, platform maintenance | Resilience |
| ~~Postel's Law~~ | ~~Postel, 1980~~ | ~~Be conservative in what you send, liberal in what you accept — API design as org policy~~ | **draft** — `postels-law.md` |
| ~~The Law of the Instrument~~ | ~~Kaplan, 1964~~ | ~~Deep expertise in one tool makes you reach for it even when a different one solves the problem better~~ | **draft** — `maslows-hammer.md` (titled "Maslow's Hammer") |
| ~~The Flywheel Effect~~ | ~~Collins, 2001~~ | ~~A thousand small, consistent turns compound into momentum. No single turn looks like the breakthrough~~ | **draft** — `flywheel-effect.md` |
| ~~The Kelly Criterion~~ | ~~Kelly, 1956~~ | ~~A Bell Labs engineer solved a telephone noise problem in 1956 and accidentally invented the formula gamblers and investors use to size every bet~~ | **draft** — `kelly-criterion.md` |

---

## Org Structure & System Design
*Like Conway's Law: structural observations about how org design produces technical outcomes.*

| Concept | Source | Post Angle | Closest Existing |
|---|---|---|---|
| The Inverse Conway Maneuver | Skelton & Pais, 2019 | Restructure the org to get the architecture you want, not the other way around | Conway's Law (draft) |
| ~~The Dunbar Number~~ | ~~Dunbar, 1992~~ | ~~Cognitive limit on stable relationships (~150) — where informal coordination breaks and formal process begins~~ | **draft** — `dunbars-number-isnt-a-number.md` (titled "Dunbar's Number Isn't a Number") |
| The Bus Factor | — | Knowledge concentration risk — how many people need to leave before the system becomes unmaintainable | Team Topologies |
| The Seam Problem | Feathers, 2004 | Complexity and failure accumulate at team and system boundaries — where Conway's Law bites hardest | Conway's Law (draft) |
| The Two-Pizza Rule | Bezos, 2002 | Team size as a proxy for communication overhead — the operational version of the Dunbar Number | Team Topologies |
| ~~The Theory of Constraints~~ | ~~Goldratt, 1984~~ | ~~Every system has one bottleneck; optimizing anything else is waste — applied to pipelines and team throughput~~ | **draft** — `theory-of-constraints.md` |
| The Ringelmann Effect | Ringelmann, 1913 | Individual contribution decreases as group size increases — the productivity math behind small teams | Brooks' Law |
| Cargo Cult Architecture | — | Adopting Netflix's architecture without Netflix's org structure — why microservices produce distributed monoliths | Conway's Law (draft) |
| Sociotechnical Systems Theory | Trist & Bamforth, 1951 | Technical and social systems must be jointly optimized — the academic ground Conway's Law stands on | Team Topologies |
| ~~Conway's Law in the Age of Agentic AI~~ | ~~Conway, 1968~~ | ~~AI agent access permissions as a communication structure~~ | **draft** — `conways-law-agentic-ai.md` |

---

## Social & Organizational Dynamics
*Like the Overton Window and Spiral of Silence: how groups form, suppress, and shift beliefs.*

| Concept | Source | Post Angle | Closest Existing |
|---|---|---|---|
| Preference Falsification | Kuran, 1995 | People publicly support positions they privately reject — why org culture shifts seem to happen overnight | Spiral of Silence |
| The Abilene Paradox | Harvey, 1974 | Groups take actions none of the members wanted — the roadmap nobody believes in but everyone approved | Spiral of Silence |
| Groupthink | Janis, 1972 | Teams make bad decisions to preserve harmony — how cohesion becomes a liability in high-stakes reviews | Spiral of Silence |
| The Cassandra Problem | — | Being right but not believed — how credible warnings get dismissed before the cost of dismissal is clear | Overton Window |
| Shifting Baselines | Pauly, 1995 | Each team accepts the dysfunction they inherited as normal — the slow drift nobody notices | Normalization of Deviance |
| The Sycophancy Trap | — | Information flowing upward gets filtered positively — leaders making decisions on a curated view of reality | Spiral of Silence |
| Voice, Exit, Loyalty | Hirschman, 1970 | Three responses to organizational decline — why the engineers who see problems most clearly leave instead of speaking | Turn the Ship Around! |
| The Shirky Principle | Shirky, 2010 | Institutions preserve the problem they solve — platform teams, internal tooling, compliance functions | Normalization of Deviance |
| Moral Hazard | Arrow, 1963 | Protection from consequences encourages riskier behavior — on-call automation, alert suppression, SLO waivers | Just Culture |
| The Iron Law of Bureaucracy | Pournelle, 1990s | Those committed to the organization outcompete those committed to its mission | Shirky Principle |
| ~~The Overton Window~~ | ~~Overton, 1990s~~ | ~~How to shift the range of acceptable proposals before making the ask~~ | **draft** — `overton-window.md` |
| ~~The Spiral of Silence~~ | ~~Noelle-Neumann, 1974~~ | ~~People with minority views stay quiet to avoid social isolation. Their silence gets read as agreement, which makes the dominant view look stronger than it is~~ | **draft** — `spiral-of-silence.md` |

---

## Cognitive Biases & Mental Models
*Non-obvious distortions in how people reason, relevant to engineering decisions and leadership.*

| Concept | Source | Post Angle | Closest Existing |
|---|---|---|---|
| The Fundamental Attribution Error | Ross, 1977 | Attributing behavior to character rather than situation — the cognitive root of blame culture | Just Culture |
| The Availability Heuristic | Tversky & Kahneman, 1973 | Judging probability by ease of recall — why the most recent incident distorts risk assessment | LLM Context Biases |
| The Anchoring Effect | Tversky & Kahneman, 1974 | First number heard disproportionately influences all subsequent estimates — project scoping, salary, timelines | Planning Fallacy |
| Optimism Bias | Sharot, 2011 | Systematic tendency to expect better outcomes than base rates justify — the engine behind the Planning Fallacy | Vantasner Danger Meridian |
| Survivorship Bias | Wald, 1943 | We study what succeeded — distorts lessons from architecture decisions, startup advice, incident reviews | LLM Context Biases |
| The Curse of Knowledge | — | Once you know something you can't remember not knowing it — documentation, onboarding, cross-team communication | LLM Context Biases |
| The Focusing Illusion | Kahneman, 2011 | Nothing is as important as you think it is while you're thinking about it — applied to prioritization and The Dip | The Dip |
| ~~The Streetlight Effect~~ | ~~—~~ | ~~Searching where it's easy to look — teams measure what's measurable and optimize for that~~ | **draft** — `streetlight-effect.md` |
| ~~The Zeigarnik Effect~~ | ~~Zeigarnik, 1927~~ | ~~Unfinished tasks stay top of mind — the cognitive cost of context switching and WIP~~ | **draft** — `zeigarnik-effect.md` |
| The Pygmalion Effect | Rosenthal & Jacobson, 1968 | High expectations cause high performance independently of ability — how managers shape outcomes through belief | Alignment Over Authority |
| The IKEA Effect | Norton et al., 2012 | People overvalue what they helped create — NIH syndrome, legacy system attachment, build vs. buy decisions | Resilience |
| ~~Agency~~ | ~~—~~ | ~~The capacity to act on what you know, even without permission or certainty~~ | **draft** — `agency.md` |
| ~~Confabulation~~ | ~~—~~ | ~~Cognitive science already built rigorous tests for confidently wrong explanations, decades before anyone had a transformer to test~~ | **draft** — `confabulation.md` |
| ~~Curiosity~~ | ~~Loewenstein, 1994~~ | ~~Curiosity is triggered by the gap between what you know and what you want to know. Organizations that reward answers over questions close that gap the wrong way~~ | **draft** — `curiosity.md` |
| ~~First Principles~~ | ~~—~~ | ~~Barely started — just a reference link (fpt.guide), no draft content yet~~ | **draft** — `first-principles.md` |
| ~~Negative Capability~~ | ~~Keats, 1817~~ | ~~Keats' term for staying in uncertainty without reaching for premature answers, where the best debugging and architecture decisions start~~ | **draft** — `negative-capability.md` |
| ~~Satisficing~~ | ~~Simon, 1956~~ | ~~The best decision is often the fastest one you can defend, not the optimal one you never finish searching for~~ | **draft** — `satisficing.md` |
| ~~The Vantasner Danger Meridian~~ | ~~—~~ | ~~The threshold past which danger to your mission increases exponentially. Recognizing it before crossing separates deliberate risk from drift~~ | **draft** — `vantasner-danger-meridian.md` |
| ~~Disagreeable Giver~~ | ~~Grant, 2013~~ | ~~Whether feedback feels harsh or pleasant tells you nothing about whether the person giving it is trying to help you~~ | **draft** — `disagreeable-giver.md` |

---

## Org Effects & Principles
*Patterns in how organizations behave under pressure, at scale, or over time.*

| Concept | Source | Post Angle | Closest Existing |
|---|---|---|---|
| The Swiss Cheese Model | Reason, 1990 | Barriers have holes; incidents happen when holes align — layered defenses and why they still fail | Just Culture |
| High Reliability Organizations | Weick & Sutcliffe, 2001 | How some orgs operate safely in high-risk environments — the practices behind preoccupation with failure | Chaos Engineering |
| The Pre-Mortem | Klein, 2007 | Imagine the project already failed and work backwards — structured way to surface what silence suppresses | Vantasner Danger Meridian |
| Broken Windows Theory | Wilson & Kelling, 1982 | Visible disorder invites more disorder — failing tests nobody fixes, runbook drift, silenced alerts | Normalization of Deviance |
| The Peltzman Effect | Peltzman, 1975 | Safety measures lead to riskier behavior — monitoring that creates complacency instead of vigilance | Chaos Engineering |
| The Cobra Effect | Siebert, 2001 | Incentives that make the problem worse — bug bounties, on-call metrics, deployment frequency targets | Just Culture |
| ~~Chesterton's Fence~~ | ~~Chesterton, 1929~~ | ~~Don't remove what you don't understand — legacy code, inherited architecture, the flag nobody can explain~~ | **draft** — `chestertons-fence.md` |
| The Bystander Effect | Latané & Darley, 1968 | Diffusion of responsibility — nobody fixes the obvious bug because team size makes ownership ambiguous | Just Culture |
| The Tragedy of the Commons | Hardin, 1968 | Shared resources get depleted — shared codebases, shared infrastructure, accumulated technical debt | Resilience |
| The Principal-Agent Problem | Jensen & Meckling, 1976 | Misaligned incentives between those who decide and those who execute — outsourcing, vendor relationships | Alignment Over Authority |
| The Safety-II Framework | Hollnagel, 2014 | Study what goes right, not just what goes wrong — how systems succeed under variable conditions | Resilience |
| ~~The Andon Cord~~ | ~~Ohno, 1988~~ | ~~Toyota's stop-the-line authority given to any worker. The software equivalent is deploying a culture that makes stopping cheap~~ | **draft** — `andon-cord.md` |
| ~~The People Closest to the Work~~ | ~~Deming, 1986~~ | ~~The people running a process see problems and fixes that management, several layers removed, never will~~ | **draft** — `closest-to-the-work.md` |
| ~~Explore, Expand, Extract~~ | ~~Beck~~ | ~~Kent Beck's decade at Facebook watching products succeed and fail for a reason nobody named. The practices that work when you don't know what you're building kill you once you do~~ | **draft** — `explore-expand-extract.md` |

---

## Resilience Engineering
*Concepts from the academic field of Resilience Engineering — Dekker, Cook, Hollnagel, Perrow, Rasmussen. Ranked by closeness to existing posts.*

| Concept | Source | Post Angle | Closest Existing |
|---|---|---|---|
| ~~How Complex Systems Fail~~ | ~~Cook, 1998~~ | ~~18 observations on why complex systems fail. TMI and Challenger show causes distributed across time, systems, and organizational boundaries~~ | **published** — `distributed-causation.md` (titled "Distributed Causation") |
| The ETTO Principle | Hollnagel, 2009 | Efficiency-Thoroughness Tradeoff — under pressure people choose efficiency, and that choice accumulates | Normalization of Deviance |
| Drift into Failure | Dekker, 2011 | Organizations drift incrementally into unsafe states through individually rational decisions — systemic, not individual | Normalization of Deviance |
| The Rasmussen Migration Model | Rasmussen, 1997 | Economic and workload pressure continuously push orgs toward the boundary of safe operation — safety is always fighting a current | Normalization of Deviance |
| ~~Normal Accident Theory~~ | ~~Perrow, 1984~~ | ~~Complex, tightly coupled systems will inevitably fail regardless of effort~~ | **draft** — `normal-accident-theory.md` |
| ~~The Ironies of Automation~~ | ~~Bainbridge, 1983~~ | ~~Automation degrades the human skill it needs as a fallback~~ | **draft** — `ironies-of-automation.md` |
| The Practical Drift | Snook, 2000 | Gradual departure from formal procedures toward local norms that work until they don't — distinct from Normalization of Deviance | Normalization of Deviance |
| ~~Requisite Imagination~~ | ~~Adamski & Westrum, 2004~~ | ~~The capacity to imagine failure modes that haven't happened yet~~ | **draft** — `requisite-imagination.md` |
| ~~Normalization of Deviance~~ | ~~Vaughan, 1996~~ | ~~Teams gradually accept dangerous deviations as normal when nothing bad happens. Each success makes the next deviation easier to justify~~ | **draft** — `normalization-of-deviance.md` |
| ~~STAMP~~ | ~~Leveson, 2004~~ | ~~Traditional accident models assume every failure traces back to a broken part. STAMP treats safety as a control problem instead, which explains disasters where every component worked as designed~~ | **published** — `functioning-as-designed.md` (titled "Functioning As Designed") |
| ~~Robustness vs. Resilience vs. Brittleness~~ | ~~Woods, 2018~~ | ~~Notes from Todd Conklin's podcast on the 2022 Southwest meltdown. Still rough and transcript-heavy, needs the full rewrite pass it never got~~ | **published, needs rewrite** — `robust-and-brittle.md` (titled "Robust and Brittle... The Southwest Failure") |

---

## Miscellaneous

Ideas that don't fit a theme above cleanly.

| Title | File | Angle |
|---|---|---|
| The Luddites Were Right | `luddites.md` | The original Luddites weren't against machines — they were against wages being destroyed by machines, an argument being remade today |
| n% of People Are Not Cut Out to Join a Startup | `startups.md` | Very early draft, rough prose on what makes someone suited to startup chaos, not yet fact-checked or structured |
| Building Wealthfront and Benchmark Capital | `building-wealthfront-and-benchmark-capital.md` | Podcast notes on Andy Rachleff — product-market fit, hiring, and building Wealthfront. Rough, not yet structured into a post |
