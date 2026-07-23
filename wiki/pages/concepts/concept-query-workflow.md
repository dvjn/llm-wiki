# Query Workflow

> The process of answering a question by reading the relevant wiki pages and synthesizing an answer with citations, then optionally filing the synthesis back.

## Core Definition

The query workflow starts from `wiki/index.md` to identify relevant pages, reads only what is needed, and produces a synthesized answer with section-level citations. Substantial synthesis is filed back into the wiki to prevent knowledge from being lost in chat history.

## Key Properties

- **Index-first navigation**: Always start from the catalog, not by scanning all pages.
- **Section-level citations**: Answers cite `[[concept-x]] §Section Name`, not just page names.
- **Filing loop**: Durable synthesis is promoted into wiki pages (concept, comparison, decision, or synthesis) after answering.
- **Gap surfacing**: Every query is also a lightweight lint pass — missing or contradictory content is noted.
- **Follow-up drilling**: After answering, suggest 2-3 follow-up questions that would deepen the thread.

## Query Types

| Type | Description | Reading depth (in the reader's thread) |
|------|-------------|----------------------------------------|
| Lookup | Retrieve a specific fact or definition | index + 1-2 pages |
| Synthesis | Connect concepts across multiple pages | index + all relevant pages |
| Gap/contradiction | Health-check coverage or consistency | index + affected pages |

With subagents, a query is one librarian call by default: the reading happens in the subagent's thread and only a cited answer, pages consulted, and gaps noticed enter the main context ([[decision-subagent-delegation]]). Exception: a lookup may run inline when the index is already resident and the fact sits in 1-2 known pages. Filed synthesis is written inline (the answer is already in the main context); questions the wiki cannot answer can dispatch external research ([[entity-wiki-researcher]]) to capture sources for ingest. Without subagents, the same reading depths apply directly.

## Filing Threshold

File when the answer synthesizes multiple pages. File directly (without prompting) when the synthesis is clearly durable.

## Related Concepts

- [[concept-compounding-wiki]] — filing answers back is how synthesis compounds
- [[concept-type-prefixed-links]] — citation format uses prefixed links
- [[concept-lint-workflow]] — gap surfacing during queries is a mini lint pass

## Related Entities

- [[entity-index-md]] — the starting point for every query
- [[entity-wiki-librarian]] — one call per query, whatever the type
- [[entity-wiki-scribe]] — files synthesis back into the wiki
