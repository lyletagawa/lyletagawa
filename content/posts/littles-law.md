---
title: "Little's Law"
date: 2026-06-27
publishdate: 2026-06-27
lastmod: 2026-07-26
summary: "Three numbers run every queue: how much work is in flight, how fast it arrives, and how long it waits. Little's Law says you only get to pick two."
tags: ["capacity", "flow", "queueing"]
image: /images/littles-law.jpg
draft: true
---

![Three numbers run every queue: how much work is in flight, how fast it arrives, and how long it waits. Little's Law says you only get to pick two.](/images/littles-law.jpg)
*A real scrum task board. Stories, To Do, In Progress, Testing, and a sprint deadline in the corner. Photo: [Logan Ingalls (2010)](https://commons.wikimedia.org/wiki/File:Scrum_task_board.jpg). CC BY 2.0.*

## Little's Law

Your Kanban board has 30 cards in the In Progress column and a team of five. Standup is a status tour of work that started weeks ago and still hasn't shipped. Everyone is busy. Nothing is done.

The instinct is to push harder, start more, fill every idle minute. That instinct is what made the column 30 cards deep. There's an equation from 1961 that explains why the work feels stuck, and it doesn't care how hard anyone is working.

## What Little Proved

The equation is almost insultingly simple. L = λW{{< cite 1 "Little, John D. C. (1961). A Proof for the Queuing Formula: L = λW. Operations Research 9(3)." >}}. The average number of items in a system, L, equals the average rate at which they arrive, the Greek letter λ, times the average time each one spends inside, W.

John Little wrote the first general proof in a paper barely five pages long{{< cite 1 "Little, John D. C. (1961). A Proof for the Queuing Formula: L = λW. Operations Research 9(3)." >}}. Engineers had leaned on the relationship as a rule of thumb for years, sure it was true and unable to prove it held in every case until he did{{< cite 2 "Little, John D. C., and Stephen C. Graves (2008). Little's Law. In Building Intuition: Insights from Basic Operations Management Models and Principles. Springer." >}}.

What makes it strange is how little it assumes. It doesn't care whether arrivals come steady or in bursts, whether a job takes a millisecond or an hour, or whether you serve the queue first-come or last-come. Keep the system stable, watch it long enough, and the three averages lock into that one equation{{< cite 2 "Little, John D. C., and Stephen C. Graves (2008). Little's Law. In Building Intuition: Insights from Basic Operations Management Models and Principles. Springer." >}}.

## The Same Law Everywhere

Picture a coffee shop. Sixty customers walk in per hour, and each spends five minutes inside. Little's Law says five people are in the shop at any moment, on average. One per minute, times five minutes each, equals five{{< cite 2 "Little, John D. C., and Stephen C. Graves (2008). Little's Law. In Building Intuition: Insights from Basic Operations Management Models and Principles. Springer." >}}.

Swap the units and it's your backend. A service handles 500 requests per second, each taking 40 milliseconds. On average 20 requests are in flight, so a pool of 20 workers keeps pace and anything smaller grows a queue{{< cite 2 "Little, John D. C., and Stephen C. Graves (2008). Little's Law. In Building Intuition: Insights from Basic Operations Management Models and Principles. Springer." >}}. Swap them once more and it's a factory floor or a Kanban board, where work in progress equals throughput times cycle time.

## Why Cutting WIP Works

Rearrange the equation and it turns into a management lever. Cycle time equals work in progress divided by throughput. Hold throughput steady and the only way to move each item through faster is to keep fewer of them open at once{{< cite 2 "Little, John D. C., and Stephen C. Graves (2008). Little's Law. In Building Intuition: Insights from Basic Operations Management Models and Principles. Springer." >}}.

Back to that board. Thirty cards in progress, six finished per week. The average trip from start to done is thirty divided by six, which is five weeks. Cap the column at twelve and, at the same throughput, the wait falls to two weeks. Same team, same weekly output, less than half the lead time{{< cite 2 "Little, John D. C., and Stephen C. Graves (2008). Little's Law. In Building Intuition: Insights from Basic Operations Management Models and Principles. Springer." >}}. A work-in-progress limit is the first thing a Kanban board imposes, and it's what makes the work visible in the first place.

## Where It Trips People Up

**The system has to be stable.** Little's Law describes long-run averages in a system that isn't growing without bound. Point it at a traffic spike while the queue is still building and the numbers mislead you. Wait for steady state, or measure across a full cycle{{< cite 2 "Little, John D. C., and Stephen C. Graves (2008). Little's Law. In Building Intuition: Insights from Basic Operations Management Models and Principles. Springer." >}}.

**Lambda counts what flows through.** The rate in the equation is the work that actually enters and leaves, the throughput. Demand pounding on the door doesn't count until it gets in. If half your requests time out and drop, they were never in L to begin with{{< cite 2 "Little, John D. C., and Stephen C. Graves (2008). Little's Law. In Building Intuition: Insights from Basic Operations Management Models and Principles. Springer." >}}.

**Your units have to match.** A rate per second pairs with a time in seconds, a rate per week with a time in weeks. Mixing minutes and hours is the most reliable way to get a confident, wrong answer.

**The equation doesn't promise anything.** Cutting work in progress doesn't speed anything up on its own. It forces one of the other two numbers to move. Usually throughput holds and cycle time drops, but if trimming the queue also starves throughput, you wait just as long{{< cite 2 "Little, John D. C., and Stephen C. Graves (2008). Little's Law. In Building Intuition: Insights from Basic Operations Management Models and Principles. Springer." >}}.

## Put It Into Practice

Open your tracking tool and count three numbers. How many items are in progress right now, how many you finish in a typical week, and the first divided by the second. That quotient is your real cycle time, and it tends to run longer than anyone on the team would guess.

Then cap the in-progress count and watch what moves. If cycle time drops while throughput holds, the queue was the whole problem. If throughput falls too, you've uncovered a real bottleneck that piling on parallel work was hiding.

Pick one queue this week. A board, a thread pool, a support inbox. Measure its three numbers and find out which two you actually control.

## Go Further

**Operational analysis.** Little's Law anchors a toolkit for measuring live computer systems straight from their own counters, with no probability distributions required, worked out by Denning and Buzen for whole networks of queues{{< cite 3 "Denning, Peter J., and Jeffrey P. Buzen (1978). The Operational Analysis of Queueing Network Models. ACM Computing Surveys 10(3)." >}}.

**The distributional Little's Law.** The averages are only the opening. Under common conditions the entire distribution of the number in the system ties to the distribution of time in the system, a much stronger link than matching averages alone{{< cite 4 "Bertsimas, Dimitris, and Daisuke Nakazato (1995). The Distributional Little's Law and Its Applications. Operations Research 43(2)." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Little, John D. C. (1961). "A Proof for the Queuing Formula: L = λW." <em>Operations Research</em>, 9(3). <a href="https://www.jstor.org/stable/167570">https://www.jstor.org/stable/167570</a></li>
  <li id="ref-2">Little, John D. C., and Stephen C. Graves (2008). "Little's Law." In <em>Building Intuition: Insights from Basic Operations Management Models and Principles</em>. Springer. <a href="http://web.mit.edu/sgraves/www/papers/Little%27s%20Law-Published.pdf">http://web.mit.edu/sgraves/www/papers/Little%27s%20Law-Published.pdf</a></li>
  <li id="ref-3">Denning, Peter J., and Jeffrey P. Buzen (1978). "The Operational Analysis of Queueing Network Models." <em>ACM Computing Surveys</em>, 10(3). <a href="https://courses.cs.washington.edu/courses/cse567/14au/reading/operational-analysis.pdf">https://courses.cs.washington.edu/courses/cse567/14au/reading/operational-analysis.pdf</a></li>
  <li id="ref-4">Bertsimas, Dimitris, and Daisuke Nakazato (1995). "The Distributional Little's Law and Its Applications." <em>Operations Research</em>, 43(2). <a href="https://www.mit.edu/~dbertsim/papers/Queuing%20Theory/The%20distributional%20Little%27s%20law%20and%20its%20applications.pdf">https://www.mit.edu/~dbertsim/papers/Queuing%20Theory/The%20distributional%20Little%27s%20law%20and%20its%20applications.pdf</a></li>
</ol>

---

## Outtakes

**The proof itself is short.** The original 1961 paper runs pages 383 to 387 of Operations Research, five pages total, to nail down a formula still taught in every operations management textbook ([Little, 1961](https://www.jstor.org/stable/167570)).

**He had a day job in marketing.** Little spent his MIT Sloan career as a founder of marketing science, building some of the first computer models to guide marketing decisions. The queueing formula was a detour ([INFORMS, n.d.](https://www.informs.org/Recognizing-Excellence/Community-Prizes/Marketing-Science-Society/John-D.C.-Little-Best-Paper-Award/Who-Is-John-D.-C.-Little)).

**One equation, several jobs.** Little and Graves work through the same L = λW across a supermarket, a wine cellar, and a machine shop scheduling widgets, three systems that share no vocabulary but obey the identical arithmetic ([Little and Graves, 2008](http://web.mit.edu/sgraves/www/papers/Little%27s%20Law-Published.pdf)).

---

## Changelog

**2026-06-27** Initial release.  
