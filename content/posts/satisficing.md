---
title: "Satisficing"
date: 2026-05-26
publishdate: 2026-05-26
lastmod: 2026-05-26
summary: "The best decision is often the fastest one you can defend, not the optimal one you never finish searching for."
tags: ["productivity", "decision", "heuristics"]
image: /images/satisficing.svg
draft: false
---

## Satisficing

The decision was supposed to take a week. It's been six. The engineer keeps finding new angles. Community support. Licensing terms. How each option handles backpressure. What the migration story looks like in two years. The comparison doc has twelve tabs. Nobody's asking for an update anymore. They've stopped expecting one.

This is what happens when you start a search without deciding when it ends.

## What Is Satisficing?

Herbert Simon named it in 1956, a blend of "satisfying" and "sufficing" (Simon, 1956). The claim is simple. People don't actually optimize. They set a threshold, find the first option that clears it, then stops. Simon called that threshold an aspiration level.

Simon called this bounded rationality. We don't have unlimited time or perfect information, so hunting for the objectively best option is often irrational. At some point, continuing the search costs more than finding something marginally better would be worth (Simon, 1955). Simon won the Nobel Prize in Economics in 1978 for this work (Nobel Prize Committee, 1978).

The key word is threshold, not low bar. The threshold can be demanding. Simon's insight cuts against the assumption that longer deliberation produces better outcomes. Past a certain point, more analysis is just delay dressed up as diligence. The responsible choice isn't the most exhaustive one. It's the first one that clears a well-defined bar.

## Searching Isn't Free

Classical decision theory says to keep evaluating until you find the best option. Satisficing says to keep evaluating until something clears the bar, then stop. Those two strategies have different costs, and the first one treats the search as free.

It isn't.

If the evaluation takes six weeks, that's six weeks of engineering time, delayed roadmap items, and a team that's built up zero momentum. If most of those options would've worked fine, the extra five weeks didn't produce five weeks of value. They produced a marginal improvement over a decision that could've been made in week one. In uncertain environments, good-enough strategies often beat exhaustive ones anyway. The future is too unpredictable to fully optimize against (Gigerenzer and Todd, 1999). More analysis creates the feeling of certainty. It doesn't produce it.

Write the threshold down before you look at anything. "Handles 10k requests per second, p99 under 200ms, active community support." When something clears it, you're done. Not when you've evaluated everything. When something clears it.

## Where It Shows Up

**Technology selection.** The canonical case. There's always another framework to benchmark, another blog post to read, another edge case to account for. Without a defined threshold, the evaluation has no natural end. It keeps expanding.

**Architecture decisions.** Monolith vs. microservices, synchronous vs. async, managed vs. self-hosted. These involve trade-offs that depend on things you genuinely can't know yet. More analysis doesn't close that gap. It delays the decision while the gap stays open.

**Code review.** A reviewer who treats every PR as an optimization problem leaves fifty comments on a three-line change. Most of those comments improve the code marginally. The real cost is back-and-forth, context switching, and a slower pipeline. "Good enough to ship" is a legitimate standard.

**Hiring.** Teams that can't define what "good enough" looks like for a role keep interviewing indefinitely. Meanwhile, strong candidates accept offers elsewhere.

Maximizers, people who always search for the best, make objectively better choices on measurable criteria. They also report more regret and less satisfaction than satisficers, even after making the better choice (Schwartz, 2004). More searching doesn't produce better decisions. It produces more second-guessing.

## Common Mistakes

**Treating it as settling.** Settling means accepting something below your threshold. Satisficing means defining the threshold clearly and stopping when you clear it. The threshold can be demanding. The point is that it exists, and that you wrote it down before you started. Stopping at the first thing that clears a high bar isn't cutting corners. It's exactly what you set out to do.

**Starting without a threshold.** If you don't define "good enough" before you look at the first option, you'll never know when to stop. Each new option shifts the reference point. The search extends. That's not satisficing. That's browsing.

**Applying it where stakes don't allow it.** Satisficing makes sense when the cost of continuing the search exceeds the value of finding something better. That's not always true. Security vulnerabilities, safety-critical systems, and irreversible decisions may justify a much longer search. The principle isn't "always stop early." It's "know when early is right."

**Confusing it with low standards.** A team that ships sloppy code because "it works" hasn't satisficed. They haven't set a threshold. They've stopped caring. Satisficing requires a clear standard. Carelessness doesn't.

## Put It Into Practice

Before your next evaluation, write down the threshold before you look at the first option. Make it specific enough to be falsifiable. "Scalable" isn't a threshold. "Handles 10k requests per second with p99 under 200ms and has active community support" is. If you can't write it down before you start, you're not ready to evaluate yet.

When something clears it, stop. Don't keep looking. Every hour past that point is cost without guaranteed return.

For recurring decisions like code review and sprint planning, agree on the threshold as a team and write it down once. "What does a shippable PR look like?" is worth answering explicitly so you don't re-litigate it every cycle.

---

## References

Gigerenzer, Gerd and Peter M. Todd (1999). *Simple Heuristics That Make Us Smart*. Oxford University Press. [https://global.oup.com/academic/product/simple-heuristics-that-make-us-smart-9780195143812](https://global.oup.com/academic/product/simple-heuristics-that-make-us-smart-9780195143812)

Nobel Prize Committee (1978). "The Prize in Economic Sciences 1978." *Nobel Prize*. [https://www.nobelprize.org/prizes/economic-sciences/1978/simon/facts/](https://www.nobelprize.org/prizes/economic-sciences/1978/simon/facts/)

Schwartz, Barry (2004). *The Paradox of Choice: Why More Is Less*. Ecco. [https://en.wikipedia.org/wiki/The_Paradox_of_Choice](https://en.wikipedia.org/wiki/The_Paradox_of_Choice)

Simon, Herbert A. (1955). "A Behavioral Model of Rational Choice." *Quarterly Journal of Economics*, 69(1): 99-118. [https://academic.oup.com/qje/article-abstract/69/1/99/1919737](https://academic.oup.com/qje/article-abstract/69/1/99/1919737)

Simon, Herbert A. (1956). "Rational Choice and the Structure of the Environment." *Psychological Review*, 63(2): 129-138. [https://pages.ucsd.edu/~mckenzie/Simon1956PsychReview.pdf](https://pages.ucsd.edu/~mckenzie/Simon1956PsychReview.pdf)

Thumbnail SVG generated with [Claude Code](https://claude.ai/code) (2026).

---

## Outtakes

**The perfect is the enemy of the good.** Voltaire wrote "la perfection est l'ennemi du bien" in his *Dictionnaire philosophique* in 1764 (Voltaire, 1764), nearly two centuries before Herbert Simon formalized the same idea as satisficing. Simon gave it a mechanism and a name. Voltaire gave it eleven words.

**The grandmaster who considered fewer moves.** In the early 1970s, Herbert Simon and William Chase studied how chess grandmasters think (Chase and Simon, 1973). They expected grandmasters to consider more moves than beginners. They found the opposite. Pattern recognition let grandmasters ignore most possibilities immediately. They searched until a move cleared a threshold, then stopped. The best players in the world weren't optimizing. They were satisficing faster than anyone else.

**The WD-40 that wasn't the best.** WD-40 was developed in 1953 for aerospace. The name means water displacement, 40th attempt. Chemists tried 39 formulations before one worked. They stopped. Nobody looked for the 41st. Mechanics at Convair started taking it home for squeaky hinges. The company noticed and put it in a can. It now sells in 176 countries. The optimal formulation is still undiscovered.

---

## Changelog

**2026-05-26** Initial release.
