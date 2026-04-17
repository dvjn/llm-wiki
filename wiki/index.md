# Wiki — Content Catalog

> Auto-updated by the LLM on every ingest, query filing, and lint pass.

## Concepts

Domain concepts that appear across the skill's design.

- [[concept-three-layer-architecture]] — raw sources, LLM-generated pages, and catalog+log infrastructure
- [[concept-compounding-wiki]] — a wiki whose value grows with each ingest, query, and lint pass
- [[concept-type-prefixed-links]] — mandatory type prefixes on all wiki links to prevent namespace collisions
- [[concept-progressive-disclosure]] — three-tier loading strategy for skills: metadata at startup, instructions on activation, resources on demand
- [[concept-ingest-workflow]] — process of reading a raw source and folding knowledge into the wiki
- [[concept-query-workflow]] — answering questions from wiki pages with section-level citations and filing synthesis back
- [[concept-lint-workflow]] — health-check pass for broken links, orphans, stale claims, and contradictions

## Entities

Concrete files and structures the skill defines.

- [[entity-schema-document]] — `wiki/SCHEMA.md`; the config file that makes the LLM a disciplined wiki maintainer
- [[entity-skill-md]] — the skill's root instruction document; routes the LLM to ingest, query, and lint workflows
- [[entity-index-md]] — the content catalog; starting point for every query
- [[entity-log-md]] — the append-only record of every wiki operation

## Decisions

Design decisions with rationale.

- [[decision-llm-writes-wiki]] — LLM owns all page content; humans curate sources
- [[decision-type-prefixed-links]] — all links require a type prefix to eliminate namespace collisions
- [[decision-immutable-raw-sources]] — files in `wiki/raw/` are read-only to the LLM
- [[decision-schema-in-wiki-dir]] — schema always lives at `wiki/SCHEMA.md`, not in agent config files

## Comparisons

_(none yet — add when a trade-off analysis is needed)_

## Synthesis

_(none yet — add when evolving thesis or open questions are worth preserving)_

## Sources

Per-source summaries.

- [[source-karpathy-llm-wiki]] — Karpathy's original formulation of the LLM Wiki pattern; founding document for this skill
- [[source-agentskills-llms-txt]] — agentskills.io documentation index; canonical discovery point for all `.md` endpoint URLs
- [[source-agentskills-home]] — agentskills.io overview; three-audience value proposition and adoption landscape
- [[source-agentskills-what-are-skills]] — agentskills.io intro to the skill format; progressive disclosure as the core design principle
- [[source-agentskills-specification]] — normative spec for SKILL.md frontmatter fields, token budgets, and directory conventions
- [[source-agentskills-best-practices]] — richest authoring guide; gotchas sections, execution trace iteration, procedures vs. declarations
- [[source-agentskills-optimizing-descriptions]] — systematic description testing with train/validation split and trigger-rate optimization
- [[source-agentskills-evaluating-skills]] — eval-driven iteration methodology; test cases, assertions, baseline comparison, execution transcripts
- [[source-agentskills-quickstart]] — tutorial: create a dice-rolling skill; demonstrates progressive disclosure, frontmatter conventions, and description-as-routing-signal
- [[source-agentskills-using-scripts]] — guide on bundling scripts in skills; agentic design requirements (non-interactive, structured output, helpful errors, idempotency)
- [[source-agentskills-adding-skills-support]] — client implementation guide; discovery, parsing, disclosure, activation, context compaction risk

## Statistics

| Metric | Count |
|--------|-------|
| Raw sources | 11 |
| Wiki pages | 24 |
| Concepts | 7 |
| Entities | 4 |
| Decisions | 4 |
| Comparisons | 0 |
| Synthesis | 0 |
| Sources | 11 |
