# Entity: AGENTS.md

> A project-level steering file that all agents load before working — tells them the conventions of the project.

## Purpose

AGENTS.md is a generic, agent-agnostic steering file placed in the project root. Any compliant agent reads it at startup to learn project conventions, coding standards, and available tools. It is not tied to any specific agent or framework.

## Structure

Varies by project. For projects using the llm-wiki skill, the relevant section is:

- **Knowledge Base - Wiki** — tells the agent that a wiki exists at `wiki/`, to query it before working on tasks, and to use the llm-wiki skill for all wiki operations (ingest, query, lint, create)

A template for this section lives at `skills/llm-wiki/references/templates/agents-md-wiki-section.md`.

## Lifecycle

Created once when a project wants to make its wiki discoverable to agents. Updated when project conventions change. The wiki section is appended when the llm-wiki skill is set up or during a lint pass.

## Related Entities

- [[entity-skill-md]] — the llm-wiki skill entrypoint that AGENTS.md routes to
- [[entity-schema-document]] — the wiki's own configuration at `wiki/SCHEMA.md`
- [[entity-index-md]] — the content catalog agents are directed to consult
- [[entity-log-md]] — the append-only log of wiki operations

## Related Concepts

- [[concept-three-layer-architecture]] — AGENTS.md is one way the schema layer surfaces to agents
- [[concept-progressive-disclosure]] — AGENTS.md is loaded at startup (tier 1), making the wiki discoverable early

## Related Decisions

- [[decision-schema-in-wiki-dir]] — the wiki schema lives at wiki/SCHEMA.md; AGENTS.md points to it but is not the schema itself

## Sources

- [[source-karpathy-llm-wiki]] — original formulation mentioning AGENTS.md as a schema carrier
- [[source-agentskills-adding-skills-support]] — client implementation guide for skill discovery
