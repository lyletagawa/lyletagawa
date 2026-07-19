---
title: "Explore, Expand, Extract"
date: 2026-07-11
publishdate: 2026-07-11
lastmod: 2026-07-11
summary: "Kent Beck spent a decade at Facebook watching products succeed and fail for a reason nobody named. The practices that work when you don't know what you're building kill you once you do."
tags: ["strategy", "product", "teams"]
image: /images/explore-expand-extract.jpg
draft: true
---

![Kent Beck spent a decade at Facebook watching products succeed and fail for a reason nobody named. The practices that work when you don't know what you're building kill you once you do.](/images/explore-expand-extract.jpg)
*Kent Beck speaking in 2005. Photo: [Barry Mulling](https://commons.wikimedia.org/wiki/File:Kent_Beck_tells_it_like_it_is,_2005.jpg). CC BY-SA 2.0.*

## Explore, Expand, Extract

A team ships a rough prototype nobody's sure will work. Before the first hundred users see it, it needs a design review, a security audit, and a rollout plan reviewed by three other teams. Six weeks later the prototype finally ships, to the hundred users it was always going to ship to, having taught the team nothing they didn't already suspect and cost them the head start that mattered.

A different team, a year later, is running the payments system that same company now depends on for its revenue. They're still shipping the way the prototype team did: fast, loose, one engineer's judgment call away from a change going out. Nobody built the guardrails, because guardrails felt like the thing that used to slow prototypes down.

Same company, same instincts, both wrong. Kent Beck spent roughly a decade at Facebook watching mismatches like these happen in both directions, and eventually named the pattern{{< cite 1 "Kent Beck (2018). 3X Explore, Expand, Extract. YOW! 2018." >}}.

## What Is 3X?

Beck's framework, first presented publicly at YOW! 2018 in Melbourne, argues that a product moves through three distinct phases, and the practices that make you effective in one phase actively hurt you in another{{< cite 1 "Kent Beck (2018). 3X Explore, Expand, Extract. YOW! 2018." >}}.

**Explore** is what the prototype team was in. The goal, in Beck's words, is "to overcome disinterest, try many small experiments{{< cite 2 "Kent Beck (2021). Fast/Slow in 3X: Explore/Expand/Extract. Medium." >}}." You don't know what you're building yet, so the only thing worth optimizing is how many things you can try and how fast you find out which one lands.

**Expand** is what happens once something in Explore actually works. The goal shifts "to overcome bottlenecks to scaling, ease the limitations of the next rate-limiting resource{{< cite 2 "Kent Beck (2021). Fast/Slow in 3X: Explore/Expand/Extract. Medium." >}}." At Facebook this looked like engineers swarming a single knotty technical problem, whatever was currently capping growth, then moving to the next one{{< cite 5 "How Kent Beck Shapes the Software Industry. The Pragmatic Engineer." >}}.

**Extract** is where the payments team was living without admitting it. The shape of the problem is known, the goal is "to sustain growth, continually increase profitability while you finish growing{{< cite 2 "Kent Beck (2021). Fast/Slow in 3X: Explore/Expand/Extract. Medium." >}}." Playbooks, automation, and economies of scale matter more than speed of discovery, because there's nothing left to discover.

## The Math Underneath It

Beck has said he can derive 3X directly from the Kelly Criterion, the formula for how much of your bankroll to bet given the odds and the payoff{{< cite 3 "Kent Beck (2021). Twitter/X post on deriving 3X from the Kelly Criterion." >}}. Explore is the convex bet: many small stakes, most losing, occasionally paying off enormously, which is the correct shape when the probability of failure is high and you don't yet know the odds. Extract is the concave bet: large, well-understood positions where the job is protecting the margin and avoiding a catastrophic loss, not chasing a jackpot.

The math explains why the instincts actually conflict. A strategy that's rational for one bet shape is close to reckless for the other. Running your payments system like an experiment isn't just stylistically wrong. It's the wrong side of the same formula.

## What Changes In Each Phase

Quality practices are the clearest place this shows up. In Explore, the correct amount of process is close to none, TDD and pairing to move fast without breaking your own feedback loop, and nothing more. Heavy documentation and full GUI test automation suites are money spent proving something works before you know if anyone wants it{{< cite 4 "Charrett, Anne-Marie. Kent Beck's 3X Model and Quality: A Quality Coach Perspective." >}}.

In Expand, testing grows at exactly the layer that's actually scaling, API-level automation, accessibility, usability, while resisting the urge to standardize process across every team. In Extract, the calculus flips completely. The system can now absorb rigor it couldn't have afforded earlier, and it needs to, because a failure at that scale costs far more than it would have during Explore{{< cite 4 "Charrett, Anne-Marie. Kent Beck's 3X Model and Quality: A Quality Coach Perspective." >}}.

## Common Mistakes

**Applying Extract rigor before product-market fit.** Design reviews, audits, and rollout committees for something that might get thrown away next week isn't caution. It's spending Extract-phase money on an Explore-phase bet.

**Carrying Explore's looseness into Expand.** The lack of process that made the prototype fast becomes the reason nobody can debug the thing at ten times the traffic. Technical debt that was invisible at low scale becomes the whole story at higher scale.

**Delaying Extract discipline on a product that's already mature.** A payments system, an auth system, anything load-bearing, needs guardrails proportional to what happens when it breaks, not to how it felt to build originally.

**Treating this as one company-wide setting.** A single company runs Explore, Expand, and Extract simultaneously across different products, sometimes across different parts of the same product. The mistake isn't picking the wrong phase. It's assuming there's only one phase to pick.

## Put It Into Practice

Before deciding how much process a piece of work deserves, name its phase out loud. Not the company's phase, that specific initiative's phase. A new feature inside a mature product is still an Explore bet even if the codebase around it is deep into Extract.

Audit one existing process, a review step, a testing requirement, a rollout gate, and ask which phase it was actually designed for. If it was built for Extract and you're applying it in Explore, you're paying for certainty you don't need yet.

Watch for the signal that a phase has quietly changed underneath you. Rising traffic without corresponding investment in reliability is Expand pretending it's still Explore. Rising incident cost without corresponding investment in guardrails is Extract pretending it's still Expand.

## Go Further

**The full talk.** Beck's original YOW! 2018 presentation covers the framework with more nuance and more examples than any summary, including his own, can carry{{< cite 1 "Kent Beck (2018). 3X Explore, Expand, Extract. YOW! 2018." >}}.

**The Kelly Criterion itself.** For the mathematically curious, the original betting formula Beck derived 3X from has its own long history in gambling and portfolio theory, well outside software entirely.

---

## References

<ol class="references">
  <li id="ref-1">Kent Beck (2018). "3X Explore, Expand, Extract." YOW! 2018, Melbourne. <a href="https://www.youtube.com/watch?v=WazqgfsO_kY">https://www.youtube.com/watch?v=WazqgfsO_kY</a></li>
  <li id="ref-2">Kent Beck (2021). "Fast/Slow in 3X: Explore/Expand/Extract." Medium. <a href="https://medium.com/@kentbeck_7670/fast-slow-in-3x-explore-expand-extract-6d4c94a7539">https://medium.com/@kentbeck_7670/fast-slow-in-3x-explore-expand-extract-6d4c94a7539</a></li>
  <li id="ref-3">Kent Beck (2021). Post on deriving 3X from the Kelly Criterion. X (formerly Twitter). <a href="https://x.com/KentBeck/status/1379564472335880195">https://x.com/KentBeck/status/1379564472335880195</a></li>
  <li id="ref-4">Charrett, Anne-Marie. "Kent Beck's 3X Model and Quality: A Quality Coach Perspective." <a href="https://www.annemariecharrett.com/kent-becks-3x-and-quality-a-quality-coach-perspective/">https://www.annemariecharrett.com/kent-becks-3x-and-quality-a-quality-coach-perspective/</a></li>
  <li id="ref-5">"How Kent Beck Shapes the Software Industry." <em>The Pragmatic Engineer</em>. <a href="https://newsletter.pragmaticengineer.com/p/how-kent-beck-shapes-the-software">https://newsletter.pragmaticengineer.com/p/how-kent-beck-shapes-the-software</a></li>
</ol>

---

## Outtakes

**Beck developed the framework during his own decade at the company it describes.** He joined Facebook around 2011 and stayed roughly ten years, the same stretch of time during which he wrote and refined 3X ([Pragmatic Engineer, n.d.](https://newsletter.pragmaticengineer.com/p/how-kent-beck-shapes-the-software)).

**The Kelly Criterion wasn't originally about betting at all.** John Kelly derived it at Bell Labs in 1956 to solve a signal transmission problem for noisy phone lines. Gamblers only adopted it later, after Claude Shannon himself took the formula to Las Vegas ([Wikipedia, n.d.](https://en.wikipedia.org/wiki/John_Larry_Kelly_Jr.)).

---

## Changelog

**2026-07-11** Initial release. Pass 2: softened an overclaim in the intro (a citation had implied Beck described the exact fictional scenario used to illustrate the concept), and added a missing citation for the Facebook "engineers swarm a bottleneck" claim, which was stated as fact without a source.
