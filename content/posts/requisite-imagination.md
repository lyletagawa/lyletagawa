---
title: "Requisite Imagination"
date: 2026-05-22
publishdate: 2026-05-22
lastmod: 2026-05-22
summary: "Ron Westrum coined requisite imagination to name the organizational capacity to anticipate failure modes before they occur. Most safety practices are backward-looking. This one isn't."
tags: ["safety", "resilience", "incidents"]
image:
draft: true
---

## Requisite Imagination

Three days before launch, the team runs their pre-launch checklist. They test for every failure mode in their incident history. Database failover works. The CDN fallback works. Payment processor timeout handling works. The launch goes live. Forty minutes later, a previously unseen interaction between the new checkout flow and a three-year-old session management service causes a data inconsistency that silently affects one in twelve transactions.

None of the checklist items covered it. The team wasn't careless. The failure mode had no history to learn from. It couldn't be derived from the incident record. It had to be imagined.

The checklist was a transcript of the past. The system they launched was new. The gap between those two things is where requisite imagination lives.

## What Is Requisite Imagination?

Ron Westrum introduced the concept in his 1993 study of safety culture in high-stakes organizations. He observed that the most effective organizations didn't just learn from failures. They maintained an active capacity to imagine failure modes that hadn't occurred yet (Westrum, 1993).

Westrum defined requisite imagination as the ability to conceive of accidents and failures before they happen, based on the structure and behavior of the system rather than its history of incidents. The word "requisite" is specific. This is not optional creativity. It is a necessary organizational capacity in any system complex enough to produce novel failures.

The concept sits at the center of Westrum's broader typology of organizational cultures. Generative organizations, the highest performing in his framework, actively seek out information about potential failures. They treat near-misses as signals and encourage people closest to the work to surface what they're worried about, not just what has already gone wrong (Westrum, 2004).

## Why Organizations Lose It

Most engineering teams build their safety practices from incident histories. Post-mortems, runbooks, and checklists encode what has failed before. This is necessary. It is not sufficient.

Westrum's research found that pathological and bureaucratic organizations systematically suppress the signals that feed imagination. In a pathological organization, problems are hidden to avoid punishment. In a bureaucratic one, risks are processed through channels designed for known risks, and novel signals get lost in translation (Westrum, 2004). Both cultures produce teams that know their incident history but cannot see outside it.

The underlying dynamic is that imagination requires psychological safety. Engineers who identify a potential failure mode they cannot yet prove will encounter friction if they raise it. It sounds like speculation. It has no data behind it. In organizations that reward data-driven decision-making above all else, unproven imagination is indistinguishable from noise.

## Practices That Build It

The practical challenge is institutionalizing imagination as a discipline rather than relying on individual engineers to speculate in their own time.

**Pre-mortems.** Gary Klein's pre-mortem technique asks a team to imagine the project has already failed, then work backwards to identify causes (Klein, 2007). It works because it makes imagination the task rather than a side effect. The team isn't asked whether they have concerns. They're asked to generate the failure story. This changes what people feel licensed to say.

**Game days.** Chaos engineering exercises and deliberate failure injection practices surface what the team hadn't imagined. The failures that occur during a controlled game day are often different from the scenarios the team designed. The unexpected failures are the signal. They reveal the gap between what the team imagined and what the system can actually do.

**Near-miss investigation.** Near-misses are incidents where something failed but the impact was contained. They are often closed with a brief note and no follow-up. Westrum's research treats near-misses as among the most valuable data in a safety-conscious organization: evidence that a failure mode exists, arrived at low cost (Westrum, 1993). Investigating them as thoroughly as actual incidents builds the pattern recognition that imagination requires.

**Structured failure enumeration.** Threat modeling, FMEA, and fault tree analysis are systematic attempts to enumerate failure modes before they occur. Their value is not the completeness of the list. A failure mode not on the list can still happen. Their value is the practice of imagining failures as a team, out loud, on a regular schedule.

## Common Mistakes

**Using checklists as imagination.** Checklists encode known risks. Requisite imagination must reach beyond the incident record into the space of possible-but-unseen failures. A pre-launch checklist derived entirely from past incidents is a history lesson dressed as a safety practice.

**Treating near-misses as non-events.** The near-miss caught in staging and never reaching production is the most cost-effective incident the team will encounter. Treating it as evidence of a working system rather than a signal about an unseen failure mode discards the information it contains.

**Assuming imagination is individual.** Requisite imagination is not a trait of creative engineers. It is an organizational capacity that either exists in the culture or doesn't. One engineer who sees a potential failure and has no channel to surface it is not a safety asset. The channel is the asset.

**Bounding imagination by the systems you control.** The most dangerous failure modes often emerge from interactions at organizational boundaries. The team that can only imagine failures in their own service has left the largest surface unexamined.

**Conflating imagination with pessimism.** Teams that treat failure speculation as negativity discourage the behavior that builds resilience. Imagining failure is not a symptom of low confidence in the system. It is the practice that justifies confidence.

## Put It Into Practice

Start with the system you're about to change, not the one that's been running. Before your next pre-launch review, ask the team to write down one failure mode that isn't in the test plan and couldn't have come from your incident history. Collect the answers before anyone reads them aloud. The ones that appear twice are worth a conversation.

Run one near-miss review in your next sprint. Not an incident. A near-miss, meaning something that almost caused a problem but didn't. Treat it with the same investment you'd give an actual outage. The investigation process is identical. The only difference is the cost of learning.

The goal is a team that can look at a new system and describe what it hasn't done yet but could. Schedule the first session this week.

---

## References

Klein, Gary (2007). "Performing a Project Premortem." *Harvard Business Review*, 85(9): 18-19. [https://hbr.org/2007/09/performing-a-project-premortem](https://hbr.org/2007/09/performing-a-project-premortem)

Westrum, Ron (1993). "Cultures with Requisite Imagination." In Wise, J.A., Hopkin, V.D., and Stager, P. (Eds.), *Verification and Validation of Complex Systems: Human Factors Issues*. Springer. [https://doi.org/10.1007/978-3-662-02933-6_25](https://doi.org/10.1007/978-3-662-02933-6_25)

Westrum, Ron (2004). "A Typology of Organisational Cultures." *Quality and Safety in Health Care*, 13(Supplement 2): ii22-ii27. [https://pmc.ncbi.nlm.nih.gov/articles/PMC1765804/](https://pmc.ncbi.nlm.nih.gov/articles/PMC1765804/)

---

## Changelog

**2026-05-22** Initial publication.
