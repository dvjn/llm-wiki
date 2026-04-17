# Concept: Progressive Disclosure

> A loading strategy for skills: agents load only what they need, when they need it — keeping context lean at startup and expanding on demand.

## Core Definition

Progressive disclosure is the principle that a skill's content should be loaded in stages, matched to the agent's current need. Rather than loading everything at startup, the agent loads a small summary to decide relevance, then loads full instructions only when activating, then loads referenced files only when required during execution.

## The Three Tiers

As defined in the agentskills.io specification:

| Tier | Content | Size Target | When Loaded |
|------|---------|-------------|-------------|
| 1. Metadata | `name` + `description` | ~100 tokens | At agent startup, for all skills |
| 2. Instructions | Full `SKILL.md` body | <5000 tokens / <500 lines | When a task matches the skill |
| 3. Resources | `references/`, `scripts/`, `assets/` | As needed | During execution, on demand |

## Why It Matters

Without progressive disclosure, an agent with many skills would load all instruction content at startup, bloating the context window. Progressive disclosure keeps startup cheap while still making full skill depth available. The description at tier 1 acts as the routing signal — it must be precise enough to trigger the right skill and exclude irrelevant ones.

## Design Implications for Skill Authors

- **Tier 1 (description)**: Include specific keywords and the trigger condition ("Use when..."). Keep it under ~100 tokens. This is the agent's routing signal.
- **Tier 2 (SKILL.md body)**: Keep under 500 lines / 5000 tokens. Cover the main workflow but defer details to references.
- **Tier 3 (references/)**: Split detailed reference material into focused files. Agents load these on demand, so smaller files mean less wasted context.

## Relevance to This Skill

The llm-wiki skill follows progressive disclosure:
- Agents load the skill name and description to detect wiki-related tasks.
- The full `SKILL.md` body is loaded on activation — it describes the architecture, global rules, and workflow routing.
- Workflow references (`references/ingest.md`, `references/query.md`, `references/lint.md`) are loaded only when the specific workflow is needed.

This means the three workflow references are tier-3 resources: they are not loaded unless the specific workflow is required. Keeping them focused and separate from `SKILL.md` is the correct pattern per the spec.

## Sources

- [[source-agentskills-what-are-skills]] §How skills work — progressive disclosure as the core design principle
- [[source-agentskills-specification]] §Progressive disclosure — token budgets and size constraints

## Related Concepts

- [[concept-three-layer-architecture]] — structural parallel: the wiki also separates concerns into distinct layers with clear responsibilities
- [[concept-ingest-workflow]] — loaded as a tier-3 resource (`references/ingest.md`)
- [[concept-query-workflow]] — loaded as a tier-3 resource (`references/query.md`)
- [[concept-lint-workflow]] — loaded as a tier-3 resource (`references/lint.md`)

## Related Entities

- [[entity-skill-md]] — the tier-2 file; must stay under 500 lines
