---
title: "Theory of Constraints"
date: 2026-07-20
publishdate: 2026-07-20
lastmod: 2026-07-20
summary: "A Boy Scout troop hikes single file, and the whole line moves only as fast as the slowest kid. Every system has one constraint like that, and optimizing anything else is wasted effort."
tags: ["process", "throughput", "teams"]
image: /images/theory-of-constraints.jpg
draft: true
---

![A Boy Scout troop hikes single file, and the whole line moves only as fast as the slowest kid. Every system has one constraint like that, and optimizing anything else is wasted effort.](/images/theory-of-constraints.jpg)
*Backpackers on the North Inlet Trail, Rocky Mountain National Park. Photo: [Brian and Jaclyn Drum (2010)](https://commons.wikimedia.org/wiki/File:Hikers_on_the_North_Inlet_Trail.jpg). CC BY 2.0.*

## Theory of Constraints

A plant manager named Alex Rogo takes his son's Boy Scout troop on a ten-mile hike. The boys walk single file, and the line keeps stretching and bunching, stretching and bunching, because every kid walks at a different pace. Near the back is Herbie, a heavy kid nobody wants to hike behind. The gaps behind him keep growing while the front of the line gets further and further ahead{{< cite 1 "Goldratt, Eliyahu M. and Jeff Cox (1984). The Goal: A Process of Ongoing Improvement. North River Press." >}}.

Alex moves Herbie to the front and takes some weight out of his pack. The line stops stretching. Everyone arrives together, faster than before, without anyone but Herbie changing a thing.

That troop is a factory. That hike is the whole theory, in miniature.

## What Is the Theory of Constraints?

Eliyahu Goldratt introduced the Theory of Constraints (TOC) in "The Goal," a business novel about a plant manager saving his factory{{< cite 1 "Goldratt, Eliyahu M. and Jeff Cox (1984). The Goal: A Process of Ongoing Improvement. North River Press." >}}. The premise is blunt. Every system has one constraint, a single resource, step, or policy that limits how much the entire system can produce. "Every system has a limiting factor or constraint. Focusing improvement efforts to better utilize this constraint is normally the fastest and most effective way to improve profitability{{< cite 2 "Theory of Constraints Institute. Theory of Constraints of Eliyahu M. Goldratt." >}}."

Improving anything that isn't the constraint feels productive and changes nothing. Speed up a step that already waits on Herbie, and it just waits longer. The system's total output is set by its slowest link. Its average link never enters into it.

## The Five Focusing Steps

TOC gives the constraint a five-step process instead of a pep talk. Identify the constraint. Exploit it, meaning get everything out of it you already can, for free. Subordinate every other step to its pace. Elevate it, meaning spend money to increase its capacity. Then repeat, because "preventing inertia from becoming the constraint" is officially step five{{< cite 3 "Theory of Constraints Institute. Five Focusing Steps, a Process of On-Going Improvement." >}}.

Most teams skip straight from Identify to Elevate, hiring another engineer, buying another server, adding another reviewer. Exploit comes first for a reason. Herbie didn't need a faster pair of legs. He needed a lighter pack and a different spot in line, and that cost nothing.

## How the Idea Reached Software

Gene Kim's "The Phoenix Project" is a direct homage to "The Goal," down to the plot structure. Kim has said the team "attempted to mirror most of the book structure and plot elements, while making it contemporary, relevant, and hopefully more dramatic{{< cite 4 "IT Revolution. Where To Learn More About Concepts In The Phoenix Project." >}}." Instead of a factory, an IT department. Instead of Herbie, an overloaded engineer named Brent, who every deployment and outage routes through. The constraint moves as the story goes, from Brent to the deployment process to an outsourced vendor, mirroring how the constraint in "The Goal" moves from a bottlenecked robot called the NCX-10 to the heat-treat ovens to the market itself{{< cite 4 "IT Revolution. Where To Learn More About Concepts In The Phoenix Project." >}}.

The Kanban Method carries the same lineage further back. David Anderson credits Goldratt directly. "Indirect influence by Eli Goldratt whose writings on the Theory of Constraints and its Five Focusing Steps greatly influenced steps four and five{{< cite 5 "David J. Anderson School of Management. Kanban: A Recipe for Success." >}}" of his own method. A visual board that surfaces where work is piling up is Herbie made visible.

## Common Mistakes

**Optimizing a step that isn't the constraint.** A faster non-bottleneck step produces more work for the bottleneck to pile up in front of. Total throughput doesn't move.

**Tracking the wrong metrics.** Local efficiency numbers, like how busy each step looks, say nothing about whether the constraint's output actually went up. A team can hit every step's utilization target and still ship the same amount of work it shipped last quarter.

**Blaming the person instead of the system.** Brent gets treated as the problem, when the problem is a process that keeps routing every unplanned request through one person. Fixing Brent's calendar fixes nothing that the next Brent won't hit too.

**Skipping step five.** A constraint that gets elevated moves somewhere else, but the policies built around the old constraint often stay in place. A batch size, an approval chain, a review requirement designed around a bottleneck that no longer exists becomes the new, invisible one.

## Put It Into Practice

Find your Herbie. Somewhere in your pipeline is one person, process, or policy that everything else queues behind, and it's usually not who gets blamed for the delay. Trace a piece of work from request to done and watch for where it waits the longest.

Exploit before you elevate. Before you hire, buy, or approve a bigger budget, ask what the constraint could produce today if nothing else changed, fewer interruptions, a lighter load, a different spot in the queue. Herbie's fix was free.

Then subordinate everything else to that pace on purpose. A team that's fast everywhere except at the one place work actually waits is just busy in the wrong spots.

## Go Further

**The scheduling mechanism behind Exploit.** Drum-Buffer-Rope schedules an entire system around the constraint's pace instead of every station's local schedule, the mechanical version of moving Herbie to the front{{< cite 2 "Theory of Constraints Institute. Theory of Constraints of Eliyahu M. Goldratt." >}}.

**The accounting behind the theory.** Goldratt built an alternative to standard cost accounting around throughput instead of local efficiency, a deeper rabbit hole than one post can cover{{< cite 2 "Theory of Constraints Institute. Theory of Constraints of Eliyahu M. Goldratt." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Goldratt, Eliyahu M. and Jeff Cox (1984). <em>The Goal: A Process of Ongoing Improvement</em>. North River Press. <a href="https://northriverpress.com/the-goal-30th-anniversary-edition/">https://northriverpress.com/the-goal-30th-anniversary-edition/</a></li>
  <li id="ref-2">"Theory of Constraints of Eliyahu M. Goldratt." <em>Theory of Constraints Institute</em>. <a href="https://www.tocinstitute.org/theory-of-constraints.html">https://www.tocinstitute.org/theory-of-constraints.html</a></li>
  <li id="ref-3">"Five Focusing Steps, a Process of On-Going Improvement." <em>Theory of Constraints Institute</em>. <a href="https://www.tocinstitute.org/five-focusing-steps.html">https://www.tocinstitute.org/five-focusing-steps.html</a></li>
  <li id="ref-4">"Where To Learn More About Concepts In 'The Phoenix Project' (Part 1)." <em>IT Revolution</em>. <a href="https://itrevolution.com/articles/learn-more-about-concepts-in-phoenix-project/">https://itrevolution.com/articles/learn-more-about-concepts-in-phoenix-project/</a></li>
  <li id="ref-5">"Kanban: A Recipe for Success." <em>David J. Anderson School of Management</em>. <a href="https://djaa.com/kanban-recipe-success/">https://djaa.com/kanban-recipe-success/</a></li>
</ol>

---

## Outtakes

**Goldratt chose a novel over a textbook on purpose.** He believed a story would spread his ideas faster than a manual could. He was right. "The Goal" has sold more than 6 million copies ([North River Press](https://northriverpress.com/the-goal-30th-anniversary-edition/)).

**"The Phoenix Project" hid its answer on purpose.** Goldratt once noted that no hint of the solution to the plant's problems appears until deep into "The Goal." Gene Kim built "The Phoenix Project" the same way, deliberately delaying the fix ([IT Revolution](https://itrevolution.com/articles/learn-more-about-concepts-in-phoenix-project/)).

---

## Changelog

**2026-07-20** Initial release.
