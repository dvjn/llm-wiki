# Concept: Skill Review Criteria

> A rubric for judging whether an Agent Skill is well-authored — the parameters against which an existing skill (including this one) should be reviewed.

## Core Definition

Skill review criteria are the concrete, mostly-official axes for evaluating a skill's quality: hard structural limits, a value test, calibrated specificity, trigger-focused naming and descriptions, single-responsibility scope, named anti-patterns to avoid, and an evals-first discipline. They convert Anthropic's authoring guidance into a checklist a reviewer can apply.

## The Criteria

### Hard limits (structural, must pass)

- `name` ≤ **64 characters**, lowercase/numbers/hyphens, no reserved words ("anthropic"/"claude"). — [[source-anthropic-skill-best-practices]]
- `description` ≤ **1,024 characters** (frontmatter validation limit), non-empty, third person. Distinct from Claude Code's **1,536-char** runtime *listing* cap on combined `description` + `when_to_use`. — [[source-anthropic-skill-best-practices]], [[source-claude-code-skills-authoring]]
- SKILL.md body **under 500 lines**. — [[source-anthropic-skill-best-practices]]
- Reference files > **100 lines** carry a table of contents at the top. — [[source-anthropic-skill-best-practices]]
- References **one level deep** from SKILL.md (no deep nesting). — [[source-anthropic-skill-best-practices]]

### Value test — "Claude is already smart"

A skill must teach something outside Claude's default reasoning. "A skill that restates what Claude would do by default adds context without adding value." Apply the three challenge questions: does Claude really need this explanation, can I assume Claude knows it, does this paragraph justify its token cost? The Gotchas section (real failure points) is the highest-signal content. — [[source-anthropic-lessons-using-skills]], [[source-anthropic-skill-best-practices]]

### Degrees of freedom — match specificity to fragility

High freedom (text instructions) for tasks with many valid approaches; low freedom (specific scripts, few parameters) for fragile, consistency-critical operations. Avoid "railroading" a flexible task with rigid steps, and avoid under-specifying a fragile one. — [[source-anthropic-skill-best-practices]], [[source-anthropic-lessons-using-skills]]

### Naming and description

- **Gerund naming** preferred (`processing-pdfs`); avoid vague/generic names. — [[source-anthropic-skill-best-practices]]
- Description is a statement of **when to trigger**, not a summary. Write third person and deliberately **"pushy"** ("use this skill when you need to X"), because Claude tends to under-trigger. Include specific trigger terms. — [[source-anthropic-lessons-using-skills]], [[source-anthropic-skill-best-practices]]

### Single responsibility

The best skills fit cleanly into one category; ones that straddle several confuse the agent. — [[source-anthropic-lessons-using-skills]]

### Named anti-patterns (must be absent)

Time-sensitive content (dates that go wrong), inconsistent terminology, deep reference nesting, too many options (give a default with an escape hatch), and "voodoo constants" ("If you don't know the right value, how will Claude determine it?"). — [[source-anthropic-skill-best-practices]]

### Evals-first

Build evaluations **before** extensive documentation, with at least **three** scenarios testing identified gaps and a **baseline comparison** (fresh session, skill enabled vs disabled). Measure invocation and output quality separately — a skill triggering only means it was found, not that it worked. — [[source-anthropic-skill-best-practices]], [[source-claude-code-skills-authoring]]

## In This Skill

These criteria are the parameters for reviewing the llm-wiki skill itself: whether `SKILL.md` stays under 500 lines, whether the three workflow references stay one level deep, whether the description triggers reliably, and whether the shipped evals cover ≥3 scenarios against a baseline.

## Related Concepts

- [[concept-progressive-disclosure]] — the structural limits enforce it
- [[concept-context-as-scarce-resource]] — the value test and conciseness follow from it

## Sources

- [[source-anthropic-skill-best-practices]] — hard limits, degrees of freedom, naming, anti-patterns, evals-first
- [[source-anthropic-lessons-using-skills]] — value test, Gotchas, single-category, pushy descriptions
- [[source-claude-code-skills-authoring]] — 1,536-char listing cap, baseline-comparison evaluation
