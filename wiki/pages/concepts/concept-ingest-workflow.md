# Ingest Workflow

> The process of reading a raw source and folding its knowledge into the wiki's concept, entity, decision, and synthesis pages.

## Core Definition

Ingest is the primary mechanism by which knowledge enters the wiki. When a new source is added to `wiki/raw/`, the LLM reads it, creates a source summary page, updates all affected wiki pages, and logs the change. A single ingest can touch many pages — that is expected and desirable.

## Key Properties

- **Source summary always created**: Every ingested document gets a corresponding page in `wiki/pages/sources/`.
- **Cascading updates**: The ingest touches every concept, entity, decision, or comparison page affected by the source, not just a summary.
- **Stub creation**: If a concept clearly needs its own page but cannot yet be fully synthesized, a stub is created rather than leaving the term untracked.
- **Index and log updated**: Every ingest ends by updating `wiki/index.md` and appending to `wiki/log.md`.

## Ingest Steps

1. Read the source document thoroughly.
2. Discuss key takeaways when that improves synthesis (especially early in a new wiki).
3. Create a source summary page in `wiki/pages/sources/`.
4. Update relevant concept/entity/decision/comparison/synthesis pages.
5. Create new pages when the source introduces durable, wiki-worthy material.
6. Update `wiki/index.md`.
7. Append an entry to `wiki/log.md`.

## Related Concepts

- [[concept-compounding-wiki]] — ingest is how the wiki compounds over time
- [[concept-three-layer-architecture]] — raw sources live in `wiki/raw/`, pages in `wiki/pages/`
- [[concept-lint-workflow]] — lint catches gaps that ingest leaves behind

## Related Decisions

- [[decision-immutable-raw-sources]] — raw files are never modified during ingest
