# Entity: log.md

> The append-only chronological record of every ingest, lint pass, and substantial filed synthesis.

## Purpose

`wiki/log.md` provides a durable audit trail of how the wiki evolved. It answers "what changed and when" without requiring git blame on individual pages. Every operation that changes wiki state must append an entry.

## Structure

Plain markdown with one `## [YYYY-MM-DD] {operation} | {title}` header per entry, followed by a brief prose description of what changed.

## Grep Parseability

Because every entry starts with `## [YYYY-MM-DD]`, the log is parseable with standard Unix tools:

```sh
grep "^## \[" log.md | tail -5   # last 5 entries
```

This lets the LLM quickly orient itself to recent activity at the start of a session ([[source-karpathy-llm-wiki]] §Indexing and logging).

## Entry Types

| Operation | When to append |
|-----------|---------------|
| `init` | Wiki bootstrap |
| `ingest` | Source added and folded into wiki |
| `lint` | Health check pass completed |
| `filing` | Synthesis promoted from query to a wiki page |

## Entry Format

```markdown
## [YYYY-MM-DD] ingest | Source Title

Added `raw/papers/paper-example.md`. Created source summary. Updated N concept pages (...). Created N new pages (...). Updated index.
```

## Lifecycle

Bootstrapped during wiki setup. Strictly append-only — no existing entries are ever edited or deleted.

## Related Concepts

- [[concept-three-layer-architecture]] — log.md is part of the catalog+log infrastructure layer
- [[concept-ingest-workflow]] — always appends a log entry at the end
- [[concept-lint-workflow]] — always appends a log entry at the end

## Related Entities

- [[entity-index-md]] — catalogs current state; log records the history
