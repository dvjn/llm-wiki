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
- [[concept-stub-pages]] — placeholder pages for tracked-but-underdeveloped concepts; created by ingest and lint workflows
- [[concept-cross-agent-skill-portability]] — one SKILL.md serving Claude Code and Codex via the open Agent Skills standard plus thin per-agent manifests
- [[concept-skill-review-criteria]] — rubric for judging a skill: hard limits, the "Claude is already smart" value test, degrees of freedom, trigger-focused descriptions, single-responsibility, anti-patterns, evals-first
- [[concept-context-as-scarce-resource]] — main-thread context as the binding constraint; the compression ratio vs. the 15x token multiplier; the delegate-vs-inline threshold

## Entities

Concrete files and structures the skill defines.

- [[entity-schema-document]] — `wiki/SCHEMA.md`; the config file that makes the LLM a disciplined wiki maintainer
- [[entity-skill-md]] — the skill's root instruction document; routes the LLM to ingest, query, and lint workflows
- [[entity-agent-md]] — a generic project-level steering file that all agents load before working; wiki section makes the wiki discoverable
- [[entity-index-md]] — the content catalog; starting point for every query
- [[entity-log-md]] — the append-only record of every wiki operation
- [[entity-wiki-librarian]] — read-only subagent role; handles all wiki reading (lookups through audits and source analysis) as one call per operation
- [[entity-wiki-researcher]] — external-research subagent role; captures web sources as raw notes with provenance
- [[entity-wiki-scribe]] — writing subagent role; files pages and index/log updates from a standalone brief

## Decisions

Design decisions with rationale.

- [[decision-llm-writes-wiki]] — LLM owns all page content; humans curate sources
- [[decision-type-prefixed-links]] — all links require a type prefix to eliminate namespace collisions
- [[decision-immutable-raw-sources]] — files in `wiki/raw/` are read-only to the LLM
- [[decision-schema-in-wiki-dir]] — schema always lives at `wiki/SCHEMA.md`, not in agent config files
- [[decision-subagent-delegation]] — wiki operations run as ~one self-loading subagent call each, keeping wiki file access out of the main context; evidence-based exceptions for resident-index lookups and single-page filing
- [[decision-dual-plugin-packaging]] — package the repo as both a Claude Code plugin (self-hosted marketplace) and a Codex plugin around the untouched skill core

## Comparisons

Trade-off analyses.

- [[comparison-delegate-vs-inline]] — pushing work into a subagent vs. handling it in the main thread: context economy vs. spawn overhead, and the threshold rule

## Synthesis

Evolving thesis and review outcomes.

- [[synthesis-2026-07-skill-review]] — the July 2026 evidence-based skill review: what changed (spawn mechanism, lookup exception, SCHEMA.md reinstated, cruft removed) and what held

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
- [[source-claude-code-plugins]] — official Claude Code plugin/marketplace docs; manifest schema, self-hosted marketplace, namespacing, validation
- [[source-codex-skills-plugins]] — official Codex skills/plugin docs; Agent Skills adoption, discovery paths, invocation deltas, plugin manifest
- [[source-anthropic-skill-best-practices]] — Anthropic's canonical authoring guide; authoritative hard limits, "Claude is already smart", degrees of freedom, anti-patterns, evals-first
- [[source-claude-code-skills-authoring]] — Claude Code skill docs; 1,536-char listing cap, body-stays-in-context lifecycle, `context: fork`, baseline-comparison evals
- [[source-anthropic-lessons-using-skills]] — Anthropic running hundreds of internal skills; value test, Gotchas section, single-category scope, pushy descriptions
- [[source-anthropic-context-engineering]] — context as a finite resource; the 1,000-2,000 token distilled-summary compression ratio
- [[source-anthropic-multi-agent-research]] — orchestrator-worker system; ~15x token multiplier and the "spawning subagents for simple queries" anti-pattern
- [[source-claude-code-subagents-docs]] — official subagent docs; flood-vs-atomic trigger, cold-start inheritance, single-call vs resume
- [[source-subagent-token-overhead-analysis]] — independent overhead analysis; cold-start cost, fork-vs-fresh cache, 5-10x direct-edit efficiency (low confidence)

## Statistics

| Metric | Count |
|--------|-------|
| Raw sources | 20 |
| Wiki pages | 47 |
| Concepts | 11 |
| Entities | 8 |
| Decisions | 6 |
| Comparisons | 1 |
| Synthesis | 1 |
| Sources | 20 |
