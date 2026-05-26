---
title: "Goodhart's Law"
date: 2026-05-24
publishdate: 2026-05-24
lastmod: 2026-05-24
summary: "When a measure becomes a target, it ceases to be a good measure. Engineering teams discover this whenever a metric used to track health becomes the thing being managed instead."
tags: ["management", "metrics", "leadership"]
image:
draft: true
---

## Goodhart's Law

The engineering director set a goal: open bug count below 100 by end of quarter. In January the tracker showed 347. By March it showed 94.

The team had not fixed 253 bugs. They had reclassified them. Won't fix. By design. Duplicate. Known issue, not yet prioritized. Each call was defensible in isolation.

A customer filed a ticket in April describing behavior the team had marked by design six weeks earlier. The tracker showed 91 open bugs. The ticket made it 92.

## What Is Goodhart's Law?

Charles Goodhart was a British economist advising the Bank of England in the 1970s. His observation grew from monetary policy: any statistical regularity will tend to collapse once pressure is placed on it for control purposes (Goodhart, 1975). The Bank was targeting monetary aggregates as a proxy for economic control. The moment it did, the relationship between the aggregate and the underlying economy stopped holding.

Marilyn Strathern generalized the principle while studying audit practices in British universities: "When a measure becomes a target, it ceases to be a good measure" (Strathern, 1997). That rephrasing strips the economic context and exposes the mechanism.

Donald Campbell identified the same pattern in social science: the more any quantitative indicator is used for high-stakes decision-making, the more subject it becomes to corruption pressures and the more it distorts the processes it was meant to monitor (Campbell, 1979). Goodhart's Law names the technical failure. Campbell's Law names the organizational pressure that causes it.

## The Mechanism

A metric is useful because it correlates with something that matters. Deployment frequency correlates with the ability to deliver value quickly and recover from mistakes. Test coverage correlates with confidence in code correctness. The metric is not the thing. It is a signal that tracks the thing.

When a metric becomes a target, two things happen. People optimize directly for the metric. And the correlation that made the metric valuable weakens or disappears.

This is not fraud. It is rational adaptation. When deployment frequency is a target, the fastest path to hitting it is small, low-risk deployments that carry minimal value. The team deploys frequently. The underlying capability the metric was meant to track, the ability to ship meaningful changes safely, may be atrophying. The metric looks healthy. The system isn't.

Steven Kerr described this structural problem in 1975 as the folly of rewarding A while hoping for B. The measurement determines the behavior. The stated goal is irrelevant if the measurement doesn't track it (Kerr, 1975).

## Where Engineering Metrics Break

**Sprint velocity** is the textbook case. Story points are a calibration tool. A team's historical velocity helps estimate how much work fits in a sprint. Once velocity becomes a target or a cross-team comparison, estimation inflates. Engineers pad points. Work gets split artificially. The number stabilizes. The throughput of meaningful work doesn't.

**DORA metrics** are subtler. The four key metrics (deployment frequency, lead time for changes, change failure rate, and time to restore service) were developed as research-backed indicators of high-performing engineering organizations (Forsgren et al., 2018). They correlate with delivery performance because high-performing teams exhibit them naturally. When an org mandates them as targets, teams optimize the metrics directly. Deployment frequency rises because trivial config changes count as deployments. Lead time drops because work gets split into units that skip meaningful review. The metrics go green. The underlying capability is never built.

**Test coverage percentage** creates a specific failure mode. A coverage target incentivizes tests that execute code without asserting useful behavior. The percentage reaches 95. The tests pass on code that returns incorrect results because nobody verified what the tests actually check.

**Bug backlog size** invites reclassification. When teams are measured on open bug count, bugs get moved to known-issues lists, closed as by-design, or marked as duplicates. The backlog shrinks. The software quality doesn't.

## Common Mistakes

**Treating this as a reason to stop measuring.** Goodhart's Law explains why a specific metric breaks under target pressure. It does not argue against measurement. Teams without metrics have no basis for learning or decisions. The law is a warning about what happens to any metric when it becomes the goal, not a prohibition on metrics.

**Adding more metrics to compensate.** A common response is a second metric to catch gaming of the first, then a third. Each becomes a new target and the cycle repeats. The problem is not the number of metrics. It is the belief that hitting a set of numbers equals achieving the underlying goal.

**Blaming the people who adapted.** The team that inflated velocity estimates was not behaving badly. They responded rationally to the incentive structure they were given. Goodhart's Law is a systems problem. The metric created the pressure. The pressure produced the behavior. Replacing the people without changing the structure produces the same outcome with different people (Deming, 1986).

**Assuming transparency prevents gaming.** Broad dashboards, public metrics, added context: these reduce some forms of gaming. They don't eliminate structural pressure. When team survival or promotion depends on a number, people find ways to produce that number. Visibility changes which methods are socially acceptable, not whether gaming happens.

## Put It Into Practice

Look at the metrics your team is currently measured on. For each one, ask: what behavior would a rational person exhibit if this were their only goal? If that behavior diverges from what you actually want, the metric is already working against you, whether or not anyone is gaming it yet.

Then ask how the metric is being used. A metric consulted in a retrospective to understand trends does different work than a metric reported upward as evidence of team health. The same number does different damage under different levels of organizational pressure.

Pick one metric that has become a target. Spend thirty minutes with the team asking what it would look like for that metric to appear healthy while the underlying reality got worse. Write down the answers. Those are the gaming strategies your team already knows are available, and that list is where the real conversation starts.

## References

Campbell, Donald T. (1979). "Assessing the Impact of Planned Social Change." *Evaluation and Program Planning*, 2(1): 67-90. [https://ideas.repec.org/a/eee/epplan/v2y1979i1p67-90.html](https://ideas.repec.org/a/eee/epplan/v2y1979i1p67-90.html)

Deming, W. Edwards (1986). *Out of the Crisis*. MIT Press. [https://direct.mit.edu/books/monograph/4192/Out-of-the-Crisis](https://direct.mit.edu/books/monograph/4192/Out-of-the-Crisis)

Forsgren, Nicole, Jez Humble, and Gene Kim (2018). *Accelerate: The Science of Lean Software and DevOps*. IT Revolution Press. [https://itrevolution.com/product/accelerate/](https://itrevolution.com/product/accelerate/)

Goodhart, Charles (1975). "Problems of Monetary Management: The UK Experience." *Papers in Monetary Economics*, Reserve Bank of Australia. [https://www.semanticscholar.org/paper/Problems-of-Monetary-Management%3A-The-UK-Experience-Goodhart/0ae623749b30de53a39cf05813f5f3842e422c01](https://www.semanticscholar.org/paper/Problems-of-Monetary-Management%3A-The-UK-Experience-Goodhart/0ae623749b30de53a39cf05813f5f3842e422c01)

Kerr, Steven (1975). "On the Folly of Rewarding A, While Hoping for B." *Academy of Management Journal*, 18(4): 769-783. [https://www.jstor.org/stable/255378](https://www.jstor.org/stable/255378)

Strathern, Marilyn (1997). "Improving Ratings: Audit in the British University System." *European Review*, 5(3): 305-321. [https://gwern.net/doc/statistics/decision/1997-strathern.pdf](https://gwern.net/doc/statistics/decision/1997-strathern.pdf)

## Changelog

**2026-05-24** Initial publication.
