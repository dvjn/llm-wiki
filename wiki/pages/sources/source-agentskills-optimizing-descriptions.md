# Source: Optimizing Skill Descriptions — agentskills.io

**Type**: article
**URL**: https://agentskills.io/skill-creation/optimizing-descriptions.md
**Date Added**: 2026-04-17
**Key Sections**: How skill triggering works, Writing effective descriptions, Designing trigger eval queries, Testing, Avoiding overfitting, The optimization loop

## Summary

Systematic guide to testing and improving the `description` field so it triggers reliably on relevant prompts without over-triggering. Introduces a structured eval methodology: create ~20 queries (should-trigger and should-not-trigger), run each multiple times to compute trigger rates, split into train/validation sets to avoid overfitting, and iterate with an optimization loop. Key insight: agents typically only consult skills for tasks requiring knowledge beyond their defaults — so a simple task may not trigger even if the description matches.

## Key Extracts

> "An under-specified description means the skill won't trigger when it should; an over-broad description means it triggers when it shouldn't."

> "Use imperative phrasing. Frame the description as an instruction: 'Use this skill when...' rather than 'This skill does...'"

> "Err on the side of being pushy. Explicitly list contexts where the skill applies, including cases where the user doesn't name the domain directly."

> "Avoid adding specific keywords from failed queries — that's overfitting. Instead, find the general category or concept those queries represent and address that."

## Key Insights for This Project

- **The description is the only routing signal at tier 1**: it must include the right trigger phrases ("wiki", "ingest", "query", "lint", "knowledge base") and the "Use when..." imperative framing.
- **Near-miss negatives are critical**: the wiki skill's description could accidentally trigger for generic note-taking, markdown writing, or RAG-related questions. These should be tested explicitly.
- **Nondeterminism**: model behavior varies across runs — a single test pass is not enough to confirm triggering reliability.
- **The 1024-character limit**: long descriptions tend to grow during optimization; monitor length.

## Before/After Pattern

The guide demonstrates a concrete improvement:
```yaml
# Before (too narrow)
description: Process CSV files.

# After (specific about what + broad about when)
description: >
  Analyze CSV and tabular data files — compute summary statistics,
  add derived columns, generate charts, and clean messy data. Use this
  skill when the user has a CSV, TSV, or Excel file and wants to
  explore, transform, or visualize the data, even if they don't
  explicitly mention "CSV" or "analysis."
```

This pattern applies directly to the wiki skill's description.

## Related Concepts

- [[concept-progressive-disclosure]] — the description is the tier-1 routing mechanism described here

## Related Entities

- [[entity-skill-md]] — the `description` field this guide is about

## Related Sources

- [[source-agentskills-llms-txt]] — index this page was discovered from
- [[source-agentskills-best-practices]] — companion piece on skill content quality
