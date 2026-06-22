---
title: "Curiosity"
date: 2026-06-17
publishdate: 2026-06-17
lastmod: 2026-06-17
summary: "Curiosity is triggered by the gap between what you know and what you want to know. Organizations that reward answers over questions gradually close that gap the wrong way."
tags: ["curiosity", "learning", "culture"]
image:
draft: true
---

![]()

## Curiosity

A senior engineer joined a new team and spent the first two weeks asking questions the team had stopped asking years ago. Why does this cron job run every 90 seconds? Nobody knew. It turned out to be a workaround for a race condition that had since been fixed. The job generated unnecessary load for months. The team had inherited the interval and stopped seeing it.

Curiosity is not the ability to ask questions. It's the inclination to keep asking them after you've already received an answer.

## What Is Curiosity?

George Loewenstein's 1994 review established the dominant theory. Curiosity is triggered by an information gap, the perceived distance between what you know and what you want to know {{< cite 1 "Loewenstein, George (1994). The Psychology of Curiosity: A Review and Reinterpretation. Psychological Bulletin, 116(1): 75-98." >}}. A gap too small produces no curiosity. A gap too large produces anxiety. Curiosity lives in the middle, close enough to the unknown to feel reachable.

The theory explains something counterintuitive. Experts are often less curious about their own domain than novices are. Familiarity fills in the gaps. The expert knows enough to feel like they know what's left. The novice knows less but feels the distance more acutely.

Kashdan and colleagues identified five distinct expressions of curiosity: joyous exploration, deprivation sensitivity, stress tolerance, social curiosity, and thrill-seeking {{< cite 2 "Kashdan, Todd B., et al. (2018). The Five-Dimensional Curiosity Scale. Journal of Research in Personality, 73: 130-149." >}}. Most frameworks collapse these into one trait. That's a mistake. The engineer who gets obsessive about debugging is expressing deprivation sensitivity, the discomfort of an unresolved question. The one who takes on unfamiliar problems is expressing stress tolerance, the willingness to sit with not knowing.

## Why It Compounds

Ian Leslie tracked what happens when curiosity gets sustained attention versus when it gets deprioritized {{< cite 3 "Leslie, Ian (2014). Curious: The Desire to Know and Why Your Future Depends On It. Basic Books." >}}. The difference compounds fast. A curious person makes unexpected connections. An insight from distributed systems informs how they think about org design. A question about caching strategy leads to a better mental model for user behavior. The connections multiply because the questions multiply.

A non-curious person optimizes the path they're on. Execution improves. Insight does not.

When curiosity is activated, people think more deeply and rationally about decisions and produce more creative solutions {{< cite 4 "Gino, Francesca (2018). The Business Case for Curiosity. Harvard Business Review, September-October." >}}. Despite acknowledging its value, most leaders actively suppress it, citing concerns about inefficiency and risk. The suppression is rational in the short term. The costs accrue slowly and show up somewhere else.

## How Organizations Suppress It

Three mechanisms reliably kill curiosity in engineering organizations.

**Execution metrics without slack.** When every hour is accounted for and throughput is what gets measured, asking "why does this work the way it does?" is invisible work. It doesn't ship. It doesn't close a ticket. It stops happening.

**Rewarding answers over questions.** Organizations that hire for confidence and promote for decisiveness train people to minimize visible uncertainty. Admitting you don't know something signals weakness. The curious person asks questions. The person optimizing for performance gives answers.

**Scope enforcement.** "That's not your area" is the most efficient way to kill curiosity in someone with a broad sense of the system. The engineer who wants to understand why the data model was designed a certain way isn't overstepping. They're building context that will make their work better. Treating curiosity as a boundary violation ensures people know progressively less outside their immediate scope.

## How Teams Get This Wrong

**Treating curiosity as a fixed trait.** Hiring for "intellectual curiosity" in interviews and then optimizing every process for speed is incoherent. Curiosity is maintained or suppressed by the environment. The same person is curious on a team with slack and unresolved problems and incurious on a team where every question is a deviation from the plan.

**Confusing curiosity with distraction.** Broad curiosity can become procrastination. The distinction is resolution. Distraction defers the question. Curiosity pursues it to a conclusion that updates how you work. If the exploration doesn't change anything, it's browsing.

**Scheduling curiosity away.** Teams that run at full capacity for extended periods stop asking why. There's no time. The work gets done. The questions that would have made next month's work better never get asked. Slack isn't inefficiency. It's the precondition for insight.

## Put It Into Practice

Loewenstein's information gap framework is practical. Curiosity requires noticing what you don't know, which requires exposure to the edge of your knowledge, not reinforcement of what you already understand.

Write down the last three things you executed without fully understanding why. Pick the most consequential one. Spend two hours finding out why it works the way it does.

When someone gives you an answer, ask what they considered and rejected. The answer is the output. The rejected alternatives reveal the reasoning.

In your next design review or postmortem, count how many questions get asked versus statements made. If statements dominate, add questions. The ratio reveals the team's actual relationship with what it doesn't know.

Curiosity doesn't require a personality transplant. It requires friction with the unknown, enough slack to follow the question, and an environment that doesn't punish the asking.

---

## References

<ol class="references">
  <li id="ref-1">Loewenstein, George (1994). "The Psychology of Curiosity: A Review and Reinterpretation." <em>Psychological Bulletin</em>, 116(1): 75-98. <a href="https://doi.org/10.1037/0033-2909.116.1.75">https://doi.org/10.1037/0033-2909.116.1.75</a></li>
  <li id="ref-2">Kashdan, Todd B., et al. (2018). "The Five-Dimensional Curiosity Scale: Capturing the Bandwidth of Curiosity and Identifying Four Unique Subgroups of Curious People." <em>Journal of Research in Personality</em>, 73: 130-149. <a href="https://doi.org/10.1016/j.jrp.2017.11.011">https://doi.org/10.1016/j.jrp.2017.11.011</a></li>
  <li id="ref-3">Leslie, Ian (2014). <em>Curious: The Desire to Know and Why Your Future Depends On It</em>. New York: Basic Books. <a href="https://www.basicbooks.com/titles/ian-leslie/curious/9780465097166/">https://www.basicbooks.com/titles/ian-leslie/curious/9780465097166/</a></li>
  <li id="ref-4">Gino, Francesca (2018). "The Business Case for Curiosity." <em>Harvard Business Review</em>, September-October. <a href="https://hbr.org/2018/09/the-business-case-for-curiosity">https://hbr.org/2018/09/the-business-case-for-curiosity</a></li>
</ol>

---

## Outtakes

**The information gap and Wikipedia rabbit holes.** Loewenstein's theory explains the Wikipedia spiral. Every article closes one gap and opens two more. You start on Byzantine history and end up reading about medieval siege engines. The gaps regenerate faster than you can close them.

**"Curiouser and curiouser."** Carroll has Alice express curiosity as a verb in intensifying form. She doesn't become more curious. She becomes curiouser. Curiosity as an active state of deepening, not a background disposition.

**What Einstein couldn't stop noticing.** As a teenager, Einstein imagined riding alongside a beam of light and asked what he would see. The question sat unresolved for a decade before it produced the special theory of relativity. An information gap he couldn't close.

---

## Changelog

**2026-06-17** Initial draft.
