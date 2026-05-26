---
title: "Normalization of Deviance"
date: 2026-05-22
publishdate: 2026-05-22
lastmod: 2026-05-22
summary: "Diane Vaughan coined the term studying the Challenger disaster. Teams gradually accept dangerous deviations as normal when nothing bad happens. Each successful outcome makes the next deviation easier to justify."
tags: ["safety", "incidents", "resilience"]
image:
draft: true
---

## Normalization of Deviance

The deploy process has a validation step that takes twenty minutes. It was added two years ago, after a rollback cost four engineers a Saturday. The step catches a class of configuration errors before they reach production.

Under release pressure, someone skips it. The deploy is fine. It gets skipped again the next sprint. Also fine. After six months and two dozen clean releases, the step exists only in the documentation. Engineers who joined after the incident have no idea why it was there. Engineers who were there have stopped thinking about it.

The configuration error the step was designed to catch appears in a Tuesday afternoon release. By the time the incident is resolved, the step is back in the process. Nobody can explain how it disappeared.

## What Is Normalization of Deviance?

Sociologist Diane Vaughan coined the term while studying the 1986 Space Shuttle Challenger disaster. Her research found that NASA engineers had observed O-ring erosion on previous shuttle flights. Erosion was a known deviation from the design specification. But each flight that returned without catastrophe was treated as evidence that erosion was an acceptable risk. Over time, the deviation was normalized. The night before the Challenger launch, engineers raised concerns about O-ring performance in cold temperatures. The launch proceeded (Vaughan, 1996).

Vaughan defined normalization of deviance as the process by which a group repeatedly accepts a practice that departs from safety rules or standards, until the departure becomes the new standard (Vaughan, 1996). The deviation is not hidden. It is visible, documented, and discussed. It stops being treated as a problem because the problems it causes have not yet appeared.

The concept applies beyond aerospace. It describes a mechanism that operates wherever repeated success with a known risk gets interpreted as evidence the risk is not real.

## The Ratchet

Each successful outcome with a deviation makes the next deviation easier to accept.

The first time a team skips a process step, there is friction. Someone notes the skip. It is justified by schedule pressure or confidence in the engineer's judgment. The justification sounds reasonable because nothing bad happens. The second skip has less friction. The first skip ended fine. By the tenth skip, there is no friction. The justification is no longer needed. The skip is the process.

The ratchet has a second element. Success is not neutral evidence. In complex systems, the absence of failure is not proof the risk has been managed. It may mean the risk has not been triggered yet. A system running outside its design margins can return normal outputs for months before crossing into failure. Teams that interpret continued operation as safety confirmation are reading a result that does not carry the meaning they are assigning to it. The Rogers Commission identified this pattern precisely in NASA's decision-making before the Challenger launch (Rogers Commission, 1986).

## Where Engineering Teams Feel It

**Alert fatigue.** An alert fires repeatedly without causing an incident. The team adds it to a list of known noisy alerts. Weeks later, the alert fires for a different reason and no one investigates. Normalized alerts are not ignored alerts. The team has made an explicit organizational decision that this signal is not worth examining. That decision rarely gets revisited.

**Runbook drift.** Steps get removed from runbooks when they seem redundant. The removal is rarely documented. The institutional memory of why the step existed erodes with team turnover. The runbook reflects current practice, not safe practice. Both look identical from the outside.

**SLO erosion.** Error budgets are useful when teams treat a burn rate as a decision trigger. When the budget runs out and no action follows, the budget becomes a reporting metric. Teams that have operated outside their SLO for multiple consecutive quarters have often stopped treating the SLO as binding. The number is still calculated. The constraint no longer exists.

**Accepted vulnerabilities.** Security scanners surface findings that teams triage as accepted risk. The acceptance is appropriate when the risk is genuinely understood and bounded. It becomes normalization when the finding ages for twelve months without review and the acceptance is renewed by default. The original reasoning behind the acceptance is long gone.

## Common Mistakes

**Treating it as negligence.** Normalization of deviance is not carelessness. The engineers at NASA were not reckless. They were applying rational judgment to available evidence, and the evidence repeatedly told them the risk was manageable. The mechanism is more dangerous than negligence precisely because it operates within ordinary decision-making (Vaughan, 1996).

**Assuming post-incident reviews prevent recurrence.** Incident reviews identify what deviated in a specific incident. They rarely surface the full scope of normalized deviations that have accumulated. A blameless post-mortem can correctly identify the immediate cause without ever auditing the broader environment that made that cause invisible.

**Believing the process is the safeguard.** Written procedures are not active. They do not enforce themselves. A process that was followed correctly six months ago and has since drifted does not provide the protection the documentation describes.

**Conflating familiarity with safety.** Teams that have worked alongside a known risk for years have operational familiarity with it. That is not the same as having reduced the risk. Long exposure to a risk without incident can erode the urgency needed to address it.

## Put It Into Practice

Run a deviation audit on one critical process. Take the documented steps and walk through the last five times the process was actually executed. Note every step that was skipped, abbreviated, or substituted. Do not start with why. Start with the list. The gap between documentation and practice is the exposure.

For alerts, apply one test: if this alert fires right now, what does the team do? If the answer is "check if it's the usual thing," the alert has been reinterpreted as noise. The team has lost its ability to distinguish the signal from the pattern it expects. That is not an alert. It is a placeholder.

Pick one item from the list and reopen it. Not to assign blame. To restore the decision to a conscious one. Ask whether the risk is still understood, still bounded, and still accepted for documented reasons. That question, asked on a regular cadence, is the only mechanism that interrupts the ratchet.

---

## References

Rogers Commission (1986). *Report of the Presidential Commission on the Space Shuttle Challenger Accident*. NASA. [https://www.nasa.gov/history/rogersrep/genindex.htm](https://www.nasa.gov/history/rogersrep/genindex.htm)

Vaughan, Diane (1996). *The Challenger Launch Decision: Risky Technology, Culture, and Deviance at NASA*. University of Chicago Press. [https://press.uchicago.edu/ucp/books/book/chicago/C/bo22781921.html](https://press.uchicago.edu/ucp/books/book/chicago/C/bo22781921.html)

---

## Changelog

**2026-05-22** Initial publication.
