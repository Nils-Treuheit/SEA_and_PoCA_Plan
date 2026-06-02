# LLM Wiki — Your Second Brain

A structured, compounding knowledge base maintained by an LLM. You curate and query; the LLM reads, synthesizes, files, and maintains.

## Structure

```
.
├── CLAUDE.md      ← Schema: the rules the LLM follows (operating manual)
├── README.md      ← you are here
├── raw/           ← your source documents (immutable; never modify)
│   ├── articles/
│   ├── papers/
│   ├── reports/
│   ├── notes/
│   ├── webclips/
│   ├── media/
│   ├── data/
│   └── transcripts/
└── wiki/          ← LLM-written (don't hand-edit; tell the LLM to change)
    ├── index.md   ← content catalog
    ├── log.md     ← chronological log
    ├── entities/  ← people, companies, tools, places
    ├── concepts/  ← ideas, frameworks, methods
    ├── syntheses/ ← cross-source analysis, evolving thesis
    ├── comparisons/ ← tool vs tool, approach vs approach
    ├── reviews/   ← critical reviews, fact-checks
    ├── indices/   ← specialized indexes
    └── temp/      ← drafts (cleaned after merge)
```

## How to Start

1. Drop a source into `raw/` (an article, paper, note, anything).
2. Tell the LLM: **"Ingest [filename]"**.
3. The LLM reads it, creates pages, updates related pages, and files the result.

## Commands You Can Use

- **"Ingest [filename]"** — process a new source
- **"What do we know about [topic]?"** — query the wiki
- **"Create a page about [topic]"** — add without a source
- **"Run a lint pass"** — health check
- **"What gaps exist?"** — find missing knowledge
- **"What's new?"** — recent activity

## Key Files

- **CLAUDE.md** — full schema (page conventions, workflows, rules). The LLM follows this.
- **wiki/index.md** — what's in the wiki, organized by category.
- **wiki/log.md** — what's happened, in chronological order.

## Why This Works

The wiki is a **persistent, compounding artifact**. Cross-references are already there. Contradictions are flagged. Synthesis reflects everything you've read. The maintenance burden is near zero because the LLM handles it.

---

*Built with the LLM Wiki pattern. See the original idea doc for the full concept.*
