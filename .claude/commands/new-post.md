---
description: Author a new Hugo blog post or edit an existing one. Enforces voice, style, citation, and structure rules.
argument-hint: "[topic] or [path to existing post]"
disable-model-invocation: true
---

# Blog Post

Author a new Hugo blog post or edit an existing one. Follow all rules below precisely.

## Writing Rules

### Voice and Style
- Write in second person ("you") or third person. Avoid first person.
- Casual, conversational tone. Write like a smart friend who read the book so the reader doesn't have to, not like a summary of the book itself.
- Use contractions freely. "You don't" beats "one does not."
- Prefer active voice. Passive voice distances the reader.
- Put statements in positive form (Strunk & White, *The Elements of Style*, Rule 12). Describe things with affirmative language instead of negation. "The system is reliable" beats "The system isn't unreliable."
- Sentence fragments are fine for emphasis. Use them deliberately.
- Simple sentence structure. One idea per sentence. Short sentences are better.
- Vary sentence length for rhythm.
- Minimize consecutive short sentences. Three or more in a row feels staccato. Break the pattern with a longer sentence or by combining ideas.
- Do not repeat yourself. A phrase, claim, or example may be repeated once per post, deliberately, to emphasize the single most important point. Cut every other repetition.
- No em-dashes, en-dashes, or semicolons. Rewrite to avoid them.
- Use straight quotation marks (`"`) and straight apostrophes (`'`). Never use curly/smart quotes (`"` `"` `'` `'`).
- Minimize colons in prose. Don't use a colon to introduce a list when a new sentence or fragments work better. "The cost is real. Back-and-forth, context switching, a slower pipeline." beats "The cost is real: back-and-forth, context switching, pipeline slowdown." Colons in frontmatter and References formatting are fine.
- No emoji.
- Numbers and units follow AP style:
  - Dollar amounts of $1 million or more: use the `$` symbol before the numeral, then spell out "million"/"billion"/"trillion." "$5 million," not "5 million dollars." In a range, repeat the symbol on both ends: "$38 million to $188 million."
  - Foreign currency (euros, yen, etc.): spell out both the numeral and the currency name, no symbol. "60 million euros," not "€60 million."
  - Plain counts zero through nine: spell out. "four cables."
  - Plain counts 10 and above: use numerals, with commas at the thousands. "6,000 incidents."
  - Units of measurement (distance, weight, speed, etc.): always use numerals, even below 10. "5 kilometers," not "five kilometers."
- Define obscure acronyms on first use. Write out the full term followed by the abbreviation in parentheses, e.g. "feed-forward network (FFN)." Use the abbreviation alone on subsequent mentions.
- No rhetorical questions used as section openers or transitions.
- No filler phrases: "it's worth noting," "it's important to remember," "in other words," "at the end of the day," "needless to say," "this is crucial."
- Banned phrases:
  - "is a testament"
  - "stands as a testament to"
  - "a testament to the power of"
  - "underscores its importance/significance"
  - "underscores the importance of"
  - "emphasizing the importance of"
  - "reflects broader"
  - "reflects the continued relevance of"
  - "symbolizing its ongoing/enduring/lasting"
  - "setting the stage for"
  - "paving the way for"
  - "marking/shaping the"
  - "represents/marks a shift"
  - "key turning point"
  - "evolving landscape"
  - "in today's fast-paced world"
  - "in today's [adjective] landscape"
  - "in an era where"
  - "focal point"
  - "indelible mark"
  - "deeply rooted"
  - "a vital/significant/crucial/pivotal/key role/moment"
  - "plays a vital/crucial/pivotal role"
  - "honest take"
  - "load bearing"
  - "sit with that/this/it"
  - "you already know"
  - "that's/this is the whole point/game/thing"
  - "is the entire point/game/business model"
  - "the punchline"
  - "it's worth noting that"
  - "highlighting the need for"
  - "a rich tapestry of"
  - "at the heart of"
  - "serves as a reminder that"
  - "has garnered significant attention"
  - "the importance of X cannot be overstated"
  - "deep dive"
  - "dive deep into"
  - "delve into"
  - "let's explore"
  - "paradigm shift"
- Banned words:
  - Verbs: "boasts," "bolster," "foster," "garner," "delve," "leverage," "utilize," "harness," "streamline," "underscore," "navigate" (metaphorical), "spearhead," "embark," "unpack" (metaphorical), "unravel" (metaphorical), "unlock," "empower," "elevate," "forecloses."
  - Adjectives: "vibrant," "genuine," "honest," "exact" (unless part of an established technical term, like "exact match"), "pivotal," "robust," "innovative," "seamless," "cutting-edge," "groundbreaking," "multifaceted," "nuanced" (as a compliment), "comprehensive," "dynamic," "revolutionary," "game-changing," "unprecedented," "transformative," "invaluable," "meticulous," "intricate," "noteworthy."
  - Nouns: "interplay," "intricacies," "tapestry," "seam," "landscape" (metaphorical), "realm," "synergy," "testament," "underpinnings," "paradigm," "endeavor," "cornerstone," "catalyst," "beacon," "game-changer."
  - Adverbs: "genuinely," "honestly," "exactly" (unless part of an established technical term, like "exactly-once delivery"), "arguably," "undeniably," "remarkably," "notably," "importantly."
- No hedging language: "somewhat," "rather" (as a hedge or intensifier, e.g. "rather difficult," not the comparative "rather than"), "quite," "very," "fairly." State the claim at full strength.
- No meta-commentary about the article itself ("this article explores," "we will examine," "as discussed above").
- No academic register: avoid "it can be observed," "this suggests," "one might argue," "the literature indicates." Say the thing directly.
- Avoid lead-ins that introduce citations like footnotes, such as "Research by X shows that..." or "According to X..." Fold the person into the sentence naturally, or state the finding and cite it inline.
- Avoid vague attributions and overgeneralizations: "many experts believe," "researchers agree," "some argue," "critics say," "people often think," "it is widely accepted." Name the source or cut the attribution.
- Assertions must be backed by an inline citation using the cite shortcode: `{{< cite n "Author (Year). Title. Publisher." >}}`. Assign numbers sequentially in order of first appearance. All cited works must appear in the References section with a valid URL. Place the shortcode immediately after the preceding word with no space before it and no space after it: `word{{< cite 1 "..." >}}.` not `word {{< cite 1 "..." >}}.`
- Prefer original sources over third-party sources. Order of preference: the primary document (the paper, book, patent, statute, transcript, or firsthand account itself) > an official publisher or organization page > reputable journalism > secondary blogs or aggregators. Avoid citing Wikipedia, Medium, and other crowdsourced or blogging platforms when a primary or official source is available and verifiable. Only fall back to a third-party source when no better one exists. Crowdsourced sites and blogs are fine to use during research to locate and identify the underlying primary source, the restriction is on what gets cited, not what's read along the way. archive.org is a good tool for both, it often hosts the scanned original document itself (old journal issues, out-of-print books, primary records), which makes it a legitimate citation target, not just a research stop.
- The hero image alt text must be the frontmatter `summary` value verbatim: `![{summary text}](image-url)`.
- Never include tracking parameters in URLs: strip `utm_source=`, `utm_medium=`, `utm_campaign=`, `utm_term=`, `utm_content=`, and `referrer=` query arguments before using any URL.
- Avoid patterns that read as AI-generated:
  - Excessive parallelism in bullet lists.
  - Transitions that summarize what was just said.
  - Conclusions that restate the introduction verbatim.
  - Overly formal academic language.
  - The "it's not X, it's Y" reframe construction, including variants like "X, not Y" and "not X, but Y."
  - The "don't verb it, verb it" construction.
  - The "the X is real, and/not..." construction.
  - The trailing significance clause: a sentence ending in "...reflecting the growing need for," "...highlighting the importance of," or "...paving the way for future developments," instead of showing why something matters with specifics. Cut these entirely rather than trimming them.
  - Generic importance inflation: not everything is "a pivotal moment" or "a critical juncture." Describe things accurately.
  - Formulaic openings and closings: never open with "In today's..." or close with "In conclusion" or "In summary."

**Tone and humor:**
- Use dry humor to land key points, especially after concrete examples.
- Write as if the reader has already seen this pattern in their own org. Create recognition, not revelation. "Your team has probably run this playbook without calling it that."
- Be direct about obvious failures. Don't hedge around dysfunction. "The bank paid $3 billion in settlements to learn the difference." beats a paragraph of careful qualification.
- Openings can start mid-scene. Drop the reader into a specific situation without preamble. "Twelve open roles, 30 days left in the fiscal year." Context comes after.
- Irreverence is welcome. Corporate dysfunction and well-intentioned bad decisions are fair targets. The tone should be "smart friend who has seen this before," not consultant report.
- **Specificity is the humor vehicle.** A precise number or operational detail makes absurdity concrete without a punchline. "The team ran 14 postmortems, fixed 14 root causes, and had 14 new incidents the next quarter." hits harder than any clever observation about postmortem culture.
- **One-liner zingers after buildup.** After 3–4 sentences of context and tension, a standalone one- or two-word sentence lands hard. "It worked." "Nobody noticed." "They were wrong." Let it breathe as its own line, not tacked onto the prior sentence.
- **Understatement for high-stakes moments.** When describing something that's actually a big deal, use a flat, matter-of-fact tone. The gap between magnitude and casualness is where the humor lives. "We decided in the car to start over." beats "We made the difficult decision to fundamentally rethink our approach."
- **Name the pattern.** Give a recurring, unnamed situation a short label the reader can reuse in their own head, dropped once, plainly, mid-paragraph rather than as a section header. A recurring meeting that only exists because someone's calendar defaults to it becomes memorable the moment you call it the Zombie Meeting.
- **Undercut your own authority once, deliberately.** A brief, honest admission that the advice-giver is improvising too keeps the "smart friend" voice from tipping into "consultant with all the answers." Use it sparingly, once per post at most, and only where it's true.

### Structure
- **Word count**: 750–1,350 words (body content only, excluding frontmatter, References, Outtakes, and Changelog).
- **Frontmatter summary**: under 36 words. Two sentences maximum. No hedging or filler. SEO-optimized: work the post's primary keyword or topic phrase naturally into the first sentence.
- **Frontmatter tags**: Three tags maximum. Single words only. No hyphenated phrases.
- **Reading grade level**: 10–12 (Flesch-Kincaid or equivalent). Prefer concrete nouns and active verbs over abstract nominalizations. Use simple, direct language.
- **Section headings**: use `##` for top-level sections, `###` for subsections. Keep headings short (2–5 words). Make them opinionated or imperative rather than neutral noun phrases. "Throw away bad ideas" and "Fix the right thing" are sharper than "Idea Management" and "The Fix Process." Verb-first headings with a point of view in them.
- **Intro section**: Open with a specific, concrete situation or observation that creates tension or curiosity. Drop the reader mid-scene without preamble. Avoid dictionary definitions and "Have you ever..." constructions.

### Closing Section
Choose the title that best fits the post's intent:
- **"Put It Into Practice"** — when the post is actionable and the reader can change behavior immediately.
- **"What To Do About It"** — when the post diagnoses a problem and the closing is remediation-focused.
- **"How to Succeed"** — when the concept is a positive practice and the closing is about doing it well.
- **"Conclusion"** — when the concept is primarily explanatory and a direct call to action would feel forced.
- Other titles are allowed if none of the above fit.

Regardless of title, the closing section must be 2–3 paragraphs maximum, end with a direct call to action or closing thought, and not summarize what the article already said.

### Sections After Body
The body sections must be followed in this order:
1. `## [Further Reading Section]` — optional. Points the reader toward advanced topics the post doesn't cover. Use bold inline headers for each item (2–4 items). Each item names an advanced topic, describes what the reader will find, and includes an inline citation. Title options: "What This Doesn't Cover," "Go Further," "Keep Going," "Dig Deeper," or similar. Choose based on tone.
2. `## References` — an HTML `<ol class="references">` where each `<li id="ref-n">` matches the cite shortcode number. Format: author, year, title (use `<em>` for book and journal titles), publisher, and the full URL as link text. List items in order of first citation.
3. `## Outtakes` — optional. 2–5 short anecdotes that didn't fit the main article. See the Template for format rules.
4. `## Changelog` — one entry per calendar day, listed in reverse chronological order with the newest entry at the top. Format: `**YYYY-MM-DD** Brief description of changes.` If an entry for a given date already exists, fold new changes into that day's entry rather than adding a second entry for the same day. The first (oldest) entry, marking the post's creation, stays simple and needs no description of changes (for example `Initial release`, `Initial draft`, or `Initial publish`).

### Verification Checklist
Confirm every item below before reporting the work done. This is the single checklist for both authoring and editing.
- Word count 750–1,350 (body only, excluding frontmatter, References, Outtakes, Changelog).
- Frontmatter summary under 36 words, two sentences maximum.
- Frontmatter tags: three maximum, single words only.
- No em-dashes, en-dashes, or semicolons anywhere in the post.
- No curly/smart quotes anywhere.
- Colons minimized in prose (frontmatter and References formatting are exempt).
- No emoji.
- No rhetorical questions used as section openers or transitions.
- No filler phrases, banned phrases, or banned words (see Voice and Style) present.
- No repeated phrase, claim, or example, except at most one deliberate repetition for emphasis.
- No hedging language present.
- No meta-commentary about the article itself.
- No academic-register phrases.
- No footnote-style citation lead-ins.
- No vague attributions or overgeneralizations.
- Every assertion has an inline citation, numbered sequentially in order of first appearance.
- Every citation has a matching References entry with a valid URL.
- Cited sources prefer primary or official sources over third-party ones.
- Hero image alt text matches the frontmatter summary verbatim.
- No tracking parameters in any URL.
- No patterns that read as AI-generated.
- Section headings are 2–5 words, verb-first and opinionated.
- Intro opens mid-scene, no dictionary-definition opener.
- A critical section and a closing section are both present.
- Closing section is 2–3 paragraphs, ends with a direct call to action, and doesn't summarize the post.
- Sections after the body are in order: Further Reading (optional), References, Outtakes (optional), Changelog.
- Each Outtake is under 50 words and has a working href.
- Changelog has an entry for today, folded into an existing same-day entry if one already exists.

## Template

The full skeleton lives in `new-post-template.md`, in this directory. Read it when drafting a new post (step 3 below). Editing an existing post never needs it.

## Instructions

### Authoring a new post
1. Ask the user for the topic if not provided as an argument.
2. Research the topic: find the primary source(s) and 2–3 supporting references with valid URLs before drafting.
3. Read `new-post-template.md` and draft the full post using its skeleton. Set `draft: true`.
4. Run the Verification Checklist above.
5. Report word count and any rule violations found.

### Editing an existing post
Run two full passes. Complete all steps of pass 1 before starting pass 2.

**Pass 1**
1. Read the file before making any changes.
2. Apply the same writing rules above to all edits — voice, style, structure, citation, and word count constraints apply regardless of whether content is new or revised. Do not change the `draft` field.
3. Do a tightening pass: cut filler and redundant phrases per Voice and Style above, remove LLM-cliché transitions (e.g. "in conclusion"), and shorten without changing meaning.
4. Do a consistency pass: confirm all reference URLs are valid, confirm every factual assertion has an inline citation, confirm assertions are consistent with the evidence presented in the article, confirm that any example used more than once is described consistently each time.
5. After editing, update `lastmod` in the frontmatter to today's date.
6. Record the change in the Changelog: `**YYYY-MM-DD** Brief description.  ` (include the trailing whitespace). Entries run newest-first, so place today's entry at the top of the list. If an entry for today's date already exists, revise it to cover this change rather than adding a second entry for the same day.
7. Run the Verification Checklist above.
8. Report word count before and after, and any rule violations found.

**Pass 2**
9. Re-read the file as edited.
10. Repeat steps 3–4: a second tightening and consistency pass to catch anything the first pass missed.
11. Apply any remaining changes. If no changes remain, report "Pass 2: no further changes."
