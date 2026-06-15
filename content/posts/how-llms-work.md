---
title: "How LLMs Work"
date: 2026-06-14
publishdate: 2026-06-14
lastmod: 2026-06-14
summary: "Working notes on how LLMs train and generate text. Probably mostly right, certainly incomplete."
tags: ["ai", "architecture", "learning"]
image: /images/how-llms-work.jpg
draft: false
---

![](/images/how-llms-work.jpg)
*"Library card catalog." Photo: AKT.UZ (2018). Public domain.*

## How LLMs Work

Someone asks an LLM how many r's are in "strawberry" and the model says two (Hacker News, 2024). You might assume the model can't count. The real problem is that the model never saw the word "strawberry" in the first place.

Depending on the model, "strawberry" might be tokenized into ["s", "traw", "berry"] or ["straw", "berry"]. The letters never existed as separate inputs. The model received a different task than the one you described (0xkato, 2026).

Most model failures make more sense once you know what's happening inside.

## Your Prompt Becomes Numbers

Before the model processes a single word, your text converts to integers. A tokenizer breaks input into subword pieces (e.g., “straw”, “berry”) and maps each piece to an ID in a vocabulary of tens of thousands of entries. GPT-2 uses 50,257 tokens. Capitalization and spacing matter, so "The," "the," and " the" are three distinct entries (Grinberg, 2024). Different model families use different tokenizers. GPT models use Byte Pair Encoding. LLaMA-family models use SentencePiece. A prompt that works well in one model may behave differently in another.

Each token ID indexes into an embedding matrix and retrieves a learned vector, typically 4,096 numbers long for a 7B model (Touvron et al., 2023). Semantically similar tokens develop similar vectors through training, but not by design. It emerges from predicting text at scale.

A simpler approach to next-token prediction would record, for every possible token sequence, which token followed it. But representing all possible 1,024-token contexts over a 50,000-token vocabulary would require a table with roughly 10^4812 rows (log₁₀(50000^1024) = 1024 × log₁₀(50000) ≈ 4812). Neural networks learn a function that approximates those probabilities without enumerating every possible context (Grinberg, 2024).

## How the Model Learned

Pre-training is the first stage. Feed the model roughly 2 trillion tokens of text, ask it to predict the next word at each step, and update the weights when it's wrong. Training Llama 2 70B required 1.7 million GPU-hours, about 36 days on 2,000 A100s (Touvron et al., 2023). The result is lossy compression, not exact recall.

Not all models train the same way. Encoder-style models like BERT use masked language modeling. Hide a token, predict it from the context on both sides. That bidirectionality makes them better at tasks requiring the full picture. Decoder-style models like GPT train autoregressively. Each token is predicted from only the tokens before it, then fed back as input for the next. They generate text sequentially. Most frontier chat models are decoder-only (Raschka, 2023).

Training data quantity matters as much as model size. Chinchilla, at 70 billion parameters, outperformed GPT-3 at 175 billion by training on 4.7x more tokens (Hoffmann et al., 2022). Bigger models trained on less data lose to smaller models trained longer.

Fine-tuning is the second stage. Around 13,000 curated prompt-response examples steer it toward being useful (Ouyang et al., 2022). A third stage, reinforcement learning from human feedback (RLHF), refines further using human preferences about which responses are better (Ouyang et al., 2022). Pre-training is where knowledge comes from. Fine-tuning and RLHF are where the personality comes from.

## Attention Is the Core Operation

Every transformer block runs attention. Each token generates three vectors. The Query vector captures what this token is looking for. The Key describes what this token contains. The Value is the actual content that gets passed along. Queries compare against keys via dot products, higher scores mean closer matches, and softmax converts those scores into weights. Each token then takes a weighted average of value vectors across the sequence (Vaswani et al., 2017). The result is context-dependent meaning. "Bank" after "river" develops a different representation than "bank" after "loan."

Decoder-only models use causal masking. A token can only attend to earlier tokens, making generation sequential. The model predicts a token, appends it, and repeats. The KV cache saves previously computed key and value vectors to avoid recomputing them on every new token, which is what makes generation fast in practice (Alammar, 2019).

Each transformer block adds its outputs to the incoming token vector rather than replacing it. These residual connections let the model stack dozens of layers. Without them, the training signal fades before it reaches the early layers and the model stops improving (He et al., 2016). Layer normalization runs before each sub-block in most modern models, keeping the numbers flowing through the network from growing too large or collapsing (Alammar, 2019). The token vector accumulates each block's contribution as it passes through the stack.

## Where Facts Live

After attention, each token passes through a feed-forward network (FFN) independently. The FFN expands the vector to a larger dimension, applies a non-linearity, then compresses it back down. These layers hold most of the model's parameters, and specific factual associations are stored in specific FFN weights. Researchers have found that knowledge like "The Eiffel Tower is in [Paris]" can be traced to identifiable weight patterns, enabling targeted edits that change what the model "believes" about specific facts (Meng et al., 2022). The knowledge is in the weights. New facts require new training.

## Where This Breaks Down

**The middle of your context gets underweighted.** Models attend most reliably to information at the beginning and end of long contexts. Information buried in the middle gets systematically underweighted regardless of importance. Adding more context can actively degrade performance by pushing critical information further from where attention concentrates (Liu et al., 2023).

**Temperature shapes answers, not just randomness.** After the final layer, each token's output vector multiplies against the embedding matrix to produce one score per vocabulary entry. Temperature scales those scores before softmax. Low temperature makes the model pick the highest-probability token almost every time. High temperature flattens the distribution and pulls in lower-probability options. Top-k sampling is an alternative which restricts the pool to the k highest-scoring candidates before sampling (Alammar, 2019). The same prompt with different sampling settings can produce radically different outputs.

**The model can't slow down to think.** Generation is word-by-word sampling. There's no internal deliberation loop before each token. Karpathy calls this System 1 thinking. Fast, instinctive, pattern-matched (Karpathy, 2023). Chain-of-Thought prompting forces reasoning steps before an answer, using the context window as working memory. The thinking happens in the tokens, not before them.

**Architecture isn't weights.** GPT, Gemini, and LLaMA share the transformer skeleton. What differs is training data, scale, configuration, and post-training. When two similar architectures diverge sharply on the same task, the architecture usually isn't the explanation. The weights are (Raschka, 2023).

## Conclusion

The transformer architecture described in 2017 is still, in substance, what runs today's frontier models (Vaswani et al., 2017). Nine years of refinement to position encoding, activation functions, attention variants, and scaling built on that base without replacing it (Raschka, 2023). The core mechanic is unchanged. Tokenize, embed, attend, feed-forward, predict.

The model doesn't understand your prompt. It generates statistically plausible continuations by pattern-matching against compressed training data (Grinberg, 2024). Useful output doesn't require comprehension. Knowing that changes how you verify what comes back.

When something unexpected happens, check how your prompt tokenizes. Most providers offer free tools for this. Move critical information to the start or end of long contexts. Adjust temperature down if outputs are erratic, or up if they're repetitive. All three are adjustable without touching the model.

---

## References

AKT.UZ (2018). "Library card catalog." Flickr. Public domain. [https://www.flickr.com/photos/akt_uz/45188906785/](https://www.flickr.com/photos/akt_uz/45188906785/)

Alammar, Jay (2019). "The Illustrated GPT-2 (Language Models)." [https://jalammar.github.io/illustrated-gpt2/](https://jalammar.github.io/illustrated-gpt2/)

Grinberg, Miguel (2024). "How LLMs Work, Explained Without Math." *Miguel Grinberg Blog*. [https://blog.miguelgrinberg.com/post/how-llms-work-explained-without-math](https://blog.miguelgrinberg.com/post/how-llms-work-explained-without-math)

Hacker News (2024). "All 3 models you ranked cannot get 'how many r's are in strawberry?' correct." [https://news.ycombinator.com/item?id=41058318](https://news.ycombinator.com/item?id=41058318)

He, Kaiming, et al. (2016). "Deep Residual Learning for Image Recognition." *Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition*. [https://arxiv.org/abs/1512.03385](https://arxiv.org/abs/1512.03385)

Hoffmann, Jordan, et al. (2022). "Training Compute-Optimal Large Language Models." *Advances in Neural Information Processing Systems*, 35. [https://arxiv.org/abs/2203.15556](https://arxiv.org/abs/2203.15556)

Karpathy, Andrej (2023). "Intro to Large Language Models." YouTube. [https://www.youtube.com/watch?v=zjkBMFhNj_g](https://www.youtube.com/watch?v=zjkBMFhNj_g)

0xkato (2026). "How LLMs Actually Work." 0xkato.xyz. [https://www.0xkato.xyz/how-llms-actually-work/](https://www.0xkato.xyz/how-llms-actually-work/)

Liu, Nelson F., et al. (2023). "Lost in the Middle: How Language Models Use Long Contexts." *Transactions of the Association for Computational Linguistics*, 12. [https://arxiv.org/abs/2307.03172](https://arxiv.org/abs/2307.03172)

Meng, Kevin, et al. (2022). "Locating and Editing Factual Associations in GPT." *Advances in Neural Information Processing Systems*, 35. [https://arxiv.org/abs/2202.05262](https://arxiv.org/abs/2202.05262)

Ouyang, Long, et al. (2022). "Training Language Models to Follow Instructions with Human Feedback." *Advances in Neural Information Processing Systems*, 35. [https://arxiv.org/abs/2203.02155](https://arxiv.org/abs/2203.02155)

Raschka, Sebastian (2023). "Understanding Large Language Models." *Ahead of AI*. [https://magazine.sebastianraschka.com/p/understanding-large-language-models](https://magazine.sebastianraschka.com/p/understanding-large-language-models)

Touvron, Hugo, et al. (2023). "Llama 2: Open Foundation and Fine-Tuned Chat Models." arXiv:2307.09288. [https://arxiv.org/abs/2307.09288](https://arxiv.org/abs/2307.09288)

Vaswani, Ashish, et al. (2017). "Attention Is All You Need." *Advances in Neural Information Processing Systems*, 30. [https://arxiv.org/abs/1706.03762](https://arxiv.org/abs/1706.03762)

---

## Outtakes

**The strawberry problem, solved.** OpenAI's o1 model counts letters in "strawberry" correctly. The model was codenamed "Strawberry" as an inside joke (Axios, 2024). The tokenization didn't change. o1 reasons through the problem step by step, using chain-of-thought as a workaround for a structural limitation.

**"Attention Is All You Need."** The base transformer trained on 8 GPUs in 12 hours (Vaswani et al., 2017). The paper that defined modern AI was an afternoon experiment. By 2023, training comparable frontier models would take millions of GPU-hours.

**"The" vs "the."** "The," "the," and " the" are three distinct tokens in the GPT-2 vocabulary (Grinberg, 2024). The model has no concept of a word. It sees token sequences that happen to look like words to you.

---

## Changelog

**2026-06-14** Initial release.
