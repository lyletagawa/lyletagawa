---
title: "The Elevator Algorithm"
date: 2026-08-01
publishdate: 2026-08-01
lastmod: 2026-08-08
summary: "Every disk scheduler in the elevator algorithm family exists to minimize how far a mechanical arm has to travel. Flash storage removed that problem, and the simplest scheduler often wins."
tags: ["algorithms", "performance", "systems"]
image: /images/elevator-algorithms.jpg
draft: true
---

![Every disk scheduler in the elevator algorithm family exists to minimize how far a mechanical arm has to travel. Flash storage removed that problem, and the simplest scheduler often wins.](/images/elevator-algorithms.jpg)
*A plain two-button elevator panel, up and down. Photo: [Takami Chie (2014)](https://commons.wikimedia.org/wiki/File:%E3%81%A8%E3%81%98%E3%82%8B_%E3%81%B2%E3%82%89%E3%81%8F_%E3%81%82%E3%81%8C%E3%82%8B_%E3%81%95%E3%81%8C%E3%82%8B_(13036104054).jpg). CC BY 2.0.*

## The Elevator Algorithm

You're standing in a building lobby with a destination kiosk instead of buttons. You type your floor, and it assigns you to Car C, fifteen feet away with its doors closing, while Car A sits empty right in front of you.

The same tension shows up somewhere that has nothing to do with elevators. Inside every spinning hard drive, a mechanical arm faces an almost identical choice, deciding which request to serve next, since serving them in arrival order would waste enormous amounts of time.

## What Is Disk Scheduling?

A hard disk drive is a stack of spinning platters with a read and write head mounted on an arm. Retrieving data means physically moving that arm to the right track, then waiting for the platter to spin the right sector underneath it. That physical move, the seek, is slow. On a busy system, dozens of read and write requests can be waiting at once, scattered across the disk, and the order you service them in changes how long the arm spends traveling instead of working{{< cite 1 "Denning, Peter J. (1967). Effects of Scheduling on File Memory Operations. AFIPS Spring Joint Computer Conference, Vol. 30." >}}.

The naive policy is first-come, first-served (FCFS). Simple, fair in a narrow sense, and rough on the arm, which ends up crisscrossing the disk in whatever order requests happened to arrive. Shortest-seek-time-first (SSTF) fixes the crisscrossing by always jumping to the nearest pending request, but it introduces a starvation problem. A steady stream of requests near the current position can keep the arm busy indefinitely while a single request at the far edge of the disk waits{{< cite 1 "Denning, Peter J. (1967). Effects of Scheduling on File Memory Operations. AFIPS Spring Joint Computer Conference, Vol. 30." >}}.

The fix is to keep moving in one direction, servicing everything along the way, and only reverse once nothing is left ahead. Peter Denning formalized and compared these policies in 1967, years before anyone in computing called this an elevator{{< cite 1 "Denning, Peter J. (1967). Effects of Scheduling on File Memory Operations. AFIPS Spring Joint Computer Conference, Vol. 30." >}}.

## Where The Name Comes From

The name arrived a year later, almost by accident. In 1968, Donald Knuth used a real elevator, the one in Caltech's Mathematics building, as a teaching example in *The Art of Computer Programming*. He wanted to demonstrate coroutines and doubly linked lists, and an elevator's request queue was a clean way to do it. He wrote up "the simplest set of rules that explain all the phenomena observed during several hours of experimentation"{{< cite 2 "Knuth, Donald E. (1968). The Art of Computer Programming, Volume 1: Fundamental Algorithms. Section 2.2.5." >}}.

The family that grew out of Knuth's aside has a full roster of names by now. FCFS and SSTF came first, the naive baseline and its greedy, starvation-prone fix. SCAN, LOOK, C-SCAN, and C-LOOK are the elevator-style sweep policies, each trimming a different inefficiency out of the basic idea. Production Linux systems mostly run a newer, separate set of names for the same job, the old single-queue CFQ and Deadline schedulers, and their multi-queue successors mq-deadline, BFQ, and Kyber, alongside a literal do-nothing option called none{{< cite 3 "The Linux Kernel Documentation (2017). The kernel's command-line parameters, v4.14." >}}. The 1970s names are still what gets taught in class. The newer names are what actually ship in production.

The comparison outlived the textbook footnote by decades. The Linux kernel documents a boot parameter literally named elevator, used to pick the default scheduler for a disk{{< cite 3 "The Linux Kernel Documentation (2017). The kernel's command-line parameters, v4.14." >}}.

## SCAN Has Cousins

Plain SCAN sweeps to one end, reverses, and sweeps back, but has an unevenness problem of its own. A request that just missed the arm has to wait for a full round trip, while one right behind it gets served almost immediately. The named variants each fix a different piece of that.

LOOK reverses as soon as the last pending request in a direction is served, instead of always traveling to the disk's physical end. C-SCAN sweeps in one direction only, jumping back to the start without servicing anything on the return trip, spreading wait times more evenly than plain SCAN. C-LOOK does the same jump but stops short of the physical edge instead of running all the way there. N-Step-SCAN batches whatever requests exist at a pass's start and defers mid-sweep arrivals to the next one, closing off the starvation edge case{{< cite 4 "Coffman, E. G., L. A. Klimko, and B. Ryan (1972). Analysis of Scanning Policies for Reducing Disk Seek Times. SIAM Journal on Computing, 1(3)." >}}.

Every variant tunes the same lever, how strictly to enforce sweeping in one direction before reversing. Coffman, Klimko, and Ryan gave the family its first rigorous treatment in 1972, deriving expected response times instead of relying on Knuth's informal experiments{{< cite 4 "Coffman, E. G., L. A. Klimko, and B. Ryan (1972). Analysis of Scanning Policies for Reducing Disk Seek Times. SIAM Journal on Computing, 1(3)." >}}.

## Real Elevators Moved On

While computing refined the same sweep-and-reverse idea for half a century, actual elevators mostly abandoned it. Otis had already been running SCAN-like "Collective Control" since the 1920s, well before Knuth named it{{< cite 5 "Elevator World. The History of Operatorless Elevators: Traffic Control Systems, Part One." >}}. By 1982, Otis replaced it with something far more complicated. A proprietary scoring system called RSR, Relative System Response, patented by engineer Joseph Bittar, scores every car against every new call, weighing arrival time, car load, whether sending two cars the same direction wastes capacity, and whether a car sits idle nearby{{< cite 6 "Bittar, Joseph (1982). Relative System Response Elevator Call Assignments. US Patent 4,363,381." >}}.

Modern destination dispatch goes further, asking riders for their floor before boarding so the system can group nearby destinations onto the same car{{< cite 7 "John (n.d.). Elevator Algorithms. john.fun/elevators." >}}. Fewer stops carries a secondary benefit too, less wear on motors, brakes, and doors{{< cite 8 "Kroll, Karen (2015). Elevators and Destination Dispatch Technology. FacilitiesNet." >}}.

More information should mean better decisions, and sometimes it does. But interactive simulations comparing these strategies under different loads found that as flow rate climbs, plain LOOK starts outperforming RSR, despite having far more information available{{< cite 7 "John (n.d.). Elevator Algorithms. john.fun/elevators." >}}.

Elevator researcher Richard Peters found the same pattern studying real building traffic. With full knowledge of every passenger's destination, "it is possible for an intelligent two-button dispatching algorithm to match, or even marginally improve upon destination dispatch{{< cite 9 "Peters, Richard D. (2006). Understanding the Benefits and Limitations of Destination Dispatch. Elevator Technology 16, Proceedings of ELEVCON 2006." >}}."

## Then The Disks Changed

Every scheduling policy in this story, elevator-named or otherwise, exists to solve one problem, minimizing how far a mechanical arm has to travel, because physically moving it is slow. Solid-state drives don't have an arm. Any address on a flash chip costs about the same to reach as any other, so reordering requests to minimize arm travel is pure overhead.

Red Hat's own guidance for modern systems recommends the none scheduler, which passes requests straight through unmodified, for NVMe storage, while spinning disks still default to a deadline-based descendant of the same elevator family{{< cite 10 "Red Hat (2024). I/O Scheduler Recommendations for RHEL with Virtualization." >}}.

The physical cost just moved to a layer invisible to the OS scheduler. A flash cell tolerates on the order of 10,000 program and erase cycles before it wears out, so SSD controllers run their own wear leveling and garbage collection, spreading writes evenly across cells instead of wearing out the same ones{{< cite 11 "Handy, Jim (2013). How Controllers Maximize SSD Life. SNIA SSSI Tech Notes." >}}.

## Where People Get This Wrong

**They assume reordering always helps.** Every scheduling policy in this family trades a small amount of latency for one request against less total arm travel. That trade only pays off when arm travel is actually expensive, the situation on a spinning disk. Flash storage is cheap to reach anywhere, so the same reordering that helps a spinning disk can just add queueing delay on an SSD{{< cite 10 "Red Hat (2024). I/O Scheduler Recommendations for RHEL with Virtualization." >}}.

**They assume more information means a better decision.** RSR and destination dispatch both have more visibility into the full request queue than plain SCAN or a two-button panel ever did. Peters's own research shows that visibility doesn't automatically convert into a better outcome, especially once the overhead of collecting and acting on it is counted{{< cite 9 "Peters, Richard D. (2006). Understanding the Benefits and Limitations of Destination Dispatch. Elevator Technology 16, Proceedings of ELEVCON 2006." >}}.

**They tune for the wrong hardware.** A scheduler benchmarked and tuned on rotational disks carries assumptions about seek cost that only apply to rotational media. Copying last decade's disk-scheduling defaults onto this decade's hardware is a quiet, common source of wasted performance.

## Put It Into Practice

Before choosing or tuning a scheduler, know what physical cost you're actually paying for. If seek time actually dominates your storage medium, an elevator-style policy earns its complexity. On flash storage, that complexity is usually just overhead.

Measure under your real access pattern before trusting a default that was tuned for different hardware than yours.

Next time you're tuning a scheduler, whether it's moving disk requests or moving elevator cars, ask what physical or logistical cost the added complexity is actually buying you. If nobody can answer that question, you're probably paying for overhead dressed up as sophistication.

## Dig Deeper

**Starvation shows up everywhere.** SSTF's tendency to strand far-away disk requests indefinitely is the same failure mode CPU schedulers guard against with aging, gradually raising a waiting process's priority the longer it sits idle, a fix N-Step-SCAN independently reinvented for disks{{< cite 4 "Coffman, E. G., L. A. Klimko, and B. Ryan (1972). Analysis of Scanning Policies for Reducing Disk Seek Times. SIAM Journal on Computing, 1(3)." >}}.

**Destination dispatch's real advantages.** Peters's fuller paper is worth reading for the balanced version rather than the highlight reel. It documents where destination dispatch reliably wins, especially in under-elevated buildings during up-peak traffic{{< cite 9 "Peters, Richard D. (2006). Understanding the Benefits and Limitations of Destination Dispatch. Elevator Technology 16, Proceedings of ELEVCON 2006." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Denning, Peter J. (1967). "Effects of Scheduling on File Memory Operations." <em>AFIPS Spring Joint Computer Conference</em>, Vol. 30. <a href="https://dl.acm.org/doi/10.1145/1465482.1465485">https://dl.acm.org/doi/10.1145/1465482.1465485</a></li>
  <li id="ref-2">Knuth, Donald E. (1968). <em>The Art of Computer Programming, Volume 1: Fundamental Algorithms</em>. Section 2.2.5. Addison-Wesley. <a href="https://archive.org/details/artofcomputerpro0001knut_l0h13rdedition">https://archive.org/details/artofcomputerpro0001knut_l0h13rdedition</a></li>
  <li id="ref-3">The Linux Kernel Documentation (2017). "The kernel's command-line parameters, v4.14." <a href="https://www.kernel.org/doc/html/v4.14/admin-guide/kernel-parameters.html">https://www.kernel.org/doc/html/v4.14/admin-guide/kernel-parameters.html</a></li>
  <li id="ref-4">Coffman, E. G., L. A. Klimko, and B. Ryan (1972). "Analysis of Scanning Policies for Reducing Disk Seek Times." <em>SIAM Journal on Computing</em>, 1(3). <a href="https://epubs.siam.org/doi/10.1137/0201018">https://epubs.siam.org/doi/10.1137/0201018</a></li>
  <li id="ref-5">Elevator World. "The History of Operatorless Elevators: Traffic Control Systems, Part One." <a href="https://elevatorworld.com/article/the-history-of-operatorless-elevators-traffic-control-systems-part-one/">https://elevatorworld.com/article/the-history-of-operatorless-elevators-traffic-control-systems-part-one/</a></li>
  <li id="ref-6">Bittar, Joseph (1982). "Relative System Response Elevator Call Assignments." <em>US Patent 4,363,381</em>. Otis Elevator Co. <a href="https://patents.google.com/patent/US4363381A/en">https://patents.google.com/patent/US4363381A/en</a></li>
  <li id="ref-7">John (n.d.). "Elevator Algorithms." <a href="https://john.fun/elevators">https://john.fun/elevators</a></li>
  <li id="ref-8">Kroll, Karen (2015). "Elevators and Destination Dispatch Technology." <em>FacilitiesNet</em>. <a href="https://www.facilitiesnet.com/elevators/tip/Elevators-and-Destination-Dispatch-Technology--34492">https://www.facilitiesnet.com/elevators/tip/Elevators-and-Destination-Dispatch-Technology--34492</a></li>
  <li id="ref-9">Peters, Richard D. (2006). "Understanding the Benefits and Limitations of Destination Dispatch." <em>Elevator Technology 16</em>, Proceedings of ELEVCON 2006. <a href="https://download.peters-research.com/library/Understanding_the_Benefits_and_Limitations_of_Destination_Dispatch.pdf">https://download.peters-research.com/library/Understanding_the_Benefits_and_Limitations_of_Destination_Dispatch.pdf</a></li>
  <li id="ref-10">Red Hat (2024). "I/O Scheduler Recommendations for RHEL with Virtualization." <a href="https://access.redhat.com/solutions/5427">https://access.redhat.com/solutions/5427</a></li>
  <li id="ref-11">Handy, Jim (2013). "How Controllers Maximize SSD Life." <em>SNIA SSSI Tech Notes</em>. <a href="https://www.snia.org/sites/default/files/SSSITECHNOTES_HowControllersMaximizeSSDLife.pdf">https://www.snia.org/sites/default/files/SSSITECHNOTES_HowControllersMaximizeSSDLife.pdf</a></li>
</ol>

---

## Outtakes

**Thirty years too early.** An Australian engineer named Leo Port patented the first destination dispatch system in 1961, describing what modern kiosks now do. Mechanical relays couldn't run the logic. Port let the patent lapse in 1977, decades before microprocessors made his idea practical ([Peters, 2006](https://download.peters-research.com/library/Understanding_the_Benefits_and_Limitations_of_Destination_Dispatch.pdf)).

**The first one that actually worked.** Schindler's Miconic 10, invented by engineer Paul Friedli, became the first commercially successful destination dispatch system in 1990, installed first at Germany's Hamburg Electric Company before reaching the United States in 1993 ([Schindler, Miconic 10](https://sweets.construction.com/swts_content_files/620/241912.pdf)).

**Cars get the elevator treatment too.** Some automated parking garages use robotic elevator shuttles instead of ramps, one elevator per car, and researchers have studied scheduling a whole group of them the way building elevators get scheduled, down to genetic-algorithm tuning for rush hour ([Debnath, Serpen, and Dagli, 2015](https://scholars.utoledo.edu/display/pub-WOS000373845000074)).

**Warehouses got there first.** Daifuku built Japan's first automated storage and retrieval system in 1966, a stacker crane moving along a rack at a Matsushita Electric warehouse, tackling the same one-axis travel problem a year before Denning formalized it for disks ([Daifuku](https://www.daifuku.com/solution/technology/automatedwarehouse/)).

---

## Changelog

**2026-08-02** Rewrote several negated sentences into positive form per the updated style rules.  
**2026-08-01** Initial release.  
