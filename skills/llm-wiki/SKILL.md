---
name: llm-wiki
description: Build and maintain a knowledge wiki using LLMs. This skill manages a three-layer architecture (raw sources, LLM-generated wiki pages, and schema documentation) with workflows for ingesting sources, querying knowledge, and linting for consistency. Use this skill whenever the user wants to create a knowledge base, manage research notes, build a wiki, ingest documents for synthesis, or maintain structured documentation that compounds over time. Essential for research projects, book notes, competitive analysis, course notes, or any domain where knowledge accumulates and cross-references matter.
---

# LLM Wiki Skill

Build and maintain a knowledge wiki using LLMs. The wiki accumulates understanding over time. You curate sources; the LLM writes and maintains pages.

## ⚠️ LAZY LOADING REQUIREMENT

**Only load references when explicitly needed. Never read all references.**

| Task    | Load Only This                                           |
|---------|----------------------------------------------------------|
| Ingest  | `references/ingest.md`                                    |
| Query   | `references/query.md`                                     |
| Lint    | `references/lint.md`                                     |
| Setup   | `references/wiki-setup.md` (never otherwise)             |
| Create  | `references/page-templates.md` (load ONE template only) |

**Violations:** Loading conventions.md for query, loading wiki-setup.md when wiki exists, reading all 6 templates when creating one page.

## Directory Structure

```
wiki/
├── raw/           # Immutable sources (read only)
├── pages/         # LLM-generated pages (concepts/, entities/, decisions/, comparisons/, synthesis/, sources/)
├── index.md       # Content catalog
└── log.md         # Append-only changelog
```

## Global Rules

- Links: `[[type-name]]` with prefix (concept, entity, decision, comparison, synthesis, source)
- Names: lowercase with hyphens
- Update `index.md` on page create/rename/material change
- Append to `log.md` on ingest, lint, and substantial synthesis
- Create stubs for important but underdeveloped concepts
- **Prefer helper scripts over manual file editing** — scripts enforce naming conventions and formatting automatically

## Helper Scripts

Use these scripts to reduce repetitive work:

| Script                      | Purpose                           | Usage                                    |
| --------------------------- | --------------------------------- | ---------------------------------------- |
| `scripts/wiki-new-page`       | Create pages with correct format  | `wiki-new-page <type> <name>`             |
| `scripts/wiki-update-index`   | Add/remove index entries          | `wiki-update-index add <type> <name> <desc>` |
| `scripts/wiki-log`            | Append standardized log entries   | `wiki-log ingest <title> <summary>`       |

**Always prefer scripts over manual file editing.** Scripts enforce naming conventions and formatting automatically.

## Workflow Routing

### Ingest
New source → fold into wiki. Read `references/ingest.md`. Uses `scripts/wiki-new-page`, `scripts/wiki-update-index`, `scripts/wiki-log`.

### Query
Answer from wiki. Read `references/query.md`. Suggest filing answers with `scripts/wiki-*`.

### Lint
Health check. Read `references/lint.md`. Uses `scripts/wiki-log`.

### Setup
Initialize new wiki. Read `references/wiki-setup.md`.

### Create Pages
Need a template. Read `references/page-templates.md` and load ONLY the template for the page type you're creating.
