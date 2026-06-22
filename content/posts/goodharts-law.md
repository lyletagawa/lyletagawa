---
title: "Goodhart's Law"
date: 2026-06-07
publishdate: 2026-06-07
lastmod: 2026-06-17
summary: "Every metric used to track progress will eventually be used to fake progress instead."
tags: ["management", "metrics"]
image: /images/goodharts-law.png
draft: false
---

![](/images/goodharts-law.png)
*Image generated with Google Gemini (2026).*

## Goodhart's Law

The engineering org had twelve open roles and 30 days left in the fiscal year. Leadership introduced a new metric with a 30-day time-to-fill target per role.

Phone screens got shorter and panels went from four rounds to two. Reference checks moved faster, and nobody announced a lower bar. Speed was just what got measured now.

Time-to-fill dropped to 23 days, down from 54. Leadership cited it as proof the process had improved. Six months later, four of those hires had already left.

That's Goodhart's Law. Your team has probably run this playbook without calling it that.

## What Is Goodhart's Law?

Here's the academic version. "Any statistical regularity will tend to collapse once pressure is placed on it for control purposes" {{< cite 1 "Goodhart, Charles (1975). Problems of Monetary Management: The UK Experience. Papers in Monetary Economics, Reserve Bank of Australia." >}}. Charles Goodhart was writing about British monetary policy. The Bank of England targeted money supply figures to steer the economy. Once those figures became a target, their relationship with the underlying economy broke down.

Marilyn Strathern distilled it to a single line. "When a measure becomes a target, it ceases to be a good measure" {{< cite 2 "Strathern, Marilyn (1997). Improving Ratings: Audit in the British University System. European Review, 5(3): 305-321." >}}.

Both describe the same thing. A metric is valuable because it correlates with something real. Once you start managing the metric instead of the thing it tracks, that correlation breaks. You're left with a number that looks fine while the underlying reality deteriorates.

Campbell's Law names the organizational pressure behind the same pattern. "The more any quantitative social indicator is used for social decision-making, the more subject it will be to corruption pressures and the more apt it is to distort and corrupt the social processes it is intended to monitor" {{< cite 3 "Campbell, Donald T. (1979). Assessing the Impact of Planned Social Change. Evaluation and Program Planning, 2(1): 67-90." >}}. Goodhart names the technical failure. Campbell names the incentive structure that causes it.

## How the Correlation Breaks

A metric is useful because it correlates with something that matters. Deployment frequency correlates with the ability to ship value quickly and recover from mistakes. Test coverage correlates with confidence in code correctness. The metric tracks a signal, not the underlying reality itself.

When the metric becomes a target, two things happen. People optimize directly for it. And the correlation that made it valuable weakens or disappears.

Nobody is cheating. They're doing exactly what the incentive structure asks of them.

When deployment frequency is a target, the fastest path to hitting it is small, low-risk deployments that carry minimal value. The team deploys frequently. But the thing the metric was tracking, the ability to ship meaningful changes safely, may be quietly getting worse. The numbers look healthy, but the system may not be.

The measurement determines the behavior. The stated goal is irrelevant if the measurement doesn't track it.

## Where Engineering Metrics Break

**Sprint velocity** is the textbook case. Story points are a calibration tool. A team's historical velocity helps estimate how much work fits in a sprint. Once velocity becomes a target or a cross-team comparison, estimation inflates. Engineers pad points and work gets split artificially. The throughput of meaningful work doesn't improve and may actually regress. The velocity number goes up, but the output doesn't.

**DORA metrics** are subtler. Deployment frequency, lead time for changes, change failure rate, and time to restore service were identified as markers of high-performing engineering organizations {{< cite 4 "Forsgren, Nicole, Jez Humble, and Gene Kim (2018). Accelerate: The Science of Lean Software and DevOps. IT Revolution Press." >}}. They work because capable teams naturally produce them. When an org mandates them as targets, teams optimize directly for the metrics. Deployment frequency rises on trivial config changes. Lead time drops when features get carved into meaningless units. The numbers improve, but the engineering doesn't.

**Bug backlog size** encourages reclassification. When teams are measured on open bug count, bugs get moved to known-issues lists, closed as by-design, or marked as duplicates. The backlog shrinks, but the software doesn't get better.

## How Teams Get This Wrong

**Treating this as a reason to stop measuring.** Goodhart's Law explains why a specific metric breaks under target pressure. It doesn't argue against measurement. Teams without metrics have no basis for learning or decisions. It's a warning about what happens to any metric when it becomes the goal, not a prohibition on measurement itself.

**Adding more metrics to compensate.** The classic response is a second metric to catch gaming of the first, then a third for the second. Each becomes a new target and the cycle repeats. The real problem is believing that hitting a set of numbers equals achieving the underlying goal. More metrics doesn't fix that. It just creates more targets.

**Blaming the people who adapted.** The team that inflated velocity estimates wasn't behaving badly. They responded rationally to the incentive structure they were given. Goodhart's Law is a systems problem. The metric created the pressure that produced the behavior. Replace the people without changing the structure and you get the same outcome with different people {{< cite 5 "Deming, W. Edwards (1986). Out of the Crisis. MIT Press." >}}. It's obvious in retrospect and invisible in the moment you're living it.

**Assuming transparency fixes it.** Public dashboards and broader visibility reduce some forms of gaming. They don't eliminate structural pressure. When team survival or promotion depends on a number, people find ways to produce that number. Transparency changes which methods are socially acceptable. It doesn't change whether gaming happens.

## Put It Into Practice

Look at the metrics your team is currently measured on. For each one, ask what behavior a rational person would exhibit if this were their only goal. If that behavior diverges from what you actually want, the metric is already working against you, whether or not anyone is gaming it yet.

Then ask how the metric is actually being used. A metric consulted in a retrospective to understand trends does different work than the same metric reported upward as evidence of team health. The same number carries very different damage potential depending on how it's used.

Pick one metric that has become a target. Spend thirty minutes with the team asking what it would look like for that metric to appear healthy while the underlying reality got worse. Write down the answers. Those are the gaming strategies your team already knows are available, and that list is where the real conversation starts.

---

## References

<ol class="references">
  <li id="ref-1">Goodhart, Charles (1975). "Problems of Monetary Management: The UK Experience." <em>Papers in Monetary Economics</em>, Reserve Bank of Australia. <a href="https://www.semanticscholar.org/paper/Problems-of-Monetary-Management%3A-The-UK-Experience-Goodhart/0ae623749b30de53a39cf05813f5f3842e422c01">https://www.semanticscholar.org/paper/Problems-of-Monetary-Management%3A-The-UK-Experience-Goodhart/0ae623749b30de53a39cf05813f5f3842e422c01</a></li>
  <li id="ref-2">Strathern, Marilyn (1997). "Improving Ratings: Audit in the British University System." <em>European Review</em>, 5(3): 305-321. <a href="https://gwern.net/doc/statistics/decision/1997-strathern.pdf">https://gwern.net/doc/statistics/decision/1997-strathern.pdf</a></li>
  <li id="ref-3">Campbell, Donald T. (1979). "Assessing the Impact of Planned Social Change." <em>Evaluation and Program Planning</em>, 2(1): 67-90. <a href="https://ideas.repec.org/a/eee/epplan/v2y1979i1p67-90.html">https://ideas.repec.org/a/eee/epplan/v2y1979i1p67-90.html</a></li>
  <li id="ref-4">Forsgren, Nicole, Jez Humble, and Gene Kim (2018). <em>Accelerate: The Science of Lean Software and DevOps</em>. IT Revolution Press. <a href="https://itrevolution.com/product/accelerate/">https://itrevolution.com/product/accelerate/</a></li>
  <li id="ref-5">Deming, W. Edwards (1986). <em>Out of the Crisis</em>. MIT Press. <a href="https://direct.mit.edu/books/monograph/4192/Out-of-the-Crisis">https://direct.mit.edu/books/monograph/4192/Out-of-the-Crisis</a></li>
</ol>

---

## Outtakes

**Wells Fargo.** Wells Fargo employees opened millions of unauthorized accounts over more than a decade to meet cross-selling targets (CFPB, 2016). The metric was accounts opened per customer. The goal was customer relationships. The bank paid $3 billion in settlements.

**UK emergency wait times.** When the NHS introduced a four-hour emergency department wait target, some hospitals held patients in ambulances outside to avoid starting the clock (Bevan and Hood, 2006). The wait time metric improved while patients waited longer.

**The Soviet nail quota.** A 1957 cartoon in the Soviet satirical magazine *Krokodil* showed a factory worker proudly holding one giant nail. Production quotas by count produced tiny, unusable nails. Planners switched to weight quotas and factories made massive, unusable nails instead (Nove, 1977).

**The cobra effect.** Colonial administrators in 1870s British India offered bounties for dead cobras to reduce the cobra population. Locals began breeding cobras for the reward. When the program ended, breeders released their snakes and the population increased (Siebert, 2001).

---

## Changelog

**2026-06-07** Initial publication.
