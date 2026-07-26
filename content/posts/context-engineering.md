---
title: "Context Engineering"
date: 2026-07-20
publishdate: 2026-07-20
lastmod: 2026-07-20
summary: "Tobi Lütke coined 'context engineering' in June 2025, and Karpathy's endorsement made it stick. A model's context window is scarce, and what you leave out matters as much as what you put in."
tags: ["ai", "agents", "context"]
image: /images/context-engineering.jpg
draft: true
---

![Tobi Lütke coined 'context engineering' in June 2025, and Karpathy's endorsement made it stick. A model's context window is scarce, and what you leave out matters as much as what you put in.](/images/context-engineering.jpg)
*Mise en place at a restaurant's hot station. Photo: [Charles Haynes (2007)](https://commons.wikimedia.org/wiki/File:Mise_en_place_for_hot_station.jpg). CC BY-SA 2.0.*

## Context Engineering

An agent has the whole codebase indexed, every tool it might need wired up, and the full conversation history sitting in its context window. Ask it something that depends on a detail from forty messages back, and it answers as if that message never happened. Nothing crashed. The context window was never as full of usable information as it looked.

More tokens in context doesn't mean more signal reaching the model. Anthropic's own engineering team measured this directly. As the number of tokens in the context window increases, the model's ability to accurately recall information from that context decreases{{< cite 1 "Anthropic (2025). Effective Context Engineering for AI Agents. Anthropic Engineering Blog." >}}.

## What Is Context Engineering?

On June 19, 2025, Shopify CEO Tobi Lütke posted that he liked the term "context engineering" better than "prompt engineering." "It describes the core skill better, the art of providing all the context for the task to be plausibly solvable by the LLM{{< cite 2 "Lütke, Tobi (2025). Post on context engineering. X." >}}." Six days later, Andrej Karpathy amplified it with a sharper distinction. Prompts are the short task descriptions you type into a chat box. Inside a real agent, "context engineering is the delicate art and science of filling the context window with just the right information for the next step{{< cite 3 "Karpathy, Andrej (2025). Post on context engineering. X." >}}."

The shift in framing matters. Prompt engineering is wordsmithing instructions. Context engineering covers everything else the model sees at inference time too. Karpathy's own list is long, task descriptions and explanations, few-shot examples, retrieved documents, tool definitions, and conversation state and history{{< cite 3 "Karpathy, Andrej (2025). Post on context engineering. X." >}}. A perfectly worded prompt sitting on top of a context window stuffed with irrelevant files still produces a distracted model.

## Why More Context Backfires

Transformer attention compares every token against every other token. Add tokens and that comparison grows faster than the useful information does, a problem researchers call context rot{{< cite 1 "Anthropic (2025). Effective Context Engineering for AI Agents. Anthropic Engineering Blog." >}}. Position compounds the problem. Models attend most reliably to the start and end of a long context, and information buried in the middle gets systematically underweighted regardless of how important it actually is{{< cite 4 "Liu, Nelson F., et al. (2023). Lost in the Middle: How Language Models Use Long Contexts. TACL 12." >}}.

Anthropic's own guidance for handling this is blunt. Find the smallest set of high-signal tokens that maximize the likelihood of your desired outcome{{< cite 1 "Anthropic (2025). Effective Context Engineering for AI Agents. Anthropic Engineering Blog." >}}. The smallest set that still works beats the largest set that might.

## Techniques That Hold Up

LangChain's engineering team sorts these same moves into four verbs, write context outside the window, select only what's needed back in, compress what's already there, and isolate a subtask into its own window{{< cite 5 "LangChain (2025). Context Engineering for Agents. LangChain Blog." >}}. Different labels, same four moves.

**Compaction.** As a conversation nears its context limit, summarize it and reinitiate with the summary plus recent messages, instead of letting the whole history ride along untouched{{< cite 1 "Anthropic (2025). Effective Context Engineering for AI Agents. Anthropic Engineering Blog." >}}. Claude Code triggers this automatically at 95 percent of its context window{{< cite 5 "LangChain (2025). Context Engineering for Agents. LangChain Blog." >}}.

**Structured note-taking.** An agent that writes its own notes to a file outside the context window can pick up a long task later without needing every prior step replayed into memory.

**Just-in-time retrieval.** Fetch a file or a record when it's needed instead of preloading everything that might be needed. A file path is cheap. The file's full contents, multiplied across a long session, are not.

**Sub-agents.** Hand a narrow task to a separate agent with its own clean context window, and have it report back a condensed result instead of a full transcript. The parent agent's context stays small no matter how much work the sub-agent did to get there. The tradeoff is real. Multi-agent systems can burn through up to 15 times more tokens than a single chat session to get that clean result back{{< cite 1 "Anthropic (2025). Effective Context Engineering for AI Agents. Anthropic Engineering Blog." >}}.

## Common Mistakes

Drew Breunig names four specific ways context goes wrong{{< cite 6 "Breunig, Drew (2025). How Long Contexts Fail. dbreunig.com." >}}. Poisoning is a hallucination that gets repeated back as fact. Distraction happens when a long context pulls focus away from what the model learned in training. Confusion sets in when superfluous content degrades the response, and clash means new information conflicting with something already sitting in context.

**Preloading instead of retrieving.** Dumping an entire codebase or document set into context "just in case" burns the budget on things that never get read.

**Never compacting a long-running agent loop.** Context that was relevant at message five is often dead weight by message fifty, and it's still sitting there, still competing for attention.

**Wiring up every tool available instead of the ones the task needs.** Each tool definition costs tokens before a single one gets called.

**Treating the context window as a quota to fill.** A full context window is usually a sign nobody decided what to leave out.

## Put It Into Practice

Look at what's actually sitting in your agent's context right now, instead of what you assume is there. Every system instruction, every tool definition, every retrieved document. Ask whether each one earns its place for the task in front of the model, instead of some task it might face later.

Swap preloading for retrieval wherever you can. Fetch the file when the agent needs it instead of loading it at the start of every run, and watch your average context size drop along with your token bill.

The smallest set of high-signal tokens beats the largest set of plausible ones, every time it's been measured. Build for that.

## Go Further

**The mechanism behind context rot.** Liu and colleagues' original study is worth reading directly for how they measured the U-shaped recall curve, strong at the edges of context, weak in the middle{{< cite 4 "Liu, Nelson F., et al. (2023). Lost in the Middle: How Language Models Use Long Contexts. TACL 12." >}}.

**How far sub-agent isolation goes.** Anthropic's engineering post covers multi-agent orchestration patterns beyond what fits here, including how much a parent agent should trust a sub-agent's summary versus verifying it directly{{< cite 1 "Anthropic (2025). Effective Context Engineering for AI Agents. Anthropic Engineering Blog." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Anthropic (2025). "Effective Context Engineering for AI Agents." <em>Anthropic Engineering Blog</em>. <a href="https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents">https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents</a></li>
  <li id="ref-2">Lütke, Tobi (2025). Post on context engineering. X. <a href="https://x.com/tobi/status/1935533422589399127">https://x.com/tobi/status/1935533422589399127</a></li>
  <li id="ref-3">Karpathy, Andrej (2025). Post on context engineering. X. <a href="https://x.com/karpathy/status/1937902205765607626">https://x.com/karpathy/status/1937902205765607626</a></li>
  <li id="ref-4">Liu, Nelson F., et al. (2023). "Lost in the Middle: How Language Models Use Long Contexts." <em>Transactions of the Association for Computational Linguistics</em>, 12. <a href="https://arxiv.org/abs/2307.03172">https://arxiv.org/abs/2307.03172</a></li>
  <li id="ref-5">LangChain (2025). "Context Engineering for Agents." <em>LangChain Blog</em>. <a href="https://www.langchain.com/blog/context-engineering-for-agents">https://www.langchain.com/blog/context-engineering-for-agents</a></li>
  <li id="ref-6">Breunig, Drew (2025). "How Long Contexts Fail." <em>dbreunig.com</em>. <a href="https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html">https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html</a></li>
</ol>

---

## Outtakes

**Lütke wasn't naming a new idea, just a better one.** People had been describing pieces of this practice for months. His tweet is what made the label stick enough for Gartner to declare prompt engineering out and context engineering in later that year ([Lütke, 2025](https://x.com/tobi/status/1935533422589399127)).

**Karpathy's other framing was a computer metaphor.** He described the LLM as a kind of processor and its context window as RAM, a way of thinking about context engineering as memory management rather than writing ([Karpathy, 2025](https://x.com/karpathy/status/1937902205765607626)).

**Even prompt engineering's defender came around.** Simon Willison had previously written in defense of prompt engineering as a real discipline worth taking seriously. He still concluded that "context engineering" was the more accurate name for what practitioners actually do ([Willison, 2025](https://simonwillison.net/2025/Jun/27/context-engineering/)).

**A pelican costume with an uninvited sign.** Willison asked ChatGPT to put his dog in a pelican costume, and it added a "Half Moon Bay" sign to the background, unprompted, because he'd once mentioned living there ([Willison, 2025](https://simonwillison.net/2025/May/21/chatgpt-new-memory/)).

---

## Changelog

**2026-07-20** Initial release.  
