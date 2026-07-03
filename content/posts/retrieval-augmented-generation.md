---
title: "Retrieval-Augmented Generation"
date: 2026-06-27
publishdate: 2026-06-27
lastmod: 2026-06-27
summary: "RAG gives a language model an open book at answer time. It fixes stale knowledge and cuts hallucination, but only as far as the retriever reaches."
tags: ["ai", "architecture", "search"]
image: /images/retrieval-augmented-generation.png
draft: true
---

![RAG gives a language model an open book at answer time. It fixes stale knowledge and cuts hallucination, but only as far as the retriever reaches.](/images/retrieval-augmented-generation.png)
*"Reading room, New York Public Library." Photo: [Diliff (2015)](https://commons.wikimedia.org/wiki/File:New_York_Public_Library_Rose_Main_Reading_Room_Jan_2017.jpg). CC BY-SA 4.0.*

## Retrieval-Augmented Generation

You ask a chatbot about your company's refund policy. It answers in confident, fluent prose. The policy it describes changed eight months ago, and the version it quoted never existed. The model didn't lie. It did exactly what it was built to do, which is predict plausible text from what it memorized during training.

The knowledge baked into a model's weights is frozen at training time and impossible to update without retraining. It also blurs together. Pre-training compresses trillions of tokens into a fixed set of weights, and specific facts come back fuzzy{{< cite 1 "Lewis, Patrick, et al. (2020). Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks. NeurIPS 33." >}}.

Retrieval-augmented generation (RAG) is the workaround. Hand the model an open book at answer time.

## What Is RAG?

RAG combines two kinds of memory. The first is parametric, the knowledge stored in the model's weights. The second is non-parametric, an external store the model can search on demand. Patrick Lewis and colleagues at Facebook AI introduced the architecture in 2020, pairing a sequence-to-sequence model with a dense vector index of Wikipedia{{< cite 1 "Lewis, Patrick, et al. (2020). Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks. NeurIPS 33." >}}.

The flow is simple. Take the user's question, search the store for relevant passages, paste the best ones into the prompt, and let the model generate an answer grounded in that text. The original system split a December 2018 Wikipedia dump into roughly 21 million hundred-word passages and pulled the top few for each query using Dense Passage Retrieval{{< cite 1 "Lewis, Patrick, et al. (2020). Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks. NeurIPS 33." >}}.

The payoff was immediate. RAG set the state of the art on three open-domain question answering tasks and produced more specific and factual language than a model relying on weights alone{{< cite 1 "Lewis, Patrick, et al. (2020). Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks. NeurIPS 33." >}}. Update the knowledge by swapping the index. No retraining required.

## How Retrieval Actually Works

The magic word is "relevant," and it hides most of the difficulty. The system can't grep for keywords. A user asking about "time off" should match a document titled "vacation accrual," and a keyword search never will.

So RAG leans on embeddings. An embedding model turns each passage into a vector, a list of numbers that captures meaning rather than spelling. Passages about similar things land near each other in that vector space, even when they share no words. At query time the question becomes a vector too, and the system finds the passages whose vectors sit closest{{< cite 2 "Karpukhin, Vladimir, et al. (2020). Dense Passage Retrieval for Open-Domain Question Answering. EMNLP." >}}.

Closeness is usually cosine similarity, the angle between two vectors. Small angle, similar meaning. Searching millions of vectors for the nearest few sounds expensive, and done naively it is. Approximate nearest neighbor indexes make it fast by trading a sliver of accuracy for a large speedup, which is the same bargain every other sketch structure makes.

## Why Chunking Decides Everything

Before any of that, you have to cut your documents into pieces. This step looks like janitorial work and quietly determines whether the whole system works.

Chunk too large and each passage covers several topics. Its embedding becomes an average of all of them, sharp on nothing, and retrieval gets vague. Chunk too small and you sever the context a fact depends on. A sentence reading "It expires after 90 days" is useless when the chunk boundary cut off what "it" was. The original RAG used flat hundred-word passages, and most production systems still tune chunk size and overlap by hand for their corpus{{< cite 1 "Lewis, Patrick, et al. (2020). Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks. NeurIPS 33." >}}.

There's no universal right answer. A legal contract chunks differently than a chat log. The boring part is where the accuracy lives.

## Where RAG Breaks Down

RAG fails in ways that look like the model being wrong, when the real culprit is the plumbing around it. A study of production systems catalogued seven distinct failure points, most of them in retrieval rather than generation{{< cite 3 "Barnett, Scott, et al. (2024). Seven Failure Points When Engineering a Retrieval Augmented Generation System. CAIN." >}}.

**The right passage is never retrieved.** If the correct document sits below the cutoff, or the embedding fails to capture the question's intent, the model never sees it. It then answers from its weights, confidently, with no signal that anything is missing{{< cite 3 "Barnett, Scott, et al. (2024). Seven Failure Points When Engineering a Retrieval Augmented Generation System. CAIN." >}}.

**The right passage is buried in the middle.** Even when retrieval works, position matters. Language models attend well to the start and end of a long context and sag in the middle, a U-shaped curve that can erase a 20-percent accuracy gap depending on where the answer sits{{< cite 4 "Liu, Nelson F., et al. (2023). Lost in the Middle: How Language Models Use Long Contexts. Transactions of the Association for Computational Linguistics, 12." >}}.

**More context makes it worse.** Stuffing the prompt with the top 20 passages feels safe and backfires. Irrelevant passages dilute the signal, and the noise can drop performance below what the model scored with no retrieval at all{{< cite 4 "Liu, Nelson F., et al. (2023). Lost in the Middle: How Language Models Use Long Contexts. Transactions of the Association for Computational Linguistics, 12." >}}.

**Retrieval succeeds and the model ignores it.** The grounding passage is right there in the prompt, and the model still answers from its priors{{< cite 3 "Barnett, Scott, et al. (2024). Seven Failure Points When Engineering a Retrieval Augmented Generation System. CAIN." >}}. Grounding is an input, not a guarantee.

## Put It Into Practice

Treat retrieval as the product, not the model. When a RAG system gives a wrong answer, the first question is whether the right passage was even retrieved. Log what gets pulled for each query and read it. Most of what looks like a reasoning failure is a passage that never made it into the prompt.

Start by fixing the unglamorous parts. Tune chunk size against your actual documents, rerank the retrieved passages so the strongest sits first, and resist the urge to cram the context window full. Put the best passage where the model reads best, at the top or the bottom.

Pick one question your system answers badly. Pull the passages it retrieved and check whether the answer was findable in them at all. You'll learn more from that one trace than from any prompt you rewrite.

## Go Further

**Reranking with cross-encoders.** A first-stage retriever optimizes for speed, so it returns a rough shortlist. A second-stage cross-encoder reads each query and passage together and reorders them, closing the gap between "retrieved" and "retrieved in a useful order"{{< cite 5 "Nogueira, Rodrigo, and Kyunghyun Cho (2019). Passage Re-ranking with BERT. arXiv." >}}. Pair it with hybrid search, dense embeddings plus keyword matching, to catch what each retriever misses alone{{< cite 2 "Karpukhin, Vladimir, et al. (2020). Dense Passage Retrieval for Open-Domain Question Answering. EMNLP." >}}.

**RAG versus long context.** As context windows grow to millions of tokens, you can sometimes skip retrieval and paste in everything. A strong retriever feeding a long-context model often beats either approach alone on both cost and accuracy{{< cite 6 "Xu, Peng, et al. (2023). Retrieval meets Long Context Large Language Models. arXiv." >}}.

**Letting the model retrieve on its own.** Basic RAG retrieves once, up front, whether the question needs it or not. Newer systems let the model decide when to search and critique what comes back, looping through several retrievals before it answers{{< cite 7 "Asai, Akari, et al. (2023). Self-RAG: Learning to Retrieve, Generate, and Critique through Self-Reflection. arXiv." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Lewis, Patrick, et al. (2020). "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks." <em>Advances in Neural Information Processing Systems</em>, 33. <a href="https://arxiv.org/abs/2005.11401">https://arxiv.org/abs/2005.11401</a></li>
  <li id="ref-2">Karpukhin, Vladimir, et al. (2020). "Dense Passage Retrieval for Open-Domain Question Answering." <em>EMNLP 2020</em>. <a href="https://arxiv.org/abs/2004.04906">https://arxiv.org/abs/2004.04906</a></li>
  <li id="ref-3">Barnett, Scott, et al. (2024). "Seven Failure Points When Engineering a Retrieval Augmented Generation System." <em>CAIN 2024</em>. <a href="https://arxiv.org/abs/2401.05856">https://arxiv.org/abs/2401.05856</a></li>
  <li id="ref-4">Liu, Nelson F., et al. (2023). "Lost in the Middle: How Language Models Use Long Contexts." <em>Transactions of the Association for Computational Linguistics</em>, 12. <a href="https://arxiv.org/abs/2307.03172">https://arxiv.org/abs/2307.03172</a></li>
  <li id="ref-5">Nogueira, Rodrigo, and Kyunghyun Cho (2019). "Passage Re-ranking with BERT." <em>arXiv</em>. <a href="https://arxiv.org/abs/1901.04085">https://arxiv.org/abs/1901.04085</a></li>
  <li id="ref-6">Xu, Peng, et al. (2023). "Retrieval meets Long Context Large Language Models." <em>arXiv</em>. <a href="https://arxiv.org/abs/2310.03025">https://arxiv.org/abs/2310.03025</a></li>
  <li id="ref-7">Asai, Akari, et al. (2023). "Self-RAG: Learning to Retrieve, Generate, and Critique through Self-Reflection." <em>arXiv</em>. <a href="https://arxiv.org/abs/2310.11511">https://arxiv.org/abs/2310.11511</a></li>
</ol>

---

## Outtakes

**The name almost didn't stick.** The 2020 paper introduced two variants, RAG-Sequence and RAG-Token, differing in whether the model commits to one retrieved passage for the whole answer or can switch passages per word (Lewis, 2020). Nobody remembers the distinction. Everybody remembers the acronym.

**Parametric is just memory you can't edit.** A model's weights are non-volatile storage with no write API after training. RAG bolts on the missing filesystem. The framing of "two memories" comes straight from the original paper, and it's the cleanest way to explain why the technique exists.

**Retrieval predates the transformer.** Pulling relevant documents to answer a question is information retrieval, a field older than most programming languages. RAG's contribution was wiring a half-century-old idea to a model that could write fluent prose about whatever it found.

---

## Changelog

**2026-06-27** Initial release.  
**2026-06-27** Defined the RAG acronym on first use and cited the fourth failure mode. Tightening pass.  
**2026-06-27** Rebuilt Go Further with three advanced topics and fresh sources: cross-encoder reranking, retrieval versus long context, and self-directed retrieval.  
