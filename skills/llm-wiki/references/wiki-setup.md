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
## Sources
```

`log.md`:
```markdown
# Wiki Log

Append-only record of wiki operations.
```

## Customization

Start default; adapt only when domain needs it:
- `raw/rfcs/` for standards-heavy work
- `pages/decisions/` for architecture wikis
- Emphasize `pages/synthesis/` for research

## AGENTS.md Integration

After creating the wiki structure, check if AGENTS.md exists in the project root.

- If AGENTS.md exists: Suggest adding the "Knowledge Base - Wiki" section (see templates/agents-md-wiki-section.md)
- If AGENTS.md doesn't exist: Suggest creating it with the wiki section

This makes the wiki discoverable by any AGENTS.md-reading agent.