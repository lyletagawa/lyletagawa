---
title: "Goodhart's Law"
date: 2026-06-07
publishdate: 2026-06-07
lastmod: 2026-06-07
summary: "When a measure becomes a target, it ceases to be a good measure. Engineering teams discover this whenever a metric used to track health becomes the thing being managed instead."
tags: ["management", "metrics", "leadership"]
image: /images/goodharts-law.png
draft: false
---

![](/images/goodharts-law.png)
*Image generated with Google Gemini (2026).*

## Goodhart's Law

The engineering org had twelve open roles and a deadline. Leadership wanted headcount filled before the fiscal year closed, and the recruiting team got a new time-to-fill metric with a 30-day target per role.

Phone screens got shorter, hiring panels went from four rounds to two, and reference checks got faster. The quality bar didn't vanish, but speed was what got measured.

Time-to-fill dropped to 23 days, down from 54. Leadership cited it as evidence the process had improved. Six months later, four of those new hires had already left.

## What Is Goodhart's Law?

Any statistical regularity will tend to collapse once pressure is placed on it for control purposes (Goodhart, 1975). That observation came from monetary policy. The Bank of England was targeting money supply figures as a lever for steering the broader economy. Once those figures became a target, the relationship with the underlying economy broke down.

A study of British university audits captured the same pattern more broadly. "When a measure becomes a target, it ceases to be a good measure" (Strathern, 1997). That formulation strips the economic context and exposes the mechanism.

The same pattern appears in social science. The more any quantitative indicator is used for high-stakes decision-making, the more subject it becomes to corruption pressures and the more it distorts the processes it was meant to monitor (Campbell, 1979). Goodhart's Law names the technical failure. Campbell's Law names the organizational pressure that causes it.

## The Mechanism

A metric is useful because it correlates with something that matters. Deployment frequency correlates with the ability to deliver value quickly and recover from mistakes. Test coverage correlates with confidence in code correctness. A metric tracks a signal, not the underlying reality.

When a metric becomes a target, two things happen. People optimize directly for the metric. And the correlation that made the metric valuable weakens or disappears.

This is rational adaptation, not fraud. When deployment frequency is a target, the fastest path to hitting it is small, low-risk deployments that carry minimal value. The team deploys frequently. The underlying capability the metric was meant to track, the ability to ship meaningful changes safely, may be atrophying. The metric looks healthy. The system isn't.

The measurement determines the behavior. The stated goal is irrelevant if the measurement doesn't track it. Rewarding A while hoping for B is the underlying folly (Kerr, 1975).

## Where Engineering Metrics Break

**Sprint velocity** is the textbook case. Story points are a calibration tool. A team's historical velocity helps estimate how much work fits in a sprint. Once velocity becomes a target or a cross-team comparison, estimation inflates. Engineers pad points, work gets split artificially, and the number stabilizes. The throughput of meaningful work doesn't.

**DORA metrics** are subtler. The four key metrics (deployment frequency, lead time for changes, change failure rate, and time to restore service) were developed as research-backed indicators of high-performing engineering organizations (Forsgren et al., 2018). They correlate with delivery performance because high-performing teams exhibit them naturally. When an org mandates them as targets, teams optimize the metrics directly. Deployment frequency rises because trivial config changes count as deployments. Lead time drops because work gets split into units that skip meaningful review. The metrics go green. The underlying capability is never built.

**Test coverage percentage** creates a specific failure mode. A coverage target incentivizes tests that execute code without asserting useful behavior. The percentage reaches 95. The tests pass on code that returns incorrect results because nobody verified what the tests actually check.

**Bug backlog size** invites reclassification. When teams are measured on open bug count, bugs get moved to known-issues lists, closed as by-design, or marked as duplicates. The backlog shrinks. The software quality doesn't.

## Common Mistakes

**Treating this as a reason to stop measuring.** Goodhart's Law explains why a specific metric breaks under target pressure. It does not argue against measurement. Teams without metrics have no basis for learning or decisions. The law is a warning about what happens to any metric when it becomes the goal, not a prohibition on metrics.

**Adding more metrics to compensate.** A common response is a second metric to catch gaming of the first, then a third. Each becomes a new target and the cycle repeats. The real problem is believing that hitting a set of numbers equals achieving the underlying goal.

**Blaming the people who adapted.** The team that inflated velocity estimates was not behaving badly. They responded rationally to the incentive structure they were given. Goodhart's Law is a systems problem. The metric created the pressure that produced the behavior. Replacing the people without changing the structure produces the same outcome with different people (Deming, 1986).

**Assuming transparency prevents gaming.** Broad dashboards, public metrics, and added context reduce some forms of gaming. They don't eliminate structural pressure. When team survival or promotion depends on a number, people find ways to produce that number. Visibility changes which methods are socially acceptable, not whether gaming happens.

## Put It Into Practice

Look at the metrics your team is currently measured on. For each one, ask what behavior a rational person would exhibit if this were their only goal. If that behavior diverges from what you actually want, the metric is already working against you, whether or not anyone is gaming it yet.

Then ask how the metric is being used. A metric consulted in a retrospective to understand trends does different work than a metric reported upward as evidence of team health. The same number does different damage under different levels of organizational pressure.

Pick one metric that has become a target. Spend thirty minutes with the team asking what it would look like for that metric to appear healthy while the underlying reality got worse. Write down the answers. Those are the gaming strategies your team already knows are available, and that list is where the real conversation starts.

---

## References

Campbell, Donald T. (1979). "Assessing the Impact of Planned Social Change." *Evaluation and Program Planning*, 2(1): 67-90. [https://ideas.repec.org/a/eee/epplan/v2y1979i1p67-90.html](https://ideas.repec.org/a/eee/epplan/v2y1979i1p67-90.html)

Deming, W. Edwards (1986). *Out of the Crisis*. MIT Press. [https://direct.mit.edu/books/monograph/4192/Out-of-the-Crisis](https://direct.mit.edu/books/monograph/4192/Out-of-the-Crisis)

Forsgren, Nicole, Jez Humble, and Gene Kim (2018). *Accelerate: The Science of Lean Software and DevOps*. IT Revolution Press. [https://itrevolution.com/product/accelerate/](https://itrevolution.com/product/accelerate/)

Goodhart, Charles (1975). "Problems of Monetary Management: The UK Experience." *Papers in Monetary Economics*, Reserve Bank of Australia. [https://www.semanticscholar.org/paper/Problems-of-Monetary-Management%3A-The-UK-Experience-Goodhart/0ae623749b30de53a39cf05813f5f3842e422c01](https://www.semanticscholar.org/paper/Problems-of-Monetary-Management%3A-The-UK-Experience-Goodhart/0ae623749b30de53a39cf05813f5f3842e422c01)

Kerr, Steven (1975). "On the Folly of Rewarding A, While Hoping for B." *Academy of Management Journal*, 18(4): 769-783. [https://www.jstor.org/stable/255378](https://www.jstor.org/stable/255378)

Strathern, Marilyn (1997). "Improving Ratings: Audit in the British University System." *European Review*, 5(3): 305-321. [https://gwern.net/doc/statistics/decision/1997-strathern.pdf](https://gwern.net/doc/statistics/decision/1997-strathern.pdf)

---

## Outtakes

**The cobra effect.** British colonial administrators in India offered bounties for dead cobras to reduce the population. Locals began breeding cobras for the reward. When the program ended and breeders released their snakes, the cobra population increased (Siebert, 2001).

**The Soviet nail quota.** Soviet factories were given production quotas that were measured by the number of nails made tiny, unusable nails. Planners switched to weight quotas. Factories made massive, unusable nails. (Nove, 1977).

**Wells Fargo.** Wells Fargo employees opened millions of unauthorized accounts over more than a decade to meet cross-selling targets (CFPB, 2016). The metric was accounts opened per customer. The goal was customer relationships. The bank paid $3 billion in settlements.

**UK emergency wait times.** When the National Health Service introduced a four-hour emergency department wait target, some hospitals held patients in ambulances outside to avoid starting the clock. The wait time metric improved while patients waited longer (Bevan and Hood, 2006).

---

## Changelog

**2026-06-07** Initial publication.
