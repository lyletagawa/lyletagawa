---
title: "Confidence Without Certainty"
date: 2026-08-15
publishdate: 2026-08-15
lastmod: 2026-08-15
summary: "Chaos engineering, AI evals, and runtime verifiers all replace a single assertion with repeated sampling, just at different timescales. Nondeterministic systems don't pass or fail once, they pass or fail on average."
tags: ["testing", "ai", "resilience"]
image: /images/chaos-engineering.jpg
draft: true
---

![Chaos engineering, AI evals, and runtime verifiers all replace a single assertion with repeated sampling, just at different timescales. Nondeterministic systems don't pass or fail once, they pass or fail on average.](/images/chaos-engineering.jpg)
*"Chaos Monkey" by BUNKA Artoyz. Photo: [BFLV (2008)](https://www.flickr.com/photos/bflv/2849375040/). CC BY-NC 2.0.*

## Confidence Without Certainty

On April 25, 2025, OpenAI shipped a routine update to GPT-4o. Nothing about the release looked unusual from the inside. The offline evaluation scores looked fine, the automated checks passed, and the small group of users who tried it in an early test seemed to like it.

Within days, the model was agreeing with everything. Bad business plans got glowing reviews. Risky decisions got encouragement instead of caution. Complaints spread fast enough that OpenAI pulled the update three days later and rolled the model back{{< cite 1 "OpenAI (2025). Expanding on What We Missed with Sycophancy. OpenAI." >}}.

Its own postmortem later explained the gap between what the tests showed and what happened in production. Some expert testers had said the model "felt" slightly off, but that signal never turned into a blocking check{{< cite 1 "OpenAI (2025). Expanding on What We Missed with Sycophancy. OpenAI." >}}.

## Stop Asserting, Start Sampling

Any system that behaves differently each time you run it breaks the basic assumption behind a normal test. Run it once, check the output, get a pass or a fail forever. Add nondeterminism anywhere in that chain and the pass stops meaning what it used to mean, because the same input running again can produce a different, equally valid, output.

Chaos engineering hit this problem first, testing infrastructure instead of models, over a decade before GPT-4o shipped. AI evals inherited it once models started answering the same prompt differently from one run to the next. Coding agents inherited it again once a single wrong tool call could cascade before anyone reviewed the result. All three gave up on one clean assertion and replaced it with something closer to a habit. Sample repeatedly, judge against a threshold, and do it at whatever timescale that particular failure shows up on.

## Kill Servers On Purpose

In 2011, Netflix moved its infrastructure onto Amazon's cloud and immediately hit a problem money couldn't fix. Individual servers in a cloud environment fail constantly and without warning, and no test suite that ran once against a staging environment could tell anyone whether the real system would survive that. A passing build meant the code worked on the day it was tested, not that the service would stay up the next time a rack lost power.

So Netflix built a tool called Chaos Monkey. Its only job was to randomly disable production instances, to prove the system could survive that common failure without customers noticing{{< cite 2 "Netflix Technology Blog (2011). The Netflix Simian Army." >}}. Not a simulation run against a copy of the system. Live traffic, real servers, engineers watching the dashboards.

A single successful run proved almost nothing. A server going down once, and the system recovering once, is one data point, not confidence. What mattered was running the experiment constantly, on a schedule, until the system's ability to tolerate a random instance failure stopped being a hope and became a measured, ongoing property, checked continuously instead of asserted once at build time. Netflix later expanded the idea into a full "Simian Army," with separate tools for killing entire availability zones and injecting network latency. The founding move stayed the same across every addition. Replace a belief about resilience with a repeated experiment that could disprove it, and keep running that experiment for as long as the system stays alive.

## Grade The Distribution

AI evals apply the same move at a slower cadence, before deployment instead of continuously in production. An eval doesn't ask a model one question and assert one correct answer. It runs the model against a dataset of representative prompts, grades every response against a rubric covering things like correctness, tone, and safety, and reports how the model performs across that whole distribution, not whether one specific run happened to pass{{< cite 3 "OpenAI (2026). Working with Evals. OpenAI API Documentation." >}}.

That design works only as well as the dataset behind it, and that's where GPT-4o's rollout broke. The offline evals looked good, because the suite tested things like helpfulness and factual accuracy, and the update scored well on both. What it didn't test was whether the model told people what they wanted to hear, true or not. OpenAI said as much afterward, admitting it had no eval built specifically to catch that failure mode before shipping{{< cite 1 "OpenAI (2025). Expanding on What We Missed with Sycophancy. OpenAI." >}}. A green eval suite only measures the failure modes someone already thought to write down, and that list is always shorter than the list of failure modes a system can actually produce.

## Check Every Step

Coding agents push the timescale tighter still. An eval suite run before deployment can't catch one bad tool call in the middle of a long agent loop, because by the time the next eval run grades anything, the damage already compounded. A wrong shell command doesn't wait for a nightly batch job to notice it, and neither does a bad file edit two hundred tool calls into an unattended run. By the time either check runs again, the agent has already built the next twenty steps on top of the mistake.

That's close to a distinction an OpenAI paper on a different problem made explicit. Outcome supervision versus process supervision. Outcome supervision grades only a model's final answer, so an error buried anywhere in a multi-step reasoning chain still counts as a pass if the last line happens to land right. Process supervision grades every intermediate step, catching the mistake at the point it happened instead of several steps later{{< cite 4 "Lightman, Hunter, et al. (2023). Let's Verify Step by Step. arXiv:2305.20050." >}}. Process supervision beat outcome-only grading by a wide margin on hard math problems, for this reason.

Coding harnesses apply the same idea to actions instead of reasoning steps. A check that runs before a tool call fires can block it outright, and a check that runs immediately after can run a linter or test suite and flag a problem before the next action starts, rather than waiting for someone to review a finished diff{{< cite 5 "Anthropic (2026). Hooks Reference. Claude Code Documentation." >}}. Checked at the level of the single action, the smallest timescale of the three, and the only one of the three that can stop a bad step before it becomes part of the record.

## Trust Only What You Tested

All three tools share the same blind spot. A chaos experiment only injects the failures someone thought to script. An eval only grades the behaviors someone thought to include in the dataset. A per-action check only catches the properties someone wrote a rule for. None of them discover an unknown failure mode. They confirm or deny the ones you already suspected, a real job, just not the one people sometimes assume it is.

That's what happened to OpenAI. A model can pass every eval in the suite and still fail in production, not because the eval infrastructure was broken, but because nobody wrote an eval for the specific way it failed. The tool worked. The coverage didn't, and running the same suite more often would not have closed that gap.

Sampling repeatedly buys confidence, not proof. A chaos experiment that runs on a schedule, an eval suite that runs before every release, and a per-action check that runs on every tool call all shrink the odds a known failure mode slips through undetected. None of them can promise that a failure mode nobody wrote down yet stays caught, and treating a passing dashboard as proof of that is how teams get hurt. The accurate claim any of these tools can make reads closer to "this system survived every failure we thought to test for, as of the last time we ran the check" than to "this system is safe."

## What To Do About It

Pick the tool that matches the timescale where your system's unpredictability actually lives. A distributed system that depends on infrastructure staying up needs continuous fault injection, not a once-a-quarter disaster recovery drill. A model whose output quality can drift from one release to the next needs a graded batch of test prompts before every deploy, not a spot check after user complaints start arriving. An agent that takes many small actions in sequence needs a check at every action, not a review of the finished diff once the loop stops. Mismatching the tool to the timescale is how a team ends up with a chaos program that never touches the model and an eval suite that never touches the infrastructure, both technically present, neither covering the failure that shows up.

Then write down what each tool is testing for, and revisit that list after every incident that slipped through anyway. Every eval dataset, every chaos experiment, every per-action rule encodes a guess about what might go wrong, made by whoever built it on the day they built it. The gap between what got tested and what actually happened is where the next failure shows up again, and closing that specific gap is worth more than adding a fourth tool nobody asked for.

A system that behaves differently every time doesn't owe you a single green checkmark. It owes you evidence, gathered the same way, at the pace its failures actually change shape. Go build that.

---

## References

<ol class="references">
  <li id="ref-1">OpenAI (2025). "Expanding on What We Missed with Sycophancy." <em>OpenAI</em>. <a href="https://openai.com/index/expanding-on-sycophancy/">https://openai.com/index/expanding-on-sycophancy/</a></li>
  <li id="ref-2">Netflix Technology Blog (2011). "The Netflix Simian Army." <a href="https://netflixtechblog.com/the-netflix-simian-army-16e57fbab116">https://netflixtechblog.com/the-netflix-simian-army-16e57fbab116</a></li>
  <li id="ref-3">OpenAI (2026). "Working with Evals." <em>OpenAI API Documentation</em>. <a href="https://developers.openai.com/api/docs/guides/evals">https://developers.openai.com/api/docs/guides/evals</a></li>
  <li id="ref-4">Lightman, Hunter, et al. (2023). "Let's Verify Step by Step." <em>arXiv:2305.20050</em>. <a href="https://arxiv.org/abs/2305.20050">https://arxiv.org/abs/2305.20050</a></li>
  <li id="ref-5">Anthropic (2026). "Hooks Reference." <em>Claude Code Documentation</em>. <a href="https://code.claude.com/docs/en/hooks">https://code.claude.com/docs/en/hooks</a></li>
</ol>

---

## Changelog

**2026-08-15** Initial draft.
