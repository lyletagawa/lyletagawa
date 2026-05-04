---
title: "The Overton Window"
date: 2026-05-03
publishdate: 2026-05-03
lastmod: 2026-05-03
summary: "Why good ideas get dismissed before evaluation and how leaders can shift the range of acceptable proposals to make previously radical ideas inevitable."
tags: ["leadership", "management", "cognitive"]
image:
draft: false
---

## The Overton Window

You propose adopting Rust for a critical service. The response: "That's too radical. We're a Java shop." Six months later, after three production incidents caused by null pointer exceptions, someone else proposes Rust. The response: "Interesting idea. Let's do a proof of concept."

Nothing changed about Rust. What changed was the Overton Window: the range of ideas considered acceptable for serious consideration.

For technical leaders, this is the difference between proposals dismissed as "too extreme" and the same proposals adopted as "obviously the right direction."

## What Is the Overton Window?

Joseph Overton, a policy analyst at the Mackinac Center, developed this framework in the 1990s to explain how public policy ideas move from unthinkable to popular (Mackinac Center, 2006). The window represents the range of policies acceptable to the mainstream at any given time.

Ideas outside the window are dismissed without serious consideration. Ideas inside the window get debated. The window shifts not through compromise, but through deliberate effort to make previously unacceptable ideas seem reasonable.

In technical organizations, the same dynamic applies. What looks radical today becomes standard practice tomorrow, not through gradual acceptance, but through strategic window-shifting.

## The Technical Overton Window

In technical organizations, the window defines what's considered reasonable to propose:

**Outside the window (unthinkable):**
- "Let's rewrite everything in Haskell"
- "We should delete all our tests"
- "Let's move to a blockchain architecture"

**Inside the window (debatable):**
- "Let's adopt TypeScript for new services"
- "We should increase test coverage to 80%"
- "Let's evaluate moving to Kubernetes"

**Center of the window (current policy):**
- "We use Java for backend services"
- "We maintain 60% test coverage"
- "We deploy to AWS EC2 instances"

The window isn't about technical merit. Rust might be objectively better than Java for your use case. If it's outside the window, your proposal gets dismissed before technical evaluation begins.

## Why This Matters

**The proposal graveyard.** Most technical leaders have a graveyard of rejected proposals. Not because the ideas were bad, but because they were outside the window. You proposed microservices when the organization was committed to monoliths. You suggested event-driven architecture when everyone believed in synchronous APIs. The proposals weren't evaluated on merit. They were dismissed as "not how we do things here."

**The innovation paradox.** Organizations claim they want innovation. But innovation means doing things differently. If your proposal is genuinely innovative, it's probably outside the current window. The more innovative your idea, the more likely it gets rejected before anyone evaluates it.

**The timing problem.** Good ideas proposed at the wrong time fail. The same idea proposed when the window has shifted succeeds. This isn't about the idea improving. It's about context changing. Senior engineers often propose the right solution years before the organization is ready to hear it (Fournier, 2017). By the time the organization accepts the idea, the original proposer has left in frustration.

## How the Window Shifts

### 1. Anchor with Extremes

To make a moderate idea acceptable, introduce a more extreme version first. The moderate idea becomes the reasonable compromise.

You want to adopt TypeScript. Propose rewriting everything in Haskell. When that gets rejected as too extreme, TypeScript looks moderate by comparison. Will Larson advises proposing stretch goals alongside realistic ones (Larson, 2021). The stretch goal shifts the window, making the realistic goal seem conservative.

### 2. Create Proof Points

The window shifts when previously unthinkable ideas prove successful elsewhere. Early adopters create proof points that shift the window for the mainstream (Moore, 1991). Don't argue for adoption. Build a small proof of concept. A working prototype using your proposed technology shifts the window more than any RFC or architecture document.

### 3. Exploit Crises

Crises temporarily expand the Overton Window. Ideas previously unthinkable become acceptable when the current approach has clearly failed. After a major production incident, proposals that were "too risky" yesterday become "necessary changes" today. Have your proposal ready before the crisis, not after.

### 4. Change the Comparison Set

The window is relative to what's considered normal. Change the comparison set, and you change the window. Your organization thinks Kubernetes is too complex. Show them how other companies in your industry use it. Suddenly, Kubernetes isn't "too complex." It's "industry standard." Senior ICs shift windows by changing what the organization compares itself to (Reilly, 2022).

### 5. Build Coalitions

The window shifts faster when multiple people advocate for change. A single voice proposing Rust is "that person with weird ideas." Five teams independently proposing Rust is "a trend we should investigate." Distributed advocacy is more powerful than centralized authority for shifting organizational norms (McChrystal et al., 2015).

## Common Mistakes

**Arguing on merit alone.** Technical merit doesn't shift windows. If your idea is outside the window, no amount of technical argument will get it accepted. Shift the window first, then make the case.

**Proposing too early.** Proposing before the window has shifted wastes credibility. You become "that person who always proposes impractical ideas." Either wait for the window to move, or move it deliberately first.

**Moving too slowly.** Windows shift without you. If you wait too long, your innovative idea becomes obvious and someone else gets credit for implementing it (Fournier, 2017).

## Measuring Window Position

**Signs the window has shifted:**
- People reference your idea without attributing it to you
- New hires ask "why aren't we doing X?"
- Competitors announce they're doing X
- Leadership asks "should we be looking at X?"

**Signs you're outside the window:**
- Your proposal gets dismissed without technical evaluation
- People say "that's not how we do things here"
- Your idea is called "interesting" but never discussed again

## Put It Into Practice

Technical merit matters, but only for ideas inside the window. Ideas outside get dismissed regardless of quality. This is why technically correct proposals fail and mediocre ones succeed: the window determines what gets evaluated, not what's true.

Before your next proposal, map the window. Would your idea be debated or dismissed outright? If it would be dismissed, shift the window first. Introduce an extreme anchor, build a proof of concept, or change what your organization compares itself to. Have your proposal ready when the next crisis expands the window temporarily.

Pick one proposal currently sitting outside the window. Identify the smallest proof point that would move it inside. Start there.

## References

Fournier, Camille (2017). *The Manager's Path: A Guide for Tech Leaders Navigating Growth and Change*. Sebastopol, CA: O'Reilly Media. [https://www.oreilly.com/library/view/the-managers-path/9781491973882/](https://www.oreilly.com/library/view/the-managers-path/9781491973882/)

Larson, Will (2021). *Staff Engineer: Leadership Beyond the Management Track*. Self-published. [https://staffeng.com/book](https://staffeng.com/book)

Mackinac Center for Public Policy (2006). "The Overton Window." [https://www.mackinac.org/OvertonWindow](https://www.mackinac.org/OvertonWindow)

McChrystal, Stanley, et al. (2015). *Team of Teams: New Rules of Engagement for a Complex World*. New York: Portfolio. [https://www.penguinrandomhouse.com/books/317066/team-of-teams-by-general-stanley-mcchrystal-tantum-collins-david-silverman-and-chris-fussell/](https://www.penguinrandomhouse.com/books/317066/team-of-teams-by-general-stanley-mcchrystal-tantum-collins-david-silverman-and-chris-fussell/)

Moore, Geoffrey A. (1991). *Crossing the Chasm: Marketing and Selling High-Tech Products to Mainstream Customers*. New York: HarperBusiness. [https://www.harpercollins.com/products/crossing-the-chasm-3rd-edition-geoffrey-a-moore](https://www.harpercollins.com/products/crossing-the-chasm-3rd-edition-geoffrey-a-moore)

Reilly, Tanya (2022). *The Staff Engineer's Path: A Guide for Individual Contributors Navigating Growth and Change*. Sebastopol, CA: O'Reilly Media. [https://www.oreilly.com/library/view/the-staff-engineers/9781098118723/](https://www.oreilly.com/library/view/the-staff-engineers/9781098118723/)

## Changelog

**2026-05-03** Converted to current format; removed "The Individual Contributor Perspective" section; restructured conclusion as "Put It Into Practice"; added inline citations and reference URLs.
