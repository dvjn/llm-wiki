# Entity: wiki/SCHEMA.md

> The wiki's configuration file — defines structure, conventions, and workflow expectations so any LLM agent can maintain the wiki consistently.

## Purpose

`wiki/SCHEMA.md` is the schema layer of the three-layer architecture. It sits inside the wiki directory itself, making it portable across agents and tools. Any LLM can read it and behave as a disciplined wiki maintainer without needing agent-specific config files.

## What It Contains

A well-formed schema document defines:

- The wiki's purpose and boundaries
- The exact directory structure
- The page types in use and any template adjustments
- Naming and link conventions
- Ingest, query, and lint expectations
- Any integration with the codebase or external systems

## File Path

Always `wiki/SCHEMA.md` — never an agent-specific config file. Keeping the schema inside the wiki directory makes it self-contained and agent-agnostic.

## Co-Evolution

The schema should be co-evolved with the LLM over time. As you discover what conventions work for your domain, update `wiki/SCHEMA.md` so future sessions behave consistently without re-explaining the rules.

## Lifecycle

Created during wiki setup. Updated incrementally as conventions stabilize or change. Never deleted.

## Related Concepts

- [[concept-three-layer-architecture]] — schema is the explicit third layer
- [[concept-ingest-workflow]] — schema defines the ingest procedure
- [[concept-query-workflow]] — schema defines query and filing conventions
- [[concept-lint-workflow]] — schema defines lint expectations

## Related Decisions

- [[decision-schema-in-wiki-dir]] — why the schema lives in `wiki/SCHEMA.md` rather than agent config files

## Sources

- [[source-karpathy-llm-wiki]] §Architecture — "the key configuration file — it's what makes the LLM a disciplined wiki maintainer rather than a generic chatbot"
