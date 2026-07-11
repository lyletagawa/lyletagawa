---
title: "Napkin Maths"
date: 2026-07-04
publishdate: 2026-07-04
lastmod: 2026-07-04
summary: "A back-of-envelope calculation gets you within 10x of the truth in five minutes, using nothing but a handful of memorized numbers. That's often more useful than a benchmark you don't have time to run."
tags: ["performance", "systems"]
image: /images/napkin-maths.jpg
draft: true
---

![A back-of-envelope calculation gets you within 10x of the truth in five minutes, using nothing but a handful of memorized numbers. That's often more useful than a benchmark you don't have time to run.](/images/napkin-maths.jpg)
*A napkin sketch by NASA engineer Frank "Bill" Burcham that became the Propulsion-Controlled Aircraft system. Photo: [NASA (1994)](https://commons.wikimedia.org/wiki/File:A_simple_sketch_on_a_TWA_napkin_by_NASA_Dryden_engineer_Frank_W_%22Bill%22_Burcham_led_to_development_and_validation_of_the_Propulsion-Controlled_Aircraft_concept_(EC94-42805-1).jpg). Public domain.*

## Napkin Maths

Someone in the design review asks whether a single Redis instance can handle 100k requests per second on the box you've picked. Nobody has benchmarked this exact workload. The meeting has ten more minutes on the agenda.

You could say "let's find out" and schedule a follow-up. Or you could do the maths on the spot. A single Redis round trip takes at most 1ms, so one blocking connection tops out around 1k ops/sec. A few hundred concurrent connections close that gap easily, comfortably past the 100k you need. Ten seconds later you have an answer within an order of magnitude, close enough to keep the meeting moving.

That's napkin maths. Not a precise answer. A fast one, cheap enough to throw away and repeat the moment new information shows up.

## What Is Napkin Maths?

Simon Eskildsen, a principal engineer at Shopify, built an entire newsletter and conference talk around the practice of estimating system performance from first principles instead of from a profiler{{< cite 1 "Eskildsen, Simon. Napkin Math." >}}. The method combines two ingredients. A small set of memorized reference numbers, things like how long a memory access or a network round trip takes. And a willingness to make reasonable assumptions and multiply them together instead of waiting for perfect data.

The name isn't new. Business writers were already using "back of napkin" for a rough, informal estimate as far back as 1972, decades before anyone applied it to systems performance{{< cite 2 "Back of a Napkin: Meaning and Origin. Word Histories." >}}. What it points to is a standard of precision, not a standard of proof. You're not proving anything. You're getting inside an order of magnitude of the truth, fast enough to use the answer before the question stops mattering.

## Fermi Problems

Fermi problems are named after physicist Enrico Fermi. He posed his students a specific puzzle, like how many piano tuners work in Chicago. Nobody researched an exact answer. You chain assumptions instead, how many people live in Chicago, how many own a piano, how often it needs tuning, how many tunings one tuner finishes in a year. Multiply it out and the estimate comes close to the real number{{< cite 3 "Fermi Problem. Wikipedia." >}}.

Fermi did the same thing with much higher stakes at the Trinity nuclear test in 1945. He dropped scraps of paper as the blast wave passed and measured how far they scattered. From that alone he estimated a yield of about 10 kilotons of TNT. The actual figure was closer to 21 kilotons. Using nothing but paper and a stopwatch, he landed within a factor of two, while physicists nearby measured the same blast with real instruments{{< cite 3 "Fermi Problem. Wikipedia." >}}.

That's the bar napkin maths clears. Not exact. Right enough that the actual number doesn't change what you'd decide to do.

## The Numbers Worth Memorizing

Software napkin maths runs on its own reference table, and the canonical version comes from a talk Jeff Dean gave at Google, later archived from a 2009 industry keynote{{< cite 4 "Dean, Jeff (2009). Numbers Everyone Should Know. LADIS Keynote." >}}. Peter Norvig had published a similar list years earlier, and the two have circulated together long enough that most copies online no longer separate which number came from which{{< cite 5 "Norvig, Peter. 21-Day Refactoring Challenge, Answers Section." >}}. A few anchor points from it, rounded for memory rather than precision.

An L1 cache reference costs about half a nanosecond. A main memory reference costs about 100 nanoseconds, 200 times slower. A round trip within the same datacenter costs about 500 microseconds. Reading a megabyte off a spinning disk costs about 20 milliseconds. A packet's round trip from California to the Netherlands and back costs about 150 milliseconds.

The specific numbers matter less than the gaps between them. Memory to disk is a jump of roughly five orders of magnitude. Once that spacing is memorized, you can place almost any new operation somewhere on the same ladder and get a usable estimate, the same way Fermi's students could place an unfamiliar quantity between two familiar ones.

## Common Mistakes

**Treating the answer as precise.** Napkin maths earns its speed by giving up precision. An estimate that comes out to 96,000 requests per second isn't meaningfully different from one that comes out to 110,000.

**Skipping the sanity check.** Every napkin maths estimate should get compared against one real number you already trust, a known benchmark, a production metric, anything concrete. If the estimate and the real number disagree by more than the order of magnitude you were expecting, an assumption is wrong.

**Using numbers that quietly went stale.** Jeff Dean's original table reflects hardware from around 2009. NVMe SSDs and faster networks have moved several of these numbers by an order of magnitude since. Memorize the ladder, not the specific values, and refresh the values against current metrics when necessary.

## Put It Into Practice

Memorize ten numbers. Cache reference, memory reference, an SSD random read, a datacenter round trip, and a cross-continent round trip cover most of the estimates you'll ever need in a systems discussion.

Then practice on real questions your team already argues about. How many database connections can this service actually use concurrently. Whether a queue will drain before the next spike hits. Whether that new cache layer is worth building before anyone's measured the hit rate it would need.

Next time a question like that comes up in a meeting, don't wait for the benchmark. Do the maths out loud, say where you rounded, and let the room correct you if a number's wrong. That's faster than the benchmark, and usually just as useful.

## Go Further

**Advanced napkin maths.** Eskildsen's SREcon talk goes past single operations into combining several estimates into a full system model, the technique behind questions like sizing a cache or a connection pool{{< cite 6 "Eskildsen, Simon (2019). Advanced Napkin Math: Estimating System Performance from First Principles. SREcon19 EMEA." >}}.

**Hearing the reasoning out loud.** The Changelog's interview with Eskildsen walks through several estimates live, which shows the back-and-forth of the method better than any table of numbers can{{< cite 7 "Eskildsen, Simon (2020). Estimating Systems with Napkin Math. The Changelog, Episode 412." >}}.

**Real latency, not estimated latency.** For actual, continuously updated numbers rather than a 2009 estimate, Cloudflare Radar publishes real-world latency and jitter percentiles measured across its own global network{{< cite 8 "Cloudflare Radar. Internet Quality." >}}.

**A living version of the same table.** Brendan Gregg's Systems Performance keeps its own updated take on these numbers, Table 2.2, for current hardware, along with the broader methodology for reasoning about latency instead of just memorizing it{{< cite 9 "Gregg, Brendan (2020). Systems Performance, 2nd Edition, Section 2.3.2. Pearson." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Eskildsen, Simon. "Napkin Math." <a href="https://sirupsen.com/napkin/">https://sirupsen.com/napkin/</a></li>
  <li id="ref-2">"Back of a Napkin: Meaning and Origin." Word Histories. <a href="https://wordhistories.net/2024/11/02/back-of-napkin/">https://wordhistories.net/2024/11/02/back-of-napkin/</a></li>
  <li id="ref-3">"Fermi Problem." Wikipedia. <a href="https://en.wikipedia.org/wiki/Fermi_problem">https://en.wikipedia.org/wiki/Fermi_problem</a></li>
  <li id="ref-4">Dean, Jeff (2009). "Numbers Everyone Should Know." LADIS Keynote. <a href="http://www.cs.cornell.edu/projects/ladis2009/talks/dean-keynote-ladis2009.pdf">http://www.cs.cornell.edu/projects/ladis2009/talks/dean-keynote-ladis2009.pdf</a></li>
  <li id="ref-5">Norvig, Peter. "21-Day Refactoring Challenge, Answers Section." <a href="http://norvig.com/21-days.html#answers">http://norvig.com/21-days.html#answers</a></li>
  <li id="ref-6">Eskildsen, Simon (2019). "Advanced Napkin Math: Estimating System Performance from First Principles." SREcon19 EMEA. <a href="https://www.youtube.com/watch?v=IxkSlnrRFqc">https://www.youtube.com/watch?v=IxkSlnrRFqc</a></li>
  <li id="ref-7">Eskildsen, Simon (2020). "Estimating Systems with Napkin Math." <em>The Changelog</em>, Episode 412. <a href="https://changelog.com/podcast/412">https://changelog.com/podcast/412</a></li>
  <li id="ref-8">Cloudflare Radar. "Internet Quality." <a href="https://radar.cloudflare.com/quality">https://radar.cloudflare.com/quality</a></li>
  <li id="ref-9">Gregg, Brendan (2020). <em>Systems Performance, 2nd Edition</em>, Section 2.3.2, Table 2.2. Pearson. <a href="https://learning.oreilly.com/library/view/systems-performance-2nd/9780136821694/ch02.xhtml#ch02tab02">https://learning.oreilly.com/library/view/systems-performance-2nd/9780136821694/ch02.xhtml#ch02tab02</a></li>
</ol>

---

## Outtakes

**What the Laffer Curve says.** Arthur Laffer sketched the idea on a napkin for Cheney and Rumsfeld in 1974, showing tax revenue at zero for both a 0% and a 100% rate, so some rate between the two raises the most money ([Boissoneault, 2017](https://www.smithsonianmag.com/smithsonian-institution/restaurant-doodle-launched-political-movement-180963466/)).

**The classic Fermi brainteaser didn't make the cut.** Laszlo Bock, Google's head of People Operations, later revealed that brainteaser puzzles like "how many golf balls fit an airplane" had essentially zero correlation with job performance([Bock, 2015](https://www.hachettebookgroup.com/titles/laszlo-bock/work-rules/9781455554799/)).

**Google turned the table into a printable poster.** Google's own SRE team now publishes these same latency figures as an official one-page rule-of-thumb reference card, sized for a standard printer, that any engineer can pin above a desk ([Google SRE, n.d.](https://sre.google/static/pdf/rule-of-thumb-latency-numbers-letter.pdf)).

**Grace Hopper carried nanoseconds in her pocket.** Starting in the 1960s, Hopper handed out 11.8-inch wires, the distance light travels in a billionth of a second, so admirals impatient with satellite delay could feel why the wait wasn't anyone's fault, just physics ([Kottke, 2019](https://kottke.org/19/05/grace-hopper-explains-a-nanosecond)).

**A napkin sketch landed a plane on engines alone.** NASA engineer Frank "Bill" Burcham sketched an idea on a TWA napkin during a 1994 flight. It became the Propulsion-Controlled Aircraft system, and in 1995 an MD-11 test flight landed using only engine thrust, with hydraulics deliberately disabled ([NASA, 1994](https://www.nasa.gov/centers/dryden/multimedia/imagegallery/PCA/EC94-42805-1.html)).

---

## Changelog

**2026-07-04** Initial release.  
