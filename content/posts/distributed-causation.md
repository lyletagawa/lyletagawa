---
title: "Distributed Causation"
date: 2026-06-01
publishdate: 2026-06-01
lastmod: 2026-06-17
summary: "Complex failures don't have a single root cause. They have many causes distributed across time, systems, and organizational boundaries, and fixing just one leaves the rest in place."
tags: ["safety", "incidents", "resilience"]
image: /images/distributed-causation.jpg
draft: false
---

![](/images/distributed-causation.jpg)
*Photo: [U.S. Nuclear Regulatory Commission (1979)](https://www.flickr.com/photos/69383258@N08/7447157202). CC BY-NC-ND 2.0.*

## Distributed Causation

The postmortem closes with a root cause. The database connection pool exhausted under peak traffic. The fix ships within the week, a configuration change and a new alert. The incident is resolved.

Eight months later, a different service fails at a different time of day with the same pattern of cascading failures. The postmortem finds another root cause.

The team has been finding root causes for years. The incidents keep coming.

## What Is Distributed Causation?

On March 28, 1979, a pressure relief valve stuck open at the Three Mile Island (TMI) nuclear plant in Pennsylvania. Cooling water escaped and the reactor core partially melted. It was the most serious nuclear accident in American history.

U.S. President Jimmy Carter appointed a commission to investigate, chaired by John Kemeny. The commission rejected the most convenient explanation. The accident wasn't caused by the valve, not by operator error alone, and not by equipment design alone. Metropolitan Edison (the plant's operator) made staffing and training decisions that contributed. Babcock and Wilcox (the reactor manufacturer) knew about similar valve events at other facilities and didn't share those warnings. The Nuclear Regulatory Commission (NRC) failed to act on those events or improve its oversight requirements {{< cite 1 "Kemeny Commission (1979). Report of the President's Commission on the Accident at Three Mile Island. U.S. Government Printing Office." >}}.

This is distributed causation. Failures in complex systems don't originate in one place. They pile up across organizational boundaries, across time, and across contributing factors that look unrelated until they converge.

Twenty-four years later, the Columbia Accident Investigation Board (CAIB) reached the same conclusion about the Columbia space shuttle disaster. The shuttle broke apart on reentry in February 2003 after foam breached the heat shield during launch. The board named the foam strike as the physical cause. It also named NASA's organizational culture, eroded safety margins, and a communication structure that suppressed dissenting voices {{< cite 2 "Columbia Accident Investigation Board (2003). Columbia Accident Investigation Board Report, Volume 1. NASA." >}}.

Every participant followed their training, their protocols, and their oversight requirements. But the system still failed {{< cite 1 "Kemeny Commission (1979). Report of the President's Commission on the Accident at Three Mile Island. U.S. Government Printing Office." >}}{{< cite 2 "Columbia Accident Investigation Board (2003). Columbia Accident Investigation Board Report, Volume 1. NASA." >}}.

## Why Root Cause Fails

The desire to find a root cause isn't irrational. A single cause is actionable, but a distributed explanation requires simultaneous changes to multiple systems, multiple processes, and multiple organizations, which is slow and expensive.

The problem is that complex systems don't produce failures that way. Catastrophe requires multiple failures, with latent conditions throughout the system waiting to combine with active failures {{< cite 3 "Cook, Richard I. (1998). How Complex Systems Fail. Cognitive Technologies Laboratory, University of Chicago." >}}. A single root cause, where it exists, usually explains only one contributing factor in a chain that required several.

The SNAFUcatchers workshop documented in 2017 how software incidents consistently involve multiple interacting factors across organizational and system boundaries. Assigning a single root cause understates the complexity of what happened and the scope of changes required to prevent recurrence {{< cite 4 "Cook, Richard I., et al. (2017). STELLA: Report from the SNAFUcatchers Workshop on Coping with Complexity. SNAFUcatchers." >}}.

## Where Engineering Teams Feel It

**Postmortem format.** Most postmortems are structured to produce a root cause section. That format creates pressure to reduce a distributed failure to a single entry. "We identified the root cause" is used as evidence of rigor. The result is postmortems that are technically complete and organizationally useless {{< cite 3 "Cook, Richard I. (1998). How Complex Systems Fail. Cognitive Technologies Laboratory, University of Chicago." >}}.

**Cross-team incidents.** Distributed failures cross team boundaries. Each team contributed a factor, but no team caused the incident alone. The root cause framework assigns ownership to one team, which relieves the others of any obligation to change. The same distributed conditions produce another incident months later, attributed to a different team.

**Latent conditions.** Latent conditions are failures waiting to happen, present in the system before any incident occurs {{< cite 3 "Cook, Richard I. (1998). How Complex Systems Fail. Cognitive Technologies Laboratory, University of Chicago." >}}. A postmortem that finds a root cause has found the active failure that triggered the incident, but rarely surfaces the latent conditions that made it consequential. Those conditions remain in place until the next combination.

**Vendor and dependency failures.** Incidents involving third-party services or cloud provider outages reveal the same pattern TMI demonstrated {{< cite 1 "Kemeny Commission (1979). Report of the President's Commission on the Accident at Three Mile Island. U.S. Government Printing Office." >}}. The failure is distributed across the organization's own systems and the external dependency. Attributing it entirely to the vendor obscures whatever internal factors made that external failure so damaging.

## How Teams Get This Wrong

**Fixing the trigger and calling it done.** Patching the vulnerable dependency, alerting on the metric that wasn't alerting, and changing the failed configuration are all real improvements worth making. But a postmortem that stops at the trigger has identified one factor in a distributed failure. The latent conditions that amplified it remain.

**Treating organizational contributors as context, not cause.** Staffing decisions, training programs, and regulatory culture all contributed to TMI {{< cite 1 "Kemeny Commission (1979). Report of the President's Commission on the Accident at Three Mile Island. U.S. Government Printing Office." >}}. Most postmortems treat these as background when they belong in the cause list.

**Confusing blame-free with cause-free.** Blameless postmortems correctly remove individual blame, but they sometimes remove distributed causation as well, treating the absence of a blameworthy person as evidence that no structural failure occurred {{< cite 5 "Dekker, Sidney (2006). The Field Guide to Understanding Human Error. Ashgate." >}}.

**Misreading incident patterns.** Teams that track incidents by root cause category watch for recurrence in that category, but distributed causation means the next incident will look different. The service changes and the trigger changes, but the underlying combination of latent conditions stays the same. The next incident looks new. It isn't.

## Put It Into Practice

Review your last three postmortems. For each incident, list every team or system whose behavior contributed to the outcome. Compare them against the (identified) root cause. The missing systems are where the latent conditions still live.

For your next postmortem, write a contributing factors section before the root cause. List every factor whose absence would have changed the outcome. That list often has three or more contributing factors, and picking any one as the root cause doesn't accurately reflect what happened.

---

## References

<ol class="references">
  <li id="ref-1">Kemeny Commission (1979). <em>Report of the President's Commission on the Accident at Three Mile Island</em>. U.S. Government Printing Office. <a href="https://www.osti.gov/servlets/purl/5082863">https://www.osti.gov/servlets/purl/5082863</a></li>
  <li id="ref-2">Columbia Accident Investigation Board (2003). <em>Columbia Accident Investigation Board Report, Volume 1</em>. NASA. <a href="https://digital.library.unt.edu/ark:/67531/metadc1282018/">https://digital.library.unt.edu/ark:/67531/metadc1282018/</a></li>
  <li id="ref-3">Cook, Richard I. (1998). "How Complex Systems Fail." Cognitive Technologies Laboratory, University of Chicago. <a href="https://how.complexsystemsfail.com/">https://how.complexsystemsfail.com/</a></li>
  <li id="ref-4">Cook, Richard I., John Allspaw, David D. Woods, et al. (2017). <em>STELLA: Report from the SNAFUcatchers Workshop on Coping with Complexity</em>. SNAFUcatchers. <a href="https://snafucatchers.github.io/">https://snafucatchers.github.io/</a></li>
  <li id="ref-5">Dekker, Sidney (2006). <em>The Field Guide to Understanding Human Error</em>. Ashgate. <a href="https://www.routledge.com/The-Field-Guide-to-Understanding-Human-Error/Dekker/p/book/9781472439055">https://www.routledge.com/The-Field-Guide-to-Understanding-Human-Error/Dekker/p/book/9781472439055</a></li>
</ol>

---

## Outtakes

**The Five Whys and the Andon Cord.** Both the Five Whys and the Andon Cord come from the Toyota Production System. Ohno designed the Five Whys for linear mechanical failures on the assembly line. Postmortem culture adopted it for distributed failures it was never designed to handle (Ohno, 1988).

**Tenerife.** The deadliest aviation accident in history (583 killed, 1977) is usually summarized as a captain who took off without clearance. The investigation also found overlapping radio transmissions, non-standard terminology, dense fog, and a runway layout change forced by a bomb threat at another airport (Weick, 1990).

**Knight Capital.** Knight Capital lost $440 million in 45 minutes in 2012 after a deployment activated dormant code. The post-incident report named the deployment error. Investigation also found inadequate testing, no rollback capability, and a regulatory change that had created the latent condition months earlier (SEC, 2013).

---

## Changelog

**2026-06-01** Initial release.
