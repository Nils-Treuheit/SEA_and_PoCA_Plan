# LLM Wiki — Schema & Rules

This file is the operating manual for this LLM Wiki. Follow these rules on every interaction.

---

## 1. Purpose

This is a **persistent, compounding knowledge base**. The wiki lives between you (the curator) and your raw source documents. You source and explore; the LLM reads, synthesizes, files, cross-references, and maintains.

---

## 2. Directory Structure

```
.
├── CLAUDE.md          ← you are here (the schema; we co-evolve this)
├── README.md          ← (optional) brief description for humans
│
├── raw/               ← immutable sources (never modify; only add)
│   ├── articles/      ← web articles (from Obsidian Web Clipper or similar)
│   ├── papers/        ← academic papers, preprints
│   ├── reports/       ← business reports, analyses, whitepapers
│   ├── notes/         ← personal notes, meeting notes, transcripts
│   ├── webclips/      ← Obsidian Web Clipper exports
│   ├── media/         ← downloaded images, screenshots
│   ├── data/          ← CSVs, JSONs, datasets
│   └── transcripts/   ← call recordings transcripts, podcast notes
│
├── wiki/              ← LLM-written (never hand-edit except for notes)
│   ├── index.md       ← content catalog (category-grouped page list with summaries)
│   ├── log.md         ← chronological append-only record
│   │
│   ├── entities/      ← people, companies, tools, places, things
│   ├── concepts/      ← ideas, frameworks, theories, methods
│   ├── comparisons/   ← tool vs tool, approach vs approach
│   ├── syntheses/     ← cross-source analysis, evolving thesis, summaries
│   ├── reviews/       ← critical reviews, fact-checks, gap analyses
│   ├── indices/       ← specialized indexes (by tag, by date, by source, etc.)
│   └── temp/          ← draft pages, work-in-progress (clean out after merge)
│
└── .obsidian/         ← Obsidian config (unmanaged by this wiki; exists for browsing)
```

---

## 3. Source Protocol (raw/)

- **Never modify** anything in `raw/`. New sources → always a new file.
- Name sources descriptively: `YYYY-MM-DD_descriptive-name.ext` (e.g., `2026-06-02_attention-is-all-you-need.pdf`).
- Download images inline when clipping articles.
- If a source has multiple formats (PDF + HTML), keep the richer one and symlink or note the other.

---

## 4. Wiki Page Conventions

### Page Frontmatter (every wiki page)

```yaml
---
title: "Page Title"
type: entity | concept | comparison | synthesis | review | index
tags: [tag1, tag2]
sources: [source-file-name.md, source-file-name.pdf]
created: 2026-06-02
updated: 2026-06-02
status: draft | stable | superseded
related: [[page1]], [[page2]]
---
```

### Page Format

```markdown
# Title (from frontmatter title)

## Summary
One-paragraph overview of what this page covers.

## Key Points
- Bullet points of the main findings or claims
- Include specific numbers, names, dates where relevant

## Details
Full write-up with sections as needed.

## Sources
- [Source Name](../../raw/path/to/source) — date ingested, confidence (high/medium/low)

## Related
- [[page1]]
- [[page2]]

## Questions
Open questions this page raises (for later investigation).
```

### Page Naming
- Use **kebab-case** with full descriptive names: `attention-mechanism.md`, not `att.md`
- Entity pages: use the standard name (e.g., `adam-optimizer.md`, not `adam.md` if ambiguous)
- Avoid overly generic names: `transformer.md` → `transformer-architecture.md` if needed

---

## 5. Index File (wiki/index.md)

Regenerated on every ingest. Format:

```markdown
# Wiki Index

> Last updated: YYYY-MM-DD HH:MM
> Total pages: N | Total sources: M

## Entities
| Name | Summary | Updated |
|------|---------|---------|
| [[page-name]] | One-line summary | YYYY-MM-DD |

## Concepts
| Name | Summary | Updated |
|------|---------|---------|
| [[page-name]] | One-line summary | YYYY-MM-DD |

## Syntheses
| Name | Summary | Updated |
|------|---------|---------|
| [[page-name]] | One-line summary | YYYY-MM-DD |

## Comparisons
| Name | Summary | Updated |
|------|---------|---------|
| [[page-name]] | One-line summary | YYYY-MM-DD |

## Reviews
| Name | Summary | Updated |
|------|---------|---------|
| [[page-name]] | One-line summary | YYYY-MM-DD |
```

---

## 6. Log File (wiki/log.md)

Append-only. Every action gets a log entry. Format:

```markdown
# Wiki Log

## [YYYY-MM-DD] ingest | Source Title
- **Source**: [name](../../raw/path/to/file)
- **Summary**: One-line summary
- **Pages created**: [[page1]], [[page2]]
- **Pages updated**: [[page3]], [[page4]]
- **Notes**: Any relevant notes

## [YYYY-MM-DD] query | Question Topic
- **Question**: "What was asked?"
- **Answer**: [[result-page]] or summary
- **Insights**: Key takeaways

## [YYYY-MM-DD] lint
- **Issues found**: contradictions, orphans, stale claims
- **Actions taken**: what was fixed

## [YYYY-MM-DD] schema | Schema Change
- **Change**: What was modified in this schema file
- **Reason**: Why
```

---

## 7. Ingest Workflow

When you add a source to `raw/` and tell me to process it:

1. **Read** the source. If it's an article from Obsidian Web Clipper, read the markdown first, then view referenced images if needed.
2. **Identify** what kind of content it is and where it belongs (entities, concepts, syntheses, comparisons).
3. **Create summary page** in the appropriate wiki subdirectory.
4. **Update** any existing pages that the source affects (correct, add claims, flag contradictions).
5. **Update `wiki/index.md`** with the new page.
6. **Append to `wiki/log.md`**.
7. **Report** what you did and ask if you want me to go deeper on any aspect.

### Ingest Depth

- **Light**: summary page + log entry (for low-value sources).
- **Standard**: full write-up + updates to related pages (default).
- **Deep**: full write-up + contradiction analysis + gap identification + new entity pages for anything significant not yet in the wiki (for high-value sources).

---

## 8. Query Workflow

When you ask a question:

1. **Read `wiki/index.md`** first to find relevant pages.
2. **Drill into** the most relevant pages (read full content, not just frontmatter).
3. **Synthesize** an answer citing specific pages and sources.
4. **Create a new wiki page** if the answer is substantial or could be filed for future reference.
5. **Append to `wiki/log.md`**.

---

## 9. Lint Workflow

Periodically (or when you ask), I will:

1. **Check contradictions**: cross-reference pages for conflicting claims.
2. **Check staleness**: identify claims that newer sources have superseded.
3. **Check orphans**: pages with no inbound links or no `related:` frontmatter entries.
4. **Check gaps**: important topics mentioned across pages but lacking their own page.
5. **Report** findings and suggest fixes. You approve, I apply.

---

## 10. Cross-Reference Rules

- Every page MUST have a `related:` frontmatter field listing at least one other page.
- Every page MUST have a `Sources:` section with clickable links back to raw sources.
- When updating a page, add or update its `updated:` timestamp.
- When creating a page, update ALL related pages with a back-reference to the new page.
- Use Obsidian link syntax: `[[page-name]]`. Include full relative path if in a different directory: `[[entities/page-name]]`.

---

## 11. Status Conventions

- `draft` — newly created, not yet reviewed for completeness.
- `stable` — reviewed and believed to be reasonably complete.
- `superseded` — newer source(s) have made this inaccurate or outdated. Don't delete; just mark and point to the updated page.

---

## 12. How to Interact With Me

You can say things like:

- **"Ingest [filename]"** — process a new source.
- **"What do we know about [topic]?"** — query the wiki.
- **"Create a page about [topic]"** — add something without a source.
- **"Run a lint pass"** — health check.
- **"Update the index"** — regenerate wiki/index.md.
- **"What's new?"** — show recent log entries.
- **"What gaps exist?"** — find missing knowledge.
- **"Show me the graph"** — describe the wiki's structure (entities as hubs, concepts as bridges).

---

## 13. Schema Evolution

This file IS the schema. We will evolve it together:

- When you notice a pattern that works, tell me to formalize it here.
- When something doesn't work, we'll debug and update.
- Never modify this file yourself — tell me what to change and I'll do it.
