---
title: "Chesterton's Fence"
date: 2026-05-24
publishdate: 2026-05-24
lastmod: 2026-05-24
summary: "Don't remove what you don't understand. The constraint that looks arbitrary usually isn't, and the cost of removing it surfaces only after the thing it was preventing has already happened."
tags: ["resilience", "management"]
image:
draft: true
---

## Chesterton's Fence

A service had a 30-second timeout on all outbound HTTP calls. It had been there since the initial deploy, set by an engineer who had since moved to a different team. No comment explained it. The logs showed it had never fired. A new engineer, cleaning up the configuration, removed it.

Three weeks later, a third-party payment processor had a partial outage. Their API was accepting connections but not responding. Requests to the payment service started hanging. No timeout meant no release. The connection pool filled. The service stopped responding, and everything that called it cascaded.

The processor recovered in forty minutes. The cascading failure took six hours to fully resolve. The engineer who set the timeout had been through an identical incident at a previous company. The fence was invisible because it had been working.

## What Is Chesterton's Fence?

G.K. Chesterton introduced the principle in 1929 in "The Thing: Why I Am a Catholic." The passage describes a reformer who encounters a fence across a road, sees no purpose for it, and proposes to tear it down. Chesterton's answer: if you don't know why the fence was built, you don't yet have the information needed to decide whether it should come down (Chesterton, 1929).

The principle is not a defense of the status quo. Chesterton was explicit that fences can be removed. The condition is that the person proposing removal can explain why the fence was built. Understanding the original purpose is what enables a sound judgment about whether that purpose still applies.

The constraint on removal is epistemic, not conservative. Age is not the argument. The argument is that you cannot distinguish between things that can be safely removed and things that cannot without first knowing what the thing was for.

## Why Fences Disappear

The engineer who builds the fence understands the incident or edge case it prevents. That understanding rarely makes it into the code. It lives in a commit message, a closed ticket, a Slack channel that has been archived, or the memory of someone who has left.

Code gets read far more than commit history. Configuration gets read more than code, usually during incidents or refactors when someone is moving fast and context is the last thing they are looking for. When the context is absent, the artifact looks arbitrary. Arbitrary things invite removal.

Turnover thins this further. Each cycle brings engineers who were not there for the incident that motivated the fence. After two or three rotations, nobody on the team can explain why the thing exists. "Nobody can explain this" becomes the justification for removing it, when it should be the reason to investigate (Feathers, 2004).

## Where Engineers Find Fences

**Timeouts and circuit breakers.** A timeout that has never fired in production logs is doing its job. The absence of triggers is not evidence of uselessness. It is evidence that the condition being defended against has not occurred yet. Removing it changes the system's posture from defended to exposed without any visible signal (Nygard, 2018).

**Database constraints.** Database constraints encode assumptions about data integrity that application code may or may not independently enforce. CHECK constraints, UNIQUE constraints, NOT NULL declarations: each one is a law the database will enforce even when the application forgets to. Removing them to speed up a migration trades a known guarantee for an unknown risk.

**Backward compatibility layers.** An old request format still being accepted, a header added to every response, a deprecated endpoint that redirects: these exist because something is still using them. Internal observability rarely sees all external consumers. The safe assumption is that something you cannot see is relying on the behavior you are about to remove.

**Process steps.** A deploy checklist with a manual verification step, a migration approval gate, a code review requirement that seems redundant given CI: these often exist because someone skipped them and paid a cost large enough to create a process. The process encodes the lesson. Removing the step removes the memory.

## Common Mistakes

**Using "nobody can explain it" as justification.** The absence of a living explainer is not evidence of purposelessness. It is evidence of institutional memory loss. Those are different diagnoses. One calls for removal. The other calls for investigation.

**Treating age as evidence of obsolescence.** Old code in a production system has survived because it has not been wrong in a way that got it replaced. A timeout set in 2019 is more likely to reflect hard-won experience than code written last week, not less.

**Confusing the fence with dead code.** Chesterton's principle applies to things that could be doing something protective. It does not apply to a flag that has been false in every environment for three years with no observable effect, or an import of a module that no longer exists. The diagnostic question is whether the thing could be load-bearing. If the answer is clearly no, the principle does not apply.

**Skipping the investigation to save time.** The justification for skipping is usually urgency. The removal is scoped for a sprint. Going back to find the original reason costs time that is not in the estimate. This is the same reasoning that normalizes deviance: individually defensible decisions that accumulate into a system with no memory of its own constraints.

## Put It Into Practice

When you encounter something you don't understand and are inclined to remove it, check the git log first. Run `git log -p` on the file and look for the commit that introduced it. The commit message may explain it directly. If not, the hash leads to the pull request, which often holds the context the code doesn't.

If the history is inconclusive, look for the incident or bug report that predates the change. Search the issue tracker for terms from the code. Fifteen minutes of investigation changes "nobody knows why this is here" into a decision you can defend.

If you cannot find why the fence was built and cannot afford the time to look, do not remove it quietly. File a ticket, record the uncertainty, and come back. The difference between "removed after investigation" and "removed because nobody remembered" is the difference between a justified decision and a deferred incident.

## References

Chesterton, G.K. (1929). *The Thing: Why I Am a Catholic*. Dodd, Mead and Company. [https://archive.org/details/thethingorwhyiam00ches](https://archive.org/details/thethingorwhyiam00ches)

Feathers, Michael (2004). *Working Effectively with Legacy Code*. Prentice Hall. [https://www.oreilly.com/library/view/working-effectively-with/0131177052/](https://www.oreilly.com/library/view/working-effectively-with/0131177052/)

Nygard, Michael T. (2018). *Release It! Design and Deploy Production-Ready Software* (2nd ed.). Pragmatic Bookshelf. [https://pragprog.com/titles/mnee2/release-it-second-edition/](https://pragprog.com/titles/mnee2/release-it-second-edition/)

## Changelog

**2026-05-24** Initial publication.
