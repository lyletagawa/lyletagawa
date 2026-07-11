---
title: "Off the Critical Path"
date: 2026-07-04
publishdate: 2026-07-04
lastmod: 2026-07-04
summary: "The critical path is the sequence of tasks that determines how fast a system can run. Netflix's live captioning patent shows why moving work off it is often the fastest optimization available."
tags: ["systems", "latency", "pipelines"]
image: /images/off-the-critical-path.svg
draft: true
---

![The critical path is the sequence of tasks that determines how fast a system can run. Netflix's live captioning patent shows why moving work off it is often the fastest optimization available.](/images/off-the-critical-path.svg)

## Off the Critical Path

A live match is streaming to a million viewers in eight languages. The caption for the knockout punch arrives 3.8 seconds after impact, even though the captioner typed it in under a second. The rest of that delay is the system, sequential steps each waiting for the last.

Netflix filed a patent in June 2024 to fix exactly this{{< cite 1 "Concolato, Cyril (2024). Techniques for Performing Live Captioning. U.S. Patent Application US20250324117A1, Netflix Inc." >}}. The fix doesn't make any single step faster. It makes steps concurrent.

## What Is the Critical Path?

Morgan Walker and James Kelley developed the Critical Path Method (CPM) at DuPont in 1957. The problem was scheduling chemical plant construction and maintenance, where hundreds of interdependent tasks had to complete within a fixed window. Shutdowns were expensive. Every extra day cost money{{< cite 2 "Kelley, James E., and Morgan R. Walker (1959). Critical-Path Planning and Scheduling. Proceedings of the Eastern Joint Computer Conference, 16." >}}.

The insight was simple. Not all tasks are equal. Some can slip without delaying the project, but others can't move at all. The critical path is the longest chain of dependent tasks from start to finish. It sets the minimum possible duration. Shorten anything on it and the project finishes sooner. Shorten anything off it and the end date doesn't move.

DuPont saved $1 million in the first year of applying CPM to plant shutdowns{{< cite 2 "Kelley, James E., and Morgan R. Walker (1959). Critical-Path Planning and Scheduling. Proceedings of the Eastern Joint Computer Conference, 16." >}}. The method spread to construction, aerospace, and infrastructure.

## The Netflix Patent

The conventional live captioning pipeline is sequential. Audio and video arrive at a hardware caption encoder. The encoder waits for the captioning service to generate captions, combines them, then forwards everything downstream to the distribution encoder. Caption encoding sits on the critical path, and every step it takes adds to viewer latency. Hardware encoders also cap language support, some handling no more than four Latin languages per event.

Netflix's solution runs caption generation and video distribution encoding concurrently. The live event server sends the full feed directly to the distribution encoder without waiting for captions. It simultaneously sends a lower-quality copy to the captioning service. Both run independently.

The distribution encoder generates video and audio segments and creates empty placeholder caption segments. The captioning service generates caption data and stores it with timestamps in a database. A caption processing cluster watches for two signals. It needs a notification that a video segment is complete and the caption data for that time window. When both arrive, it fills the placeholder. The video never waits.

A pre-computed "caption delay" handles synchronization. It's the difference between how long the video pipeline takes and how long caption generation takes. Because distribution encoding typically takes longer than captioning, the captions arrive before the placeholder notification does. The caption is already waiting when the segment is ready{{< cite 1 "Concolato, Cyril (2024). Techniques for Performing Live Captioning. U.S. Patent Application US20250324117A1, Netflix Inc." >}}.

Caption encoding is off the critical path. Viewer latency drops, and language support becomes a software constraint, not a hardware one.

## Where It Shows Up

**Deployment pipelines.** Security scans, integration tests, and compliance checks often run sequentially after a build completes. Most can run concurrently against the same artifact instead. Buildkite parallelized its own RSpec suite across 60 agents and cut the run from roughly three hours to under five minutes{{< cite 3 "Buildkite. CI/CD Best Practices." >}}. The pipeline takes as long as the longest sequential chain, not the sum of all steps.

**Code review.** A PR blocked on sequential reviews, each waiting for the previous approval, puts reviewer latency on the critical path. Shopify's merge queue had a 90th-percentile wait of 693 minutes, over eleven hours, before it started letting independent changes merge in parallel instead of queuing one behind another, a change that cut that same wait to about 41 minutes{{< cite 4 "Graphite. How Shopify Cut Merge Queue Delays from Hours to Minutes with the Graphite Merge Queue." >}}. Parallel review doesn't change total reviewer time. It changes time to merge.

**Schema migrations.** Teams that run migrations synchronously during deploys put the migration on the critical path of the release. The expand-contract pattern, formalized by Danilo Sato as parallel change, decouples them{{< cite 5 "Sato, Danilo (2014). Parallel Change. martinfowler.com." >}}. First apply an additive schema change both old and new code can handle, deploy the new code, then remove what's no longer needed. The migration never blocks the deploy because the two steps are separated in time.

**Incident response.** Sequential triage puts every handoff on the critical path of time-to-resolution. Google's own incident management structure runs investigation threads in parallel under an incident commander who coordinates them, rather than forcing every action through one person or one queue{{< cite 6 "Google (2017). Managing Incidents. Site Reliability Engineering: How Google Runs Production Systems. O'Reilly Media." >}}. Parallel investigation with explicit coordination pulls multiple threads at once.

## Common Mistakes

**Treating concurrency as free.** Concurrent tasks still require synchronization. Netflix's timestamp mechanism exists because caption data and video segments need to meet at the right moment. Moving work off the critical path creates coordination overhead. Account for it.

**Optimizing what isn't on the critical path.** Teams often speed up the slowest-feeling step, not the critical one. A step that's slow but runs concurrently with a slower one doesn't affect overall duration. The question isn't "which step is slowest?" It's "which step is on the critical path?"

**Ignoring that the critical path shifts.** Fix one bottleneck and the next one surfaces, which is Goldratt's Theory of Constraints in miniature. Every system has exactly one binding constraint at a time{{< cite 7 "Goldratt, Eliyahu M., and Jeff Cox (1984). The Goal: A Process of Ongoing Improvement. North River Press." >}}, so removing caption encoding from the critical path doesn't eliminate latency. It just relocates the constraint. The work never really finishes.

**Leaving sequential what doesn't have to be.** Most sequential pipelines weren't designed that way. They accumulated. A step that once depended on the previous output no longer does. The tooling changed, the architecture changed, but the dependency stayed. It's historical, not structural.

## Put It Into Practice

Draw your deployment process as a dependency graph, not a list. A list hides what's sequential from what could be concurrent. For each step that waits on the previous one, ask whether that dependency is structural (the output of step A is required input to step B) or incidental (step B follows A by convention or tooling default).

For each incidental dependency, ask what it would take to parallelize it. The answer is usually cheaper than the latency it's producing.

Pick one step in your build, deploy, or review process that doesn't strictly depend on the previous one. Move it off the critical path.

---

## References

<ol class="references">
  <li id="ref-1">Concolato, Cyril (2024). "Techniques for Performing Live Captioning." U.S. Patent Application US20250324117A1. Netflix Inc. <a href="https://patents.google.com/patent/US20250324117A1/">https://patents.google.com/patent/US20250324117A1/</a></li>
  <li id="ref-2">Kelley, James E., and Morgan R. Walker (1959). "Critical-Path Planning and Scheduling." <em>Proceedings of the Eastern Joint Computer Conference</em>, 16: 160-173. <a href="https://dl.acm.org/doi/10.1145/1460299.1460318">https://dl.acm.org/doi/10.1145/1460299.1460318</a></li>
  <li id="ref-3">Buildkite. "CI/CD Best Practices." <a href="https://buildkite.com/resources/blog/ci-cd-best-practices/">https://buildkite.com/resources/blog/ci-cd-best-practices/</a></li>
  <li id="ref-4">Graphite. "How Shopify Cut Merge Queue Delays from Hours to Minutes with the Graphite Merge Queue." <a href="https://graphite.com/customer/shopify-merge-queu">https://graphite.com/customer/shopify-merge-queu</a></li>
  <li id="ref-5">Sato, Danilo (2014). "Parallel Change." martinfowler.com. <a href="https://martinfowler.com/bliki/ParallelChange.html">https://martinfowler.com/bliki/ParallelChange.html</a></li>
  <li id="ref-6">Google (2017). "Managing Incidents." <em>Site Reliability Engineering: How Google Runs Production Systems</em>. O'Reilly Media. <a href="https://sre.google/sre-book/managing-incidents/">https://sre.google/sre-book/managing-incidents/</a></li>
  <li id="ref-7">Goldratt, Eliyahu M., and Jeff Cox (1984). <em>The Goal: A Process of Ongoing Improvement</em>. North River Press. <a href="https://northriverpress.com/the-goal-a-process-of-ongoing-improvement/">https://northriverpress.com/the-goal-a-process-of-ongoing-improvement/</a></li>
</ol>

---

## Outtakes

**The Polaris program arrived early.** The U.S. Navy developed PERT in 1958 to coordinate 10,000 contractors on the Polaris submarine missile program. DuPont developed CPM the same year for different reasons. Both converged on the same insight. The Polaris program, projected to take seven years, delivered two years early (Malcolm et al., 1959).

**The 787 that taught Boeing a lesson.** Boeing outsourced 787 subassembly to 50 global suppliers, expecting concurrent development. Instead it created a critical path through coordination. Incomplete parts arrived at Everett requiring rework on the assembly floor. The plane delivered three years late. Moving work off the critical path requires knowing where the new one lands.

---

## Changelog

**2026-07-04** Initial draft.
