# Decision: Raw Sources Are Immutable

**Status**: Accepted
**Date**: 2026-04-17
**Context**: The wiki needs a stable ground truth that synthesis can be traced back to. Without immutable sources, the LLM might edit raw files to "clean them up" or patch contradictions, destroying the audit trail.

## Decision

All files in `wiki/raw/` are read-only to the LLM. The LLM reads raw sources during ingest but never edits, moves, or deletes them. Only humans add files to `wiki/raw/`.

## Rationale

Immutable raw sources serve as the ground truth layer. When a wiki page contradicts a source, the source wins — the page should be updated, not the source. If sources were mutable, there would be no stable reference point for verifying synthesis quality or tracing the origin of claims.

This constraint also makes the wiki auditable: any claim in a wiki page should be traceable to a specific section of a raw source.

## Consequences

- Positive: Ground truth is preserved regardless of LLM errors.
- Positive: Raw sources can be re-ingested to refresh wiki pages without risk.
- Positive: Contradictions between pages and sources have a clear resolution: trust the source.
- Negative: Typos or errors in raw sources cannot be silently fixed.
- Risk: Stale raw sources (outdated specs) will still appear as ground truth — requires human curation.

## Related

- [[concept-three-layer-architecture]] — raw sources are the foundation layer
- [[concept-ingest-workflow]] — reads raw sources but never modifies them
- [[decision-llm-writes-wiki]] — the complementary constraint: LLM owns pages, not raw files
