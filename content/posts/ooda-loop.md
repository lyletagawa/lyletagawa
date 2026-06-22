---
title: "OODA Loop"
date: 2026-04-30
publishdate: 2026-05-02
lastmod: 2026-06-17
summary: "How John Boyd's OODA Loop (Observe, Orient, Decide, Act) applies to technology leadership, and practical strategies for eliminating the bottlenecks that slow your competitive cycles."
tags: ["adaptation", "awareness"]
image: /images/ooda-loop-f86.jpg
---

![](/images/ooda-loop-f86.jpg)
*Photo: [U.S. National Archives (1981)](https://nara.getarchive.net/media/four-f-86f-sabre-aircraft-from-the-japanese-blue-impuse-precision-flying-team-1277ac). Public domain.*

## OODA Loop

Colonel John Boyd developed the OODA Loop (Observe, Orient, Decide, Act) while studying aerial combat in the 1950s and 1960s. Boyd observed that pilots who could cycle through observation, orientation, decision, and action faster than their opponents consistently won engagements, regardless of aircraft superiority {{< cite 1 "Boyd, John R. (1976). Destruction and Creation. U.S. Army Command and General Staff College." >}}.

Same story in business and technology. Loop faster than your competition and compound your advantage.

## The Four Phases Explained

The OODA Loop consists of four interconnected phases forming a continuous cycle {{< cite 2 "Boyd, John R. (1987). Organic Design for Command and Control. Unpublished briefing." >}}. Observe gathers data from system telemetry, customer feedback, and market research. Orient synthesizes this data through organizational culture and domain expertise, transforming information into clear insight. Decide selects a course of action, balancing analysis with acceptable risk. Act executes decisions and feeds results back into observation.

These phases form a continuous loop, not a sequential checklist {{< cite 2 "Boyd, John R. (1987). Organic Design for Command and Control. Unpublished briefing." >}}. While you act on one decision, you simultaneously observe its effects, orient to new information, and prepare subsequent decisions.

## Why Speed Matters

Boyd's central insight was that faster OODA cycles create disorientation in opponents {{< cite 1 "Boyd, John R. (1976). Destruction and Creation. U.S. Army Command and General Staff College." >}}. When you complete your loop faster than competitors, you act on current information while they respond to outdated conditions. Each iteration compounds this advantage.

Companies with faster build-measure-learn cycles consistently outperform slower competitors {{< cite 3 "Ries, Eric (2011). The Lean Startup. Crown Business." >}}. Elite performers deploy code 973x more frequently than low performers, with direct business impact {{< cite 4 "Forsgren, Nicole, et al. (2018). Accelerate: The Science of Lean Software and DevOps. IT Revolution Press." >}}.

Organizations that iterate weekly learn 52 times per year. Those that iterate quarterly learn four times. Over multiple years, this difference becomes insurmountable {{< cite 3 "Ries, Eric (2011). The Lean Startup. Crown Business." >}}.

## Impediments to Fast Loops

**Observation bottlenecks** emerge from poor instrumentation and siloed data. When teams lack visibility into production behavior, orientation suffers {{< cite 4 "Forsgren, Nicole, et al. (2018). Accelerate: The Science of Lean Software and DevOps. IT Revolution Press." >}}.

**Orientation failures** come from cognitive biases and outdated mental models. Companies orient based on past success rather than current reality, creating blind spots {{< cite 5 "Argyris, Chris (1977). Double Loop Learning in Organizations. Harvard Business Review, 55(5): 115-125." >}}. Cognitive load and communication structures shape how orgs orient to technical challenges {{< cite 6 "Skelton, Matthew, and Manuel Pais (2019). Team Topologies. IT Revolution Press." >}}.

**Decision paralysis** shows up in committee-driven processes and excessive risk aversion. Decision velocity correlates strongly with competitive advantage {{< cite 4 "Forsgren, Nicole, et al. (2018). Accelerate: The Science of Lean Software and DevOps. IT Revolution Press." >}}.

**Action delays** result from deployment friction and technical debt. Reducing batch sizes and automating pipelines directly impacts throughput {{< cite 7 "Kim, Gene, et al. (2016). The DevOps Handbook. IT Revolution Press." >}}.

Each impediment multiplies cycle time. An org that observes infrequently, orients poorly, decides cautiously, and acts slowly operates at a fraction of its potential velocity.

## Practical Applications for Tech Leadership

**Incident response** maps directly to OODA. Effective incident management observes anomalies quickly, orients through runbooks and diagnostics, decides on remediation, and acts through automated procedures. Agentic AI is compressing the observe-orient phases from minutes to seconds, keeping humans in the decision and action loop. Mean time to recovery depends more on loop speed than on preventing all failures {{< cite 8 "Beyer, Betsy, et al. (2016). Site Reliability Engineering: How Google Runs Production Systems. O'Reilly Media." >}}.

**Product development** accelerates through continuous deployment. Feature flags enable observation, A/B testing provides orientation, and rapid deployment enables quick action. Organizations practicing continuous delivery deploy 46x more frequently with 440x faster lead times {{< cite 4 "Forsgren, Nicole, et al. (2018). Accelerate: The Science of Lean Software and DevOps. IT Revolution Press." >}}.

**Strategic planning** benefits from continuous adjustment rather than annual cycles {{< cite 3 "Ries, Eric (2011). The Lean Startup. Crown Business." >}}. Observe market dynamics continuously, orient strategy quarterly, decide on tactical adjustments monthly, and act on opportunities as they emerge.

**Agentic reliability engineering.** Production signals exceed any team's triage capacity. Alert fatigue is a symptom of Orientation overwhelmed by input. Agentic Reliability Engineering (ARE) treats intelligence as a first-class reliability primitive {{< cite 9 "Jambor, David (2026). Agentic Reliability Engineering. O'Reilly Media." >}}. Agents handle Observe and Orient continuously, filtering noise and surfacing what needs a decision. Humans move above the loop, designing intent and validating outcomes rather than triaging every signal.

## How Teams Get This Wrong

**Neglecting the Orientation phase.** Observation and Action are the visible parts of the loop. Orientation is where the loop is won or lost. Boyd spent more time on Orientation than the other three phases combined {{< cite 2 "Boyd, John R. (1987). Organic Design for Command and Control. Unpublished briefing." >}}. It filters incoming data through experience, mental models, and organizational norms. Teams that rush from Observe to Decide skip the phase that determines whether their speed is applied to the right problem.

**Confusing OODA with a REPL.** Engineers sometimes treat the Read-Eval-Print Loop as a metaphor for fast iteration. The loop resembles OODA but lacks Orientation. Each cycle reacts without building a mental model. Geoffrey Huntley calls the agentic AI version the Ralph Wiggum Loop. It describes an agent that iterates on its own outputs until it stumbles onto a correct result, with no orientation between cycles {{< cite 10 "Huntley, Geoffrey (2025). Ralph Wiggum as a 'software engineer'. ghuntley.com." >}}.

**Sustaining high-velocity indefinitely.** Constant high velocity exhausts teams, decreasing productivity and degrading decision quality {{< cite 4 "Forsgren, Nicole, et al. (2018). Accelerate: The Science of Lean Software and DevOps. IT Revolution Press." >}}. Some advantages come from deep research that can't be rushed, not from cycling faster. Decisions that demand methodical, effortful reasoning can't be compressed without degrading their quality {{< cite 11 "Kahneman, Daniel (2011). Thinking, Fast and Slow. Farrar, Straus and Giroux." >}}.

## Put It Into Practice

Speed without orientation is just faster mistakes. Boyd spent more time on the Orient phase than the other three combined because that's where the loop is won or lost.

Audit your org's loop. Find where observations get delayed, what distorts orientation, what causes decision paralysis, and what impedes action. Each impediment is an opportunity. Eliminate them, one cycle at a time.

---

## References

<ol class="references">
  <li id="ref-1">Boyd, John R. (1976). <em>Destruction and Creation</em>. U.S. Army Command and General Staff College. <a href="https://www.coljohnboyd.com/static/documents/1976-09-03__Boyd_John_R__Destruction_and_Creation.pdf">https://www.coljohnboyd.com/static/documents/1976-09-03__Boyd_John_R__Destruction_and_Creation.pdf</a></li>
  <li id="ref-2">Boyd, John R. (1987). <em>Organic Design for Command and Control</em>. Unpublished briefing. <a href="https://www.coljohnboyd.com/static/documents/1987-05__Boyd_John_R__Organic_Design_for_Command_and_Control__PPT-PDF.pdf">https://www.coljohnboyd.com/static/documents/1987-05__Boyd_John_R__Organic_Design_for_Command_and_Control__PPT-PDF.pdf</a></li>
  <li id="ref-3">Ries, Eric (2011). <em>The Lean Startup</em>. Crown Business. <a href="https://theleanstartup.com/book">https://theleanstartup.com/book</a></li>
  <li id="ref-4">Forsgren, Nicole, Jez Humble, and Gene Kim (2018). <em>Accelerate: The Science of Lean Software and DevOps</em>. IT Revolution Press. <a href="https://itrevolution.com/product/accelerate/">https://itrevolution.com/product/accelerate/</a></li>
  <li id="ref-5">Argyris, Chris (1977). "Double loop learning in organizations." <em>Harvard Business Review</em>, 55(5): 115-125. <a href="https://hbr.org/1977/09/double-loop-learning-in-organizations">https://hbr.org/1977/09/double-loop-learning-in-organizations</a></li>
  <li id="ref-6">Skelton, Matthew, and Manuel Pais (2019). <em>Team Topologies: Organizing Business and Technology Teams for Fast Flow</em>. IT Revolution Press. <a href="https://itrevolution.com/product/team-topologies/">https://itrevolution.com/product/team-topologies/</a></li>
  <li id="ref-7">Kim, Gene, Jez Humble, Patrick Debois, and John Willis (2016). <em>The DevOps Handbook</em>. IT Revolution Press. <a href="https://itrevolution.com/product/the-devops-handbook/">https://itrevolution.com/product/the-devops-handbook/</a></li>
  <li id="ref-8">Beyer, Betsy, Chris Jones, Jennifer Petoff, and Niall R. Murphy (2016). <em>Site Reliability Engineering: How Google Runs Production Systems</em>. O'Reilly Media. <a href="https://sre.google/sre-book/">https://sre.google/sre-book/</a></li>
  <li id="ref-9">Jambor, David (2026). <em>Agentic Reliability Engineering</em>. O'Reilly Media. <a href="https://www.oreilly.com/library/view/agentic-reliability-engineering/0642572294809">https://www.oreilly.com/library/view/agentic-reliability-engineering/0642572294809</a></li>
  <li id="ref-10">Huntley, Geoffrey (2025). "Ralph Wiggum as a 'software engineer'." ghuntley.com. <a href="https://ghuntley.com/ralph/">https://ghuntley.com/ralph/</a></li>
  <li id="ref-11">Kahneman, Daniel (2011). <em>Thinking, Fast and Slow</em>. New York: Farrar, Straus and Giroux. <a href="https://us.macmillan.com/books/9780374533557/thinkingfastandslow/">https://us.macmillan.com/books/9780374533557/thinkingfastandslow/</a></li>
</ol>

---

## Outtakes

**The kill ratio.** Boyd's original insight came from a puzzle. U.S. pilots in F-86 Sabres were shooting down MiGs 10:1 despite the MiG-15 being the superior aircraft (on paper). His explanation wasn't pilot skill. The F-86's bubble canopy gave better situational awareness. Its hydraulic controls gave faster stick response. Pilots who could see more and react faster completed their loops first.

**Napoleon's corps system.** A century and a half before Boyd, Napoleon restructured the French army into semi-autonomous corps that could act independently without waiting for central command. Each corps completed its own loop without needing permission.

**The Fighter Mafia.** Boyd pushed for a lightweight, maneuverable fighter using his Energy-Maneuverability theory. The Air Force establishment wanted the opposite, the F-15, a large, twin-engine fighter with advanced radar and a growing price tag. Boyd launched a parallel competition. The winning design, the YF-16, became the F-16. By the time it entered service, the Air Force had added radar, avionics, and weight until it had drifted back toward the aircraft Boyd fought against.

---

## Changelog

**2026-06-07** Added Agentic Reliability Engineering.  
**2026-05-16** Added Outtakes section. Removed Senior IC section.  
**2026-05-02** Updated Boyd reference links to direct PDFs via the Colonel John Boyd Digital Archive.
