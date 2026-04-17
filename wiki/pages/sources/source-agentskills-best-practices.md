# Source: Best Practices for Skill Creators — agentskills.io

**Type**: article
**URL**: https://agentskills.io/skill-creation/best-practices.md
**Date Added**: 2026-04-17
**Key Sections**: Start from real expertise, Refine with real execution, Spending context wisely, Calibrating control, Patterns for effective instructions

## Summary

The richest authoring guide in the agentskills.io documentation. Covers how to ground skills in real domain expertise (not generic LLM knowledge), how to iterate via execution traces, how to spend context tokens effectively, and five reusable instruction patterns: gotchas sections, output format templates, checklists, validation loops, and plan-validate-execute. Strong emphasis on extracting skills from real task completions rather than generating them from scratch.

## Key Extracts

> "A common pitfall in skill creation is asking an LLM to generate a skill without providing domain-specific context — relying solely on the LLM's general training knowledge. The result is vague, generic procedures."

> "Read agent execution traces, not just final outputs."

> "The highest-value content in many skills is a list of gotchas — environment-specific facts that defy reasonable assumptions."

> "When an agent makes a mistake you have to correct, add the correction to the gotchas section."

> "A skill should teach the agent *how to approach* a class of problems, not *what to produce* for a specific instance."

## Key Insights for This Project

- **Extract from real task completions**: the wiki skill itself should be iterated by running real ingest/query/lint tasks, noting corrections, and folding them into the skill. This is how the skill gets better over time.
- **Gotchas section**: the wiki skill should have a dedicated gotchas section capturing non-obvious behaviors (e.g., "always append to log.md, never overwrite," "use type prefixes on all links"). These are the kind of corrections that accumulate during real use.
- **Procedures over declarations**: the wiki skill should teach the LLM *how to approach* each workflow, not just list rules. The reference files already do this — but gotchas capture the non-obvious edge cases.
- **Execution traces reveal wasted work**: if the LLM is doing unnecessary steps during ingest or query, that's a signal to clarify the relevant workflow reference.
- **Provide defaults, not menus**: the wiki skill should not present equal options when one approach is clearly right. Pick the default and mention alternatives briefly.

## Related Concepts

- [[concept-progressive-disclosure]] — "Structure large skills with progressive disclosure" section validates the references/ pattern
- [[concept-ingest-workflow]] — should be checked against the gotchas and procedures guidance here
- [[concept-query-workflow]] — same
- [[concept-lint-workflow]] — same

## Related Sources

- [[source-agentskills-llms-txt]] — index this page was discovered from
- [[source-agentskills-evaluating-skills]] — companion piece on systematic iteration
- [[source-agentskills-optimizing-descriptions]] — companion piece on description quality
