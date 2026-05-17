---
title: "The Overton Window"
date: 2026-05-03
publishdate: 2026-05-16
lastmod: 2026-05-16
summary: "Why good ideas get dismissed before evaluation and how leaders can shift the range of acceptable proposals to make previously unthinkable ideas inevitable."
tags: ["leadership", "management", "cognitive"]
image:
draft: true
---

## The Overton Window

You propose adopting Rust for a critical service. "That's unthinkable. We're a C++ shop." Six months later, after three production incidents caused by memory corruption bugs, someone else proposes Rust. "Interesting idea. Let's do a proof of concept."

Nothing changed about Rust. What changed was the Overton Window. The range of ideas the organization would seriously consider had shifted.

For technical leaders, this is the difference between proposals dismissed as "unthinkable" and the same proposals adopted as "obviously the right direction."

## What Is the Overton Window?

Joseph Overton, a policy analyst at the Mackinac Center, developed this framework in the 1990s to explain how public policy ideas move from unthinkable to popular (Mackinac Center, 2006). The window represents the range of policies acceptable to the mainstream at any given time.

Ideas outside the window are dismissed without serious consideration. Ideas inside the window get debated. The window shifts not through compromise, but through deliberate effort to make previously unacceptable ideas seem reasonable.

In technical organizations, the same dynamic applies. What looks unthinkable today becomes standard practice tomorrow, not through gradual acceptance, but through strategic window-shifting.

**Outside the window (unthinkable)**
- "Let's rewrite everything in Rust"
- "Let's run everything on bare metal"

**Inside the window (debatable)**
- "Let's adopt Go for new services"
- "Let's evaluate moving to Kubernetes"

**Center of the window (current policy)**
- "We use C++ for backend services"
- "We deploy to AWS EC2 instances"

The window isn't about technical merit. Rust might be objectively better than C++ for your use case. If it's outside the window, your proposal gets dismissed before technical evaluation begins.

## Why This Matters

**The proposal graveyard.** Most technical leaders have a graveyard of rejected proposals. Not because the ideas were bad, but because they were outside the window. You proposed microservices when the organization was committed to monoliths. You suggested event-driven architecture when everyone believed in synchronous APIs. The proposals weren't evaluated on merit. They were dismissed as "not how we do things here."

**The innovation paradox.** Organizations claim they want innovation. But innovation means doing things differently. If your proposal is genuinely innovative, it's probably outside the current window. The more innovative your idea, the more likely it gets rejected before anyone evaluates it.

**The timing problem.** Good ideas proposed at the wrong time fail. The same idea proposed when the window has shifted succeeds. This isn't about the idea improving. It's about context changing. Senior engineers often propose the right solution years before the organization is ready to hear it (Fournier, 2017). By the time the organization accepts the idea, the original proposer has left in frustration.

## How the Window Shifts

**Anchor with extremes.** Introduce a more extreme version of your idea first. The moderate idea becomes the reasonable compromise. You want Go. Propose Rust first. When that gets rejected, Go looks moderate by comparison (Larson, 2021).

**Create proof points.** The window shifts when previously unthinkable ideas prove successful elsewhere. Don't argue for adoption. Build a proof of concept. A working prototype shifts the window more than any RFC or architecture document (Moore, 1991).

**Exploit crises.** Crises temporarily expand the window. After a major production incident, proposals that were "too risky" yesterday become "necessary changes" today. Have your proposal ready before the crisis, not after.

**Change the comparison set.** The window is relative to what's considered normal. Your organization thinks Kubernetes is too complex. Show them how other companies in your industry use it. Suddenly it's "industry standard" (Reilly, 2022).

**Build coalitions.** A single voice proposing Rust is "that person with weird ideas." Five teams independently proposing it is "a trend we should investigate." Distributed advocacy shifts norms faster than centralized authority (McChrystal et al., 2015).

## Common Mistakes

**Arguing on merit alone.** Technical merit doesn't shift windows. If your idea is outside the window, no amount of technical argument will get it accepted. Shift the window first, then make the case.

**Proposing too early.** Proposing before the window has shifted wastes credibility. You become "that person who always proposes impractical ideas." Either wait for the window to move, or move it deliberately first.

**Moving too slowly.** Windows shift without you. If you wait too long, your innovative idea becomes obvious and someone else gets credit for implementing it (Fournier, 2017).

## Put It Into Practice

Technical merit matters, but only for ideas inside the window. Ideas outside get dismissed regardless of quality. This is why technically correct proposals fail and mediocre ones succeed. The window determines what gets evaluated, not what's true.

Before your next proposal, map the window. The window has shifted when others reference your idea without attribution, new hires ask why you're not doing it, or competitors announce they are. You're outside it when proposals get dismissed without evaluation, called "interesting," and never revisited. If your idea would be dismissed, shift the window first. Introduce an extreme anchor, build a proof of concept, or change what your organization compares itself to. Have your proposal ready when the next crisis expands the window temporarily.

Pick one proposal currently sitting outside the window. Identify the smallest proof point that would move it inside. Start there.

---

## References

Fournier, Camille (2017). *The Manager's Path: A Guide for Tech Leaders Navigating Growth and Change*. Sebastopol, CA: O'Reilly Media. [https://www.oreilly.com/library/view/the-managers-path/9781491973882/](https://www.oreilly.com/library/view/the-managers-path/9781491973882/)

Larson, Will (2021). *Staff Engineer: Leadership Beyond the Management Track*. Self-published. [https://staffeng.com/book](https://staffeng.com/book)

Mackinac Center for Public Policy (2006). "The Overton Window." [https://www.mackinac.org/OvertonWindow](https://www.mackinac.org/OvertonWindow)

McChrystal, Stanley, et al. (2015). *Team of Teams: New Rules of Engagement for a Complex World*. New York: Portfolio. [https://www.penguinrandomhouse.com/books/317066/team-of-teams-by-general-stanley-mcchrystal-tantum-collins-david-silverman-and-chris-fussell/](https://www.penguinrandomhouse.com/books/317066/team-of-teams-by-general-stanley-mcchrystal-tantum-collins-david-silverman-and-chris-fussell/)

Moore, Geoffrey A. (1991). *Crossing the Chasm: Marketing and Selling High-Tech Products to Mainstream Customers*. New York: HarperBusiness. [https://www.harpercollins.com/products/crossing-the-chasm-3rd-edition-geoffrey-a-moore](https://www.harpercollins.com/products/crossing-the-chasm-3rd-edition-geoffrey-a-moore)

Reilly, Tanya (2022). *The Staff Engineer's Path: A Guide for Individual Contributors Navigating Growth and Change*. Sebastopol, CA: O'Reilly Media. [https://www.oreilly.com/library/view/the-staff-engineers/9781098118723/](https://www.oreilly.com/library/view/the-staff-engineers/9781098118723/)

---

## Outtakes

**Semmelweis.** In 1847, Ignaz Semmelweis proposed that doctors should wash their hands before delivering babies. The idea was outside the window. "A gentleman's hands cannot be unclean." He was institutionalized in 1865 and died shortly after. Pasteur's germ theory shifted the window a decade later. The [Semmelweis reflex](https://en.wikipedia.org/wiki/Semmelweis_reflex) is now a named bias for rejecting ideas that contradict established norms before evaluation.

**The framework's own window.** Joseph Overton died in a plane crash in 1995, before his framework had any public profile. The [Mackinac Center](https://www.mackinac.org/OvertonWindow) published it posthumously. In 2006, RedState founder Josh Trevino spread it through conservative think tank circles as a playbook for moving ideas like school vouchers and homeschooling from fringe to mainstream.

---

## Changelog

**2026-05-16** Added Outtakes section.  
**2026-05-03** Removed "The Individual Contributor Perspective" section. Added inline citations.
