# Decision: Schema Lives in wiki/SCHEMA.md

**Status**: Accepted
**Date**: 2026-04-17
**Context**: The schema layer needs to be readable by any LLM agent. Agent-specific config files vary by tool and are often outside the wiki directory, making the wiki dependent on the host agent's conventions.

## Decision

The wiki schema is always stored at `wiki/SCHEMA.md` — inside the wiki directory itself. Agent-specific config files are not used as the schema layer.

## Rationale

Placing the schema inside the wiki directory makes the wiki fully self-contained and portable. Any LLM agent can read `wiki/SCHEMA.md` directly without needing to know where the agent's own config file lives or what it is called.

Agent config files serve a different purpose: telling the agent how to behave in the project overall. The wiki schema is narrower and wiki-specific — it belongs with the wiki, not with the agent.

## Consequences

- Positive: The wiki is fully agent-agnostic. Switch agents without changing the wiki structure.
- Positive: The schema is co-located with the content it governs — easy to find and update.
- Positive: No ambiguity about which file is authoritative for wiki conventions.
- Negative: The agent's own config file may need a one-line pointer to `wiki/SCHEMA.md` so the agent knows to read it.

## Related

- [[entity-schema-document]] — the entity this decision governs
- [[concept-three-layer-architecture]] — schema is the explicit third layer
