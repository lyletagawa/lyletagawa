---
title: "Distributed Causation"
date: 2026-06-01
publishdate: 2026-06-01
lastmod: 2026-06-01
summary: "Complex failures don't have a single root cause. Complex failures have many causes that are distributed across time, systems, and organizational boundaries, and fixing just one leaves the rest in place."
tags: ["safety", "incidents", "resilience"]
image: 
draft: false
---

![](/images/distributed-causation.jpg)
*Photo: U.S. Nuclear Regulatory Commission (1979). CC BY-NC-ND 2.0.*

## Distributed Causation

The postmortem closes with a root cause. The database connection pool exhausted under peak traffic. The fix ships within the week, a configuration change and a new alert. The incident is resolved.

Eight months later, a different service fails at a different time of day with the same pattern of cascading failures. The postmortem finds another root cause.

The team has been finding root causes for years. The incidents keep coming.

## What Is Distributed Causation?

On March 28, 1979, a pressure relief valve stuck open at the Three Mile Island (TMI) nuclear plant in Pennsylvania. Cooling water escaped and the reactor core partially melted. It was the most serious nuclear accident in American history.

President Jimmy Carter appointed a commission to investigate, chaired by John Kemeny, president of Dartmouth College. The commission rejected the most convenient explanation. The accident wasn't caused by the valve, not by operator error alone, nor by equipment design alone. Metropolitan Edison had made staffing and training decisions that contributed. Babcock and Wilcox had known about similar valve events at other facilities and hadn't circulated warnings. The NRC had failed to act on those events or improve its oversight requirements (Kemeny Commission, 1979).

This is distributed causation. Failures in complex systems don't originate in one place but accumulate across organizational boundaries, across time, and across multiple contributing factors that appear unrelated until they combine.

Twenty-four years later, the Columbia Accident Investigation Board (CAIB) reached the same conclusion about a different disaster. The shuttle broke apart on reentry in February 2003 after foam breached the heat shield during launch. The board named the foam strike as the physical cause. It also named NASA's organizational culture, a budget environment that had steadily eroded safety margins, and communication structures that suppressed dissenting engineering voices as causes of equal standing (CAIB, 2003).

Every participant followed their training, their protocols, their oversight requirements. Each node was working. The system failed anyway (Kemeny Commission, 1979; CAIB, 2003).

## The Root Cause Problem

The desire to find a root cause is not irrational. A single cause is actionable, but a distributed explanation requires simultaneous changes to multiple systems, multiple processes, and multiple organizations, which is slow and expensive.

The problem is that complex systems don't produce failures that way. Catastrophe requires multiple failures, with latent conditions throughout the system waiting to combine with active failures (Cook, 1998). A single root cause, where it exists, usually explains only one contributing factor in a chain that required several.

The same argument has been made specifically for software. The 2017 SNAFUcatchers workshop documented how software incidents consistently involve multiple interacting factors across organizational and system boundaries. Assigning a single root cause, the workshop found, understates both the complexity of what happened and the scope of changes required to prevent recurrence (Cook et al., 2017).

## Where Engineering Teams Feel It

**Postmortem format.** Most postmortems are structured to produce a root cause section. That format creates pressure to reduce a distributed failure to a single entry. Teams that comply aren't being lazy. They're responding to organizational expectations that treat "we identified the root cause" as evidence of rigor. The result is postmortems that are technically complete and organizationally useless (Cook, 1998).

**Cross-team incidents.** Distributed failures cross team boundaries. Each team contributed a factor, but no team caused the incident alone. The root cause framework assigns ownership to one team, which relieves the others of any obligation to change. The same distributed conditions produce another incident months later, attributed to a different team.

**Latent conditions.** Cook describes latent conditions as failures waiting to happen, present in the system before any incident occurs (Cook, 1998). A postmortem that finds a root cause has found the active failure that triggered the incident, but rarely surfaces the latent conditions that made it consequential. Those conditions remain in place until the next combination.

**Vendor and dependency failures.** Incidents involving third-party services or cloud provider outages reveal the same pattern TMI demonstrated (Kemeny Commission, 1979). The failure is distributed across the organization's own systems and the external dependency. Attributing it entirely to the vendor obscures whatever internal factors made that external failure so damaging.

## Common Mistakes

**Stopping at the proximate cause.** The proximate cause is real and worth fixing, but it's not enough. A postmortem that stops at the failed component has identified one factor in a distributed failure. The other factors remain.

**Treating organizational contributors as context, not cause.** The Kemeny Commission found that staffing decisions, training programs, and regulatory culture all contributed to TMI (Kemeny Commission, 1979). Most postmortems treat these as background when they belong in the cause list.

**Fixing the trigger and calling it done.** Patching the vulnerable dependency, alerting on the metric that wasn't alerting, and changing the failed configuration are all real improvements. They address the active failure, but the latent conditions that amplified it remain.

**Confusing blame-free with cause-free.** Blameless postmortems correctly remove individual blame, but they sometimes remove distributed causation as well, treating the absence of a blameworthy person as evidence that no structural failure occurred (Dekker, 2006).

**Misreading incident patterns.** Teams that track incidents by root cause category watch for recurrence in that category, but distributed causation means the next incident will look different. The service changes and the proximate failure changes, but the underlying combination of latent conditions stays the same. The pattern stays invisible inside the root cause framework.

## Put It Into Practice

Review your last three postmortems. For each incident, list every team or system whose behavior contributed to the outcome. Compare them against the (identified) root cause. The missing systems are where the latent conditions live.

For your next postmortem, write a contributing factors section before the root cause. List every factor whose absence would have changed the outcome. That list rarely has fewer than four items, and picking any one as the root cause is organizational convenience, not an accurate account of what happened.

The team that keeps finding root causes isn't doing postmortems wrong. They're doing them right, by the wrong definition. The incidents keep coming because the root cause framework is designed to find only a single cause. The rest are still in place.

---

## References

Columbia Accident Investigation Board (2003). *Columbia Accident Investigation Board Report, Volume 1*. NASA. [https://digital.library.unt.edu/ark:/67531/metadc1282018/](https://digital.library.unt.edu/ark:/67531/metadc1282018/)

Cook, Richard I. (1998). "How Complex Systems Fail." Cognitive Technologies Laboratory, University of Chicago. [https://how.complexsystemsfail.com/](https://how.complexsystemsfail.com/)

Cook, Richard I., John Allspaw, David D. Woods, et al. (2017). *STELLA: Report from the SNAFUcatchers Workshop on Coping with Complexity*. SNAFUcatchers. [https://snafucatchers.github.io/](https://snafucatchers.github.io/)

Dekker, Sidney (2006). *The Field Guide to Understanding Human Error*. Ashgate. [https://www.routledge.com/The-Field-Guide-to-Understanding-Human-Error/Dekker/p/book/9781472439055](https://www.routledge.com/The-Field-Guide-to-Understanding-Human-Error/Dekker/p/book/9781472439055)

Kemeny Commission (1979). *Report of the President's Commission on the Accident at Three Mile Island*. U.S. Government Printing Office. [https://www.osti.gov/servlets/purl/5082863](https://www.osti.gov/servlets/purl/5082863)

Nuclear Regulatory Commission (2013). "Three Mile Island Plant Site in 1979." Flickr. CC BY-NC-ND 2.0. [https://www.flickr.com/photos/69383258@N08/7447157202](https://www.flickr.com/photos/69383258@N08/7447157202)

---

## Outtakes

**Apollo 13.** The oxygen tank that exploded in 1970 was dropped during testing in 1968, damaging an internal component. It passed inspections. The explosion combined that damage with a thermostat flaw and a ground test that exceeded design limits. Three contributing factors, two years apart (NASA, 1970).

**Tenerife.** The deadliest aviation accident in history (583 killed, 1977) is usually summarized as a captain who took off without clearance. The investigation also found overlapping radio transmissions, non-standard terminology, dense fog, and a runway layout forced by a bomb threat at another airport (Weick, 1990).

**Knight Capital.** Knight Capital lost $440 million in 45 minutes in 2012 after a deployment activated dormant code. The post-incident report named the deployment error. Investigation also found inadequate testing, no rollback capability, and a regulatory change that had created the latent condition months earlier (SEC, 2013).

---

## Changelog

**2026-06-01** Initial release.
