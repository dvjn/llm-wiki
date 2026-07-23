---
name: llm-wiki
description: Build and maintain a knowledge wiki using LLMs. Use when a `wiki/` folder exists in the project OR the user explicitly asks to build a wiki. When wiki is present, query it first for project information and use it to document significant changes.
---

# LLM Wiki

Build and maintain a knowledge wiki using LLMs. The wiki accumulates understanding over time. You curate sources; the LLM writes and maintains pages.

## Lazy Loading

Load exactly one reference per task, when the task starts — never read all references.

| Task    | Load Only This                                           |
|---------|----------------------------------------------------------|
| Ingest  | `references/ingest.md`                                    |
| Query   | `references/query.md`                                     |
| Lint    | `references/lint.md`                                     |
| Setup   | `references/wiki-setup.md` (never otherwise)             |
| Create a page directly (no subagents) | `references/templates/<type>.md` (one of: concept, entity, decision, comparison, synthesis, source, stub) |

## Directory Structure

```
wiki/
├── raw/           # Immutable sources (read only)
├── pages/         # LLM-generated pages (concepts/, entities/, decisions/, comparisons/, synthesis/, sources/)
├── SCHEMA.md      # Per-project conventions: page types, domain rules, ingest scope
├── index.md       # Content catalog
└── log.md         # Append-only changelog
```

## Global Rules

- Links: `[[type-name]]` with prefix (concept, entity, decision, comparison, synthesis, source)
- Names: lowercase with hyphens
- Update `index.md` on page create/rename/material change
- Append to `log.md` on ingest, lint, and substantial synthesis
- Create stubs for important but underdeveloped concepts
- `wiki/SCHEMA.md` holds project-specific conventions; it co-evolves with the project

## Subagent Delegation

Subagents exist to keep wiki file access out of the main context: each wiki operation groups all its reads/writes into one subagent thread and returns a compact report. When subagents are available, prefer delegating over reading or writing wiki files in the main thread. Judgment (discussing takeaways, approving plans and fixes) happens in the main thread over reports, not raw material.

To delegate, spawn a subagent with a SHORT prompt — do not read the role file yourself:

> First read `<absolute path to skill>/references/agents/<role>.md` and follow everything below its `---` separator as your instructions. Then: <the task>.

Roles: `librarian` (all wiki reading: lookups, synthesis, gap checks, lint audit, source analysis — read-only), `researcher` (external research → raw notes in `wiki/raw/`; needs web access), `scribe` (all writing from a standalone brief — append the templates directory path). If the harness supports model selection, route read-only calls to a faster, cheaper model. The workflow references define the calls per operation.

Without subagent support, perform the operations directly in the main thread. The workflow remains the same either way.

## Gotchas

- Never run two scribes in parallel — `index.md`/`log.md` are shared write targets.
- The scribe has no conversation context: its brief must stand alone (decisions made, source paths, templates path).
- The researcher writes ONLY under `wiki/raw/`; pages/index/log belong to the scribe.
- Filing a single already-synthesized answer is cheaper inline than via a scribe — delegate writing only when it's multi-page (ingest, lint fixes).
- A single-call lint audit holds to roughly a 150-page wiki; beyond that, chunk the audit by page type.
- Don't pass a role prompt's body verbatim as the spawn prompt — have the subagent read the file itself, or you pay for the prompt twice in the main thread.
