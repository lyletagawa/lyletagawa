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
- Vary sentence length for rhythm, but default short.
- No em-dashes, en-dashes, or semicolons. Rewrite to avoid them.
- Minimize colons in prose. Don't use a colon to introduce a list when a new sentence or fragments work better. "The cost is real. Back-and-forth, context switching, a slower pipeline." beats "The cost is real: back-and-forth, context switching, pipeline slowdown." Colons in frontmatter and References formatting are fine.
- No emoji.
- No rhetorical questions used as section openers or transitions.
- No filler phrases: "it's worth noting," "it's important to remember," "in other words," "at the end of the day," "needless to say," "this is crucial."
- No hedging language: "somewhat," "rather," "quite," "very," "fairly."
- No meta-commentary about the article itself ("this article explores," "we will examine," "as discussed above").
- No academic register: avoid "it can be observed," "this suggests," "one might argue," "the literature indicates." Say the thing directly.
- Avoid lead-ins that introduce citations like footnotes: "Research by X shows that..." or "According to X..." — fold the person into the sentence naturally or state the finding and cite it inline.
- Assertions must be backed by an embedded inline citation in the format (Author, Year). All cited works must appear in the References section with a valid URL.
- Avoid patterns that read as AI-generated: excessive parallelism in bullet lists, transitions that summarize what was just said, conclusions that restate the introduction verbatim, overly formal academic language.

### Structure
- **Word count**: 800–1,100 words (body content only, excluding frontmatter, References, and Changelog).
- **Frontmatter summary**: under 36 words. Two sentences maximum. No hedging or filler.
- **Frontmatter tags**: Three tags maximum. Single words only. No hyphenated phrases.
- **Reading grade level**: 10–12 (Flesch-Kincaid or equivalent). Prefer concrete nouns and active verbs over abstract nominalizations. Use simple, direct language.
- **Section headings**: use `##` for top-level sections, `###` for subsections. Keep headings short (3–5 words).

### Opening
The introductory section must earn the reader's attention. Open with a specific, concrete situation or observation that creates tension or curiosity. Hint at the resolution without giving it away. Avoid dictionary definitions as openers. Avoid "Have you ever..." constructions.

### Closing: "Put It Into Practice"
- The final content section must be titled exactly `## Put It Into Practice`.
- It must be short: 2–3 paragraphs maximum.
- It must end with a direct call to action — a specific thing the reader can do.
- Do not summarize what the article already said. Move forward.

### Sections After Body
The body sections must be followed in this order:
1. `## References` — all cited works, formatted as: `Author, First (Year). *Title*. Publisher. [url](url)` for books, or `Author, First (Year). "Title." *Journal/Publication*, Vol(Issue): pages. [url](url)` for articles and posts.
2. `## Changelog` — one entry per revision, format: `**YYYY-MM-DD** Brief description of changes.`

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
4. After drafting, verify: word count is 1,000–1,250, frontmatter summary is under 36 words, no em-dashes or en-dashes are present, all assertions have inline citations, all citations have corresponding references with URLs, a critical section and a closing section are present, the post ends with the closing section followed by References then Changelog.
5. Report word count and any rule violations found.

### Editing an existing post
1. Read the file before making any changes.
2. Apply the same writing rules above to all edits — voice, style, structure, citation, and word count constraints apply regardless of whether content is new or revised.
3. After editing, update `lastmod` in the frontmatter to today's date.
4. Add a Changelog entry describing the change: `**YYYY-MM-DD** Brief description.  ` (include the trailing whitespace)
5. Re-verify the full rule checklist: word count 1,000–1,250, no em-dashes or en-dashes or semicolons, all assertions cited, all citations in References with URLs.
6. Report word count and any rule violations found.
