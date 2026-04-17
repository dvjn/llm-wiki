# Lint Workflow

> A health-check pass over the wiki that finds broken links, orphan pages, stale claims, missing concepts, and contradictions.

## Core Definition

Linting scans all wiki pages for structural and semantic issues that accumulate over time: links pointing to nonexistent pages, pages with no inbound links, frequently mentioned terms without their own pages, and statements that conflict across pages. The LLM proposes fixes and applies non-trivial ones after confirmation.

## Key Properties

- **Structural checks**: Broken links, orphan pages (no inbound links), pages missing from the index.
- **Semantic checks**: Stale claims superseded by newer sources, contradictions between pages.
- **Gap detection**: Frequently mentioned terms that still lack pages; research gaps that could be resolved by adding sources.
- **Log entry required**: Every lint pass appends a summary entry to `wiki/log.md`.

## Lint Checks (in order)

1. Scan for contradictions across related pages.
2. Identify stale claims that newer sources supersede.
3. Find orphan pages with no inbound links from the index or other pages.
4. Identify frequently mentioned terms that still lack their own pages.
5. Find missing cross-references between clearly related pages.
6. Identify research gaps resolvable by adding sources or running a web search.

## Typical Fixes

- Repair broken or missing internal links
- Create stub pages for missing but recurring concepts
- Update stale summaries or synthesis pages
- Flag contradictions explicitly for user review
- Suggest missing external research

## Related Concepts

- [[concept-compounding-wiki]] — linting keeps the compounding wiki healthy over time
- [[concept-type-prefixed-links]] — broken link detection depends on the prefix convention
- [[concept-ingest-workflow]] — ingest and lint are complementary; ingest adds, lint cleans

## Related Entities

- [[entity-log-md]] — lint passes are always recorded here
- [[entity-index-md]] — orphan detection starts from the index
