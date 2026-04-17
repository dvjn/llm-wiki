# Source: Evaluating Skill Output Quality — agentskills.io

**Type**: article
**URL**: https://agentskills.io/skill-creation/evaluating-skills.md
**Date Added**: 2026-04-17
**Key Sections**: Designing test cases, Running evals, Writing assertions, Grading outputs, Aggregating results, Analyzing patterns, Iterating on the skill

## Summary

A structured methodology for eval-driven skill improvement. Test cases go in `evals/evals.json` (prompt + expected output + optional input files). Each iteration runs cases with and without the skill, captures timing data, grades assertions (pass/fail with evidence), aggregates into benchmarks, and feeds failures + execution traces + human feedback back into the skill. The loop continues until improvement plateaus. Emphasizes concrete evidence over subjective judgment and blind comparison for holistic quality.

## Key Extracts

> "Does it work reliably — across varied prompts, in edge cases, better than no skill at all?"

> "Require concrete evidence for a PASS. Don't give the benefit of the doubt."

> "Execution transcripts reveal *why* things went wrong. If the agent ignored an instruction, the instruction may be ambiguous."

> "Keep the skill lean. Fewer, better instructions often outperform exhaustive rules."

> "Reasoning-based instructions ('Do X because Y tends to cause Z') work better than rigid directives ('ALWAYS do X, NEVER do Y')."

## Key Insights for This Project

- **Baseline comparison is essential**: the wiki skill should be tested against no-skill baseline to verify it actually adds value — not just assumed to.
- **Token/time delta matters**: a skill that adds significant tokens per session but only marginally improves output quality is a bad trade-off.
- **Remove assertions that always pass in both configurations**: they inflate scores without reflecting skill value.
- **Execution transcripts**: during wiki operations, if the LLM is spending time on steps the workflow references don't intend, those transcripts reveal which instructions are being misread.
- **The evals/ directory convention**: worth adopting for this project — `skills/llm-wiki/evals/evals.json` with test prompts for ingest, query, and lint tasks.

## Related Concepts

- [[concept-ingest-workflow]] — primary target for eval test cases
- [[concept-query-workflow]] — primary target for eval test cases
- [[concept-lint-workflow]] — primary target for eval test cases

## Related Sources

- [[source-agentskills-llms-txt]] — index this page was discovered from
- [[source-agentskills-best-practices]] — companion piece; feeds into the iteration step here
