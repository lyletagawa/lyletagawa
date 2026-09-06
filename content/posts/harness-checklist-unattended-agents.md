---
title: "The Harness Checklist for Unattended Agents"
date: 2026-08-14
publishdate: 2026-08-14
lastmod: 2026-08-14
summary: "A loop is a harness with the human-supplied stop condition removed. Four properties, verifiers, reversibility, a bounded blast radius, and a real stop condition, decide whether removing that person is safe or catastrophic."
tags: ["ai", "agents", "safety"]
image: /images/harness-checklist-unattended-agents.jpg
draft: true
---

![A loop is a harness with the human-supplied stop condition removed. Four properties, verifiers, reversibility, a bounded blast radius, and a real stop condition, decide whether removing that person is safe or catastrophic.](/images/harness-checklist-unattended-agents.jpg)
*A champion team of Percheron draft horses at work on an Indiana stock farm, c. 1900s. Public domain via [New York Public Library, Robert N. Dennis collection / Wikimedia Commons](https://commons.wikimedia.org/wiki/File:A_champion_team_of_Percheron_draft_horses_at_work_on_an_Indiana_stock_farm_(NYPL_b11707465-G90F188_016F).tiff).*

## The Loop Made 340 Commits

By the time anyone looked at the dashboard, the loop had made 340 commits overnight. The change that kicked it off looked minor: a coding agent's tool-call parser got a little more permissive, so it would stop rejecting a class of malformed function calls the team kept seeing in the logs. Nobody wrote an eval for it. It looked too small to need one. It shipped.

The parser was now permissive enough to also accept a different malformed call, one the agent itself started generating a few hours later, and instead of raising an error it silently truncated a file path. The harness had checks. None of them covered this shape of failure. Nothing was watching for it, either, because the loop had no defined point at which it was supposed to stop and ask a person. So it kept going, all night, compounding the same silent truncation across the codebase, one commit at a time.

This is a harness problem. A loop just turned it into 340 of them.

## Loops Are Harnesses Minus You

Harness engineering names the layer of guardrails, verifiers, and permanent fixes that wraps an agent so it can act, run commands, edit files, call tools, without a mistake it already made once being able to repeat itself{{< cite 1 "Hashimoto, Mitchell (2026). My AI Adoption Journey. mitchellh.com." >}}. Teams that took it seriously shipped products built entirely by an agent, 1 million lines of code in one case, by treating that wrapper as the actual product surface{{< cite 2 "Lopopolo, Ryan (2026). Harness engineering: leveraging Codex in an agent-first world. OpenAI." >}}. The model underneath it was almost incidental.

Loop engineering takes a layer away. Boris Cherny, who built Claude Code at Anthropic, put it plainly. He doesn't prompt Claude anymore. He has loops running that prompt it for him{{< cite 3 "Osmani, Addy (2026). Loop Engineering. Elevate." >}}. The person who used to look at each output and decide whether to continue is gone from the seat. Everything that person used to catch by eye now has to be caught by something you built instead.

That's a mechanical claim. A loop is a harness with its human-supplied stop condition swapped for an engineered one. Whether that swap is safe depends entirely on what the harness was already carrying before you made it.

## What the Harness Needs

**Verifiers.** An automated check that catches a bad action before it compounds. A log line that records the damage afterward doesn't count. This is the piece that keeps getting harder as agents improve. For modern coding agents, generating a plausible candidate solution is now often easier than reliably verifying it, which is the opposite of the classical assumption that checking is cheaper than doing{{< cite 4 "Wang, Binghai, et al. (2026). The Verification Horizon: No Silver Bullet for Coding Agent Rewards. arXiv." >}}. A harness that hasn't caught up to that is watching less than it looks like it's watching.

**Reversibility.** Every action the loop can take has a cheap, available undo, a property distinct from merely logging what happened{{< cite 5 "Nygard, Michael T. (2018). Release It! Design and Deploy Production-Ready Software, 2nd ed. Pragmatic Bookshelf." >}}. If undoing a bad commit costs more than a human afternoon, it isn't reversible in any sense that matters to a system running at 2 a.m.

**A bounded blast radius.** A hard ceiling on how much a single iteration can touch, whether that's files, dollars, or systems, enforced whether or not the verifiers happen to be working correctly that iteration. Verifiers can miss things. A blast-radius limit is what keeps a miss small instead of letting it spread for 8 hours.

**A real stop condition.** An explicit success, failure, or escalation criterion. A loop with no stop condition just keeps going rather than pausing to ask, which is exactly how one bad parser change spiraled into an overnight mess.

## Common Mistakes

**Treating reversibility and a bounded blast radius as the same guarantee.** "We can always roll it back" is a claim about what happens after damage occurs. A blast-radius limit is a claim about how much damage can occur in the first place. A system can have excellent rollback tooling and still let a bad iteration touch 10,000 files before anyone rolls anything back.

**Treating a verifier as a stop condition.** A verifier catches a bad action. A stop condition ends the loop. A harness can have first-rate verifiers, catch a failure correctly every time, and still run indefinitely, because catching a problem and stopping for it are two different pieces of engineering, and only one of them was built.

## What To Do About It

Before turning a harness into a loop, ask what a person was actually doing in that seat. If they were catching mistakes by eye, that's a verifier gap. If they were the reason a bad action didn't spread further, that's a blast-radius gap. If they were deciding when enough was enough, that's a missing stop condition. If undoing their own mistakes would have taken them an afternoon, reversibility was never really there either, a person was just quietly absorbing that cost.

A loop doesn't create new failure modes. It removes the person who used to compensate for the ones the harness never fixed, and then runs for 8 hours before anyone notices which one was missing.

---

## References

<ol class="references">
  <li id="ref-1">Hashimoto, Mitchell (2026). "My AI Adoption Journey." <em>mitchellh.com</em>. <a href="https://mitchellh.com/writing/my-ai-adoption-journey">https://mitchellh.com/writing/my-ai-adoption-journey</a></li>
  <li id="ref-2">Lopopolo, Ryan (2026). "Harness engineering: leveraging Codex in an agent-first world." <em>OpenAI</em>. <a href="https://openai.com/index/harness-engineering/">https://openai.com/index/harness-engineering/</a></li>
  <li id="ref-3">Osmani, Addy (2026). "Loop Engineering." <em>Elevate</em>. <a href="https://addyo.substack.com/p/loop-engineering">https://addyo.substack.com/p/loop-engineering</a></li>
  <li id="ref-4">Wang, Binghai, et al. (2026). "The Verification Horizon: No Silver Bullet for Coding Agent Rewards." <em>arXiv</em>. <a href="https://arxiv.org/abs/2606.26300">https://arxiv.org/abs/2606.26300</a></li>
  <li id="ref-5">Nygard, Michael T. (2018). <em>Release It! Design and Deploy Production-Ready Software</em>, 2nd ed. Raleigh: Pragmatic Bookshelf. <a href="https://pragprog.com/titles/mnee2/release-it-second-edition/">https://pragprog.com/titles/mnee2/release-it-second-edition/</a></li>
</ol>

---

## Changelog

**2026-08-14** Initial publish.
