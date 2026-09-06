---
title: "The Squirrel Threat Model"
date: 2026-08-02
publishdate: 2026-08-02
lastmod: 2026-08-08
summary: "Squirrels have shut down Nasdaq trading twice. Beavers, cows, and sharks have taken down networks too, but misconfiguration, sabotage, and copper theft cause far more damage than any animal ever has."
tags: ["networking", "reliability", "incidents"]
image: /images/wan-failures.jpg
draft: false
---

![Squirrels have shut down Nasdaq trading twice. Beavers, cows, and sharks have taken down networks too, but misconfiguration, sabotage, and copper theft cause far more damage than any animal ever has.](/images/wan-failures.jpg)
*Two squirrels on a utility cable in Brampton, Ontario. Photo: [Pierre André Leclercq (2017)](https://commons.wikimedia.org/wiki/File:Brampton.-_Ecureuils_canadiens.JPG). CC BY-SA 4.0.*

## The Squirrel Threat Model

On December 9, 1987, a squirrel got into a power substation in Trumbull, Connecticut and shorted equipment that fed Nasdaq's data center. Trading halted for 82 minutes, and a Nasdaq official later estimated the outage kept more than 20 million shares from changing hands{{< cite 1 "UPI (1987). Power failure interrupts NASDAQ, hurts computer. UPI Archives, December 9, 1987." >}}.

On August 2, 1994, it happened again. A different squirrel found its way into the same power substation and halted Nasdaq trading a second time. Trading resumed within 34 minutes{{< cite 2 "Neumann, Peter G., ed. (1994). Squirrels bring down NASDAQ. The RISKS Digest, Volume 16, Issue 30, August 2, 1994." >}}.

Somewhere there is a security budget built around nation-state actors and zero-day exploits. But a squirrel still took down the network underlying the US stock market.

## Wildlife Outages

**Squirrels.** John Inglis, a former deputy director of the National Security Agency, said on record that "the No. 1 threat experienced to date by the U.S. electrical grid is squirrels{{< cite 3 "Smithsonian Magazine (2015). Move Over Hackers, Squirrels Are the Power Grid's Greatest Foe." >}}." A crowdsourced tracker called Cyber Squirrel 1 logged more than 2,500 animal-caused power incidents before it stopped updating in 2021, nearly half of them squirrels{{< cite 4 "Cyber Squirrel 1 (2021). cybersquirrel1.com." >}}.

**Beavers.** In April 2021, beavers in Tumbler Ridge, British Columbia, dug three feet down alongside a creek, chewed through a 4.5-inch protective conduit, and then chewed through the fiber cable inside it. About 900 people lost internet service for 36 hours. A Telus spokesperson called it "a bizarre and uniquely Canadian turn of events{{< cite 5 "CBC News (2022). For the second year in a row, a beaver is to blame for phone and internet outages in northern B.C." >}}." It happened again to the same region the following year.

**Cows.** In May 2020, Google's SVP of technical infrastructure, Urs Hölzle, live-tweeted an explanation for repeated short outages on a fiber path through Oregon. The fiber ran above ground along power poles, and a farmer had started grazing cattle nearby. Whenever a cow stepped on the fallen line, it bent enough to interrupt traffic{{< cite 6 "Hölzle, Urs (2020). Twitter/X post, May 21, 2020." >}}.

**Sharks.** Bite marks turned up in an undersea data line off the Canary Islands in 1985. Google now wraps some shoreline cables in protective armor, but marine biologists and telecom engineers are blunt about the actual scale of the threat. "This is probably one of the biggest myths we see cited in the press{{< cite 7 "Marquez, Melissa Cristina (2020). Our Underwater World Is Full Of Cables, That Are Sometimes Attacked By Sharks. Forbes." >}}." Nearly all undersea cable damage comes from something far less exciting: ship anchors and fishing trawlers.

None of the animal-caused outages come close to the damage humans do to their own networks (on purpose or by accident).

## One Bad Command

On October 4, 2021, a Facebook engineer ran a routine command meant to check spare capacity on the company's backbone network. The command carried a bug, and a separate audit tool built to catch this kind of mistake had a bug of its own. The command took down every connection in Facebook's backbone{{< cite 8 "Meta Engineering (2021). More details about the October 4 outage." >}}.

The failure cascaded fast. With the backbone gone, Facebook's own DNS servers judged themselves unreachable and withdrew their BGP route advertisements, the same mechanism meant to route around a failure. That made the DNS servers unreachable too, even though the physical machines never stopped running. Facebook, Instagram, and WhatsApp disappeared from the internet for about six hours, and engineers even reportedly struggled to get into the affected buildings, since the badge readers ran on that same downed network{{< cite 8 "Meta Engineering (2021). More details about the October 4 outage." >}}.

Other companies made the same mistake. In January 2017, an engineer at GitLab tried to fix a lagging secondary database, but triggered a destructive command against the primary production database by mistake. Though the command was terminated within two seconds, 300GB of production data had already been removed. Several backup mechanisms failed as well, forcing GitLab to restore from a snapshot from 6 hours prior{{< cite 9 "GitLab (2017). Postmortem of database outage of January 31. about.gitlab.com." >}}.

A month later, an AWS engineer mistyped a routine maintenance command against Amazon S3, and removed far more servers than intended. A large share of the internet was taken down, including Slack, DockerHub, and GitHub, for about four hours{{< cite 10 "Gremlin (2025). After the Retrospective: The 2017 Amazon S3 Outage. Gremlin Blog." >}}. Amazon's status dashboard couldn't update to tell customers what was happening, because it depended on the same system that had just failed{{< cite 11 "Amazon Web Services (2017). Summary of the Amazon S3 Service Disruption in the Northern Virginia (US-EAST-1) Region. aws.amazon.com." >}}.

No squirrel has ever done that much damage in a single afternoon.

## Criminal Mischief

On December 25, 2024, the oil tanker Eagle-S dragged its anchor roughly 100 kilometers across the Baltic seabed, severing the Estlink-2 power cable and four telecom cables between Finland and Estonia. Finnish investigators later matched the seabed drag mark to the ship's anchor. The Eagle-S is flagged in the Cook Islands but is part of Russia's shadow fleet, tankers that move sanctioned oil while avoiding Western insurance and registry rules{{< cite 12 "NPR (2024). What to know about Finland, Russia's 'shadow fleet' and a severed undersea cable." >}}.

Estlink-2 stayed offline for over seven months, and repairs cost an estimated 60 million euros. Whether the anchor drag was deliberate sabotage or reckless navigation is still contested. Finnish prosecutors charged the ship's officers with aggravated criminal mischief rather than sabotage{{< cite 12 "NPR (2024). What to know about Finland, Russia's 'shadow fleet' and a severed undersea cable." >}}. Several other Baltic cables have been severed the same way since 2023.

## Salvage Value

On March 28, 2011, a 75-year-old woman scavenging for copper near Tbilisi struck a buried fiber-optic cable with her shovel. The cable carried most of Armenia's internet traffic, and the country went offline for hours, along with parts of Georgia and Azerbaijan{{< cite 13 "Parfitt, Tom (2011). Georgian woman cuts off web access to whole of Armenia. The Guardian, April 6, 2011." >}}.

Between June and December 2024, nearly 6,000 incidents of telecom theft and vandalism disrupted service nationwide, affecting an estimated 1.5 million customers. Almost all of it targeted copper. A stolen spool of copper wire might fetch a few hundred dollars at a scrapyard. The societal cost, measured in lost productivity and disrupted access to emergency services, was estimated at $38 million to $188 million{{< cite 14 "Lopez, Edward J. (2025). The Real Costs of Communications Outages due to Infrastructure Theft or Vandalism. NCTA." >}}.

Sharks and squirrels make headlines, but copper theft is often unreported even though it's one of the largest causes of communications outages in the country.

## Where People Get This Wrong

**Treating the funny story as the common story.** A shark bite or a squirrel outage makes a good story. But wildlife-caused outages are not as significant as human error or human theft.

**Spending the budget on the wrong threat.** Teams harden against dramatic, low-probability scenarios while a mistyped maintenance command or copper cable pulled out of a rural span is the far likelier way the network goes down.

## Put It Into Practice

Start by comparing your threat model to your actual incident history. Pull the last two years of outage tickets and sort by contributing factors instead of by how good the postmortem story was. If misconfiguration or physical theft dominate the list, that's where the next dollar of resilience spending belongs.

A command capable of disconnecting an entire backbone should never run without guardrails, and the safety check meant to catch that kind of mistake needs testing as carefully as the systems it protects. Peer review, staged rollouts, and continuous verification matter just as much.

Build for the boring failure. Route critical paths so a single fault can't take an entire region offline. The squirrel makes a better story. The thief who came back for the same copper three times this year is the one who takes you down.

---

## References

<ol class="references">
  <li id="ref-1">UPI (1987). "Power failure interrupts NASDAQ, hurts computer." <em>UPI Archives</em>, December 9, 1987. <a href="https://www.upi.com/Archives/1987/12/09/Power-failure-interrupts-NASDAQ-hurts-computer/6430566024400/">https://www.upi.com/Archives/1987/12/09/Power-failure-interrupts-NASDAQ-hurts-computer/6430566024400/</a></li>
  <li id="ref-2">Neumann, Peter G., ed. (1994). "Squirrels bring down NASDAQ." <em>The RISKS Digest</em>, Volume 16, Issue 30, August 2, 1994. <a href="https://catless.ncl.ac.uk/Risks/16/30">https://catless.ncl.ac.uk/Risks/16/30</a></li>
  <li id="ref-3">Smithsonian Magazine (2015). "Move Over Hackers, Squirrels Are the Power Grid's Greatest Foe." <a href="https://www.smithsonianmag.com/smart-news/move-over-hackers-squirrels-power-grid-greatest-foe-180957834/">https://www.smithsonianmag.com/smart-news/move-over-hackers-squirrels-power-grid-greatest-foe-180957834/</a></li>
  <li id="ref-4">Cyber Squirrel 1 (2021). <a href="https://www.cybersquirrel1.com/">https://www.cybersquirrel1.com/</a></li>
  <li id="ref-5">CBC News (2022). "For the second year in a row, a beaver is to blame for phone and internet outages in northern B.C." <a href="https://www.cbc.ca/news/canada/british-columbia/beaver-bc-internet-outage-1.6483965">https://www.cbc.ca/news/canada/british-columbia/beaver-bc-internet-outage-1.6483965</a></li>
  <li id="ref-6">Hölzle, Urs (2020). Twitter/X post, May 21, 2020. <a href="https://twitter.com/uhoelzle/status/1263333281891708929">https://twitter.com/uhoelzle/status/1263333281891708929</a></li>
  <li id="ref-7">Marquez, Melissa Cristina (2020). "Our Underwater World Is Full Of Cables, That Are Sometimes Attacked By Sharks." <em>Forbes</em>. <a href="https://www.forbes.com/sites/melissacristinamarquez/2020/07/20/our-underwater-world-is-full-of-cables-that-are-sometimes-attacked-by-sharks/">https://www.forbes.com/sites/melissacristinamarquez/2020/07/20/our-underwater-world-is-full-of-cables-that-are-sometimes-attacked-by-sharks/</a></li>
  <li id="ref-8">Meta Engineering (2021). "More details about the October 4 outage." <a href="https://engineering.fb.com/2021/10/05/networking-traffic/outage-details/">https://engineering.fb.com/2021/10/05/networking-traffic/outage-details/</a></li>
  <li id="ref-9">GitLab (2017). "Postmortem of database outage of January 31." <a href="https://about.gitlab.com/blog/postmortem-of-database-outage-of-january-31/">https://about.gitlab.com/blog/postmortem-of-database-outage-of-january-31/</a></li>
  <li id="ref-10">Gremlin (2025). "After the Retrospective: The 2017 Amazon S3 Outage." <em>Gremlin Blog</em>. <a href="https://www.gremlin.com/blog/the-2017-amazon-s-3-outage">https://www.gremlin.com/blog/the-2017-amazon-s-3-outage</a></li>
  <li id="ref-11">Amazon Web Services (2017). "Summary of the Amazon S3 Service Disruption in the Northern Virginia (US-EAST-1) Region." <a href="https://aws.amazon.com/message/41926/">https://aws.amazon.com/message/41926/</a></li>
  <li id="ref-12">NPR (2024). "What to know about Finland, Russia's 'shadow fleet' and a severed undersea cable." <a href="https://www.npr.org/2024/12/31/nx-s1-5243302/finland-russia-severed-undersea-cable-shadow-fleet">https://www.npr.org/2024/12/31/nx-s1-5243302/finland-russia-severed-undersea-cable-shadow-fleet</a></li>
  <li id="ref-13">Parfitt, Tom (2011). "Georgian woman cuts off web access to whole of Armenia." <em>The Guardian</em>, April 6, 2011. <a href="https://www.theguardian.com/world/2011/apr/06/georgian-woman-cuts-web-access">https://www.theguardian.com/world/2011/apr/06/georgian-woman-cuts-web-access</a></li>
  <li id="ref-14">Lopez, Edward J. (2025). "The Real Costs of Communications Outages due to Infrastructure Theft or Vandalism." <em>NCTA</em>. <a href="https://www.ncta.com/wp-content/uploads/2025/10/Economic-Impact-Study_1001_2025.pdf">https://www.ncta.com/wp-content/uploads/2025/10/Economic-Impact-Study_1001_2025.pdf</a></li>
</ol>

---

## Outtakes

**A monkey blacked out an entire country.** In 2016, a vervet monkey fell onto a transformer at Kenya's largest hydropower station, overloading nearby machines and cutting power to an estimated 4.7 million households and businesses for more than four hours. The monkey survived ([NPR, 2016](https://www.npr.org/2016/06/08/481206952/monkey-knocks-out-power-across-kenya)).

**Woodpeckers.** Ireland's ESB Networks blames great spotted woodpeckers, roughly 100 nesting pairs, for a wave of pole damage across Wicklow and Wexford. The birds hollow out wooden poles hunting for nest sites, and rot forces the scheduled outages that follow ([Irish Times, 2023](https://www.irishtimes.com/environment/2023/11/28/woodpeckers-blamed-for-power-outages-as-crews-race-to-repair-damaged-electricity-poles/)).

**Raccoon.** Before dawn in July 2025, a raccoon touched live equipment inside a New Jersey substation and set off an explosion that knocked out power across two states. Crews rerouted power to most of the affected customers within 35 minutes ([News 12, 2025](https://newyork.news12.com/raccoon-causes-multistate-power-outage)).

---

## Changelog

**2026-08-08** Fixed the Nasdaq squirrel citations.  
**2026-08-05** Added the copper-scavenging anecdote, corrected the cow citation to Hölzle's original tweet.  
**2026-08-02** Initial release.  
