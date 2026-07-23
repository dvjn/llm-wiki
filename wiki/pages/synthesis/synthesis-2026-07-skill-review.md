# Synthesis: July 2026 Skill Review — What the Evidence Changed

> A functional review, best-practices research pass, and three-dimension subagent review of the llm-wiki skill; the findings that changed the design and the ones that validated it.

## Method

Three phases, each run by subagents: (1) a functional review verifying manifests, path resolution, cross-file contract consistency, and wiki integrity; (2) two researchers capturing Anthropic's official skill-authoring guidance and subagent context-engineering evidence into `wiki/raw/` (7 sources, since ingested); (3) three parallel reviewers — layered cruft, best-practices adherence ([[concept-skill-review-criteria]]), and a measured context-economy cost model ([[concept-context-as-scarce-resource]]).

## What the evidence changed

- **Spawn mechanism**: the main agent had been reading each role file (~770 tokens) and re-sending its body verbatim as the spawn prompt — ~1,470 main-thread tokens per delegation for a static string. Replaced with subagent self-loading (~120-token spawn prompt). This was the single highest-leverage finding; it also settled the lookup dispute (see [[decision-subagent-delegation]]).
- **Lookup rule**: blanket "even lookups delegate" was wrong under the old mechanism and is now a default-with-exception — inline is allowed when the index is resident and the fact sits in 1-2 known pages.
- **Filing synthesis back**: moved inline by default; the scribe is reserved for multi-page writes.
- **SCHEMA.md**: rediscovered as Karpathy's third layer, lost during layered development — README/eval/dogfood wiki asserted it while no workflow created it. Reinstated in setup as the per-project schema, consulted by subagents for domain rules.
- **Cruft removed**: `conventions.md` (74 orphaned lines with a conflicting stub format), SKILL.md's routing/calls duplication (73 → 61 lines), command-file workflow re-narration, and the stub-directory contradiction (stubs now live in their graduation-type directory).
- **Evals**: field names realigned to the benchmark schema (`query`/`expected_behavior` assertions); a Gotchas section added to SKILL.md per [[source-anthropic-lessons-using-skills]].

## What the evidence validated

Every Anthropic hard limit passes with wide margins (61-line body vs 500; 245-char description vs 1,024). The governing context-economy principle held for synthesis, lint audit (~3k delegated vs ~27.6k inline main-thread tokens on the 40-page dogfood wiki), ingest, and research. Single-call audit, one-scribe rule, compact-report contract, terminology, degrees of freedom, and single-responsibility all reviewed clean.

## Open questions

- The ~150-page single-call audit ceiling is an estimate; revisit when a real wiki approaches it.
- Community overhead numbers (cold start ~20k tokens, 5-10x inline efficiency) are low-confidence; the design decisions rest on the official claims, with these as corroboration.
- Multi-model testing (Haiku/Sonnet/Opus) has not been done — the cheaper-model routing hint is currently unvalidated.

## Related

- [[decision-subagent-delegation]] — the decision this review revised
- [[comparison-delegate-vs-inline]] — the threshold analysis applied here
- [[concept-skill-review-criteria]] — the rubric used
- [[source-anthropic-context-engineering]], [[source-anthropic-multi-agent-research]] — the load-bearing evidence
