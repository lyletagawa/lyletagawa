---
description: Author a new Hugo blog post or edit an existing one. Enforces voice, style, citation, and structure rules.
argument-hint: "[topic] or [path to existing post]"
---

# Blog Post

Author a new Hugo blog post or edit an existing one. Follow all rules below precisely.

## Writing Rules

### Voice and Style
- Write in second person ("you") or third person. Avoid first person.
- Casual, conversational tone. Write like a smart friend who read the book so the reader doesn't have to — not like a summary of the book itself.
- Use contractions freely. "You don't" beats "one does not."
- Prefer active voice. Passive voice distances the reader.
- Sentence fragments are fine for emphasis. Use them deliberately.
- Simple sentence structure. One idea per sentence. Short sentences are better.
- Vary sentence length for rhythm.
- Minimize consecutive short sentences. Three or more in a row feels staccato. Break the pattern with a longer sentence or by combining ideas.
- No em-dashes, en-dashes, or semicolons. Rewrite to avoid them.
- Use straight quotation marks (`"`) and straight apostrophes (`'`). Never use curly/smart quotes (`"` `"` `'` `'`).
- Minimize colons in prose. Don't use a colon to introduce a list when a new sentence or fragments work better. "The cost is real. Back-and-forth, context switching, a slower pipeline." beats "The cost is real: back-and-forth, context switching, pipeline slowdown." Colons in frontmatter and References formatting are fine.
- No emoji.
- Define obscure acronyms on first use: write out the full term followed by the abbreviation in parentheses, e.g. "feed-forward network (FFN)." Use the abbreviation alone on subsequent mentions.
- No rhetorical questions used as section openers or transitions.
- No filler phrases: "it's worth noting," "it's important to remember," "in other words," "at the end of the day," "needless to say," "this is crucial."
- Banned phrases: "is a testament," "underscores its importance/significance," "reflects broader," "symbolizing its ongoing/enduring/lasting," "setting the stage for," "marking/shaping the," "represents/marks a shift," "key turning point," "evolving landscape," "focal point," "indelible mark," "deeply rooted," "a vital/significant/crucial/pivotal/key role/moment," "boasts," "bolstered," "fostering," "garner," "interplay," "intricacies," "tapestry," "vibrant," "delve," "genuinely."
- No hedging language: "somewhat," "rather," "quite," "very," "fairly."
- No meta-commentary about the article itself ("this article explores," "we will examine," "as discussed above").
- No academic register: avoid "it can be observed," "this suggests," "one might argue," "the literature indicates." Say the thing directly.
- Avoid lead-ins that introduce citations like footnotes: "Research by X shows that..." or "According to X..." — fold the person into the sentence naturally or state the finding and cite it inline.
- Avoid vague attributions and overgeneralizations: "many experts believe," "researchers agree," "some argue," "critics say," "people often think," "it is widely accepted." Name the source or cut the attribution.
- Assertions must be backed by an embedded inline citation in the format (Author, Year). All cited works must appear in the References section with a valid URL.
- Never include tracking parameters in URLs: strip `utm_source=`, `utm_medium=`, `utm_campaign=`, `utm_term=`, `utm_content=`, and `referrer=` query arguments before using any URL.
- Avoid patterns that read as AI-generated: excessive parallelism in bullet lists, transitions that summarize what was just said, conclusions that restate the introduction verbatim, overly formal academic language, the "it's not X, it's Y" reframe construction.

**Tone and humor:**
- Use dry humor to land key points, especially after concrete examples.
- Write as if the reader has already seen this pattern in their own org. Create recognition, not revelation. "Your team has probably run this playbook without calling it that."
- Be direct about obvious failures. Don't hedge around dysfunction. "The bank paid $3 billion in settlements to learn the difference." beats a paragraph of careful qualification.
- Openings can start mid-scene. Drop the reader into a specific situation without preamble. "Twelve open roles, 30 days left in the fiscal year." Context comes after.
- Irreverence is welcome. Corporate dysfunction and well-intentioned bad decisions are fair targets. The tone should be "smart friend who has seen this before," not consultant report.
- **Specificity is the humor vehicle.** A precise number or operational detail makes absurdity concrete without a punchline. "The team ran 14 postmortems, fixed 14 root causes, and had 14 new incidents the next quarter." hits harder than any clever observation about postmortem culture.
- **One-liner zingers after buildup.** After 3–4 sentences of context and tension, a standalone one- or two-word sentence lands hard. "It worked." "Nobody noticed." "They were wrong." Let it breathe as its own line, not tacked onto the prior sentence.
- **Understatement for high-stakes moments.** When describing something that's actually a big deal, use a flat, matter-of-fact tone. The gap between magnitude and casualness is where the humor lives. "We decided in the car to start over." beats "We made the difficult decision to fundamentally rethink our approach."

### Structure
- **Word count**: 800–1,100 words (body content only, excluding frontmatter, References, Outtakes, and Changelog).
- **Frontmatter summary**: under 36 words. Two sentences maximum. No hedging or filler.
- **Frontmatter tags**: Three tags maximum. Single words only. No hyphenated phrases.
- **Reading grade level**: 10–12 (Flesch-Kincaid or equivalent). Prefer concrete nouns and active verbs over abstract nominalizations. Use simple, direct language.
- **Section headings**: use `##` for top-level sections, `###` for subsections. Keep headings short (3–5 words). Make them opinionated or imperative rather than neutral noun phrases. "Throw away bad ideas" and "Fix the right thing" are sharper than "Idea Management" and "The Fix Process." Verb-first headings with a point of view in them.
- **Intro section**: Open with a specific, concrete situation or observation that creates tension or curiosity. Drop the reader mid-scene without preamble. Avoid dictionary definitions and "Have you ever..." constructions.

### Closing Section
Choose the title that best fits the post's intent — see the Template for options. Regardless of title, the closing section must be 2–3 paragraphs maximum, end with a direct call to action or closing thought, and not summarize what the article already said.

### Sections After Body
The body sections must be followed in this order:
1. `## References` — all cited works, formatted as: `Author, First (Year). *Title*. Publisher. [url](url)` for books, or `Author, First (Year). "Title." *Journal/Publication*, Vol(Issue): pages. [url](url)` for articles and posts.
2. `## Outtakes` — optional. 2–4 short anecdotes that didn't fit the main article. See the Template for format rules.
3. `## Changelog` — one entry per revision, format: `**YYYY-MM-DD** Brief description of changes.`

## Template

Use this skeleton. Replace all placeholder values. Do not alter the frontmatter field names or section order.

```markdown
---
title: ""
date: YYYY-MM-DD
publishdate: YYYY-MM-DD
lastmod: YYYY-MM-DD
summary: ""
tags: []
image: 
draft: true
---

## [Title]

[Introductory paragraph(s). Concrete, specific, creates tension or curiosity. 2–3 short paragraphs maximum.]

## What Is [Title]?

[Define the concept with historical or foundational context. Cite the primary source. 2–3 paragraphs.]

## [Section titles and structure here are determined by the topic]

[Define as many sections as the topic warrants. Section titles should reflect the specific content, not a generic formula. A post on a paradox will have different natural sections than a post on a leadership practice or a technical pattern. Use bold inline headers, numbered subsections, or plain prose as the content dictates. Include concrete examples from real organizations with citations where appropriate.]

## [Critical Section]

Choose the section title that best fits the concept:
- **"Common Mistakes"** — when the primary risk is misapplication: people understand the concept but use it wrong.
- **"Counterarguments"** — when there are legitimate opposing positions worth steelmanning.
- **"Limits"** or **"Where This Breaks Down"** — when the concept is sound but has meaningful scope constraints or failure modes.
- Other titles are allowed if none of the above fit.

Regardless of title: use bold inline headers for each point, 3–5 items maximum, cite where applicable.

## [Closing Section]

Choose the section title that best fits the concept:
- **"Put It Into Practice"** — when the post is actionable and the reader can change behavior immediately.
- **"What To Do About It"** — when the post diagnoses a problem and the closing is remediation-focused.
- **"How to Succeed"** — when the concept is a positive practice and the closing is about doing it well.
- **"Conclusion"** — when the concept is primarily explanatory and a direct call to action would feel forced.
- Other titles are allowed if none of the above fit.

Regardless of title: 2–3 tight paragraphs maximum. End with a direct, specific call to action or closing thought. Do not summarize what the article already said.

---

## References

[Full citations for all inline references, alphabetical by author last name, each with a valid URL.]

---

## Outtakes

[Optional. 2–4 short anecdotes related to the topic that didn't fit the main article. Each should be fun, quirky, or surprising — complementary to the topic but outside its narrow scope. Format each as a bold title followed by prose. No bullet points. Each outtake must be under 50 words. Inline citations are allowed in (Author, Year) format where relevant, but Outtakes citations do not appear in the References section.]

---

## Changelog

**YYYY-MM-DD** Initial release.
```

## Instructions

### Authoring a new post
1. Ask the user for the topic if not provided as an argument.
2. Research the topic: find the primary source(s) and 2–3 supporting references with valid URLs before drafting.
3. Draft the full post using the template above. Set `draft: true`.
4. After drafting, verify: word count is 800–1,100, frontmatter summary is under 36 words, no em-dashes or en-dashes are present, all assertions have inline citations, all citations have corresponding references with URLs, a critical section and a closing section are present, the post ends with the closing section followed by References then Changelog.
5. Report word count and any rule violations found.

### Editing an existing post
1. Read the file before making any changes.
2. Apply the same writing rules above to all edits — voice, style, structure, citation, and word count constraints apply regardless of whether content is new or revised. Do not change the `draft` field.
3. Do a tightening pass: cut filler and redundant phrases, remove LLM-cliché transitions (e.g. "in conclusion," "it's worth noting," "it's important to remember"), and shorten without changing meaning.
4. Do a consistency pass: confirm all reference URLs are valid, confirm every factual assertion has an inline citation, confirm assertions are consistent with the evidence presented in the article, confirm that any example used more than once is described consistently each time.
5. After editing, update `lastmod` in the frontmatter to today's date.
6. Add a Changelog entry describing the change: `**YYYY-MM-DD** Brief description.  ` (include the trailing whitespace)
7. Re-verify the full rule checklist: word count 800–1,100, no em-dashes or en-dashes or semicolons, all assertions cited, all citations in References with URLs.
8. Report word count before and after, and any rule violations found.
