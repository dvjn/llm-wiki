# Entity: index.md

> The content catalog — auto-maintained by the LLM on every ingest, query filing, and lint pass.

## Purpose

`wiki/index.md` is the single entry point for the query workflow. It lists every wiki page with a one-line description, organized by type. The LLM reads it first on every query to locate relevant pages without scanning the whole wiki.

## Structure

Organized into standard sections:

- **Concepts** — domain concepts listed as `[[concept-name]] - one-line description`
- **Entities** — concrete system objects
- **Decisions** — design decisions
- **Comparisons** — trade-off analyses
- **Sources** — per-source summaries
- **Statistics** — counts of raw sources, wiki pages, concepts, entities

## Scale Characteristics

Index-only navigation works at moderate scale (~100 sources, hundreds of pages) without needing embedding-based RAG infrastructure ([[source-karpathy-llm-wiki]] §Indexing and logging). Beyond that, a dedicated search tool like `qmd` (BM25/vector hybrid) can be added as a CLI or MCP server.

## Update Obligations

The LLM must update `index.md`:
- After every ingest (new pages added)
- After every query that files new synthesis
- After every lint pass that creates, renames, or removes pages

## Lifecycle

Bootstrapped during wiki setup. Never deleted. Grows as the wiki grows.

## Related Concepts

- [[concept-three-layer-architecture]] — index.md is part of the catalog+log infrastructure layer
- [[concept-query-workflow]] — every query starts by reading index.md
- [[concept-lint-workflow]] — orphan detection starts from the index

## Related Entities

- [[entity-log-md]] — the sibling infrastructure file; log records changes, index catalogs state
