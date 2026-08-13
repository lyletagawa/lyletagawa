---
title: "Design for 10x"
date: 2026-08-02
publishdate: 2026-08-02
lastmod: 2026-08-02
summary: "Jeff Dean's actual advice: design so your system survives 10x or 20x growth. Chasing 100x readiness before you need it costs more than it saves, and Google's GFS proved the rule by outgrowing itself anyway."
tags: ["scaling", "architecture", "systems"]
image: /images/design-for-10x.jpg
draft: true
---

![Jeff Dean's actual advice: design so your system survives 10x or 20x growth. Chasing 100x readiness before you need it costs more than it saves, and Google's GFS proved the rule by outgrowing itself anyway.](/images/design-for-10x.jpg)
*Google's data center in Council Bluffs, Iowa. Photo: [Chad Davis (2017)](https://commons.wikimedia.org/wiki/File:Google_Data_Center,_Council_Bluffs_Iowa_(49062863796).jpg). CC BY 2.0.*

## Design for 10x

A four-person startup builds its signup flow on a message queue rated for a million events a second, three regions of failover, and an auto-scaling cluster sized for a launch day that never comes. Twelve customers use the product. The infrastructure bill outpaces the two servers it replaced.

Jeff Dean, the Google Fellow behind half the systems that later became famous case studies in scale, gave this mistake a name at a 2009 keynote{{< cite 1 "Dean, Jeff (2009). Designs, Lessons and Advice from Building Large Distributed Systems. LADIS 2009 Keynote." >}}. One slide, titled "Design for Growth," says more about capacity planning than most books on the subject.

## What Dean Actually Said

The internet has flattened Dean's slide into a tidier slogan than he gave, something you can chant in a standup. Build for 10x. The real slide is more specific. Anticipate how requirements will evolve, and make sure the design still works if scale changes by 10x or 20x. The right solution for the current scale is often the wrong one two orders of magnitude out{{< cite 1 "Dean, Jeff (2009). Designs, Lessons and Advice from Building Large Distributed Systems. LADIS 2009 Keynote." >}}.

The distinction matters. The architecture that survives 20x growth and the architecture that survives 100x growth are usually two different architectures. Building the second one before you need it wastes effort you could spend on the product instead.

## Why the 100x Version Loses

A system built for 100x scale from day one carries assumptions the 1x version can skip. Partition keys chosen for a billion rows. Consensus protocols chosen for a hundred nodes. Caching layers chosen for a request rate the product may never see.

Every one of those assumptions adds a moving part, and every moving part is something to operate, monitor, and explain to the next engineer who joins the team. Dean's slides make the same point about infrastructure generally. Trying to serve every plausible future need usually produces a system that's more complex, slower to build, and worse at the load it has to carry today{{< cite 1 "Dean, Jeff (2009). Designs, Lessons and Advice from Building Large Distributed Systems. LADIS 2009 Keynote." >}}.

## GFS Outgrew Itself Anyway

Google's file system shows the same rule playing out at its own scale. The Google File System (GFS) used a single master server to track file metadata. That design held up past 10x and 20x growth for years, across Search's early explosion in traffic{{< cite 2 "Hildebrand, Dean and Denis Serenyi (2021). A Peek Behind Colossus, Google's File System. Google Cloud Blog." >}}.

Eventually the metadata alone, mostly tied to Search, outgrew what a single master could hold in memory and process fast enough. Google's fix was Colossus, a new system that moved metadata into Bigtable and scaled more than a hundred times past the largest GFS clusters ever ran{{< cite 2 "Hildebrand, Dean and Denis Serenyi (2021). A Peek Behind Colossus, Google's File System. Google Cloud Blog." >}}. GFS had fit its scale for a long time, right up until a different design became necessary at two orders of magnitude out, right at the boundary Dean's slide describes.

## Segment Overbuilt Early

Not every company gets Google's timeline to find the boundary. Segment split its data pipeline into more than 140 microservices, one per destination integration, expecting the fragmentation to keep each piece simple as the product grew{{< cite 3 "Noonan, Alexandra (2018). Goodbye Microservices: From 100s of Problem Children to 1 Superstar. Twilio Segment Engineering Blog." >}}.

The complexity spread. Shared libraries drifted out of sync across 140 codebases. Three full-time engineers spent most of their time keeping the system alive instead of shipping features. Segment published the retrospective under their own name and walked the architecture back into a single service. Deploys that once meant coordinating changes across dozens of services dropped to minutes{{< cite 3 "Noonan, Alexandra (2018). Goodbye Microservices: From 100s of Problem Children to 1 Superstar. Twilio Segment Engineering Blog." >}}. They had built for a scale of operational complexity beyond what their actual growth required.

## Where Engineers Get This Wrong

**Treating 10x as a formula instead of a check.** Ten times more users and ten times more write volume hit different bottlenecks. The rule is a reminder to test assumptions against a meaningfully larger scale. Treating it as a strict multiplier to compute defeats the purpose.

**Confusing design for 10x with building for 10x.** Anticipating how requirements evolve is a design exercise. Provisioning infrastructure is a separate decision that can wait until the load actually shows up. A design that survives 10x traffic without a rewrite can still run on hardware sized for 1x today.

**Skipping the estimate entirely.** The opposite failure gets less attention and costs just as much. A team that never asks what happens at 10x builds something that falls over the first time growth shows up, and the rewrite happens under pressure instead of on a schedule.

## Put It Into Practice

Take whatever you're building next and ask what breaks first if usage grew tenfold next quarter. If the answer is "nothing, this holds," the product has probably been over-built for where it is. If the answer is "everything," it's been under-built for where it's headed.

Write the answer down before writing the code. The estimate can be imprecise. It has to exist, so the redesign happens on purpose instead of during an outage.

## Dig Deeper

**Numbers everyone should know.** The same LADIS 2009 deck that has the "Design for Growth" slide also has Dean's latency reference table, L1 cache access through disk seeks to a cross-country round trip, that turns capacity planning into arithmetic instead of guesswork{{< cite 1 "Dean, Jeff (2009). Designs, Lessons and Advice from Building Large Distributed Systems. LADIS 2009 Keynote." >}}.

**The tail at scale.** Designing for growth changes more than throughput. Fan a request out to enough servers and the slowest response starts setting the pace for everyone, a separate problem Dean covered with Luiz André Barroso four years later{{< cite 4 "Dean, Jeffrey and Luiz André Barroso (2013). The Tail at Scale. Communications of the ACM, 56(2)." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Dean, Jeff (2009). <em>Designs, Lessons and Advice from Building Large Distributed Systems</em>. LADIS 2009 Keynote. Cornell University. <a href="https://www.cs.cornell.edu/projects/ladis2009/talks/dean-keynote-ladis2009.pdf">https://www.cs.cornell.edu/projects/ladis2009/talks/dean-keynote-ladis2009.pdf</a></li>
  <li id="ref-2">Hildebrand, Dean, and Denis Serenyi (2021). "A Peek Behind Colossus, Google's File System." <em>Google Cloud Blog</em>, April 19. <a href="https://cloud.google.com/blog/products/storage-data-transfer/a-peek-behind-colossus-googles-file-system">https://cloud.google.com/blog/products/storage-data-transfer/a-peek-behind-colossus-googles-file-system</a></li>
  <li id="ref-3">Noonan, Alexandra (2018). "Goodbye Microservices: From 100s of Problem Children to 1 Superstar." <em>Twilio Segment Engineering Blog</em>, July 10. <a href="https://www.twilio.com/en-us/blog/developers/best-practices/goodbye-microservices">https://www.twilio.com/en-us/blog/developers/best-practices/goodbye-microservices</a></li>
  <li id="ref-4">Dean, Jeffrey, and Luiz André Barroso (2013). "The Tail at Scale." <em>Communications of the ACM</em>, 56(2). <a href="https://www.barroso.org/publications/TheTailAtScale.pdf">https://www.barroso.org/publications/TheTailAtScale.pdf</a></li>
</ol>

---

## Changelog

**2026-08-02** Initial release.  
