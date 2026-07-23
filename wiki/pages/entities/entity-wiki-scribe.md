# wiki-scribe

> Writing subagent that files pages, index entries, and log entries from a standalone brief.

## Purpose

Handles the write stage of the knowledge lifecycle: creating/updating pages during ingest, filing durable synthesis after queries, and applying approved lint fixes — always ending with `wiki/index.md` and `wiki/log.md` updates. Keeps convention-heavy writing (and template loading) out of the main context.

## Structure

- Role prompt: `skills/llm-wiki/references/agents/scribe.md` — ships with every install path; the subagent self-loads it, with the templates directory path appended to the brief by the spawner
- Reserved for multi-page writes (ingest, lint fixes); filing one already-synthesized page happens inline in the main thread
- Prompt embeds naming, [[concept-type-prefixed-links]], index/log formats, and raw-immutability rules; loads page templates one at a time
- Input: a complete brief (source paths, takeaways, pages to create/update, decisions made) — it has no conversation context
- Returns: pages created/updated, index/log entries added, anything in the brief it could not act on

## Lifecycle

Spawned once per write operation. Hard rule: never two scribes in parallel — `index.md` and `log.md` are shared write targets. Only writes what the brief supports; never deletes pages or rewrites log history.

## Related

- [[entity-wiki-librarian]] — research counterpart
- [[entity-wiki-researcher]] — produces the raw notes the scribe's briefs draw on
- [[concept-ingest-workflow]], [[concept-query-workflow]], [[concept-lint-workflow]] — its three dispatch points
- [[entity-index-md]], [[entity-log-md]] — the shared files that force serialization
