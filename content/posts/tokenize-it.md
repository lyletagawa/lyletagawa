---
title: "Tokenize It"
date: 2026-09-03
publishdate: 2026-09-03
lastmod: 2026-09-03
summary: "This page's own URL runs through GPT-5's real tokenizer here, encoded into IDs with tiktoken and decoded back to the same bytes it started as. The reverse direction is the part most explainers skip."
tags: ["tokenization", "llm", "python"]
draft: true
---

## Tokenize it

Run this page's own URL through GPT-5's tokenizer and it comes back as 11 pieces. The URL itself is 41 characters and 6 words. One piece is `/token`, and the next is `ize`. The word "tokenize" didn't survive being tokenized.

A byte-pair encoding (BPE) tokenizer does this to any string that hasn't earned its own entry in the vocabulary{{< cite 1 "Radford, Alec, et al. (2019). Language Models are Unsupervised Multitask Learners. OpenAI." >}}. tiktoken, OpenAI's open-source tokenizer library, runs this on real text. Encode a string into the integers a model sees. Decode those integers back into the bytes that produced them{{< cite 2 "OpenAI. tiktoken. GitHub." >}}.

## Encode this page's URL

This post lives at `https://lyletagawa.com/posts/tokenize-it/`. Three lines of Python turn it into token IDs. GPT-5 and GPT-4o both read the same vocabulary, o200k_base, so the result holds for either model{{< cite 2 "OpenAI. tiktoken. GitHub." >}}:

```python
import tiktoken

enc = tiktoken.get_encoding("o200k_base")
ids = enc.encode("https://lyletagawa.com/posts/tokenize-it/")
print(ids)
```

```
[4172, 1684, 423, 1347, 59624, 1136, 99640, 101390, 750, 46343, 14]
```

Eleven integers, each indexing into a fixed vocabulary of 200,019 entries{{< cite 2 "OpenAI. tiktoken. GitHub." >}}. Ask the tokenizer what each one means, and the split gets specific:

```python
for token_id in ids:
    print(token_id, enc.decode_single_token_bytes(token_id))
```

```
4172 b'https'
1684 b'://'
423 b'ly'
1347 b'let'
59624 b'agawa'
1136 b'.com'
99640 b'/posts'
101390 b'/token'
750 b'ize'
46343 b'-it'
14 b'/'
```

`lyletagawa` splits into three tokens, `ly`, `let`, `agawa`, because the full name never showed up often enough in training data to earn one token of its own. `/posts` and `.com` did, whole and intact.

## Decode it back

Feed those same 11 integers to `decode()` and the URL comes back byte for byte:

```python
enc.decode(ids) == "https://lyletagawa.com/posts/tokenize-it/"
```

```
True
```

That's not luck. Byte-pair encoding starts from the 256 possible byte values and only ever merges bytes into bigger tokens. It never invents a token that can't be broken back down to bytes{{< cite 1 "Radford, Alec, et al. (2019). Language Models are Unsupervised Multitask Learners. OpenAI." >}}. Any string, including one the tokenizer has never seen, decodes back to itself. There's no placeholder swallowing information on the way in.

## Common mistakes

**You assume a token is a word.** `tokenize` is one word and three tokens, `/token`, `ize`, `-it`. Token boundaries follow what showed up often in training data, independent of spelling or grammar. Counting words to estimate token usage undercounts or overcounts depending on the string.

**You decode a single token by itself.** The full sequence always decodes to valid text, but one token pulled out of context can be a fragment of a multi-byte character. Encode the rare Chinese characters "龘龖" and o200k_base splits them into four tokens, `54462`, `246`, `54462`, `244`. None of those four is valid UTF-8 on its own. Only the full sequence, decoded together, is{{< cite 2 "OpenAI. tiktoken. GitHub." >}}.

**You assume token counts port across models.** GPT-5 and GPT-4o share o200k_base, but GPT-4 and GPT-3.5-turbo use an older vocabulary, cl100k_base{{< cite 2 "OpenAI. tiktoken. GitHub." >}}. The same string produces a different token count depending on which encoding you ask for. A budget calculated against one model's tokenizer isn't valid for another's.

## Put it into practice

Install tiktoken and run it against your own strings before you trust a word-count estimate for context budgets or API costs. `pip install tiktoken`, then the four lines above are the whole tool.

Check which encoding your model uses before comparing token counts across providers or model generations. The mapping changes as new models ship, and a count computed against the wrong vocabulary is worse than no count at all.

If you're debugging why a model misreads part of a string, encode it and look at the pieces. The split usually explains the mistake before any other theory does.

---

## References

<ol class="references">
  <li id="ref-1">Radford, Alec, Jeffrey Wu, Rewon Child, David Luan, Dario Amodei, and Ilya Sutskever (2019). "Language Models are Unsupervised Multitask Learners." OpenAI. <a href="https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf">https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf</a></li>
  <li id="ref-2">OpenAI. "tiktoken." GitHub. <a href="https://github.com/openai/tiktoken">https://github.com/openai/tiktoken</a></li>
</ol>

---

## Changelog

**2026-09-03** Initial release.
