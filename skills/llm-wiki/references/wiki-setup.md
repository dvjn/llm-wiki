# Wiki Setup Guide

**Only read when initializing a new wiki. Never otherwise.**

## Bootstrap Structure

```
wiki/
├── raw/
│   ├── specs/
│   ├── papers/
│   ├── articles/
│   └── assets/
├── pages/
│   ├── concepts/
│   ├── entities/
│   ├── decisions/
│   ├── comparisons/
│   ├── synthesis/
│   └── sources/
├── SCHEMA.md
├── index.md
└── log.md
```

Create full tree even if some directories start empty.

## Minimal Starter Files

`index.md`:
```markdown
# Wiki - Content Catalog

> Auto-updated on ingest, query filing, and lint.

## Concepts
## Entities
## Decisions
## Comparisons
## Synthesis
## Sources
```

`log.md`:
```markdown
# Wiki Log

Append-only record of wiki operations.
```

`SCHEMA.md` — the per-project schema layer; write it customized to THIS project (ask the user about their domain if unclear):
```markdown
# Wiki Schema — [Project Name]

[One paragraph: what this wiki tracks and why.]

## Page Types in Use
[The standard type table, trimmed/extended for the domain — e.g. add when-to-create guidance per type.]

## Domain-Specific Conventions
[Naming, linking, or content rules specific to this project.]

## Ingest Scope
[What kinds of sources belong in this wiki.]
```

The schema co-evolves with the project — subagents and workflows consult it for domain rules, so keep it current as conventions emerge.

## Customization

Start default; adapt only when domain needs it (record adaptations in SCHEMA.md):
- `raw/rfcs/` for standards-heavy work
- `pages/decisions/` for architecture wikis
- Emphasize `pages/synthesis/` for research

## AGENTS.md Integration — STRONGLY RECOMMENDED

After creating the wiki structure, you MUST add the "Knowledge Base - Wiki" section to AGENTS.md in the project root.

- If AGENTS.md exists: Add the wiki section using ONLY the template at `references/templates/agents-md-wiki-section.md`. Do NOT add anything extra.
- If AGENTS.md doesn't exist: Create it with ONLY the wiki section using the template

This makes the wiki discoverable by any AGENTS.md-reading agent. DO NOT skip this step. Use the template content verbatim — do not paraphrase, expand, or add any other content to this section.