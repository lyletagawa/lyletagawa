---
title: "PagerDuty Summit 2020"
date: 2020-09-24
summary: "Learn how you can move faster and focus on the things that matter by using incident analysis as your secret weapon. Operating at speed and at scale tests the capabilities of even the most experienced engineering teams. In this software world, it is inevitable that things will break. When they do, what do you do? Pick up the pieces and carry on? What if that’s not enough? Learning from incidents has taught us that broken things can lead to powerful opportunities, but only when we’re looking at them through the right lens."
image: https://assets.swoogo.com/uploads/medium/487256-5ea875614e45f.png
tags: ["learning", "incidents", "resilience"]
---

## [PagerDuty Summit 2020](https://summit.pagerduty.com/global)

40+ hours of sessions and keynotes – How much time do you have?

- 2 minutes? Read my notes on either:
  - [Incident Analysis: Your Organization's Secret Weapon]({{< relref "#incident" >}}) by Nora Jones
  - [CI/CD and the Rise of CV]({{< relref "#CV" >}}) by Casey Rosenthal
- 20 minutes? Watch either:
  - [Incident Analysis: Your Organization's Secret Weapon]({{< relref "#incident" >}}) by Nora Jones
  - [CI/CD and the Rise of CV]({{< relref "#CV" >}}) by Casey Rosenthal
- 2 hours? Add these to your playlist:
  - [Building Resilient Engineers]({{< relref "#Resilient" >}}) by Brian Rutkin
  - [Building and Scaling SRE Teams]({{< relref "#SRE" >}}) by Tammy Bryant (Butow)
  - [In the Heat of the Page: Coping with the Root Cause of Incident Stress]({{< relref "#stress" >}}) by J. Paul Reed

## Sessions

### Incident Analysis: Your Organization's Secret Weapon {#incident}

[Nora Jones](https://summit.pagerduty.com/global/speaker/140805/nora-jones), Co-founder, Jeli

{{< youtube lGXgHb-_GI8 >}}

> Successful organizations don’t just react to incidents—they use them to learn how to be proactive. To build a stronger system, safeguarded from making the same mistakes again. Extensive research has mapped out Incident Analysis as a strategy to help teams learn, adapt, and prioritize by focusing on what matters. More focused efforts help us move faster. And farther. Which is why incident analysis could be your organization’s secret weapon. It could change the game, and keep you on top of yours. So, are you ready to turn your “failures” into success?

The incident:

- is a catalyst to understanding where you need to improve your [sociotechnical system](https://en.wikipedia.org/wiki/Sociotechnical_system)
- is a catalyst to showing you what your Organization is good at, and what needs improvement
- helps you understand the difference between
  - how you structure your on-call teams in theory vs.
  - how you structure them in practice

Good incident analysis can help you with:

- Headcount
- Training
- Unlocking tribal knowledge
- Quantifying how much coordination efforts during incidents cost
- Understanding bottlenecks

What can you do today to improve incident analysis?

- Give folks time and space to get better at analysis. This can be trained
- Come up with different metrics: Look at the people
- Investigator on-call rotations
- Allow time to investigate the “big ones”

How do you know if it’s working?

- More folks are (voluntarily) reading the incident review
- More folks are attending the incident review
- You’re not seeing the same folks (i.e. heros) pop into every incident
- Folks feel more confident
- Teams are collaborating more
- Better shared understanding of the definition of an incident

### CI/CD and the Rise of CV {#CV}

[Casey Rosenthal](https://summit.pagerduty.com/global/speaker/140773/casey-rosenthal), Co-Founder, Verica

{{< youtube OOLSaYiBzXc >}}

> Like CI/CD, Continuous Verification is born out of a need to navigate increasingly complex systems. Modern organizations can’t validate that the internal machinations of the system work as intended, so instead they verify that the output of the system macthes expectations.

Continuous Verification is a proactive, experimentation tool for verifying system behavior.

Myths in complex system design and operations. Improve your availability by:

- Myth 1: Removing the people who cause accidents (i.e. the “bad apple” concept)
- Myth 2: Documenting best practices and runbooks
- Myth 3: Defending against prior root causes (“at best, you’re wasting your time if you’re doing RCA”)
- Myth 4: Enforcing procedures (“confusing management’s notion of work as idealized, with the engineer’s notion of work actually as done”)
- Myth 5: Avoiding risk
- Myth 6: Simplifying a complex system
- Myth 7: Adding redundancy

> Chaos Engineering is the facilitation of experiments to uncover systemic weakness
>
> <https://principlesofchaos.org/>

### Building Resilient Engineers {#Resilient}

[Brian Rutkin](https://summit.pagerduty.com/global/speaker/140125/brian-rutkin), Senior Staff SRE, Twitter

{{< youtube LpCVXMe2OMo >}}

> It’s not bad or wrong for a human to need to take action, we just want to reduce that burden of taking action to the safest, quickest, and easiest solution possible and get back to the system taking care of itself.

Communication:

- Get better at communication and everything gets easier

Simplify:

- Make answers obvious/easy to find
  - owners, escalation path, documentation, dashboards, error index
- Make answers easy to understand
  - ensure a 3am sleep addled brain can understand the problem
- Bias answers toward action
  - know what steps to take to mitigate
- Answers should empower individuals to have agency
  - escalation means you have more work to do

Oncall and Incident Preparation:

- Runbook
- Training/Onboarding/Shadowing
- Roles during incident  (active troubleshooter, communications, support)
- Escalation agreement
- Practice dealing with incidents (game days, chaos testing, failovers, load testing)

Fatigue and Burnout:

- Each stressor that we bear reduces the energy we have left to apply directly to our work
- A lack of agency or the ability to bring about change
- Having an unclear mission or unclear directive/goal
- Invisible stressors

Building Resilience:

- (Business/Employee) Resource Groups 
- [Allies](https://blog.twitter.com/en_us/topics/company/2020/allyship-right-now-black-lives-matter.html)
- Mentorships
- Sponsorships
- Unconscious Bias Training
- Focus Time
- Self-Care

### Building and Scaling SRE Teams {#SRE}

[Tammy Bryant (Butow)](https://summit.pagerduty.com/global/speaker/140126/tammy-bryant), Principal SRE, Gremlin

### In the Heat of the Page: Coping with the Root Cause of Incident Stress {#stress}

[J. Paul Reed](https://summit.pagerduty.com/global/speaker/138504/j.-paul-reed), Senior Applied Resilience Engineer, CORE Team, Netflix

{{< youtube X8CKbttmdws >}}

> Most SREs, if they're being truly honest, will admit that being involved in an incident, either as a responder or incident commander, is... inherently stressful. But why is that? What makes incidents "stressful?" We’ll take a look at some of the underlying reasons why we experience stress when engaged in incidents.

Team Common Ground

- Basic Compact
- Goal Alignment and Commitment
- Interpredictability (with other actors in the system)
- Sustain and Repair

Reducing (cognitive) stress during incidents

- Practice interpredictability
- Learning to “move” through time: Practice assessing system state and “moving” between different timespans of discretion
  - Venn diagram: probable, plausible, possible, preferable

Adaptive Capacity

- Get a sense of your current team’s capacity for adaptation
- Understand “degrees of freedom” and constraints
- Deliberately create space to share more stories
