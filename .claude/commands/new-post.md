# New Blog Post

Author a new Hugo blog post for this site. Follow all rules below precisely.

## Writing Rules

### Voice and Style
- Write in second person ("you") or third person. Avoid first person.
- Short sentences. Vary sentence length for rhythm, but default short.
- No em-dashes, en-dashes, or semicolons. Rewrite to avoid them.
- No emoji.
- No rhetorical questions used as section openers or transitions.
- No filler phrases: "it's worth noting," "it's important to remember," "in other words," "at the end of the day," "needless to say," "this is crucial."
- No hedging language: "somewhat," "rather," "quite," "very," "fairly."
- No meta-commentary about the article itself ("this article explores," "we will examine," "as discussed above").
- Assertions must be backed by an embedded inline citation in the format (Author, Year). All cited works must appear in the References section with a valid URL.
- Avoid patterns that read as AI-generated: excessive parallelism in bullet lists, transitions that summarize what was just said, conclusions that restate the introduction verbatim.

### Structure
- **Word count**: 1,000–1,250 words (body content only, excluding frontmatter, References, and Changelog).
- **Frontmatter summary**: under 36 words. Two sentences maximum. No hedging or filler.
- **Reading grade level**: 12–14 (Flesch-Kincaid or equivalent). Prefer concrete nouns and active verbs over abstract nominalizations.
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

## Common Mistakes

[One section covering counterpoints: common mistakes people make applying this concept, common misconceptions about it, and known liabilities or failure modes. Use bold inline headers for each point. 3–5 items maximum. Cite where applicable.]

## Put It Into Practice

[2–3 tight paragraphs. End with a direct call to action.]

## References

[Full citations for all inline references, alphabetical by author last name, each with a valid URL.]

## Changelog

**YYYY-MM-DD** Initial publication.
```

## Instructions

When invoked:
1. Ask the user for the topic if not provided as an argument.
2. Research the topic: find the primary source(s) and 2–3 supporting references with valid URLs before drafting.
3. Draft the full post using the template above.
4. After drafting, verify: word count is 1,000–1,250, frontmatter summary is under 36 words, no em-dashes or en-dashes are present, all assertions have inline citations, all citations have corresponding references with URLs, a Common Mistakes section is present, the post ends with Put It Into Practice followed by References then Changelog.
5. Report word count and any rule violations found.
