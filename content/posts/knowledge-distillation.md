---
title: "Knowledge Distillation"
date: 2026-08-02
publishdate: 2026-08-02
lastmod: 2026-08-08
summary: "A reviewer rejected Jeff Dean's 2014 paper as unlikely to matter. Every small model Google ships today, including Gemini Flash, runs on the technique that paper described."
tags: ["ai", "models", "efficiency"]
image: /images/knowledge-distillation.jpg
draft: true
---

![A reviewer rejected Jeff Dean's 2014 paper as unlikely to matter. Every small model Google ships today, including Gemini Flash, runs on the technique that paper described.](/images/knowledge-distillation.jpg)
*Copper pot stills at ASW Distillery, Atlanta. Photo: [Thechadwix (2016)](https://commons.wikimedia.org/wiki/File:ASW_Distillery's_copper_pot_stills,_manufactured_by_Vendome_Copper_%26_Brass_Works.jpg). CC BY-SA 4.0.*

## Knowledge Distillation

In a 2026 Y Combinator interview, the host reminds Jeff Dean that rejection comes with the territory of building anything. Dean has a story ready. In 2014 he wrote a paper with Geoffrey Hinton and Oriol Vinyals about training a small, efficient model to copy a much larger one{{< cite 1 "Dean, Jeff (2026). The 1% Rule for Building in AI. Y Combinator Startup Podcast." >}}. A conference reviewer read it and passed. "This work is incremental and unlikely to have much impact," the review said{{< cite 2 "Dean, Jeff (2019). Reply on the Distilling the Knowledge in a Neural Network paper. X (formerly Twitter)." >}}.

Every small language model Google ships today runs on that rejected idea. Gemini Flash is distilled from Gemini Pro. Dean's closing line in the interview does the work of a whole paragraph. "That's the lesson I would distill from that."

## What Is Knowledge Distillation?

Hinton, Vinyals, and Dean published the paper anyway, on arXiv, under the title "Distilling the Knowledge in a Neural Network"{{< cite 3 "Hinton, Geoffrey, Oriol Vinyals, and Jeff Dean (2015). Distilling the Knowledge in a Neural Network. arXiv:1503.02531." >}}. Their problem was practical. A large, accurate model, or an ensemble of several, is often too slow and expensive to serve to millions of users. A small model is cheap to run but usually less accurate. Distillation trains the small "student" model to reproduce what the large "teacher" model actually outputs instead of the labeled dataset the teacher was originally trained on.

The insight is in what "actually outputs" means. A teacher model does more than name the correct answer. It assigns a probability to every possible answer, and those probabilities carry information a single correct label throws away. A photo of a BMW has only a tiny chance of being labeled a garbage truck, but that chance is still many times larger than the chance of it being labeled a carrot{{< cite 3 "Hinton, Geoffrey, Oriol Vinyals, and Jeff Dean (2015). Distilling the Knowledge in a Neural Network. arXiv:1503.02531." >}}. That ranking, garbage truck over carrot, encodes real knowledge about how cars relate to other things in the world.

## Why Soft Targets Win

Training the student directly on that full probability distribution, called a soft target, transfers far more of what the teacher learned than training on hard labels alone. The paper's own MNIST experiment shows the effect starkly. A large network trained on handwritten digits, then used to teach a much smaller one through soft targets, gave it a probability distribution over every digit for every training image. One image of a 2 might get a probability of one in a million of being a 3, and one in a billion of being a 7. A different, more ambiguous 2 might get those odds reversed{{< cite 3 "Hinton, Geoffrey, Oriol Vinyals, and Jeff Dean (2015). Distilling the Knowledge in a Neural Network. arXiv:1503.02531." >}}. That ratio is where the teacher's real knowledge about handwriting lives.

The technique includes a "temperature" setting that turns up how soft those probabilities are during training, then turns back down to normal for the finished model. Pushed far enough, the effect gets strange in a good way. The researchers trained a student on a transfer set with every example of the digit 3 deleted. The student had never seen a labeled 3. Once the researchers corrected a systematic bias against the unseen class, it classified 98.6 percent of test 3s correctly, still without ever training on a labeled 3{{< cite 3 "Hinton, Geoffrey, Oriol Vinyals, and Jeff Dean (2015). Distilling the Knowledge in a Neural Network. arXiv:1503.02531." >}}. The same approach, tested on the acoustic model behind Android voice search, let a single small model match nearly all of the accuracy gain of a ten-model ensemble{{< cite 3 "Hinton, Geoffrey, Oriol Vinyals, and Jeff Dean (2015). Distilling the Knowledge in a Neural Network. arXiv:1503.02531." >}}.

## Gemini Flash Runs On This

Dean's 2026 retelling did more than reminisce. He connected the story directly to what ships today. Google trains its Pro-scale models first, then distills them down into the Flash-scale models built for speed and cost{{< cite 1 "Dean, Jeff (2026). The 1% Rule for Building in AI. Y Combinator Startup Podcast." >}}. Flash models rank among the strongest in the industry for their size and latency class, and Dean credits that directly to the technique the reviewer waved off{{< cite 1 "Dean, Jeff (2026). The 1% Rule for Building in AI. Y Combinator Startup Podcast." >}}.

The arrangement works because Google owns both ends of it. The teacher and the student belong to the same company, trained on infrastructure Google controls, so it's clear who's allowed to learn from whom.

## Distillation Without Permission

That question becomes the whole story once the teacher belongs to someone else. In a February 2026 memo to the U.S. House Select Committee on Strategic Competition, OpenAI accused the Chinese lab DeepSeek of distilling its models without authorization, calling it part of "ongoing efforts to free-ride on the capabilities developed by OpenAI and other U.S. frontier labs"{{< cite 4 "Seetharaman, Deepa, and Fabiola Arámburo (2026). OpenAI Accuses China's DeepSeek of Distilling US Models to Gain an Edge. Bloomberg." >}}. OpenAI's memo went further, stating that DeepSeek employees had "developed code to access U.S. AI models and obtain outputs for distillation in programmatic ways," using obfuscated routers to get around access limits{{< cite 4 "Seetharaman, Deepa, and Fabiola Arámburo (2026). OpenAI Accuses China's DeepSeek of Distilling US Models to Gain an Edge. Bloomberg." >}}.

Distillation needs a teacher's outputs at scale, and OpenAI's usage terms bar using those outputs to train a competing model. Whether DeepSeek actually did that is still contested. DeepSeek and its parent company never responded to requests for comment. The technique itself is neutral about whose model it's pointed at.

## Where People Get This Wrong

**Confusing distillation with pruning or quantization.** Pruning removes weights from an existing model. Quantization reduces the numeric precision of those weights. Distillation trains an entirely new, smaller model from scratch, using a different model's outputs as the teaching signal. All three shrink a model. Only one of them requires a teacher.

**Assuming any output from the teacher will do.** A model's single final answer discards almost everything useful. The technique depends on the full probability distribution behind it, or at minimum a temperature-softened version of that distribution. Skip that, and nothing useful transfers.

**Treating the student as a smaller copy with unlimited capability.** A distilled model recovers most of the teacher's accuracy, and the remaining gap widens as the student shrinks further. Distillation narrows the tradeoff between size and capability. Some of that tradeoff always remains.

## Put It Into Practice

If a team is serving a frontier-scale model to every request by default, that's the moment to ask whether it needs to. Most requests need only a fraction of the full model's capability, and a distilled version trained specifically on that traffic pattern can match it closely at a fraction of the cost and latency.

Start with the teacher model already in production. Log the teacher's full output distribution on the traffic a smaller model would need to handle. A logged answer alone falls short for training. Train the student against that distribution before assuming a bigger model is the only option.

## Dig Deeper

**Where the idea started.** Hinton, Vinyals, and Dean built on earlier work by Cristian Bucilua, Rich Caruana, and Alexandru Niculescu-Mizil, who first showed an ensemble's knowledge could move into a single model, years before "distillation" had its name{{< cite 5 "Bucilua, Cristian, Rich Caruana, and Alexandru Niculescu-Mizil (2006). Model Compression. Proceedings of the 12th ACM SIGKDD International Conference on Knowledge Discovery and Data Mining." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Dean, Jeff (2026). "The 1% Rule for Building in AI." <em>Y Combinator Startup Podcast</em>. <a href="https://www.youtube.com/watch?v=CxXgV54KzpQ">https://www.youtube.com/watch?v=CxXgV54KzpQ</a></li>
  <li id="ref-2">Dean, Jeff (2019). Reply regarding "Distilling the Knowledge in a Neural Network." <em>X (formerly Twitter)</em>, September 24. <a href="https://x.com/jeffdean/status/1176906175666937856">https://x.com/jeffdean/status/1176906175666937856</a></li>
  <li id="ref-3">Hinton, Geoffrey, Oriol Vinyals, and Jeff Dean (2015). "Distilling the Knowledge in a Neural Network." <em>arXiv:1503.02531</em>. <a href="https://arxiv.org/abs/1503.02531">https://arxiv.org/abs/1503.02531</a></li>
  <li id="ref-4">Seetharaman, Deepa, and Fabiola Arámburo (2026). "OpenAI Accuses China's DeepSeek of Distilling US Models to Gain an Edge." <em>Bloomberg</em>, February 12. <a href="https://finance.yahoo.com/news/openai-accuses-deepseek-distilling-us-221629899.html">https://finance.yahoo.com/news/openai-accuses-deepseek-distilling-us-221629899.html</a></li>
  <li id="ref-5">Bucilua, Cristian, Rich Caruana, and Alexandru Niculescu-Mizil (2006). "Model Compression." <em>Proceedings of the 12th ACM SIGKDD International Conference on Knowledge Discovery and Data Mining</em>. <a href="https://dl.acm.org/doi/10.1145/1150402.1150464">https://dl.acm.org/doi/10.1145/1150402.1150464</a></li>
</ol>

---

## Changelog

**2026-08-02** Initial release.  
