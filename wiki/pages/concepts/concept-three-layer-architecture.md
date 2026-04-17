# Three-Layer Architecture

> The structural foundation of the llm-wiki skill: immutable raw sources, LLM-generated pages, and a catalog/log pair.

## Core Definition

The wiki is organized into three distinct layers: raw source documents that are never modified, LLM-generated wiki pages that synthesize those sources, and a pair of operational files (index and log) that maintain the catalog and change history. Each layer has a clear, non-overlapping responsibility.

## Key Properties

- **Immutability of raw**: Files in `wiki/raw/` are never edited by the LLM. They serve as the ground truth from which synthesis is drawn.
- **LLM ownership of pages**: All content in `wiki/pages/` is written by the LLM, not manually. Humans curate sources; the LLM handles bookkeeping.
- **Catalog and log as infrastructure**: `index.md` and `log.md` are maintained automatically on every ingest, query filing, and lint pass.

## Layer Details

Karpathy names three explicit layers: **raw sources**, **the wiki**, and **the schema**. The schema layer is what makes the LLM "a disciplined wiki maintainer rather than a generic chatbot" — it should be co-evolved with the LLM as domain-specific conventions emerge ([[source-karpathy-llm-wiki]] §Architecture).

| Layer | Path | Mutability | Owner |
|-------|------|------------|-------|
| Raw sources | `wiki/raw/` | Immutable | Human (adds files) |
| Wiki pages | `wiki/pages/` | LLM-written | LLM |
| Catalog + log | `wiki/index.md`, `wiki/log.md` | Append/update | LLM |
| Schema | `wiki/SCHEMA.md` | Co-evolved | Human + LLM |

The schema always lives at `wiki/SCHEMA.md` — inside the wiki directory itself, not in any agent-specific config file. This keeps the wiki self-contained and portable across agents. See [[entity-schema-document]] and [[decision-schema-in-wiki-dir]].

## Subdirectory Layout

```text
wiki/raw/       specs/, papers/, articles/, assets/
wiki/pages/     concepts/, entities/, decisions/, comparisons/, synthesis/, sources/
```

## Related Concepts

- [[concept-compounding-wiki]] — why this architecture produces compounding value
- [[concept-ingest-workflow]] — how raw sources enter the wiki
- [[concept-type-prefixed-links]] — the linking convention used across pages

## Related Decisions

- [[decision-immutable-raw-sources]]

## Related Entities

- [[entity-skill-md]] — entrypoint that describes this architecture
- [[entity-index-md]] — the content catalog
- [[entity-log-md]] — the append-only log
