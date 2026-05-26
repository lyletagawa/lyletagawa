---
title: "Distributed Causation"
date: 2026-05-23
publishdate: 2026-05-23
lastmod: 2026-05-23
summary: "The Kemeny Commission found that Three Mile Island had no single cause. It had many. The habit of attributing complex failures to a root cause obscures the distributed conditions that make failure possible."
tags: ["safety", "incidents", "resilience"]
image:
draft: true
---

## Distributed Causation

The post-mortem closes with a root cause. The database connection pool exhausted under peak traffic. The fix ships within the week, a configuration change and a new alert. The incident is resolved.

Eight months later, a similar incident. Different service, different time of day, the same pattern of cascading failures. The post-mortem finds another root cause.

The team has been finding root causes for years. The incidents keep coming.

## What Is Distributed Causation?

On March 28, 1979, a pressure relief valve stuck open at the Three Mile Island nuclear plant in Pennsylvania. Cooling water escaped. The reactor core partially melted. It was the most serious nuclear accident in American history.

President Jimmy Carter appointed a commission to investigate. John Kemeny, president of Dartmouth College, chaired it. The commission's report rejected the most convenient explanation before it finished the investigation. The accident was not caused by the valve. It was not caused by operator error alone, nor by equipment design alone. Metropolitan Edison, which operated the plant, had made decisions about training and staffing that contributed. Babcock and Wilcox, the reactor manufacturer, had known about similar valve events at other facilities and had not circulated warnings. The Nuclear Regulatory Commission had failed to act on those earlier events or improve its oversight requirements. Every node in the system had operated within its normal parameters. The system produced a near-catastrophe anyway (Kemeny Commission, 1979).

This is distributed causation. Failures in complex systems do not originate in one place. They accumulate across organizational boundaries, across time, and across multiple contributing factors that appear unrelated until they combine.

Twenty-four years later, the Columbia Accident Investigation Board reached the same conclusion about a different disaster. The shuttle broke apart on reentry in February 2003 after foam struck the leading edge of the wing during launch and breached the heat shield. The board named the foam strike as the physical cause. It also named NASA's organizational culture, a budget environment that had steadily eroded safety margins, and communication structures that suppressed dissenting engineering voices as causes of equal standing (CAIB, 2003).

The Kemeny Commission explicitly rejected single-cause methodology before the investigation concluded. Its investigators found that each contributing organization had internal systems that were locally rational. The operators followed their training. The utility followed guidance from the manufacturer. The manufacturer followed NRC requirements. The NRC enforced the rules it had written. Each node was working. The system failed anyway (Kemeny Commission, 1979).

## The Root Cause Problem

The desire to find a root cause is not irrational. A single cause is actionable. A distributed explanation requires simultaneous changes to multiple systems, multiple processes, and multiple organizations, which is slow and expensive.

The problem is that complex systems do not produce failures that way. Cook documented this directly. Catastrophe in complex systems requires multiple failures, and latent conditions exist throughout the system waiting to combine with active failures (Cook, 1998). A single root cause, where it exists, usually explains only one contributing factor in a chain that required several.

Dekker drew the distinction between what he called the old view and the new view of human error. The old view looks for the broken part, the wrong decision, the failed procedure. The new view treats failure as an emergent property of the system, arising from interactions between components that each behaved within normal operating parameters (Dekker, 2006). The same behaviors that produce successful outcomes can combine, under different conditions, to produce failure.

The same argument has been made specifically for software. The 2017 SNAFUcatchers workshop, a gathering of resilience engineering researchers and software operations practitioners, documented how software incidents consistently involve multiple interacting factors across organizational and system boundaries. Assigning a single root cause, the workshop found, understates both the complexity of what happened and the scope of changes required to prevent recurrence (Cook et al., 2017).

## Where Engineering Teams Feel It

**Post-mortem format.** Most post-mortems are structured to produce a root cause section. That format creates pressure to reduce a distributed failure to a single entry. Teams that comply are not being lazy. They are responding to organizational expectations that treat "we identified the root cause" as evidence of rigor. The result is post-mortems that are technically complete and organizationally useless (Cook, 1998).

**Cross-team incidents.** Distributed failures cross team boundaries. Each team contributed a factor. No team caused the incident alone. The root cause framework assigns ownership to one team, which relieves the others of any obligation to change. The same distributed conditions produce another incident months later, attributed to a different team.

**Latent conditions.** Cook describes latent conditions as failures waiting to happen, present in the system before any incident occurs (Cook, 1998). A post-mortem that finds a root cause has found an active failure that triggered the incident. It rarely surfaces the latent conditions that made the active failure consequential. Those conditions remain in place until the next combination.

**Vendor and dependency failures.** Incidents involving third-party services or cloud provider outages reveal the same pattern TMI demonstrated. The failure is distributed across the organization's own systems and the external dependency. Attributing it entirely to the vendor obscures whatever internal factors made that external failure so damaging.

## Common Mistakes

**Stopping at the proximate cause.** The proximate cause is real and worth fixing. It is also insufficient. A post-mortem that stops at the failed component has identified one factor in a distributed failure. The other factors remain.

**Treating organizational contributors as context, not cause.** The Kemeny Commission found that staffing decisions, training programs, and regulatory culture all contributed to TMI. Most post-mortems treat these as background. They belong in the cause list.

**Fixing the trigger and calling it done.** Patching the vulnerable dependency, alerting on the metric that wasn't alerting, and changing the failed configuration are all real improvements. They address the active failure. The latent conditions that amplified it remain.

**Confusing blame-free with cause-free.** Blameless post-mortems correctly remove individual blame. They sometimes remove distributed causation as well, treating the absence of a blameworthy person as evidence that no structural failure occurred. There was structural failure. It just wasn't anyone's fault (Dekker, 2006).

**Misreading incident patterns.** Teams that track incidents by root cause category watch for recurrence in that category. Distributed causation means the next incident will look different. Different service, different proximate failure, the same underlying combination of latent conditions. The pattern is invisible inside the root cause framework.

## Put It Into Practice

Run your last three post-mortems against a different question. For each incident, list every team or external system whose behavior contributed to the outcome, not just the team that owns the root cause. The teams absent from the post-mortem are where the latent conditions live.

For your next post-mortem, write a contributing factors section before you write a root cause. List every factor that, if absent, would have changed the outcome. That list is rarely fewer than four items. Any one of them labeled as the root cause is a simplification for organizational convenience, not an accurate account of what happened.

Kemeny's commission published its report in 1979. Its central finding, that attributing complex failures to a single cause is wrong before the investigation starts, has not become the default in most engineering organizations. The next incident will have a root cause section. Treat it as a starting point.

---

## References

Columbia Accident Investigation Board (2003). *Columbia Accident Investigation Board Report, Volume 1*. NASA. [https://digital.library.unt.edu/ark:/67531/metadc1282018/](https://digital.library.unt.edu/ark:/67531/metadc1282018/)

Cook, Richard I. (1998). "How Complex Systems Fail." Cognitive Technologies Laboratory, University of Chicago. [https://how.complexsystemsfail.com/](https://how.complexsystemsfail.com/)

Cook, Richard I., John Allspaw, David D. Woods, et al. (2017). *STELLA: Report from the SNAFUcatchers Workshop on Coping with Complexity*. SNAFUcatchers. [https://snafucatchers.github.io/](https://snafucatchers.github.io/)

Dekker, Sidney (2006). *The Field Guide to Understanding Human Error*. Ashgate. [https://www.routledge.com/The-Field-Guide-to-Understanding-Human-Error/Dekker/p/book/9781472439055](https://www.routledge.com/The-Field-Guide-to-Understanding-Human-Error/Dekker/p/book/9781472439055)

Kemeny, John G., et al. (1979). *Report of the President's Commission on the Accident at Three Mile Island*. U.S. Government Printing Office. [https://www.osti.gov/servlets/purl/5082863](https://www.osti.gov/servlets/purl/5082863)

---

## Changelog

**2026-05-23** Initial publication.
