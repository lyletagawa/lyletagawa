---
title: "Bikeshedding"
date: 2026-07-19
publishdate: 2026-07-19
lastmod: 2026-07-20
summary: "A committee approved a $10 million reactor in two and a half minutes, then spent 45 minutes on a bike shed. The time spent on any decision runs opposite to how much it actually matters."
tags: ["decisions", "meetings", "teams"]
image: /images/bikeshedding.jpg
draft: true
---

![A committee approved a $10 million reactor in two and a half minutes, then spent 45 minutes on a bike shed. The time spent on any decision runs opposite to how much it actually matters.](/images/bikeshedding.jpg)
*A bicycle shed. Photo: [SeppVei (2011)](https://commons.wikimedia.org/wiki/File:Bicycle_shed.JPG). CC0.*

## Bikeshedding

A finance committee once approved a $10 million nuclear reactor in two and a half minutes. Then it spent 45 minutes arguing about a $2,350 bicycle shed. Then it spent an hour and fifteen minutes on the annual coffee budget, $57{{< cite 1 "Parkinson, C. Northcote (1957). Parkinson's Law: The Pursuit of Progress." >}}.

Nobody in the room understood the reactor. Everyone understood a bike shed.

## What Is Bikeshedding?

C. Northcote Parkinson wrote up that meeting in 1957, and the pattern he named has outlived every one of its specific numbers{{< cite 1 "Parkinson, C. Northcote (1957). Parkinson's Law: The Pursuit of Progress." >}}. "The time spent on any item of the agenda will be in inverse proportion to the sum involved{{< cite 1 "Parkinson, C. Northcote (1957). Parkinson's Law: The Pursuit of Progress." >}}." A sum too large to picture gets rubber-stamped. A sum anyone can picture gets picked apart.

Confidence explains the split better than difficulty does. Complexity buys deference. A $10 million reactor comes wrapped in credentials nobody in the room can challenge, so nobody tries to. A bike shed carries no such shield, and since everyone has stood next to one, everyone has an opinion on the roof color, the material, and how much the whole thing should cost.

## How the Term Reached Software

Parkinson's committee stayed a business-school anecdote for four decades. Then in 1999, FreeBSD developer Poul-Henning Kamp watched a mailing list argue for weeks over whether `sleep()` should accept fractional seconds, a change nobody seriously opposed, and wrote an email comparing it to Parkinson's shed{{< cite 2 "Kamp, Poul-Henning (1999). The Bikeshed Email. phk.freebsd.dk." >}}. "A bike shed on the other hand. Anyone can build one of those over a weekend, and still have time to watch the game on TV{{< cite 2 "Kamp, Poul-Henning (1999). The Bikeshed Email. phk.freebsd.dk." >}}." The term stuck to open source the way "yak shaving" and "bit rot" did, shorthand for a pattern every mailing list has lived through.

## Where This Shows Up Today

Code review is the modern bike shed. A pull request restructuring a caching layer gets approved after a skim, because reviewing it properly would take a day nobody has and most reviewers can't hold the whole change in their head at once. That same pull request's variable names draw three rounds of comments, because renaming a variable takes thirty seconds, everybody understands what a name is, and everybody has a preference.

Go's tooling is a rare case of someone fixing this instead of complaining about it. Formatting arguments are the purest bike shed there is, no functional stakes and infinite opinions, so Go's creators built `gofmt` and made it non-negotiable. Rob Pike put it simply. "Gofmt's style is no one's favorite, yet gofmt is everyone's favorite{{< cite 3 "Pike, Rob (2015). Go Proverbs. Gopherfest SV." >}}." Removing the decision beat winning it.

## Common Mistakes

**Mistaking engagement for importance.** A thread with 200 comments feels like it matters more than one with three. Comment count tracks how many people feel qualified to have an opinion. It says nothing about how much is actually at stake.

**Deferring completely on the hard call.** The reactor still needs somebody accountable for it. Silent, unanimous approval is a different thing entirely, an absence of scrutiny that happens to look calm.

**Letting the trivial decision run until consensus.** Agreement on where to put a comma will happen eventually, given enough hours. Whether those hours are worth spending is a different question, and nobody in the thread is incentivized to ask it.

**Letting the loudest voice win the trivial fight.** The person who cares most about tab width is usually just the person still awake and typing at midnight. Volume and expertise run on different axes, and a trivial decision is where that gap goes unnoticed.

**Assuming the fix is telling people to stop.** Kamp's email didn't end bikeshedding on FreeBSD's mailing list, and naming a pattern rarely removes it. It just makes the pattern easier to spot. Removing the decision, the way `gofmt` did, works far better than asking people to care less about paint colors.

## Put It Into Practice

Look at your last three long threads, and sort them by comment count, then by actual stakes. If the ordering doesn't match, you've found your bike shed. Calling it that out loud works as a real check on where the time is going, but it can just as easily become a cheap way to end a debate someone still has a stake in. Use it carefully.

Time-box the trivial ones before they start. Five minutes for a naming decision, with a default owner who decides if the group hasn't converged by then. Spend the time you saved on the reactor instead, the pull request nobody fully reviewed because reviewing it properly looked expensive. On the reactor side, ask the most junior person in the room what looks wrong to them, even if it sounds basic. That's usually where the real review happens.

Where you can, remove the decision instead of moderating the debate about it. A linter enforces a rule, and the argument about that rule stops being worth having. A shed painted before anyone shows up to argue about the color never becomes a bike shed at all.

## Go Further

**The bigger law this one is a footnote to.** Parkinson's more famous claim, that work expands to fill the time available for its completion, appears in the same 1957 book and explains a wider range of organizational bloat than committee meetings alone{{< cite 1 "Parkinson, C. Northcote (1957). Parkinson's Law: The Pursuit of Progress." >}}.

**Kamp's essay in full.** The post here quotes a fragment. Kamp's own writeup covers the specific FreeBSD thread that triggered it and how the term spread through the project afterward{{< cite 2 "Kamp, Poul-Henning (1999). The Bikeshed Email. phk.freebsd.dk." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Parkinson, C. Northcote (1957). <em>Parkinson's Law: The Pursuit of Progress</em>. <a href="https://archive.org/stream/pdfy-n6mqYo6d8uKz06xm/Parkinson-s-Law_djvu.txt">https://archive.org/stream/pdfy-n6mqYo6d8uKz06xm/Parkinson-s-Law_djvu.txt</a></li>
  <li id="ref-2">Kamp, Poul-Henning (1999). "The Bikeshed Email." <em>phk.freebsd.dk</em>. <a href="http://phk.freebsd.dk/sagas/bikeshed/">http://phk.freebsd.dk/sagas/bikeshed/</a></li>
  <li id="ref-3">Pike, Rob (2015). "Go Proverbs." Gopherfest SV. <a href="https://www.youtube.com/watch?v=PAAkCSZUG1c">https://www.youtube.com/watch?v=PAAkCSZUG1c</a></li>
</ol>

---

## Outtakes

**The coefficient of inefficiency.** Parkinson also argued committees stop working once membership passes a threshold between 19.9 and 22.4 people, past which no one can hear or be heard. He called it a coefficient of inefficiency ([Parkinson, 1957](https://archive.org/stream/pdfy-n6mqYo6d8uKz06xm/Parkinson-s-Law_djvu.txt)).

**The bikeshed became a yellow card.** Inside FreeBSD, "bikeshed" turned into shorthand for calling out a derailed thread, a term of art for telling someone to drop it and move on ([Kamp, 1999](http://phk.freebsd.dk/sagas/bikeshed/)).

**Physicists took the joke seriously.** In 2008, physicists Peter Klimek, Rudolf Hanel, and Stefan Thurner built formal opinion-formation models to test Parkinson's 21-person committee threshold, an unusually rigorous callback to a joke from 1957 ([Klimek, Hanel, and Thurner, 2008](https://arxiv.org/abs/0808.1684)).

---

## Changelog

**2026-07-20** Merged in two ideas from a duplicate older draft (`parkinsons-law-of-triviality.md`, now removed): a caution about naming a bikeshed being usable to dismiss legitimate debate, and advice to invite junior engineers to question the complex proposal that usually gets rubber-stamped.  
**2026-07-19** Initial release.
