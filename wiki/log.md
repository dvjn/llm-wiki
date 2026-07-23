# Wiki Log

Append-only record of ingest, lint, and major synthesis updates.

---

## [2026-07-24] ingest | Anthropic skill authoring + subagent context-engineering research

Ingested 7 raw research notes (all in `raw/articles/`, 2026-07-24). Created 7 source summaries: [[source-anthropic-skill-best-practices]], [[source-claude-code-skills-authoring]], [[source-anthropic-lessons-using-skills]], [[source-anthropic-context-engineering]], [[source-anthropic-multi-agent-research]], [[source-claude-code-subagents-docs]], [[source-subagent-token-overhead-analysis]] (preserving the key numbers: 500-line body, 1,024-char frontmatter vs 1,536-char runtime listing cap, references one level deep, TOC>100 lines, 1,000-2,000 token distilled summaries, ~15x multi-agent multiplier, and the low-confidence community estimates). Created 2 concept pages ([[concept-skill-review-criteria]] — skill judging rubric; [[concept-context-as-scarce-resource]] — main-thread context as the binding constraint and the delegate-vs-inline threshold) and the first comparison page ([[comparison-delegate-vs-inline]]). Updated [[concept-progressive-disclosure]] with the authoritative body/nesting/TOC numbers, and [[decision-subagent-delegation]] — added the ~15x token multiplier and no-cache-reuse cost to Consequences, and recorded (UNDER REVIEW) that external evidence challenges the blanket "even lookups delegate" rule without changing the Decision. Updated index (46 wiki pages, 11 concepts, 1 comparison, 20 sources, 20 raw sources).

## [2026-04-18] decision | Subagent delegation for search-heavy operations

Created [[decision-subagent-delegation]]: delegate synthesis queries, gap/contradiction queries, and lint checks (orphans, missing refs, term gaps, contradictions, research gaps) to subagents when available. Falls back to direct execution when unavailable. Updated index (29 pages, 5 decisions).

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

---

## [2026-07-23] ingest | Claude Code plugins + Codex skills/plugins research

Added `raw/articles/article-claude-code-plugins.md` and `raw/articles/article-codex-skills-plugins.md` (research notes from official docs). Created 2 source summaries ([[source-claude-code-plugins]], [[source-codex-skills-plugins]]), 1 concept page ([[concept-cross-agent-skill-portability]]), 1 decision page ([[decision-dual-plugin-packaging]]). Updated index (13 raw sources, 33 wiki pages, 9 concepts, 6 decisions, 13 sources).

---

## [2026-07-23] synthesis | Bundled subagents (wiki-librarian, wiki-researcher, wiki-scribe)

Added `agents/` to the plugin with three role-based subagents covering the knowledge lifecycle: acquire ([[entity-wiki-researcher]]), retrieve ([[entity-wiki-librarian]]), write ([[entity-wiki-scribe]]). Revised [[decision-subagent-delegation]] from generic "delegate when available" to role-based delegation with three-tier fallback (bundled agents → generic subagents → direct). Created 3 entity pages. Updated [[concept-query-workflow]], [[concept-lint-workflow]], [[concept-ingest-workflow]] with delegation points. Updated index (36 wiki pages, 8 entities).

---

## [2026-07-23] synthesis | Canonical role prompts for dynamic subagents

Moved the three subagent role prompts into the skill at `references/agents/{librarian,researcher,scribe}.md` so harnesses with dynamic (not predefined) subagents can spawn them verbatim. Claude Code `agents/*.md` became thin loaders over the same files — single source of truth across all tiers. Revised [[decision-subagent-delegation]] (dynamic tier, thin-loader trade-off, instruction-only boundaries outside Claude Code) and the three agent entity pages.

---

## [2026-07-23] synthesis | Dropped static agents; dynamic subagents only

Removed the Claude Code `agents/*.md` thin loaders — delegation now happens exclusively by spawning dynamic subagents from the role prompts at `references/agents/{librarian,researcher,scribe}.md`, on every install path. Boundary enforcement via tool restrictions deliberately traded for a single delegation mechanism. Revised [[decision-subagent-delegation]] and the three agent entity pages; updated index descriptions.

---

## [2026-07-24] synthesis | Context-economy principle: one subagent call per wiki operation

Made context reduction the governing principle of [[decision-subagent-delegation]]: subagents group a wiki operation's tool calls into a separate thread so each operation costs the main agent ~one call plus a compact report, and the main agent never reads/writes wiki files when subagents are available. Consequences applied: lookups now delegate too (previously direct); lint defaults to a single librarian audit call running all checks (parallel fan-out demoted to speed opt-in); ingest gains a librarian source-analysis call so the main agent never reads the source (judgment happens over reports). Extended the librarian role prompt with task types (lookup/synthesis/gap/audit/source analysis); updated SKILL.md, all three workflow references, commands, evals (lookup eval flipped), [[concept-query-workflow]], [[concept-lint-workflow]], [[concept-ingest-workflow]], [[entity-wiki-librarian]], and index.

---

## [2026-07-24] lint+synthesis | Evidence-based skill review applied

Functional review fixed 8 findings (leftover pre-delegation routing in SKILL.md, missing synthesis template listings, templates-path handoff gaps, stale wiki cross-references, stub-location contradiction, setup index heading). Review panel (cruft, best-practices, context-economy) drove design revisions: spawn mechanism changed to subagent self-loading role files (~1,350 tokens/spawn saved in main thread); inline-lookup exception added; single-page filing moved inline; ~150-page audit ceiling stated; SCHEMA.md reinstated in setup as Karpathy's third layer; conventions.md deleted (orphaned); SKILL.md trimmed 73→61 lines with a Gotchas section added; evals realigned to the benchmark schema. Revised [[decision-subagent-delegation]]; created [[synthesis-2026-07-skill-review]] (first synthesis page); updated [[concept-query-workflow]], [[concept-lint-workflow]], the three agent entity pages, and index (47 pages, 1 synthesis).
