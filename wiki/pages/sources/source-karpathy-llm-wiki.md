# Source: LLM Wiki (Karpathy)

**Type**: article
**URL**: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
**Date Added**: 2026-04-17
**Key Sections**: The core idea, Architecture, Operations, Indexing and logging, Optional: CLI tools, Tips and tricks, Why this works

## Summary

Andrej Karpathy's original formulation of the LLM Wiki pattern. The document argues that RAG systems fail to accumulate knowledge — they rediscover from raw documents on every query — whereas a wiki maintained by an LLM becomes a persistent, compounding artifact. It describes a three-layer architecture (raw sources, wiki pages, schema/config), three core operations (ingest, query, lint), and the roles of `index.md` and `log.md`. The document is intentionally abstract — it describes the pattern, not a specific implementation, and invites the reader to instantiate it with their LLM agent.

## Key Extracts

> "The wiki is a persistent, compounding artifact. The cross-references are already there. The contradictions have already been flagged. The synthesis already reflects everything you've read."

> "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase."

> "The tedious part of maintaining a knowledge base is not the reading or the thinking — it's the bookkeeping... Humans abandon wikis because the maintenance burden grows faster than the value."

> "The idea is related in spirit to Vannevar Bush's Memex (1945) — a personal, curated knowledge store with associative trails between documents. The part he couldn't solve was who does the maintenance. The LLM handles that."

## Architecture as Defined by Karpathy

Karpathy names three layers explicitly:
1. **Raw sources** — immutable, LLM reads but never modifies
2. **The wiki** — LLM-owned markdown files; "You read it; the LLM writes it"
3. **The schema** — an agent config or instruction file; config that makes the LLM "a disciplined wiki maintainer rather than a generic chatbot" (in this wiki, always `wiki/SCHEMA.md`)

Note: In this wiki, `wiki/SCHEMA.md` is the schema layer — kept inside the wiki directory so the wiki is agent-agnostic. See [[decision-schema-in-wiki-dir]].

## Key Insights Not Yet in Wiki

- **Scale guidance**: index.md works at ~100 sources / hundreds of pages without needing embedding-based RAG
- **Schema co-evolution**: The schema is meant to be co-evolved with the LLM over time as you learn what works for your domain
- **Output format variety**: Answers can be filed as markdown pages, comparison tables, Marp slide decks, matplotlib charts
- **CLI tools**: `qmd` as a local BM25/vector hybrid search engine when the wiki outgrows index-only navigation
- **Grep parseable log tip**: `grep "^## \[" log.md | tail -5`
- **Use cases**: personal, research, book notes, business/team, competitive analysis, course notes

## Related Concepts

- [[concept-compounding-wiki]] — directly instantiates Karpathy's core thesis
- [[concept-three-layer-architecture]] — maps to Karpathy's raw/wiki/schema layers
- [[concept-ingest-workflow]] — described in §Operations
- [[concept-query-workflow]] — described in §Operations
- [[concept-lint-workflow]] — described in §Operations

## Related Entities

- [[entity-skill-md]] — this skill's equivalent of Karpathy's schema layer
- [[entity-index-md]] — detailed in §Indexing and logging with scale guidance
- [[entity-log-md]] — detailed in §Indexing and logging with grep tip

## Impact

This is the founding document for the pattern this skill implements. Every major design decision in the skill traces back to claims made here.
