# Wiki Conventions

Shared rules for naming, links, and page creation.

## Naming

- Files in `wiki/raw/`: `{type}-{short-title}.md` (specs/, papers/, articles/)
- Wiki pages: lowercase with hyphens, matching their `[[link-name]]`

## Link Prefix System

All wiki links use mandatory type prefix: `{type}-{name}`

Allowed prefixes: `concept`, `entity`, `decision`, `comparison`, `synthesis`, `source`

Examples:
- `[[concept-three-phase-commit]]`
- `[[source-paper-consensus-raft]]`
- `[[comparison-postgres-vs-mysql]]`

Always link when mentioning a concept/entity that has its own page.

## When To Create Links

- Link when a concept/entity has a page
- Add cross-references during ingest/lint when pages should connect

## When To Create Pages

Create when:
- Concept/entity appears across 3+ sources and needs synthesis
- Design decision should be recorded
- Comparison is valuable enough to persist
- Answer synthesizes multiple sources and is reusable

## Stub Pages

For important but underdeveloped concepts:

```markdown
# [Concept Name]

> Brief definition

## Status
Stub - needs expansion

## Notes
[Brief notes]
```

## Index Format

`index.md` structure:
```markdown
## Concepts
- [[concept-name]] - one-line description

## [other types follow same pattern]
```

Update on ingest, lint (if pages change), and substantial filed synthesis.

## Log Format

`log.md` is append-only:
```markdown
## [YYYY-MM-DD] ingest | [Title]
[What changed, pages created/updated]
```

## Raw Sources

Files in `wiki/raw/` are immutable. Read only.
