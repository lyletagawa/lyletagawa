---
title: "Jevons Paradox"
date: 2026-05-02
publishdate: 2026-05-05
lastmod: 2026-06-07
summary: "Why efficiency improvements in technology often increase total consumption rather than reduce it, and how leaders can anticipate and manage the inevitable rebound."
tags: ["capacity", "management"]
image: /images/jevons-paradox-jam.jpg
---

![](/images/jevons-paradox-jam.jpg)
*Rush hour traffic jam on I-95, Miami. Photo: B137 (2012). CC BY-SA 4.0.*

## Jevons Paradox

You've just cut your cloud costs by 40%. Six months later, your bill is higher than before. You've made your API 10x faster. Now you're handling 50x more traffic and your infrastructure is buckling. You've deployed AI agents to review every pull request. Merge volume doubles. Your AI compute bill is a line item you didn't budget for.

Welcome to Jevons Paradox. Make a resource cheaper or faster, and you'll find more ways to use it.

## What Is Jevons Paradox?

In 1865, economist William Stanley Jevons observed that as coal-burning steam engines became more efficient, Britain's coal consumption increased rather than decreased (Jevons, 1865). More efficient engines made coal-powered applications economically viable in new contexts. The result was higher consumption, not lower (Sorrell, 2009).

## How It Spreads

**Direct rebound effect**. Efficiency makes the resource cheaper, so people use more of it (Khazzoom, 1980).

Compression reduces storage costs by 70%, so teams stop deleting old data because "storage is cheap." Three years later, you're storing 10x more data.

**Indirect rebound effect.** Efficiency frees up resources that get reallocated to new uses (Sorrell, 2009).

Caching reduces database load by 80%. The freed database capacity gets used for new analytics queries. Database costs stay the same, but you're now supporting workloads that weren't possible before.

## How It Manifests

**The cloud cost spiral**. Azul reported a cloud migration that reduced per-transaction costs by 42%, yet total cloud spend doubled over three years as they processed significantly more transactions and launched services that weren't viable before (Sellers, 2025).

**The performance trap**. Figma reduced file load times by 33% for the slowest loads, but immediately increased server-side decoding load by 30%, requiring a separate infrastructure optimization to absorb the new demand (Figma Engineering, 2025a, 2025b).

**The agentic AI paradox**. Cloudflare deployed a multi-agent AI code review system that automatically reviews every merge request. Within weeks, weekly merge volume increased from roughly 5,600 to over 8,700, with one week reaching 10,952 (Cloudflare, 2026). Review capacity was no longer the constraint on merge velocity.

## How to Manage Jevons Paradox

These three moves happen in sequence. Plan before you ship, constrain at launch, and monitor after.

**Plan for rebound**. When optimizing, assume consumption will increase, not decrease.

- Bad Planning: "This optimization will reduce our costs by 50%"
- Good Planning: "This optimization will reduce per-unit costs by 50%. We expect usage to increase 2-3x as new use cases become viable."

**Implement economic constraints**. Efficiency removes natural constraints. Rate limiting, cost chargebacks to teams, quotas, and approval gates for new high-volume use cases put intentional friction back in. Release capacity based on business priorities, not technical availability (Kim et al., 2016).

**Monitor leading indicators**. Track metrics that predict rebound before it impacts costs (Forsgren et al., 2018). Request rates, service counts, data growth rates, and feature usage patterns tell you where demand is heading. Unit costs tell you where it's been.

## The Counterargument

When growth is the goal, expanding consumption is the measure of success.

When your API optimization drives 50x more traffic, that is the product working. New use cases becoming economically viable is how technology markets expand. Framing it as failure misidentifies the goal. Planning for cost reduction when growth was the objective is the error, not the rebound.

The paradox assumes unbounded demand. U.S. LED adoption eventually reduced total lighting electricity consumption despite LEDs being far more efficient than incandescent bulbs, because demand saturated (U.S. Department of Energy, 2020). The rebound effect is strongest when the resource is a binding constraint on elastic demand (Sorrell, 2009). When demand is bounded by market size, human attention, or regulatory limits, rebound is partial.

Plan for rebound when cost reduction is the goal. Expect and welcome it when growth is the goal.

## Put It Into Practice

Jevons Paradox is the predictable result of removing a constraint. Consumption will increase after your next optimization. The question is whether you planned for it.

Ask what new usage patterns this enables, and whether they are the ones you want. Set intentional limits even when capacity exists. Monitor usage, not just unit costs. Document what costs would have looked like without your work. Align efficiency gains to business priorities before the rebound arrives.

---

## References

Jevons, William Stanley (1865). *The Coal Question*. London: Macmillan and Co. [https://oll.libertyfund.org/title/jevons-the-coal-question](https://oll.libertyfund.org/title/jevons-the-coal-question)

Khazzoom, J. Daniel (1980). "Economic Implications of Mandated Efficiency Standards." *The Energy Journal*, 1(4): 21-40. [https://doi.org/10.5547/ISSN0195-6574-EJ-Vol1-No4-2](https://doi.org/10.5547/ISSN0195-6574-EJ-Vol1-No4-2)

Sorrell, Steve (2009). "Jevons' Paradox revisited: The evidence for backfire." *Energy Policy*, 37(4): 1456-1469. [https://doi.org/10.1016/j.enpol.2008.09.056](https://doi.org/10.1016/j.enpol.2008.09.056)

Cloudflare (2026). "The AI engineering stack we built internally — on the platform we ship." *Cloudflare Blog*, April 20. [https://blog.cloudflare.com/internal-ai-engineering-stack/](https://blog.cloudflare.com/internal-ai-engineering-stack/)

Figma Engineering (2025a). "Speeding Up File Load Times, One Page At A Time." *Figma Engineering Blog*. [https://www.figma.com/blog/speeding-up-file-load-times-one-page-at-a-time/](https://www.figma.com/blog/speeding-up-file-load-times-one-page-at-a-time/)

Figma Engineering (2025b). "Supporting Faster File Load Times with Memory Optimizations in Rust." *Figma Engineering Blog*. [https://www.figma.com/blog/supporting-faster-file-load-times-with-memory-optimizations-in-rust/](https://www.figma.com/blog/supporting-faster-file-load-times-with-memory-optimizations-in-rust/)

Forsgren, Nicole, et al. (2018). *Accelerate: The Science of Lean Software and DevOps*. Portland, OR: IT Revolution Press. [https://itrevolution.com/product/accelerate/](https://itrevolution.com/product/accelerate/)

Kim, Gene, et al. (2016). *The DevOps Handbook*. Portland, OR: IT Revolution Press. [https://itrevolution.com/product/the-devops-handbook/](https://itrevolution.com/product/the-devops-handbook/)

Sellers, Scott (2025). "Why Cloud Efficiency is Driving More IT Spending, Not Less." *Azul Newsroom*, August 21. [https://azul.com/newsroom/why-cloud-efficiency-is-driving-more-it-spending-not-les](https://azul.com/newsroom/why-cloud-efficiency-is-driving-more-it-spending-not-les)

U.S. Department of Energy (2020). "Adoption of Light-Emitting Diodes in Common Lighting Applications." Office of Energy Efficiency and Renewable Energy. [https://www.energy.gov/cmei/ssl/led-adoption-report](https://www.energy.gov/cmei/ssl/led-adoption-report)

B137 (2012). "Rush hour traffic jam on I-95 North, Miami." Wikimedia Commons. CC BY-SA 4.0. [https://commons.wikimedia.org/wiki/File:Miami_traffic_jam,_I-95_North_rush_hour.jpg](https://commons.wikimedia.org/wiki/File:Miami_traffic_jam,_I-95_North_rush_hour.jpg)

---

## Outtakes

**Paradox.** From the Greek "paradoxon," where para means contrary to and doxa means opinion. Literally, contrary to expectation. Not a logical contradiction. Jevons Paradox fits that definition precisely.

**Wirth's Law.** Niklaus Wirth observed that software gets slower faster than hardware gets faster. Hardware efficiency gains are absorbed by software bloat. (Wirth, 1995)

**The Katy Freeway.** Houston widened the Katy Freeway to 26 lanes to reduce congestion. Congestion got worse. Traffic engineers call this induced demand. Building more road capacity generates more traffic to fill it. (Duranton and Turner, 2011)

---

## Changelog

**2026-06-07** Added hero image.  
**2026-05-16** Added Outtakes section. Removed Incentives, and IC guidance sections.  
**2026-05-05** Consolidated redundant sections. Removed duplicate database optimization case study.
