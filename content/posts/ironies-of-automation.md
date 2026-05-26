---
title: "The Ironies of Automation"
date: 2026-05-25
publishdate: 2026-05-25
lastmod: 2026-05-25
summary: "Automation degrades the human skill it needs as a fallback. The more reliable the system, the less prepared the operator when it finally fails."
tags: ["safety", "resilience", "automation"]
image: /images/ironies-of-automation.svg
draft: true
---

## The Ironies of Automation

The alert fires at 2am. The automated remediation script triggers. It has handled this failure mode hundreds of times. This time it fails halfway through, leaving the service in a state the runbook never anticipated.

The on-call engineer opens the manual procedure. It was last updated two years ago. The infrastructure it references was deprecated. The engineer who wrote it left eight months ago. The team that operated this system manually has turned over twice.

This isn't a documentation problem. The automation removed the conditions under which operational knowledge could be maintained. Now it's failed, and it has taken with it the competence to replace it.

## What Are the Ironies?

Lisanne Bainbridge published "Ironies of Automation" in 1983 in the journal Automatica. It became one of the most cited papers in human factors and safety research (Bainbridge, 1983).

The first irony. Designers automate to remove human error. But they still need humans to monitor the automation and step in when it fails. The automation degrades exactly the skill that intervention requires. The more reliable the system, the less practiced the operator, and the more likely any manual takeover goes wrong.

The second irony. Automated systems are designed by humans. Human error doesn't disappear with automation. It moves upstream into the design, into the edge cases nobody anticipated. When the automation reaches those cases, the human must manage them with degraded understanding of the underlying system.

## How Skill Decays

Skill requires practice. Monitoring doesn't provide it.

A pilot flying manually develops continuous feedback between action and outcome. Muscle memory forms. Pattern recognition sharpens. An hour of manual flight in moderate turbulence builds the reflexes that a year of autopilot monitoring can't. This isn't a discipline problem. It's how skill acquisition works.

The out-of-the-loop problem follows directly. Operators in highly automated environments become supervisors of a process they no longer perform. When automation fails and requires manual takeover, they must act quickly on a system they understand abstractly but not operationally (Bainbridge, 1983).

Air France Flight 447 demonstrates the cost. On June 1, 2009, the autopilot disengaged over the Atlantic. Unreliable airspeed readings had confused the flight management system. The crew had three minutes and thirty seconds before the aircraft hit the water. They had full control authority. They didn't have the manual flying instincts the situation required. The aircraft had been flying automatically long enough that the reflexes for manual control in a complex upset had faded (BEA, 2012).

The 2003 Northeast Blackout shows the same pattern. A software bug caused FirstEnergy's alarm system to silently stop displaying new alerts. Operators saw a normal dashboard while the grid cascaded into failure. The alarm system had become the only interface between operators and the state of the grid. When the failure was finally identified, 55 million people had lost power. The automation failed, and the operators had lost the capacity to see without it (U.S.-Canada Power System Outage Task Force, 2004).

Automation-induced complacency compounds this. The more reliable the automated system, the less vigilant the human monitor becomes, and the slower their response to any deviation (Parasuraman et al., 2000).

## Where Engineering Teams Feel It

**On-call automation.** Runbooks automated into scripts remove the need for engineers to understand the recovery procedure. When the script fails or reaches an untested state, the on-call engineer must reason about a system they've never manually operated. Failure-free operations require experience with failure, and teams that never execute procedures manually have no reservoir to draw on (Cook, 1998). The documentation describes what the script does. It doesn't explain why each step exists.

**Deployment pipelines.** Teams that automate deployment lose the ability to deploy manually. When the pipeline breaks during an incident, the engineers who can cut a release by hand are gone, or the steps have drifted so far from practice that the procedure isn't trusted.

**AI-assisted development.** Engineers who delegate code review, test writing, and architecture decisions to AI tools develop shallower engagement with the code. The tool handles cases it has seen before. Novel failures require the judgment that sustained engagement builds. The more capable the tool becomes, the less the engineer practices the reasoning it's replacing.

**Monitoring and alerting.** Teams that rely entirely on automated alerts stop developing the pattern recognition that comes from reading logs and dashboards manually. Practitioners develop and maintain expertise through exposure to varied conditions (Cook, 1998). Remove that exposure and the expertise degrades. When an incident produces an alert pattern the system has never seen, the engineers responding have few instincts to draw on.

## Common Mistakes

**Treating automation as a final solution.** Automation solves the failure modes it was designed for. It doesn't solve for the cases it wasn't, and it removes the human competence that would have handled those cases.

**Assuming monitoring maintains skill.** Watching a system operate isn't the same as operating it. A team that has supervised automated deploys for two years doesn't have two years of deployment experience.

**Optimizing for the normal case.** Automation excels in the normal operating range. Reliability metrics reward the normal case. What degrades silently is the organization's capacity to handle anything outside it.

**Blaming operators when automation fails.** An engineer who hasn't manually performed a task in three years isn't negligent for struggling when automation fails. They're in the situation the design created (Bainbridge, 1983).

**Measuring automation coverage, not operational readiness.** Teams track how much is automated. They rarely track whether engineers could operate the system without it. The second number predicts incident recovery time. The first doesn't.

## Put It Into Practice

Map your team's automation against operational readiness. For each critical automated process, identify who can execute it manually and how recently they've done so. The answer shows where your real risk is, independent of reliability metrics.

Build manual practice into normal operations rather than reserving it for drills. The team that manually deploys to one environment per quarter, reads raw logs during one incident per month, and walks through one runbook by hand per rotation sustains the skills automation makes invisible. The goal isn't to eliminate automation. It's to not let the automation eliminate the competence that backs it up.

Schedule a manual runthrough of one automated procedure in your next sprint. Pick the one your team is most confident they could execute without automation. That confidence is the hypothesis. The runthrough is the test.

---

## References

Bainbridge, Lisanne (1983). "Ironies of Automation." *Automatica*, 19(6): 775-779. [https://doi.org/10.1016/0005-1098(83)90046-8](https://doi.org/10.1016/0005-1098(83)90046-8)

Bureau d'Enquêtes et d'Analyses (2012). *Final Report: Accident on 1st June 2009 to the Airbus A330-203 registered F-GZCP operated by Air France flight AF 447*. BEA. [https://www.faa.gov/sites/faa.gov/files/AirFrance447_BEA.pdf](https://www.faa.gov/sites/faa.gov/files/AirFrance447_BEA.pdf)

Cook, Richard I. (1998). "How Complex Systems Fail." Cognitive Technologies Laboratory, University of Chicago. [https://how.complexsystems.fail/](https://how.complexsystems.fail/)

Parasuraman, Raja, Thomas B. Sheridan, and Christopher D. Wickens (2000). "A Model for Types and Levels of Human Interaction with Automation." *IEEE Transactions on Systems, Man, and Cybernetics*, 30(3): 286-297. [https://doi.org/10.1109/3468.844354](https://doi.org/10.1109/3468.844354)

U.S.-Canada Power System Outage Task Force (2004). *Final Report on the August 14, 2003 Blackout in the United States and Canada: Causes and Recommendations*. U.S. Department of Energy. [https://www.energy.gov/sites/prod/files/oeprod/DocumentsandMedia/BlackoutFinal-Web.pdf](https://www.energy.gov/sites/prod/files/oeprod/DocumentsandMedia/BlackoutFinal-Web.pdf)

---

## Outtakes

**GPS and the shrinking hippocampus.** Before GPS, drivers built mental maps. Heavy GPS use prevents that practice. Frequent GPS users show reduced grey matter in the hippocampus and perform worse on navigation tasks without the device (Dahmani and Bohbot, 2020). Bainbridge wrote about nuclear plants. The same mechanism runs in every pocket.

**Alarm fatigue and the alerts nobody hears.** Hospital monitors generate so many alarms that nurses learn to silence them. One Johns Hopkins ICU averaged 187 per patient per day, most false positives (Graham and Cvach, 2010). The system designed to catch missed events trained the human response away. The automation worked. Humans adapted to the noise.

**The chess engines that made players nervous.** Chess engines became reliable enough to evaluate any position in seconds. Players who relied on them for post-game analysis got better at recognizing engine-preferred moves and worse at calculating independently. The tool built to deepen analysis was replacing it. Most teams aren't saying the same about AI code review.

---

## Changelog

**2026-05-25** Initial release.
