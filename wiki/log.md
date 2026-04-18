# Wiki Log

Append-only record of ingest, lint, and major synthesis updates.

---

## [2026-04-17] ingest | agentskills.io — What Are Skills + Specification

Added `raw/articles/article-agentskills-what-are-skills.md` and `raw/articles/article-agentskills-specification.md`. Created source summaries [[source-agentskills-what-are-skills]] and [[source-agentskills-specification]]. Created new concept [[concept-progressive-disclosure]] (three-tier loading: metadata ~100 tokens at startup, instructions <5000 tokens on activation, resources on demand). Updated [[entity-skill-md]] with frontmatter field reference table and size budget per spec. Updated index (18 pages, 7 concepts, 3 sources, 3 raw sources).

---

## [2026-04-18] lint | Wiki health check

Created [[entity-agent-md]] entity page (generic project-level steering file that all agents load; wiki section makes wiki discoverable). Updated [[index.md]] (28 wiki pages, 5 entities). AGENTS.md wiki section: added. No broken links, no contradictions, no orphans found.

---

## [2026-04-18] create | Added AGENTS.md to project root

Created AGENTS.md with project overview and "Knowledge Base - Wiki" section (from templates/agents-md-wiki-section.md). All agents working in this project now discover the wiki at startup.

---

## [2026-04-17] decision | Schema always at wiki/SCHEMA.md

Recorded [[decision-schema-in-wiki-dir]]: schema layer always lives at `wiki/SCHEMA.md`, never in agent-specific config files. Updated [[entity-schema-document]] to reflect fixed path. Updated [[concept-three-layer-architecture]] schema row. Updated [[entity-skill-md]] to clarify it is a skill entrypoint, not the schema. Updated [[source-karpathy-llm-wiki]] note. Added decision to index (15 pages, 4 decisions).

---

## [2026-04-17] refactor | Agent-agnostic generalization

Generalized Claude Code-specific language across 4 pages. Created [[entity-schema-document]] to capture the agent-agnostic concept of the schema layer. Updated [[concept-three-layer-architecture]] to describe schema as CLAUDE.md/AGENTS.md/SKILL.md depending on agent. Updated [[entity-skill-md]] to frame itself as the Claude Code-specific implementation. Updated [[wiki/SCHEMA.md]] to note the agent-agnostic pattern. Updated index (14 pages, 4 entities).

---

## [2026-04-17] ingest | LLM Wiki (Karpathy)

Added `raw/articles/article-karpathy-llm-wiki.md`. Created source summary [[source-karpathy-llm-wiki]]. Updated 5 pages: [[concept-compounding-wiki]] (added RAG contrast examples, Obsidian IDE analogy, use cases, Memex lineage), [[concept-three-layer-architecture]] (clarified schema as explicit third layer, added co-evolution note), [[entity-index-md]] (added scale guidance ~100 sources), [[entity-log-md]] (added grep parseability tip), [[entity-skill-md]] (connected to Karpathy's schema layer concept). Updated index.

---

## [2026-04-17] init | Wiki initialized

Bootstrapped wiki for the `llm-wiki-skill` project. Created SCHEMA.md, index.md, log.md. Created 6 concept pages (three-layer-architecture, compounding-wiki, type-prefixed-links, ingest-workflow, query-workflow, lint-workflow), 3 entity pages (skill-md, index-md, log-md), and 3 decision pages (llm-writes-wiki, type-prefix-links, immutable-raw-sources). Updated index with all initial pages.

---

## [2026-04-17] ingest | agentskills.io — Quickstart + Using Scripts

Added `raw/articles/article-agentskills-quickstart.md` and `raw/articles/article-agentskills-using-scripts.md`. Created source summaries [[source-agentskills-quickstart]] and [[source-agentskills-using-scripts]]. Quickstart reinforces progressive disclosure and frontmatter conventions. Using Scripts covers agentic design requirements: non-interactive shells, structured output (JSON/CSV), helpful error messages, idempotency, and dry-run support. Updated index (11 sources, 26 pages total).

---

## [2026-04-17] lint | Wiki lint pass

Fixed 3 issues: (1) added [[decision-immutable-raw-sources]] to [[concept-ingest-workflow]] "Related Decisions" section; (2) added [[decision-immutable-raw-sources]] to [[concept-three-layer-architecture]] "Related Decisions" section (created new section); (3) fixed typo "helpLLMs" → "help LLMs" in [[source-agentskills-quickstart]]. No broken links, no contradictions, no stale claims found. 2 orphan decision pages noted — see lint report.

---

## [2026-04-17] refactor | Decision page naming — type-prefixed-links

Renamed `decision-type-prefix-links` → [[decision-type-prefixed-links]] to match [[concept-type-prefixed-links]] naming convention. Updated 2 references (index.md, concept-type-prefixed-links.md). File renamed on disk.

---

## [2026-04-18] lint | Wiki health check

Deleted obsolete [[concept-helper-scripts]] page (scripts were removed from skill). Updated 3 pages to remove script references: [[concept-stub-pages]], [[entity-skill-md]], [[index.md]]. No broken links, no contradictions, no stale claims, no orphans found. Updated index Statistics (27 wiki pages, 8 concepts).

---

## [2026-04-17] lint | Wiki health check

Fixed 1 stale-link reference in log.md (wrapped pre-rename name as code). Updated [[entity-skill-md]] structure to match current SKILL.md (removed page-templates/Core Philosophy/Why This Works; added Lazy Loading Requirement and Helper Scripts sections). Created 2 concept pages: [[concept-helper-scripts]], [[concept-stub-pages]]. Added stubs/ subdir to SCHEMA.md page-types table. Updated index Statistics (24→28 wiki pages, 7→9 concepts). No true orphans; 20 'broken' links verified as intentional in-body documentation examples.
