---
title: "Jevons Paradox"
date: 2026-05-02
publishdate: 2026-05-02
lastmod: 2026-05-02
summary: "Why efficiency improvements in technology often increase total consumption rather than reduce it, and how leaders can anticipate and manage the inevitable rebound."
tags: ["capacity", "management", "leadership"]
image: /images/jevons-paradox.jpg
---

## Jevons Paradox

You've just cut your cloud costs by 40%. Six months later, your bill is higher than before. You've made your API 10x faster. Now you're handling 50x more traffic and your infrastructure is buckling. You've deployed AI agents to review every pull request. Merge volume doubles. Your AI compute bill is a line item you didn't budget for.

Welcome to Jevons Paradox: efficiency improvements don't reduce consumption. They increase it.

## What Is Jevons Paradox?

In 1865, economist William Stanley Jevons observed that as coal-burning steam engines became more efficient, Britain's coal consumption increased rather than decreased (Jevons, 1865). More efficient engines made coal-powered applications economically viable in new contexts. Efficiency didn't reduce demand. It unleashed it.

The paradox: making something cheaper or easier doesn't reduce its use. It expands the contexts where using it makes sense, driving total consumption up (Sorrell, 2009).

For technical leaders, this isn't historical trivia. It's the invisible force behind your biggest operational surprises.

## How It Spreads

**Direct rebound effect**. Efficiency makes the resource cheaper, so people use more of it directly (Khazzoom, 1980).

Example: Compression reduces storage costs by 70%. Teams stop deleting old data because "storage is cheap." Three years later, you're storing 10x more data.

**Indirect rebound effect**: Efficiency frees up resources that get reallocated to new uses (Sorrell, 2009).

Example: Caching reduces database load by 80%. The freed database capacity gets used for new analytics queries. Database costs stay the same, but you're now supporting workloads that weren't possible before.

**Economy-wide effect**: Efficiency makes entire categories of applications economically viable (Sorrell, 2009).

Example: Serverless reduces infrastructure management overhead. This makes it viable to build hundreds of small services that would have been too expensive to operate traditionally. Your serverless bill exceeds what you spent on traditional infrastructure.

## How It Manifests

**The cloud cost spiral**. Azul reported a cloud migration that reduced per-transaction costs by 42%, yet total cloud spend doubled over three years as they processed significantly more transactions and launched services that weren't viable before (Sellers, 2025).

**The performance trap**. Figma reduced file load times by 33% for the slowest loads, but immediately increased server-side decoding load by 30%, requiring a separate infrastructure optimization to absorb the new demand (Figma Engineering, 2025a; 2025b).

**The agentic AI paradox**. Cloudflare deployed a multi-agent AI code review system that automatically reviews every merge request. Within weeks, weekly merge volume increased from roughly 5,600 to over 8,700, with one week reaching 10,952. The system ran 131,246 review cycles in a single month at $0.98 per review (Cloudflare, 2026). AI review didn’t reduce workload. It unlocked a merge velocity that human reviewers alone could not have sustained.

## How to Manage Jevons Paradox

### 1. Plan for Rebound

When optimizing, assume consumption will increase, not decrease.

**Bad Planning**: "This optimization will reduce our costs by 50%"

**Good Planning**: "This optimization will reduce per-unit costs by 50%. We expect usage to increase 2-3x as new use cases become viable."

### 2. Implement Economic Constraints

Efficiency removes natural constraints. Add intentional ones:
- Rate limiting even when technically unnecessary
- Cost allocation and chargebacks to teams
- Quotas on resources even when capacity exists
- Approval processes for new high-volume use cases

### 3. Monitor Leading Indicators

Track metrics that predict rebound before it impacts costs (Forsgren et al., 2018):
- Request rates, not just response times
- Number of services/deployments, not just deployment speed
- Data growth rates, not just storage costs
- Feature usage patterns, not just feature performance

### 4. Separate Efficiency from Capacity

Make efficiency improvements without immediately exposing new capacity:
- Optimize database, but maintain rate limits
- Improve API performance, but keep request quotas
- Reduce LLM costs, but enforce usage quotas per feature

Release capacity intentionally based on business priorities (Kim et al., 2016).

### 5. Align Incentives

**Bad Incentives**: "Your team's budget is based on last year's costs"
Result: Teams avoid optimization to maintain budget

**Good Incentives**: "Your team gets 50% of cost savings for new initiatives"
Result: Teams optimize but use gains strategically

For individual contributors, three applications stand out. Document the counterfactual: record what costs or load would have been without your work, so your impact is visible even as rebound erodes absolute gains. Negotiate scope when optimizing: establish explicit limits ("this supports current usage plus 50% growth; beyond that requires architectural changes"). Build observability into your optimizations: dashboards showing cost per transaction alongside total cost make rebound visible before it becomes a crisis, and give you evidence to advocate for formal capacity planning (Allspaw, 2008).

## Put It Into Practice

Jevons Paradox is not a failure mode. It is the predictable result of removing a constraint: systems expand to fill new space (Meadows, 2008). Every optimization changes what is economically viable, and demand follows.

The question is not whether consumption will increase after your next optimization. It will. The question is whether you planned for it.

Ask what new usage patterns does this enable, and are they the ones we want? Set intentional limits even when capacity exists. Monitor usage, not just unit costs. Document what costs would have looked like without your work. Align efficiency gains to business priorities before the rebound arrives.

## References

Jevons, William Stanley (1865). *The Coal Question*. London: Macmillan and Co. [https://oll.libertyfund.org/title/jevons-the-coal-question](https://oll.libertyfund.org/title/jevons-the-coal-question)

Khazzoom, J. Daniel (1980). "Economic Implications of Mandated Efficiency Standards." *The Energy Journal*, 1(4): 21-40. [https://doi.org/10.5547/ISSN0195-6574-EJ-Vol1-No4-2](https://doi.org/10.5547/ISSN0195-6574-EJ-Vol1-No4-2)

Sorrell, Steve (2009). "Jevons' Paradox revisited: The evidence for backfire." *Energy Policy*, 37(4): 1456-1469. [https://doi.org/10.1016/j.enpol.2008.09.056](https://doi.org/10.1016/j.enpol.2008.09.056)

Meadows, Donella H. (2008). *Thinking in Systems: A Primer*. White River Junction, VT: Chelsea Green Publishing. [https://www.chelseagreen.com/product/thinking-in-systems/](https://www.chelseagreen.com/product/thinking-in-systems/)

Cloudflare (2026). "Orchestrating AI code review at scale." *Cloudflare Blog*, April 20. [https://blog.cloudflare.com/ai-code-review/](https://blog.cloudflare.com/ai-code-review/)

Figma Engineering (2025a). "Speeding Up File Load Times, One Page At A Time." *Figma Engineering Blog*. [https://www.figma.com/blog/speeding-up-file-load-times-one-page-at-a-time/](https://www.figma.com/blog/speeding-up-file-load-times-one-page-at-a-time/)

Figma Engineering (2025b). "Supporting Faster File Load Times with Memory Optimizations in Rust." *Figma Engineering Blog*. [https://www.figma.com/blog/supporting-faster-file-load-times-with-memory-optimizations-in-rust/](https://www.figma.com/blog/supporting-faster-file-load-times-with-memory-optimizations-in-rust/)

Allspaw, John (2008). *The Art of Capacity Planning*. Sebastopol, CA: O'Reilly Media. [https://www.oreilly.com/library/view/the-art-of/9780596518578/](https://www.oreilly.com/library/view/the-art-of/9780596518578/)

Forsgren, Nicole, et al. (2018). *Accelerate: The Science of Lean Software and DevOps*. Portland, OR: IT Revolution Press. [https://itrevolution.com/product/accelerate/](https://itrevolution.com/product/accelerate/)

Kim, Gene, et al. (2016). *The DevOps Handbook*. Portland, OR: IT Revolution Press. [https://itrevolution.com/product/the-devops-handbook/](https://itrevolution.com/product/the-devops-handbook/)

Sellers, Scott (2025). "Why Cloud Efficiency is Driving More IT Spending, Not Less." *Azul Newsroom*, August 21. [https://azul.com/newsroom/why-cloud-efficiency-is-driving-more-it-spending-not-les](https://azul.com/newsroom/why-cloud-efficiency-is-driving-more-it-spending-not-les)

*Thumbnail: Watt's Steam Engine. British Cigarette Cards, George Arents Collection. The New York Public Library. [picryl.com](https://picryl.com/media/watts-steam-engine-589e99)*

## Changelog

**2026-05-02** Reorganized to consolidate redundant sections: merged "Why This Matters for Technical Leadership" and three case studies into "How It Manifests"; absorbed IC strategic guidance into "How to Manage Jevons Paradox"; removed duplicate database optimization case study.
