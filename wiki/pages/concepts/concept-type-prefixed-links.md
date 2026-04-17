# Type-Prefixed Links

> A wiki-link naming convention that prepends the page type to every link name to prevent namespace collisions.

## Core Definition

All wiki links in the llm-wiki skill use a mandatory type prefix: `[[concept-name]]`, `[[entity-name]]`, `[[decision-name]]`, `[[comparison-name]]`, `[[synthesis-name]]`, `[[source-name]]`. The prefix is the first hyphen-delimited segment and must be one of the six allowed values.

## Key Properties

- **Collision prevention**: Concepts and entities frequently share names. The prefix makes each namespace unambiguous without needing per-page disambiguation.
- **Readability**: The prefix doubles as a visual type hint — a reader can immediately see whether a link points to a concept, entity, or decision.
- **Consistency**: All names are lowercase with hyphens. No camelCase, no spaces, no special characters.

## Allowed Prefixes

| Prefix | Page type | Example |
|--------|-----------|---------|
| `concept` | Domain concept | `[[concept-authentication]]` |
| `entity` | Concrete system object | `[[entity-auth-middleware]]` |
| `decision` | Design decision (ADR) | `[[decision-use-postgresql]]` |
| `comparison` | Trade-off analysis | `[[comparison-postgres-vs-sqlite]]` |
| `synthesis` | Evolving thesis / open question | `[[synthesis-design-tensions]]` |
| `source` | Raw source summary | `[[source-paper-consensus-raft]]` |

## When to Use

Create a link whenever referencing a concept or entity that already has (or should have) its own page. Missing links are a lint signal — the lint workflow flags frequently mentioned terms that lack pages.

## Related Concepts

- [[concept-three-layer-architecture]] — the structure that pages belong to
- [[concept-lint-workflow]] — catches missing or broken links

## Related Decisions

- [[decision-type-prefixed-links]] — the rationale for requiring prefixes
