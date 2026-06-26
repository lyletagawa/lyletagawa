---
title: "How LLMs Work"
date: 2026-06-14
publishdate: 2026-06-14
lastmod: 2026-06-26
summary: "Working notes on how LLMs train and generate text. Probably mostly right, certainly incomplete."
tags: ["ai", "architecture", "learning"]
image: /images/how-llms-work.jpg
draft: false
---

![Working notes on how LLMs train and generate text. Probably mostly right, certainly incomplete.](/images/how-llms-work.jpg)
*"Library card catalog." Photo: [AKT.UZ (2018)](https://www.flickr.com/photos/akt_uz/45188906785/). Public domain.*

## How LLMs Work

Someone asks an LLM how many r's are in "strawberry" and the model says two{{< cite 1 "Hacker News (2024). All 3 models you ranked cannot get how many r's are in strawberry correct. Hacker News." >}}. You might assume the model can't count. The real problem is that the model never saw the word "strawberry" in the first place.

Depending on the model, "strawberry" might be tokenized into ["str", "aw", "berry"] or ["straw", "berry"]. The letters never existed as separate inputs. The model received a different task than the one you described{{< cite 2 "0xkato (2026). How LLMs Actually Work. 0xkato.xyz." >}}.

Most model failures make more sense once you know what's happening inside.

## Your Prompt Becomes Numbers

Before the model processes a single word, your text converts to integers. A tokenizer breaks input into subword pieces (e.g., “straw”, “berry”) and maps each piece to an ID in a vocabulary of tens of thousands of entries. GPT-2 uses 50,257 tokens. Capitalization and spacing matter, so "The," "the," and " the" are three distinct entries{{< cite 3 "Grinberg, Miguel (2024). How LLMs Work, Explained Without Math. Miguel Grinberg Blog." >}}. Different model families use different tokenizers. GPT models use Byte Pair Encoding. LLaMA-family models use SentencePiece. A prompt that works well in one model may behave differently in another.

Each token ID indexes into an embedding matrix and retrieves a learned vector, typically 4,096 numbers long for a 7B model{{< cite 4 "Touvron, Hugo, et al. (2023). Llama 2: Open Foundation and Fine-Tuned Chat Models. arXiv:2307.09288." >}}. Semantically similar tokens develop similar vectors through training, but not by design. It emerges from predicting text at scale.

A simpler approach to next-token prediction would record, for every possible token sequence, which token followed it. But representing all possible 1,024-token contexts over a 50,000-token vocabulary would require a table with roughly 10^4812 rows (log₁₀(50000^1024) = 1024 × log₁₀(50000) ≈ 4812). Neural networks learn a function that approximates those probabilities without enumerating every possible context{{< cite 3 "Grinberg, Miguel (2024). How LLMs Work, Explained Without Math. Miguel Grinberg Blog." >}}.

## How the Model Learned

Pre-training is the first stage. Feed the model roughly 2 trillion tokens of text, ask it to predict the next word at each step, and update the weights when it's wrong. Training Llama 2 70B required 1.7 million GPU-hours, about 36 days on 2,000 A100s{{< cite 4 "Touvron, Hugo, et al. (2023). Llama 2: Open Foundation and Fine-Tuned Chat Models. arXiv:2307.09288." >}}. The result is lossy compression, not exact recall.

Not all models train the same way. Encoder-style models like Bidirectional Encoder Representations from Transformers (BERT) use masked language modeling. Hide a token, predict it from the context on both sides. That bidirectionality makes them better at tasks requiring the full picture. Decoder-style models like GPT train autoregressively. Each token is predicted from only the tokens before it, then fed back as input for the next. They generate text sequentially. Most frontier chat models are decoder-only{{< cite 5 "Raschka, Sebastian (2023). Understanding Large Language Models. Ahead of AI." >}}.

Training data quantity matters as much as model size. Chinchilla, at 70 billion parameters, outperformed GPT-3 at 175 billion by training on 4.7x more tokens{{< cite 6 "Hoffmann, Jordan, et al. (2022). Training Compute-Optimal Large Language Models. NeurIPS 35." >}}. Bigger models trained on less data lose to smaller models trained longer.

Fine-tuning is the second stage. Around 13,000 curated prompt-response examples steer it toward being useful{{< cite 7 "Ouyang, Long, et al. (2022). Training Language Models to Follow Instructions with Human Feedback. NeurIPS 35." >}}. A third stage, reinforcement learning from human feedback (RLHF), refines further using human preferences about which responses are better{{< cite 7 "Ouyang, Long, et al. (2022). Training Language Models to Follow Instructions with Human Feedback. NeurIPS 35." >}}. Pre-training is where knowledge comes from. Fine-tuning and RLHF are where the personality comes from.

## Attention Is the Core Operation

Every transformer block runs attention. Each token generates three vectors. The Query vector captures what this token is looking for. The Key and Value vectors are the index and the content. Queries compare against keys via dot products. Higher scores mean closer matches, and softmax converts those scores into weights. Each token then takes a weighted average of value vectors across the sequence{{< cite 8 "Vaswani, Ashish, et al. (2017). Attention Is All You Need. NeurIPS 30." >}}. The result is context-dependent meaning. "Bank" after "river" develops a different representation than "bank" after "loan."

Decoder-only models use causal masking. A token can only attend to earlier tokens, making generation sequential. The model predicts a token, appends it, and repeats. The key-value (KV) cache saves previously computed key and value vectors to avoid recomputing them on every new token, which is what makes generation fast in practice{{< cite 9 "Alammar, Jay (2019). The Illustrated GPT-2. jalammar.github.io." >}}.

Each transformer block adds its outputs to the incoming token vector rather than replacing it. These residual connections let the model stack dozens of layers. Without them, the training signal fades before it reaches the early layers and the model stops improving{{< cite 10 "He, Kaiming, et al. (2016). Deep Residual Learning for Image Recognition. CVPR." >}}. Layer normalization runs before each sub-block in most modern models, keeping activations from growing too large or collapsing{{< cite 9 "Alammar, Jay (2019). The Illustrated GPT-2. jalammar.github.io." >}}. The token vector accumulates each block's contribution as it passes through the stack.

## Where Facts Live

After attention, each token passes through a feed-forward network (FFN) independently. The FFN expands the vector to a larger dimension, applies a non-linearity, then compresses it back down. These layers hold most of the model's parameters, and specific factual associations are stored in specific FFN weights. Knowledge like "The Eiffel Tower is in [Paris]" can be traced to identifiable FFN weight patterns, enabling targeted edits that change what the model "believes" about specific facts{{< cite 11 "Meng, Kevin, et al. (2022). Locating and Editing Factual Associations in GPT. NeurIPS 35." >}}. The knowledge is in the weights. New facts require new training.

## Where This Breaks Down

**The middle of your context gets underweighted.** Models attend most reliably to information at the beginning and end of long contexts. Information buried in the middle gets systematically underweighted regardless of importance. Adding more context can actively degrade performance by pushing critical information further from where attention concentrates{{< cite 12 "Liu, Nelson F., et al. (2023). Lost in the Middle: How Language Models Use Long Contexts. TACL 12." >}}.

**Temperature shapes answers, not just randomness.** After the final layer, each token's output vector multiplies against the embedding matrix to produce one score per vocabulary entry. Temperature scales those scores before softmax. Low temperature makes the model pick the highest-probability token almost every time. High temperature flattens the distribution and pulls in lower-probability options. Top-k sampling is an alternative that restricts the pool to the k highest-scoring candidates before sampling{{< cite 9 "Alammar, Jay (2019). The Illustrated GPT-2. jalammar.github.io." >}}. The same prompt with different sampling settings can produce radically different outputs.

**The model can't slow down to think.** Generation is word-by-word sampling. There's no internal deliberation loop before each token. Karpathy calls this System 1 thinking. Fast, instinctive, pattern-matched{{< cite 13 "Karpathy, Andrej (2023). Intro to Large Language Models. YouTube." >}}. Chain-of-Thought prompting forces reasoning steps before an answer, using the context window as working memory. The thinking happens in the tokens, not before them.

**Architecture isn't weights.** GPT, Gemini, and LLaMA share the transformer skeleton. What differs is training data, scale, configuration, and post-training. When two similar architectures diverge sharply on the same task, the architecture usually isn't the explanation. The weights are{{< cite 5 "Raschka, Sebastian (2023). Understanding Large Language Models. Ahead of AI." >}}.

## Conclusion

The transformer architecture described in 2017 is still, in substance, what runs today's frontier models{{< cite 8 "Vaswani, Ashish, et al. (2017). Attention Is All You Need. NeurIPS 30." >}}. Nine years of refinement to position encoding, activation functions, attention variants, and scaling built on that base without replacing it{{< cite 5 "Raschka, Sebastian (2023). Understanding Large Language Models. Ahead of AI." >}}. The core mechanic is unchanged. Tokenize, embed, attend, feed-forward, predict.

The model doesn't understand your prompt. It generates statistically plausible continuations by pattern-matching against compressed training data{{< cite 3 "Grinberg, Miguel (2024). How LLMs Work, Explained Without Math. Miguel Grinberg Blog." >}}. Useful output doesn't require comprehension. Knowing that changes how you verify what comes back.

When something unexpected happens, check how your prompt tokenizes. Most providers offer free tools for this. Move critical information to the start or end of long contexts. Adjust temperature down if outputs are erratic, or up if they're repetitive. All three are adjustable without touching the model.

## What This Doesn't Cover

**Attention variants.** The dot-product attention described here is the baseline. Modern models layer in query grouping, compressed attention, sparse patterns, and sliding windows to manage cost at long contexts. PyTorch's FlexAttention provides a composable programming model for generating optimized kernels across these variants without prohibitive performance penalties{{< cite 14 "He, Driss Guessous, et al. (2024). Flex Attention: A Programming Model for Generating Optimized Attention Kernels. arXiv:2412.05496." >}}.

**Mixture of Experts.** Dense models activate all parameters for every token. Mixture-of-Experts (MoE) models route each token to a subset of "expert" feed-forward layers, decoupling parameter count from per-token compute. Mixtral 8x7B, with 12.9B active parameters per token, outperformed a 70B dense model on most benchmarks{{< cite 15 "Jiang, Albert Q., et al. (2024). Mixtral of Experts. arXiv:2401.04088." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Hacker News (2024). "All 3 models you ranked cannot get 'how many r's are in strawberry?' correct." <a href="https://news.ycombinator.com/item?id=41058318">https://news.ycombinator.com/item?id=41058318</a></li>
  <li id="ref-2">0xkato (2026). "How LLMs Actually Work." 0xkato.xyz. <a href="https://www.0xkato.xyz/how-llms-actually-work/">https://www.0xkato.xyz/how-llms-actually-work/</a></li>
  <li id="ref-3">Grinberg, Miguel (2024). "How LLMs Work, Explained Without Math." <em>Miguel Grinberg Blog</em>. <a href="https://blog.miguelgrinberg.com/post/how-llms-work-explained-without-math">https://blog.miguelgrinberg.com/post/how-llms-work-explained-without-math</a></li>
  <li id="ref-4">Touvron, Hugo, et al. (2023). "Llama 2: Open Foundation and Fine-Tuned Chat Models." arXiv:2307.09288. <a href="https://arxiv.org/abs/2307.09288">https://arxiv.org/abs/2307.09288</a></li>
  <li id="ref-5">Raschka, Sebastian (2023). "Understanding Large Language Models." <em>Ahead of AI</em>. <a href="https://magazine.sebastianraschka.com/p/understanding-large-language-models">https://magazine.sebastianraschka.com/p/understanding-large-language-models</a></li>
  <li id="ref-6">Hoffmann, Jordan, et al. (2022). "Training Compute-Optimal Large Language Models." <em>Advances in Neural Information Processing Systems</em>, 35. <a href="https://arxiv.org/abs/2203.15556">https://arxiv.org/abs/2203.15556</a></li>
  <li id="ref-7">Ouyang, Long, et al. (2022). "Training Language Models to Follow Instructions with Human Feedback." <em>Advances in Neural Information Processing Systems</em>, 35. <a href="https://arxiv.org/abs/2203.02155">https://arxiv.org/abs/2203.02155</a></li>
  <li id="ref-8">Vaswani, Ashish, et al. (2017). "Attention Is All You Need." <em>Advances in Neural Information Processing Systems</em>, 30. <a href="https://arxiv.org/abs/1706.03762">https://arxiv.org/abs/1706.03762</a></li>
  <li id="ref-9">Alammar, Jay (2019). "The Illustrated GPT-2 (Language Models)." <a href="https://jalammar.github.io/illustrated-gpt2/">https://jalammar.github.io/illustrated-gpt2/</a></li>
  <li id="ref-10">He, Kaiming, et al. (2016). "Deep Residual Learning for Image Recognition." <em>Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition</em>. <a href="https://arxiv.org/abs/1512.03385">https://arxiv.org/abs/1512.03385</a></li>
  <li id="ref-11">Meng, Kevin, et al. (2022). "Locating and Editing Factual Associations in GPT." <em>Advances in Neural Information Processing Systems</em>, 35. <a href="https://arxiv.org/abs/2202.05262">https://arxiv.org/abs/2202.05262</a></li>
  <li id="ref-12">Liu, Nelson F., et al. (2023). "Lost in the Middle: How Language Models Use Long Contexts." <em>Transactions of the Association for Computational Linguistics</em>, 12. <a href="https://arxiv.org/abs/2307.03172">https://arxiv.org/abs/2307.03172</a></li>
  <li id="ref-13">Karpathy, Andrej (2023). "Intro to Large Language Models." YouTube. <a href="https://www.youtube.com/watch?v=zjkBMFhNj_g">https://www.youtube.com/watch?v=zjkBMFhNj_g</a></li>
  <li id="ref-14">He, Driss Guessous, et al. (2024). "Flex Attention: A Programming Model for Generating Optimized Attention Kernels." arXiv:2412.05496. <a href="https://arxiv.org/abs/2412.05496">https://arxiv.org/abs/2412.05496</a></li>
  <li id="ref-15">Jiang, Albert Q., et al. (2024). "Mixtral of Experts." arXiv:2401.04088. <a href="https://arxiv.org/abs/2401.04088">https://arxiv.org/abs/2401.04088</a></li>
</ol>

---

## Outtakes

**The strawberry problem, solved.** OpenAI's o1 model counts letters in "strawberry" correctly. The model was codenamed "Strawberry" as an inside joke (Axios, 2024). The tokenization didn't change. o1 reasons through the problem step by step, using chain-of-thought as a workaround for a structural limitation.

**"Attention Is All You Need."** The base transformer trained on 8 GPUs in 12 hours (Vaswani et al., 2017). The paper that defined modern AI was an afternoon experiment. By 2023, training comparable frontier models would take millions of GPU-hours.

**"The" vs "the."** "The," "the," and " the" are three distinct tokens in the GPT-2 vocabulary (Grinberg, 2024). The model has no concept of a word. It sees token sequences that happen to look like words to you.

---

## Changelog

**2026-06-26** Removed vague attribution. Added section covering attention variants and Mixture of Experts.  
**2026-06-14** Initial release.  
