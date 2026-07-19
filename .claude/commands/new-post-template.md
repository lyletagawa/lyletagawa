---
description: Skeleton for drafting a new blog post. Reference only, read from new-post.md, not invoked directly.
disable-model-invocation: true
---

# Blog Post Template

Used by `new-post.md`, "Authoring a new post" step 3. Replace all placeholder values. Do not alter the frontmatter field names or section order. Title-choice and formatting rules for the bracketed sections are in `new-post.md` (Closing Section, Sections After Body).

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

![{{summary}}]()
*"Caption text." Photo: [Author (Year)](url). License.*

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

[Choose the title and write the content per the Closing Section rules in new-post.md.]

## [Further Reading Section]

[Choose the title and write the content per the Sections After Body rules in new-post.md.]

---

## References

<ol class="references">
  <li id="ref-1">Author, First (Year). <em>Title</em>. Publisher. <a href="url">url</a></li>
</ol>

---

## Outtakes

[Optional. 2–4 short anecdotes related to the topic that didn't fit the main article. Each should be fun, quirky, or surprising — complementary to the topic but outside its narrow scope. Format each as a bold title followed by prose. No bullet points. Each outtake must be under 50 words. Every factual claim needs a citation in `([Author, Year](url))` format, the author/year wrapped in a markdown link to the source, so the reader can click through. Outtakes citations do not get a numbered `{{< cite n >}}` shortcode and do not appear in the References section, but the href is still required.]

---

## Changelog

**YYYY-MM-DD** Initial release.
```
