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
