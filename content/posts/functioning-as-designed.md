---
title: "Functioning As Designed"
date: 2026-07-09
publishdate: 2026-07-09
lastmod: 2026-07-09
summary: "Traditional accident models assume every failure traces back to a broken part. STAMP treats safety as a control problem instead, which explains disasters where every component worked exactly as designed."
tags: ["safety", "systems", "complexity"]
image: /images/functioning-as-designed.jpg
draft: false
---

![Traditional accident models assume every failure traces back to a broken part. STAMP treats safety as a control problem instead, which explains disasters where every component worked exactly as designed.](/images/functioning-as-designed.jpg)
*A U.S. Army UH-60 Black Hawk helicopter in flight. Photo: [SGT Tom Pullin, U.S. Army (2004)](https://commons.wikimedia.org/wiki/File:US_Army_UH-60_Black_Hawk_(2052956641).jpg). Public domain.*

## Functioning As Designed

On April 14, 1994, two U.S. Air Force F-15s shot down two U.S. Army Black Hawk helicopters over northern Iraq, killing all 26 aboard. The IFF identification systems worked. The radios worked. The AWACS radar plane overhead had the Black Hawks on its scope the whole time. Nothing broke{{< cite 1 "1994 Black Hawk shootdown incident. Wikipedia." >}}.

Investigators went looking for the failed component anyway, because that's how investigations usually work. They didn't find one. Nearly everyone and everything performed exactly as trained. The system killed 26 of its own people while functioning as designed.

That gap, between "nothing failed" and "26 people died," is what Nancy Leveson's STAMP was built to explain.

## What Is STAMP?

Systems-Theoretic Accident Model and Processes (STAMP), developed by MIT professor Nancy Leveson and first published in 2004, starts from a claim that's obvious once stated and routinely ignored. Accidents in complex systems can happen without any component failing at all{{< cite 2 "Leveson, Nancy (2004). A New Accident Model for Engineering Safer Systems. Safety Science, 42(4): 237-270." >}}. The danger isn't always a broken part. It's an interaction between working parts nobody designed for.

Leveson's alternative reframes safety as a control problem, not a failure problem. A system stays safe when a hierarchy of controls (regulations constraining companies, managers constraining operators, software constraining hardware) enforces a set of safety constraints, things that must never happen. An accident is a control action that should have prevented a hazard and didn't, usually because whoever issued it was working from an inaccurate picture of what was actually happening{{< cite 3 "Leveson, Nancy G. (2011). Engineering a Safer World: Systems Thinking Applied to Safety. MIT Press." >}}.

In the Black Hawk case, rules of engagement, radio protocols, IFF procedures, and AWACS oversight were all controls meant to prevent exactly this. Each looked adequate alone. Together, that day, they weren't.

## Why Chain-Of-Events Models Break Down

The dominant accident model for most of the 20th century pictures an accident as a row of dominoes, or its more forgiving cousin, holes lining up in slices of Swiss cheese. Something starts a chain, each link triggers the next, and the accident is whatever falls out the far end. Find the link that shouldn't have broken and you've fixed the problem.

That model works well for accidents caused by an actually broken part. It works badly for software, which doesn't fail the way a bolt fails, and does exactly what it was told, every time, while the danger lives in requirements nobody wrote. An investigation looking for the broken link in a system where nothing broke will force-fit a component as the culprit, or stop at "operator error."

Leveson's point isn't that component failures don't matter. It's that treating them as the only source of danger leaves a whole category of modern accidents invisible to the investigation method itself.

## The Core Concepts

**Safety constraints.** What the system must never do, stated as constraints, not goals. Not "land the plane safely" but "never allow two aircraft to occupy the same airspace."

**Hierarchical control structure.** Every complex system is a stack of controllers, each constraining the level below and receiving feedback from it, from regulator down to company, manager, operator, machine. Safety depends on every layer, not just the bottom one.

**Feedback loops.** A controller can only enforce a constraint if its picture of the system's state is accurate. Stale or misleading feedback, not the control action itself, is where most of Leveson's case studies go wrong.

**Inadequate control, not failure.** The unit of analysis is never "what broke," but "what control was supposed to prevent this, and why did it seem adequate at the time."

## STPA and CAST

STAMP produces two practical techniques, one for before an accident, one for after. **STPA (System-Theoretic Process Analysis)** is the forward-looking one. Applied during design, it asks what unsafe control actions the system could issue and works backward to the design flaws that would let that happen{{< cite 3 "Leveson, Nancy G. (2011). Engineering a Safer World: Systems Thinking Applied to Safety. MIT Press." >}}.

**CAST (Causal Analysis based on System Theory)** is retrospective, STAMP's answer to a postmortem. Instead of tracing a chain back to a root cause, it maps the control structure that existed and asks, at every level, why the controls in place didn't prevent the loss{{< cite 3 "Leveson, Nancy G. (2011). Engineering a Safer World: Systems Thinking Applied to Safety. MIT Press." >}}.

Google's SRE organization has been exploring both, framing STPA as a way to find "unknown unknowns" that traditional reliability engineering misses, and CAST as a way to "supercharge" postmortems that would otherwise stop at the first plausible root cause{{< cite 4 "Systems-Theoretic Accident Model and Processes (STAMP) at Google. Google SRE." >}}.

## From Aviation to Software

Most software postmortems are chain-of-events investigations wearing a template. "Five whys" is a chain, and so is a root-cause field in an incident ticket. Both assume the thread, followed back far enough, lands on one thing to fix, fine for a null pointer exception, bad for an incident where every service returned exactly what it believed to be true, and the outage happened anyway.

A CAST-style postmortem asks a different question at every level. Not "what broke," but "what information was this component acting on, and why did it seem sufficient at the time." A load balancer that routed traffic to an unhealthy node wasn't broken, it accurately followed stale health-check data. The bug is in the loop between "actual health" and "believed health," and fixing the balancer's logic without fixing that loop just moves the accident to a different day.

## Common Mistakes

**Treating it as root-cause analysis with better vocabulary.** STAMP's premise is that there usually isn't a single root cause worth finding. A team that draws a control structure diagram and still writes "root cause: X" at the bottom has kept the old model and added drawing time.

**Skipping the control structure.** STPA and CAST both depend on mapping who controls what before asking what went wrong. Jumping straight to "what unsafe actions could occur" without that map produces guesses, not analysis.

**Confusing no-blame with no-accountability.** STAMP doesn't ask who to punish, but it still names which control was inadequate and why. Treating "systemic" as a synonym for "nobody's job to fix" wastes the analysis.

**Only ever looking backward.** CAST after incidents without STPA before they happen leaves you permanently one accident behind. The forward-looking half is the one organizations skip most, since no incident forces it onto the calendar.

## Put It Into Practice

Pick one incident review that stopped at a single root cause, and redo it as a control structure instead. List who or what was supposed to prevent the outcome at each layer, from the on-call engineer up through alerting, the deploy pipeline, and whatever process approved the change. At each layer, ask what information that controller acted on, and whether it was accurate at the time.

You'll usually find the failure wasn't a broken link. It was a control acting correctly on a picture of the world that had quietly gone stale.

Do this once on a past incident before trying it live on your next one. The mapping step takes real practice, and an incident in progress is the worst time to learn it.

## Go Further

**The full method, step by step.** Leveson and Thomas's STPA Handbook is the working reference for actually running an analysis, well beyond what a single post can cover{{< cite 5 "Leveson, Nancy G., and John P. Thomas (2018). STPA Handbook. MIT Partnership for Systems Approaches to Safety and Security." >}}.

**A different kind of near-miss.** The U.S. Navy's SUBSAFE program, another of Leveson's case studies, has had zero loss-of-submarine incidents from non-combat causes since its creation, worth reading for what STAMP-style thinking looks like when it's actually working{{< cite 3 "Leveson, Nancy G. (2011). Engineering a Safer World: Systems Thinking Applied to Safety. MIT Press." >}}.

---

## References

<ol class="references">
  <li id="ref-1">"1994 Black Hawk shootdown incident." Wikipedia. <a href="https://en.wikipedia.org/wiki/1994_Black_Hawk_shootdown_incident">https://en.wikipedia.org/wiki/1994_Black_Hawk_shootdown_incident</a></li>
  <li id="ref-2">Leveson, Nancy (2004). "A New Accident Model for Engineering Safer Systems." <em>Safety Science</em>, 42(4): 237-270. <a href="https://doi.org/10.1016/S0925-7535(03)00047-X">https://doi.org/10.1016/S0925-7535(03)00047-X</a></li>
  <li id="ref-3">Leveson, Nancy G. (2011). <em>Engineering a Safer World: Systems Thinking Applied to Safety</em>. MIT Press. <a href="https://mitpress.mit.edu/9780262533690/engineering-a-safer-world/">https://mitpress.mit.edu/9780262533690/engineering-a-safer-world/</a></li>
  <li id="ref-4">"Systems-Theoretic Accident Model and Processes (STAMP) at Google." Google SRE. <a href="https://sre.google/resources/practices-and-processes/stpa/">https://sre.google/resources/practices-and-processes/stpa/</a></li>
  <li id="ref-5">Leveson, Nancy G., and John P. Thomas (2018). <em>STPA Handbook</em>. MIT Partnership for Systems Approaches to Safety and Security. <a href="https://www.flighttestsafety.org/images/STPA_Handbook.pdf">https://www.flighttestsafety.org/images/STPA_Handbook.pdf</a></li>
</ol>

---

## Outtakes

**Leveson also analyzed the Vioxx recall as a control problem.** Merck's drug-safety monitoring, not just the drug's chemistry, was one of the control structures Leveson examined in the book, treating a pharmaceutical recall with the same framework she applies to plane crashes ([MIT Press, 2011](https://mitpress.mit.edu/9780262533690/engineering-a-safer-world/)).

**A Canadian town's water supply is one of her case studies too.** The 2000 Walkerton, Ontario E. coli contamination, seven deaths from tainted municipal water, appears in the book as an example of a safety control structure that decayed gradually rather than failing all at once ([MIT Press, 2011](https://mitpress.mit.edu/9780262533690/engineering-a-safer-world/)).

---

## Changelog

**2026-07-09** Initial release.  
