# wiki-researcher

> External-research subagent that captures web sources as provenance-stamped raw notes.

## Purpose

Handles the acquire stage of the knowledge lifecycle: given a research question or URLs, it searches/fetches, distills findings into raw source notes under `wiki/raw/`, and reports findings plus wiki impact. Its output is input for a later ingest — it never writes pages.

## Structure

- Role prompt: `skills/llm-wiki/references/agents/researcher.md` — ships with every install path; the subagent self-loads it (the spawner passes only a short read-this-file-then-task prompt)
- Needs web search/fetch and file-write capabilities; write scope restricted by instruction to `wiki/raw/<kind>/` (articles/, papers/, specs/), with existing raw files immutable per [[decision-immutable-raw-sources]] — the spawner reviews its report for out-of-scope writes
- Every note carries provenance: URLs, retrieval date, prompting question
- Returns: findings with supporting notes, files written, affected/suggested pages, dead ends

## Lifecycle

Dispatched from lint (research gaps), ingest (URL/topic sources), and query (wiki can't answer). Its raw notes are drafts the user reviews during the subsequent ingest, preserving the human-curates-sources spirit of [[decision-llm-writes-wiki]].

## Related

- [[entity-wiki-librarian]] — surfaces the gaps the researcher fills
- [[entity-wiki-scribe]] — folds the researcher's notes into pages at ingest
- [[concept-ingest-workflow]] — consumes the researcher's raw notes
- [[concept-lint-workflow]] — dispatches it for research gaps
