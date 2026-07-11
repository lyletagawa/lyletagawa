---
title: "Maslow's Hammer"
date: 2026-07-04
publishdate: 2026-07-04
lastmod: 2026-07-04
summary: "Deep expertise in one tool makes you reach for it even when a different one solves the problem better. Psychologists named this the law of the instrument."
tags: ["expertise", "innovation", "teams"]
image: /images/maslows-hammer.jpg
draft: true
---

![Deep expertise in one tool makes you reach for it even when a different one solves the problem better. Psychologists named this the law of the instrument.](/images/maslows-hammer.jpg)
*Stanley, Stanford's autonomous Volkswagen Touareg, at the 2005 DARPA Grand Challenge. Photo: [DARPA (2005)](https://commons.wikimedia.org/wiki/File:Stanley2.JPG). Public domain.*

## Maslow's Hammer

In 2005, 196 teams entered the DARPA Grand Challenge to build a vehicle that could drive itself across 132 miles of Mojave desert with no human input. 195 of them were hardware teams. They ripped apart existing vehicles, replaced the components, and rebuilt them from scratch{{< cite 1 "Bessemer Venture Partners. Seven Lessons for Every Robotics Founder from the Godfather of Self-Driving Cars." >}}.

Sebastian Thrun's team at Stanford took a Volkswagen Touareg, named it Stanley, added sensors, and spent almost all their effort on the software. Stanley finished the course in under seven hours and won{{< cite 2 "Thrun, Sebastian, et al. (2006). Stanley: The Robot that Won the DARPA Grand Challenge. Journal of Field Robotics, 23(9)." >}}.

The challenge was solvable with a standard rental car. The hard part was making the car smart, and 195 teams full of hardware experts never saw it, because hardware was the tool they knew{{< cite 1 "Bessemer Venture Partners. Seven Lessons for Every Robotics Founder from the Godfather of Self-Driving Cars." >}}.

## What Is Maslow's Hammer?

Philosopher Abraham Kaplan named this the "law of the instrument" in 1964. Give a small boy a hammer, he wrote, and he'll find that everything he encounters needs pounding{{< cite 3 "Kaplan, Abraham (1964). The Conduct of Inquiry: Methodology for Behavioral Science. Chandler Publishing Company." >}}. Two years later, Abraham Maslow gave the idea its sharper, more famous phrasing: if the only tool you have is a hammer, it's tempting to treat everything as if it were a nail{{< cite 4 "Maslow, Abraham H. (1966). The Psychology of Science: A Reconnaissance. Harper & Row." >}}.

The mechanism is familiarity, not stupidity. The tool you've mastered is the one you reach for first, and mastery makes that reach feel like judgment instead of habit.

## Why Expertise Makes It Worse

You'd expect more expertise to fix this. Thrun found the opposite. "You get blinded by what you're confident in doing," he said, describing exactly what happened to the 195 hardware teams{{< cite 1 "Bessemer Venture Partners. Seven Lessons for Every Robotics Founder from the Godfather of Self-Driving Cars." >}}. Deep skill in one tool doesn't just make that tool available. It makes every other tool harder to see.

The DARPA teams weren't short on talent. Some of the best vehicle engineers in the country entered that race. Their skill was real, and it pointed in exactly one direction, toward a hardware solution to a problem that mostly wasn't a hardware problem. Thrun has put the general version of this more bluntly since. Experts are experts of the past, not the future. If you ask an expert about innovation, they're the least likely to say yes, it can be done{{< cite 5 "Sebastian Thrun, quoted in Design News (2025). Sebastian Thrun Is Driving a Future Where Cars Fly Themselves." >}}.

## Type A And Type B

Thrun later described two kinds of people he meets. Type A is the expert who arrives with a complete plan and wants everyone to sign onto it. "These are the people I run away from as fast as I can," he said{{< cite 6 "Sebastian Thrun, interviewed in Designboom (2013). What Makes an Innovator? An Interview with Sebastian Thrun." >}}.

Type B is rarer. They have a vision, admit they don't know how they'll get there, and aren't afraid to fail trying. Thrun calls Type B the stronger position, not because uncertainty is virtuous, but because a Type A's confidence is usually confidence in the tool, not in the problem's actual shape{{< cite 6 "Sebastian Thrun, interviewed in Designboom (2013). What Makes an Innovator? An Interview with Sebastian Thrun." >}}.

## Where It Shows Up

**The Kubernetes team's answer is always Kubernetes.** A platform team that's spent three years mastering container orchestration proposes a Kubernetes-shaped fix for a scaling problem a caching layer would have solved in an afternoon.

**The database expert's answer is always an index.** A performance problem gets diagnosed as a missing index because indexes are the tool the DBA knows best, even when the query itself is fetching data nobody needs.

**The consultant's answer is always the framework they sell.** A methodology firm brought in for a specific operational problem recommends the same restructuring template they recommend everywhere, because the template is the product, not a response to what they found.

## Common Mistakes

**Hiring for tool depth over problem range.** A team stacked with specialists in one stack solves every problem as if it lives in that stack, because collectively that's the only hammer in the room.

**Mistaking a confident answer for the right one.** A fast, detailed proposal from your most experienced person feels safer than a vague one from someone still figuring out the shape of the problem. Confidence measures familiarity, not fit.

**Rewarding tool mastery instead of problem framing.** Performance reviews that credit "deep Kubernetes expertise" over "diagnosed the actual bottleneck" train people to reach for the hammer, because that's what gets recognized.

## Put It Into Practice

Before your team proposes a solution, make them state the problem without naming any tool. "Requests are timing out under load" survives that test. "We need to migrate to Kubernetes" doesn't, because it's already an answer wearing a problem's clothes.

Put at least one person unfamiliar with the default tool on ambiguous problems. They can't reach for a hammer they don't own, so they're forced to look at the nail.

Watch for the DARPA pattern in your own team. When 195 out of 196 people reach for the same solution, that's not consensus. That's everyone owning the same hammer.

## Go Further

**Functional fixedness has its own classic experiment.** Karl Duncker's candle problem asked people to mount a candle on a wall using only a candle, matches, and a box of tacks. Most never saw that the box itself, not just its contents, was part of the available toolkit{{< cite 7 "Duncker, Karl (1945). On Problem-Solving. Psychological Monographs, 58(5)." >}}.

**Larry Page pushed Thrun past his own expertise.** Thrun has said the entire self-driving car project inside Google might not have happened without Page pressing him to think like a visionary instead of an expert, the same Type A versus Type B tension applied to Thrun himself{{< cite 8 "Mobility21, Carnegie Mellon University. Sebastian Thrun, the Godfather of the Self-Driving Car Industry, Explains How Larry Page Taught Him to Be a Visionary, Not Just an 'Expert.'" >}}.

---

## References

<ol class="references">
  <li id="ref-1">Bessemer Venture Partners. "Seven Lessons for Every Robotics Founder from the Godfather of Self-Driving Cars." <a href="https://www.bvp.com/atlas/seven-lessons-for-every-robotics-founder-from-the-godfather-of-self-driving-cars">https://www.bvp.com/atlas/seven-lessons-for-every-robotics-founder-from-the-godfather-of-self-driving-cars</a></li>
  <li id="ref-2">Thrun, Sebastian, et al. (2006). "Stanley: The Robot that Won the DARPA Grand Challenge." <em>Journal of Field Robotics</em>, 23(9). <a href="http://robots.stanford.edu/papers/thrun.stanley05.pdf">http://robots.stanford.edu/papers/thrun.stanley05.pdf</a></li>
  <li id="ref-3">Kaplan, Abraham (1964). <em>The Conduct of Inquiry: Methodology for Behavioral Science</em>. Chandler Publishing Company. <a href="https://archive.org/details/conductofinquiry00kapl">https://archive.org/details/conductofinquiry00kapl</a></li>
  <li id="ref-4">Maslow, Abraham H. (1966). <em>The Psychology of Science: A Reconnaissance</em>. Harper & Row. <a href="https://archive.org/details/psychologyofscie00masl">https://archive.org/details/psychologyofscie00masl</a></li>
  <li id="ref-5">Sebastian Thrun, quoted in Design News (2025). "Sebastian Thrun Is Driving a Future Where Cars Fly Themselves." <a href="https://www.designnews.com/motion-control/sebastian-thrun-is-driving-a-future-where-cars-fly-themselves">https://www.designnews.com/motion-control/sebastian-thrun-is-driving-a-future-where-cars-fly-themselves</a></li>
  <li id="ref-6">Sebastian Thrun, interviewed in Designboom (2013). "What Makes an Innovator? An Interview with Sebastian Thrun." <a href="https://www.designboom.com/technology/sebastian-thrun-interview-on-innovation-what-makes-an-innovator-12-02-2013/">https://www.designboom.com/technology/sebastian-thrun-interview-on-innovation-what-makes-an-innovator-12-02-2013/</a></li>
  <li id="ref-7">Duncker, Karl (1945). "On Problem-Solving." <em>Psychological Monographs</em>, 58(5): i-113. <a href="https://doi.org/10.1037/h0093599">https://doi.org/10.1037/h0093599</a></li>
  <li id="ref-8">Mobility21, Carnegie Mellon University. "Sebastian Thrun, the Godfather of the Self-Driving Car Industry, Explains How Larry Page Taught Him to Be a Visionary, Not Just an 'Expert.'" <a href="https://mobility21.cmu.edu/sebastian-thrun-the-godfather-of-the-self-driving-car-industry-explains-how-larry-page-taught-him-to-be-a-visionary-not-just-an-expert/">https://mobility21.cmu.edu/sebastian-thrun-the-godfather-of-the-self-driving-car-industry-explains-how-larry-page-taught-him-to-be-a-visionary-not-just-an-expert/</a></li>
</ol>

---

## Outtakes

**The year before, nobody finished at all.** The 2004 DARPA Grand Challenge covered 150 miles. The best team, Carnegie Mellon's Sandstorm, made it 7.32 miles before getting stuck on an embankment. The $1 million prize went unclaimed ([Wikipedia, n.d.](https://en.wikipedia.org/wiki/DARPA_Grand_Challenge_(2004))).

**The quote everyone repeats isn't what Maslow wrote.** His actual 1966 sentence reads "I suppose it is tempting, if the only tool you have is a hammer, to treat everything as if it were a nail." The punchier "everything looks like a nail" version is a later paraphrase ([Quote Investigator, 2014](https://quoteinvestigator.com/2014/05/08/hammer-nail/)).

**The quote gets misattributed to Mark Twain constantly.** Despite no evidence he ever said it, "to a man with a hammer, everything looks like a nail" circulates online as a Twain line more often than as a Maslow or Kaplan one ([Quote Investigator, 2014](https://quoteinvestigator.com/2014/05/08/hammer-nail/)).

**Kodak's own engineer invented the thing that killed it.** Steve Sasson built Kodak's first digital camera in 1975 and demoed it to executives in 1976. Their reaction: "That's cute, but don't tell anyone about it." Film was Kodak's hammer, even after Kodak built the nail ([PetaPixel, 2017](https://petapixel.com/2017/09/21/kodak-said-digital-photography-1975/)).

**Charlie Munger applied it to B.F. Skinner.** In his 1995 Harvard speech "The Psychology of Human Misjudgment," Munger said Skinner developed "man-with-a-hammer syndrome" so badly it damaged his own reputation, over-applying behaviorism to everything because it was the framework that made him famous ([James Clear, n.d.](https://jamesclear.com/great-speeches/psychology-of-human-misjudgment-by-charlie-munger)).

---

## Changelog

**2026-07-04** Initial release.
