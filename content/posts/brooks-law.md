---
title: "Brooks' Law"
date: 2026-07-11
publishdate: 2026-07-11
lastmod: 2026-07-11
summary: "Fred Brooks watched IBM add programmers to a late project and watched it get later. The math behind why still surprises engineering managers who reach for headcount as the first fix."
tags: ["planning", "teams", "management"]
image: /images/brooks-law.jpg
draft: true
---

![Fred Brooks watched IBM add programmers to a late project and watched it get later. The math behind why still surprises engineering managers who reach for headcount as the first fix.](/images/brooks-law.jpg)
*An IBM System/360 mainframe, the machine OS/360 was built to run. Photo: [Erik Pitti (2011)](https://commons.wikimedia.org/wiki/File:IBM_System360_Mainframe.jpg). CC BY 2.0.*

## Brooks' Law

In 1964, Fred Brooks was managing OS/360, the operating system for IBM's new System/360 mainframes. The project was late, and IBM's answer was the obvious one. Add more programmers.

The project got later. Between 1963 and 1966, more than 5,000 person-years went into designing, building, and documenting OS/360, a project originally budgeted at roughly $40 million that ended up costing more than $500 million{{< cite 1 "IBM System/360. Wikipedia." >}}. It shipped over a year behind schedule, full of bugs, after IBM kept throwing people at the delay.

Brooks wrote the whole story down in 1975, and it turned into one of software engineering's few genuine laws.

## What Is Brooks' Law?

"Adding manpower to a late software project makes it later{{< cite 2 "Brooks, Frederick P. (1975). The Mythical Man-Month: Essays on Software Engineering. Addison-Wesley." >}}." Brooks called this "an outrageous oversimplification" the moment he wrote it, which is exactly why it stuck. It compresses a real, counterintuitive mechanism into a sentence a manager can't forget mid-crisis.

The instinct it fights is strong. A late project feels like a math problem: not enough people, so add people. Brooks spent *The Mythical Man-Month* explaining why that math is wrong, using his own failure as the case study.

## Why Adding People Backfires

**Ramp-up time.** A new engineer doesn't start contributing on day one. Someone who already understands the system has to explain it, which means the project loses productive time from an existing person before it gains any from the new one. On a project already behind, that's borrowed time it doesn't have.

**Communication overhead.** Every pair of people who need to coordinate is a channel that can carry a misunderstanding. The number of channels grows as n(n-1)/2, so a team of five has ten channels, a team of ten has forty-five. Double the headcount and you roughly quadruple the ways two people can disagree about what the code is supposed to do.

**Task indivisibility.** Brooks' own line: "The bearing of a child takes nine months, no matter how many women are assigned{{< cite 2 "Brooks, Frederick P. (1975). The Mythical Man-Month: Essays on Software Engineering. Addison-Wesley." >}}." Some work splits cleanly across people. Most software work doesn't, because the pieces depend on each other, and depending on each other is exactly what makes ramp-up time and communication overhead expensive in the first place.

## When The Law Played Out Again

HealthCare.gov launched on October 1, 2013, and immediately fell over. Roughly 55 separate contractors had worked on it, and the team building the site didn't receive real specifications to build against until March 2013, months before launch{{< cite 3 "Lessons Learned from the HealthCare.gov Project. The Business of Government." >}}. Nobody had clear ownership of the whole system, so problems got diagnosed by committee and fixed by whichever contractor happened to be in the room.

The administration's first instinct matched IBM's in 1964. Add people. It didn't work any better this time, for the same reasons.

What actually fixed it was different. A small team under a single named lead, Jeffrey Zients, ran daily meetings in a rented room in Maryland with one explicit rule: whoever knew the most about a problem talked, regardless of rank{{< cite 4 "Obama's Trauma Team. TIME." >}}. That's dozens of people added to a late project too, but organized around clear ownership and tight, specific coordination instead of general headcount. The site was stable within two months.

## Common Mistakes

**Treating "late" as certain.** Software engineer Steve McConnell has argued Brooks' Law gets misapplied because most teams don't actually know how late they are, projects overrun by well over 100% on average, so "late" often means "less finished than we thought," not "needs more hands"{{< cite 5 "McConnell, Steve. Brooks' Law Repealed." >}}. Adding people to a project that's mis-measured, not overstaffed, fails for a different reason than Brooks described.

**Adding people without adding ownership.** HealthCare.gov's first failure and its fix used similar headcounts. The difference was whether one person could say who owned what. Headcount without ownership just multiplies the communication channels Brooks warned about.

**Assuming the law never lifts.** Brooks himself never claimed permanent truth, only that most software work is too interdependent for extra people to pay for their own ramp-up time. Early in a project, or on work that's actually separable, the math works differently.

**Reaching for people before reaching for scope.** Cutting the work left is often faster than adding capacity to do all of it. It's also the option most likely to get skipped, because it looks like admitting failure instead of fixing it.

## Put It Into Practice

Before adding anyone to a project that's behind, ask who on the current team will lose the next two weeks to onboarding them. If you can't name that person and that cost specifically, you're guessing at the tradeoff, not making it.

Check whether the work is actually behind or just underestimated. Those call for different fixes. A truly late project with well-understood scope might tolerate careful, narrow additions. A project that's simply bigger than anyone realized needs a scope conversation, not more engineers.

If you do add people, add ownership first. Name who owns which piece before the new hires start, so the communication channels Brooks warned about have a structure to run through instead of growing unchecked.

## Go Further

**The sequel law from the same book.** Brooks also named the Second System Effect, the tendency to over-engineer the project that follows a successful one, a pattern worth its own read once this one lands{{< cite 2 "Brooks, Frederick P. (1975). The Mythical Man-Month: Essays on Software Engineering. Addison-Wesley." >}}.

**The full counterargument.** McConnell's essay is worth reading in full for the cases where careful staffing additions do work, not as a rebuttal to Brooks but as the fine print his one-sentence law leaves out{{< cite 5 "McConnell, Steve. Brooks' Law Repealed." >}}.

---

## References

<ol class="references">
  <li id="ref-1">"IBM System/360." Wikipedia. <a href="https://en.wikipedia.org/wiki/IBM_System/360">https://en.wikipedia.org/wiki/IBM_System/360</a></li>
  <li id="ref-2">Brooks, Frederick P. (1975). <em>The Mythical Man-Month: Essays on Software Engineering</em>. Addison-Wesley. <a href="https://www.pearson.com/en-us/subject-catalog/p/mythical-man-month-the-essays-on-software-engineering-anniversary-edition/P200000000149/9780132119160">https://www.pearson.com/en-us/subject-catalog/p/mythical-man-month-the-essays-on-software-engineering-anniversary-edition/P200000000149/9780132119160</a></li>
  <li id="ref-3">"Lessons Learned from the HealthCare.gov Project." The Business of Government. <a href="https://www.businessofgovernment.org/sites/default/files/Viewpoints%20Dr%20Gwanhoo%20Lee.pdf">https://www.businessofgovernment.org/sites/default/files/Viewpoints%20Dr%20Gwanhoo%20Lee.pdf</a></li>
  <li id="ref-4">"Obama's Trauma Team." <em>TIME</em>. <a href="https://time.com/10228/obamas-trauma-team/">https://time.com/10228/obamas-trauma-team/</a></li>
  <li id="ref-5">McConnell, Steve. "Brooks' Law Repealed." <a href="https://stevemcconnell.com/articles/brooks-law-repealed/">https://stevemcconnell.com/articles/brooks-law-repealed/</a></li>
</ol>

---

## Outtakes

**Brooks later said he'd trade the whole book for one sentence.** In the 20th-anniversary edition, Brooks noted that if he had to reduce the entire book's argument to one law, it would be the one about adding manpower, everything else in the book supports or qualifies it ([Wikipedia, n.d.](https://en.wikipedia.org/wiki/The_Mythical_Man-Month)).

**IBM's System/360 succeeded despite the disaster that built it.** The over-budget, late, bug-ridden OS/360 project still shipped a mainframe family that hit $8.3 billion in annual sales by 1971, proof that a Brooks' Law failure and a business success can be the same project ([Wikipedia, n.d.](https://en.wikipedia.org/wiki/IBM_System/360)).

---

## Changelog

**2026-07-11** Initial release.
