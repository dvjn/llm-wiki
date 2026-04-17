# Decision: LLM Writes the Wiki

**Status**: Accepted
**Date**: 2026-04-17
**Context**: Knowledge bases fail because human maintenance burden grows faster than value. This skill needed a model where the bookkeeping stays manageable as the wiki scales.

## Decision

The LLM writes all wiki page content. Humans curate sources (adding files to `wiki/raw/`) and ask questions. The LLM handles synthesis, cross-reference maintenance, stub creation, index updates, and log entries.

## Rationale

The tedious part of maintaining a knowledge base is not reading or thinking — it is bookkeeping: updating cross-references, keeping summaries current, noting when new data contradicts old claims. LLMs can touch many files in one pass at near-zero marginal cost. Humans cannot sustain this at scale, which is why wikis are typically abandoned.

By delegating bookkeeping to the LLM, the skill makes it economically viable to maintain a wiki that compounds in value over time rather than decaying.

## Consequences

- Positive: Wiki maintenance cost stays near-zero as the knowledge base grows.
- Positive: Cross-references and contradiction detection happen automatically during ingest.
- Positive: Humans only need to invest in source curation and question-asking.
- Negative: Wiki quality depends on LLM synthesis quality — poor prompting leads to poor pages.
- Risk: Overconfident synthesis may introduce subtle inaccuracies not present in raw sources.

## Related

- [[concept-compounding-wiki]] — the core value proposition this decision enables
- [[concept-three-layer-architecture]] — the structure that makes LLM ownership tractable
- [[decision-immutable-raw-sources]] — the counterbalancing constraint that keeps raw truth accessible
