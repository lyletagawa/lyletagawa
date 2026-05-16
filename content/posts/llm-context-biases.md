---
title: "LLM Context Biases"
date: 2026-05-07
publishdate: 2026-05-07
lastmod: 2026-05-16
summary: "Primacy bias, recency bias, sycophancy, and anchoring are predictable distortions in how LLMs weight information. Understanding them changes how you prompt, evaluate, and trust model outputs."
tags: ["ai", "cognitive", "leadership"]
image: /images/illusion.png
draft: false
---

## LLM Context Biases

You post a 20-page architecture document into your LLM and ask it to identify the three riskiest assumptions. The model returns a confident answer. It got the first and third risks right, but not the one buried on page 11, the one that matters most.

This isn't a hallucination. The model read all 20 pages. The problem is that it didn't weight all 20 pages equally.

LLMs process context with systematic biases based on position, order, and stated preferences. Understanding them is the difference between using LLMs effectively and trusting answers that were shaped more by where you put information than what it said.

## What Are Context Biases?

Context biases are predictable distortions in how language models weight and process information. They are structural tendencies, not random errors, emerging from how attention mechanisms are trained.

Three are most consequential for practitioners: primacy bias, recency bias, and sycophancy. A fourth, anchoring, compounds all three. None of them are bugs in a particular product. They appear across models and persist even as model quality improves.

## Primacy and Recency Bias

Liu et al. (2023) documented the core finding in "Lost in the Middle": when relevant information is located at the beginning or end of a long context, model performance is significantly higher than when it sits in the middle. Tested across multiple models on multi-document question answering tasks, the effect was consistent. Models showed a U-shaped performance curve: strong at the start, strong at the end, degraded in the middle.

The middle of a long prompt is where information goes to be ignored.

For technical work, this has direct consequences. A 50-page policy document fed to an LLM will have its middle sections under-weighted regardless of their importance (Liu et al., 2023).

## Sycophancy

Sycophancy is the tendency of LLMs to produce responses that align with what the user appears to want, rather than what is accurate (Malmqvist, 2024). When a user pushes back on a model's answer, the model frequently reverses its position even without new evidence. When a user frames a question to suggest a preferred answer, the model gravitates toward confirming it.

This creates a specific failure mode for technical decision-making. You ask an LLM to evaluate two architectural approaches. You mention that your team prefers approach A. The model finds more merit in approach A. The evaluation wasn't neutral. It was shaped by your stated preference before the analysis began.

Sycophancy is reinforced through reinforcement learning from human feedback (RLHF), where models learn that agreement produces higher human ratings than accurate but unwelcome responses (Malmqvist, 2024). It is not a surface-level quirk. It is baked into how models are trained to please.

## Anchoring

Anchoring bias occurs when initial information disproportionately shapes subsequent judgment. Lou and Sun (2024) found that LLMs are notably sensitive to anchored prompts, with 22% to 61% of questions affected depending on the model. Most tested LLMs showed anchoring effects equivalent to or stronger than those observed in humans.

In practice: you ask an LLM to estimate the timeline for a migration. You mention that your last migration took six months. The model's estimate will be pulled toward six months, regardless of the actual scope difference. The initial number anchors the output.

Chain-of-Thought prompting and reflection prompts reduce anchoring but do not eliminate it. Lou and Sun (2024) found that comprehensive multi-angle information gathering was the most effective mitigation.

## Common Mistakes

**Trusting position over content.** Putting the most important context at the top of a prompt and assuming the model will find what matters elsewhere. Middle sections are systematically under-weighted, and adding more context can actively degrade performance by pushing critical information further in (Liu et al., 2023).

**Using pushback to get better answers.** If you disagree with an LLM's response and push back without new evidence, the model will often reverse its position. This feels like refining the answer. It is sycophancy. You are not getting a better analysis. You are getting agreement.

**Treating LLM evaluation as neutral.** Asking an LLM to compare options while including your team's stated preference contaminates the evaluation. The model will find evidence for what you have already indicated you want.

**Anchoring with existing estimates.** Mentioning a prior number, deadline, or precedent before asking for an estimate pulls the model's output toward that anchor. If you want an independent estimate, ask for it before providing your reference point.

## Put It Into Practice

These biases do not make LLMs unreliable. They make them predictably unreliable in specific ways. Knowing the pattern is most of the fix.

For long-document analysis, put the most critical information at the beginning or end of the context. Explicitly surface content in the middle: "The section on page 11 is the most important." For evaluation tasks, strip your stated preferences from the prompt entirely. Run the analysis once without your view, then share it afterward. For independent estimates, ask first and anchor second.

Pick one task you regularly use an LLM for. Before your next run, note everything in the prompt that precedes your actual question. That context shaped the answer. Adjust the order and see what changes.

---

## References

Liu, Nelson F., et al. (2023). "Lost in the Middle: How Language Models Use Long Contexts." *Transactions of the Association for Computational Linguistics*, 12. [https://arxiv.org/abs/2307.03172](https://arxiv.org/abs/2307.03172)

Lou, Jiaxu, and Yifan Sun (2024). "Anchoring Bias in Large Language Models: An Experimental Study." arXiv preprint arXiv:2412.06593. [https://arxiv.org/abs/2412.06593](https://arxiv.org/abs/2412.06593)

Malmqvist, Lars (2024). "Sycophancy in Large Language Models: Causes and Mitigations." arXiv preprint arXiv:2411.15287. [https://arxiv.org/abs/2411.15287](https://arxiv.org/abs/2411.15287)

Thumbnail: Müller-Lyer illusion. Franz Carl Müller-Lyer and Franz Brentano (1892). From *Zeitschrift für Psychologie und Physiologie der Sinnesorgane*, 3. Public domain, via [Picryl](https://picryl.com/media/muller-lyer-illusion-franz-bretano-1892-a2649a).

## Outtakes

**Bias.** A lawn bowling ball weighted on one side so that it curves predictably from a straight path (Harper, n.d.).

**Context.** From the Latin "contexere," meaning to weave together. Same root as text and textile (Harper, n.d.).

**The spinning wheel.** Kahneman and Tversky's original anchoring experiment had subjects watch a wheel spin to either 10 or 65, then estimate what percentage of African countries are in the UN. The group that saw 65 guessed significantly higher. A randomly observed number anchored their estimates. ([Kahneman, 2011](https://us.macmillan.com/books/9780374533557/thinkingfastandslow/))

**Jury research.** Studies on jury behavior show that the first and last witnesses in a trial have disproportionate influence on verdicts. Same U-shaped curve. Same mechanism. ([source](https://www.researchgate.net/publication/385853184_THE_PSYCHOLOGY_OF_JURY_DECISION-MAKING_IN_CRIMINAL_TRIALS))

---

## Changelog

**2026-05-16** Replaced Etymology with Outtakes. Added spinning wheel and jury research to Outtakes.  
**2026-05-11** Added etymology section.  
**2026-05-07** Initial publication.
