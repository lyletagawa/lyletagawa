---
title: "Confabulation"
date: 2026-07-04
publishdate: 2026-07-04
lastmod: 2026-07-04
summary: "Cognitive science already built rigorous tests for confidently wrong explanations and buried memories, decades before anyone had a transformer to test. AI research keeps reinventing the same experiments under new names."
tags: ["cognitive", "testing", "ai"]
image: /images/confabulation.jpg
draft: true
---

![Cognitive science already built rigorous tests for confidently wrong explanations and buried memories, decades before anyone had a transformer to test. AI research keeps reinventing the same experiments under new names.](/images/confabulation.jpg)

## Confabulation

A team ships a support chatbot. In the demo, someone asks it a question outside its training data, and it answers confidently, cites a policy that doesn't exist, and gets the tone exactly right. The room believes it. Nobody can say why it happened or how to catch the next one before a customer does.

That specific failure, a system that's certain and wrong at the same time, isn't new. Neuroscience diagnosed it in humans decades before anyone built a language model, and it didn't just give the failure a name. It built an experiment to prove it.

## What The Comparison Is Actually For

Calling a model's confident invention a "hallucination" instead of a "confabulation" changes nothing by itself. Renaming a bug doesn't fix it. What's useful is borrowing the experimental design cognitive scientists already built to isolate that exact failure in a different system, the human brain, and running the same design on a model.

Two examples show what that looks like. A decades-old test for confabulated reasoning, and a decades-old test for memory that fades in the middle of a list.

## The Interpreter That Makes Things Up

In split-brain patients, whose corpus callosum has been severed, the two hemispheres can't share information directly. Michael Gazzaniga and Joseph LeDoux built experiments that exploit that gap{{< cite 1 "Gazzaniga, Michael S., and Joseph E. LeDoux (1978). The Integrated Mind. Plenum Press." >}}. Flash the word "walk" to a patient's right hemisphere only, and the patient gets up and starts walking. Ask why, using the left hemisphere that controls speech and never saw the word, and the answer isn't "I don't know." It's "I wanted to get a drink of water."

In another version, the right hemisphere sees snow and the left sees a chicken claw. The patient picks a shovel with the hand the right hemisphere controls and a chicken with the hand the left controls. Asked why, the speaking hemisphere, with no access to the snow it never saw, answers instantly. The chicken claw goes with the chicken, and you need a shovel to clean out the chicken shed.

The point isn't just that people make things up. It's a controlled test. Withhold information from the part of the system that has to explain itself, then check whether the explanation admits it doesn't know or invents a story instead.

## The Same Test, Run On A Model

That's the test Miles Turpin and colleagues ran on GPT-3.5 and Claude in 2023. They took multiple-choice questions and quietly reordered the options so the correct answer sat in the same position every time, a bias the model had no legitimate reason to use. The models used it anyway, and accuracy dropped by up to 36 percent across 13 reasoning tasks{{< cite 2 "Turpin, Miles, et al. (2023). Language Models Don't Always Say What They Think: Unfaithful Explanations in Chain-of-Thought Prompting. arXiv." >}}.

The tell wasn't the wrong answer. It was the explanation. Asked to show its reasoning, the model never mentioned the answer's position. It built a fluent, plausible justification for whatever the bias had nudged it toward, the same shape as the chicken-shed story, minus a corpus callosum.

## The Middle Goes Missing, Twice

Bennet Murdock ran a simpler experiment in 1962. Show people a list of words one at a time, then ask them to recall as many as they can. Recall isn't flat across the list. It's high for the first few words, high again for the last few, and lowest for whatever sat in the middle{{< cite 3 "Murdock, Bennet B. (1962). The Serial Position Effect of Free Recall. Journal of Experimental Psychology, 64(5)." >}}. The pattern held up so well that psychology gave it a name, the serial position curve, and has replicated it for over sixty years.

In 2023, Nelson Liu and colleagues ran the equivalent test on language models. Bury the answer to a question at different positions inside a long document, then ask the model to find it. Accuracy is highest when the answer sits at the very start or end of the context and drops in the middle, sometimes below what the model scores with no supporting document at all{{< cite 4 "Liu, Nelson F., et al. (2023). Lost in the Middle: How Language Models Use Long Contexts. Transactions of the Association for Computational Linguistics, 12." >}}. Different substrate, same curve, same method. Hold the content fixed, vary only its position, plot the result.

## Common Mistakes

**Treating the metaphor as identity.** A transformer has no hemispheres or corpus callosum, and nothing about attention literally works like an interpreter module. The parallel is behavioral, not mechanistic. Borrow the test, not the biology.

**Stopping at the vocabulary.** Calling a failure confabulation instead of hallucination changes nothing if you don't also run the dissociation test that gives the word its meaning. A label without an experiment is just branding.

**Assuming a human diagnosis implies a human fix.** Psychology has studied confabulation for a century without curing it. Borrowing the test tells you where to look for a problem, not how to solve it.

**Cherry-picking the flattering parallel.** It's tempting to reach for whichever cognitive science finding makes a failure sound sophisticated, instead of checking whether the parallel predicts anything the model actually does. Test it before you cite it.

## Put It Into Practice

Next time a model fails in a way that feels oddly human, confident and wrong, forgetting the middle, changing its story under pressure, look for the closest documented failure in human cognition before writing a new eval from scratch. Decades of psychology research already isolated that failure and built a controlled way to trigger it on demand.

Then port the experimental design, not the terminology. Withhold information and check what the system claims to know. Vary position and hold content constant. Inject a bias the system has no legitimate reason to use, and see if it shows up in the stated reasoning anyway.

The next strange model bug in your eval suite has probably already been run once, on a different kind of brain, sometime in the last sixty years.

## Go Further

**Confabulation beyond split-brain patients.** Korsakoff's syndrome, caused by severe thiamine deficiency, produces confident, detailed false memories in patients with almost no ability to form new ones, a different route to the same confident-wrong failure{{< cite 5 "Arts, Nicolaas J.M., Serge J.W. Walvoort, and Roy P.C. Kessels (2017). Korsakoff's Syndrome: A Critical Review. Neuropsychiatric Disease and Treatment." >}}.

**Anchoring and prompt order.** Amos Tversky and Daniel Kahneman's anchoring research shows human estimates shift toward an arbitrary first number they're shown, a close cousin of the documented sensitivity of LLM outputs to the order information appears in a prompt{{< cite 6 "Tversky, Amos, and Daniel Kahneman (1974). Judgment under Uncertainty: Heuristics and Biases. Science, 185(4157)." >}}.

**Interpretability beyond chain-of-thought.** Turpin's paper is one entry in a growing body of mechanistic interpretability research asking whether a model's stated reasoning corresponds to its actual internal computation at all{{< cite 2 "Turpin, Miles, et al. (2023). Language Models Don't Always Say What They Think: Unfaithful Explanations in Chain-of-Thought Prompting. arXiv." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Gazzaniga, Michael S., and Joseph E. LeDoux (1978). <em>The Integrated Mind</em>. Plenum Press. <a href="https://archive.org/details/integratedmind0000gazz">https://archive.org/details/integratedmind0000gazz</a></li>
  <li id="ref-2">Turpin, Miles, et al. (2023). "Language Models Don't Always Say What They Think: Unfaithful Explanations in Chain-of-Thought Prompting." arXiv. <a href="https://arxiv.org/abs/2305.04388">https://arxiv.org/abs/2305.04388</a></li>
  <li id="ref-3">Murdock, Bennet B. (1962). "The Serial Position Effect of Free Recall." <em>Journal of Experimental Psychology</em>, 64(5): 482-488. <a href="https://doi.org/10.1037/h0045106">https://doi.org/10.1037/h0045106</a></li>
  <li id="ref-4">Liu, Nelson F., et al. (2023). "Lost in the Middle: How Language Models Use Long Contexts." <em>Transactions of the Association for Computational Linguistics</em>, 12. <a href="https://arxiv.org/abs/2307.03172">https://arxiv.org/abs/2307.03172</a></li>
  <li id="ref-5">Arts, Nicolaas J.M., Serge J.W. Walvoort, and Roy P.C. Kessels (2017). "Korsakoff's Syndrome: A Critical Review." <em>Neuropsychiatric Disease and Treatment</em>. <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC5708199/">https://pmc.ncbi.nlm.nih.gov/articles/PMC5708199/</a></li>
  <li id="ref-6">Tversky, Amos, and Daniel Kahneman (1974). "Judgment under Uncertainty: Heuristics and Biases." <em>Science</em>, 185(4157): 1124-1131. <a href="https://pubmed.ncbi.nlm.nih.gov/17835457/">https://pubmed.ncbi.nlm.nih.gov/17835457/</a></li>
</ol>

---

## Outtakes

**The surgery predates the psychology by decades.** Corpus callosotomy began in 1940 as a failed epilepsy treatment. It didn't work until Joseph Bogen and Philip Vogel severed the connection completely in 1962, accidentally creating the split-brain patients whose confabulated explanations Gazzaniga studied years later ([JNS, 2008](https://thejns.org/view/journals/j-neurosurg/108/3/article-p608.xml)).

**Confabulation used to just mean chatting.** The word comes from Latin confabulari, "to talk together." Swiss psychiatrist Eugen Bleuler gave it its clinical meaning, fabricated memories standing in for real ones, only in 1924, centuries after it meant nothing more sinister than a fireside conversation ([Etymonline, n.d.](https://www.etymonline.com/word/confabulation)).

**Sperry won a Nobel Prize for taking the two hemispheres apart.** Roger Sperry shared the 1981 Nobel Prize in Physiology or Medicine for his split-brain research, the surgical and experimental groundwork Gazzaniga later built the interpreter studies on ([Nobel Prize, 1981](https://www.nobelprize.org/prizes/medicine/1981/sperry/facts/)).

---

## Changelog

**2026-07-04** Initial release.
