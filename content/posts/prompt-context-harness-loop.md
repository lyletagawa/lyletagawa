---
title: "Prompt, Context, Harness, Loop"
date: 2026-08-12
publishdate: 2026-08-12
lastmod: 2026-08-12
summary: "AI engineering has renamed its core discipline four times since 2021, prompt to context to harness to loop engineering. The last two renames landed four months apart."
tags: ["ai", "agents", "terminology"]
image: /images/prompt-context-harness-loop.jpg
draft: true
---

![AI engineering has renamed its core discipline four times since 2021, prompt to context to harness to loop engineering. The last two renames landed four months apart.](/images/prompt-context-harness-loop.jpg)
*Codex Climaci Rescriptus, a palimpsest where an earlier Syriac text was scraped away and written over. Photo: [Denysmonroe81 (2011)](https://commons.wikimedia.org/wiki/File:Codex_Climaci_Rescriptus.jpg). CC BY-SA 3.0.*

## Prompt, Context, Harness, Loop

On February 5, 2026, Mitchell Hashimoto published a post about a habit he'd picked up while building coding agents. Every time an agent made the same mistake twice, he engineered a permanent fix into its environment so the mistake became structurally impossible to repeat. He didn't have an industry term for it yet, so he made one up. "I've grown to calling this 'harness engineering'"{{< cite 1 "Hashimoto, Mitchell (2026). My AI Adoption Journey. mitchellh.com." >}}.

Six days later, OpenAI made it official. Ryan Lopopolo described a team that shipped an internal product with zero manually written lines of code, 1 million lines total, all written by an agent operating inside a harness of guides, checks, and constraints{{< cite 2 "Lopopolo, Ryan (2026). Harness engineering: leveraging Codex in an agent-first world. OpenAI." >}}. One engineer's private habit had become a company-endorsed discipline in under a week.

Four months later, it got renamed again. On June 8, 2026, Addy Osmani published an essay called "Loop Engineering." Boris Cherny, who built Claude Code at Anthropic, told him he didn't prompt Claude anymore. He had loops running that prompted Claude for him{{< cite 3 "Osmani, Addy (2026). Loop Engineering. Elevate." >}}.

## The Rename Has Precedent

By 2021, researchers working with GPT-3 were already calling the skill prompt engineering, treating the precise wording of a request as the main lever available for improving a model's output{{< cite 4 "Reynolds, Laria, and Kyle McDonell (2021). Prompt Programming for Large Language Models: Beyond the Few-Shot Paradigm. arXiv." >}}. For years, that held. Phrasing was often the single biggest factor separating a useless answer from a good one.

Then in June 2025, Shopify CEO Tobi Lütke said he liked a different term better. Context engineering describes the core skill better, the art of providing all the context needed for a task to be plausibly solvable by the model{{< cite 5 "Lütke, Tobi (2025). Post on context engineering. X." >}}. Andrej Karpathy amplified the distinction days later, and the label stuck fast.

Both terms pointed at something real. As models got better at following instructions, wording mattered less and what the model could see mattered more. Prompt engineering optimized a sentence. Context engineering optimized a window instead.

## Four Bottlenecks, Four Names

Harness engineering names the next shift. Once context is handled well, an agent can act on it, run commands, edit files, call tools, and every one of those actions can go wrong in a way that has nothing to do with the prompt or the context. The harness is everything that wraps the model, the guardrails, the verifiers, and the permanent fixes for whatever mistake the agent already made once.

Loop engineering names the shift after that. Fixing an agent's mistakes one at a time still puts a human in the loop, deciding what to fix and when. Osmani's definition removes that seat entirely. You stop being the person who prompts the agent, and you build the system, the triggers, the verifiers, the stop conditions, that prompts it for you instead, indefinitely, unattended{{< cite 3 "Osmani, Addy (2026). Loop Engineering. Elevate." >}}.

Four terms, four different bottlenecks. Wording, then information, then execution, then autonomy. Each one was the real constraint at the moment it got named, then stopped being interesting the moment the field solved it well enough to move on.

## Evals

A fifth term shows up just as often in 2026 job postings and platform docs, eval engineering, the discipline of building and gating the evaluation suites that catch a broken agent before a user does. It sits alongside prompt, context, harness, and loop engineering in almost every roundup of the field, but it doesn't belong to the same lineage.

The other four each renamed one shifting question, what's actually limiting the agent right now. Eval engineering answers a different question, whether the other four are actually working{{< cite 6 "FutureAGI (2026). What is Evals Engineering? The Discipline Behind Production LLMs in 2026. FutureAGI Blog." >}}.

## Common Mistakes

**Treating the newest term as a replacement for the old ones.** A harness with no context engineering behind it still fails, because the agent it's protecting is acting on the wrong information. A loop with no harness under it still misbehaves, just on a longer, unsupervised timescale. Each term adds a layer on top of the last, and every earlier one stays necessary.

**Chasing the label instead of the bottleneck.** Rewriting your job title or your team's strategy doc every few months to match whichever term is trending treats vocabulary as the problem. The actual question is narrower than the label. Figure out what's actually slowing your system down right now.

## What To Do About It

Ask which of these is actually yours right now, and ignore whichever term is trending in your feed. Vague prompts, missing information, an agent that's free to act but keeps repeating mistakes nobody's fixed, you still deciding its next move yourself when a loop could handle that instead, or nobody able to tell whether last week's changes actually helped or hurt.

If you're unsure, look at where your team's time actually goes. Rewording instructions over and over is a prompt problem. Hunting for the right file or example, every single time, is a context problem. An agent that fails the same way twice, because nobody built the fix in, has a harness problem. Deciding its next move yourself, step by step, is a loop problem. And shipping changes with no way to tell if they helped or hurt is an eval problem.

The next rename is coming regardless. Fix the bottleneck you actually have, and let the label catch up whenever it gets around to it.

---

## References

<ol class="references">
  <li id="ref-1">Hashimoto, Mitchell (2026). "My AI Adoption Journey." <em>mitchellh.com</em>. <a href="https://mitchellh.com/writing/my-ai-adoption-journey">https://mitchellh.com/writing/my-ai-adoption-journey</a></li>
  <li id="ref-2">Lopopolo, Ryan (2026). "Harness engineering: leveraging Codex in an agent-first world." <em>OpenAI</em>. <a href="https://openai.com/index/harness-engineering/">https://openai.com/index/harness-engineering/</a></li>
  <li id="ref-3">Osmani, Addy (2026). "Loop Engineering." <em>Elevate</em>. <a href="https://addyo.substack.com/p/loop-engineering">https://addyo.substack.com/p/loop-engineering</a></li>
  <li id="ref-4">Reynolds, Laria, and Kyle McDonell (2021). "Prompt Programming for Large Language Models: Beyond the Few-Shot Paradigm." <em>arXiv</em>. <a href="https://arxiv.org/abs/2102.07350">https://arxiv.org/abs/2102.07350</a></li>
  <li id="ref-5">Lütke, Tobi (2025). Post on context engineering. <em>X</em>. <a href="https://x.com/tobi/status/1935533422589399127">https://x.com/tobi/status/1935533422589399127</a></li>
  <li id="ref-6">FutureAGI (2026). "What is Evals Engineering? The Discipline Behind Production LLMs in 2026." <em>FutureAGI Blog</em>. <a href="https://futureagi.com/blog/what-is-evals-engineering-2026/">https://futureagi.com/blog/what-is-evals-engineering-2026/</a></li>
</ol>

---

## Changelog

**2026-08-12** Initial publish.  
