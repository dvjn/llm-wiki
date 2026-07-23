# wiki-librarian

> Read-only subagent that researches the wiki and returns distilled, cited answers.

## Purpose

Handles ALL wiki reading on the main agent's behalf — the main agent never opens wiki files itself. Task types: lookups, synthesis queries, gap/contradiction searches, the full lint audit (all six checks in one thread), and pre-ingest source analysis (takeaways + proposed page plan). Each operation is one call; the reading happens in the librarian's thread and only a compact cited report enters the main context. Parallel fan-out (one librarian per lint check) is a speed opt-in, not the default.

## Structure

- Role prompt: `skills/llm-wiki/references/agents/librarian.md` — ships with every install path; the subagent self-loads it (the spawner passes only a short read-this-file-then-task prompt)
- Header instructs the spawner to grant read/grep/glob only; read-only is by instruction, not enforcement
- Prompt embeds the wiki layout, [[concept-type-prefixed-links]] scheme, and index-first query workflow
- Returns: synthesized answer with `§`-level citations, pages consulted, and gaps noticed

## Lifecycle

Spawned per query/check by the main agent from the role prompt; stateless between invocations. Without subagent support the main agent performs the role directly ([[decision-subagent-delegation]]).

## Related

- [[entity-wiki-scribe]] — writes what the librarian's answers become
- [[entity-wiki-researcher]] — fills the gaps the librarian surfaces
- [[concept-query-workflow]] — primary consumer
- [[concept-lint-workflow]] — runs its checklist as one librarian audit call
- [[entity-index-md]] — its mandatory starting point
