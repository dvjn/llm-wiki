# Source: Anthropic Official — Skill Authoring Best Practices

**Type**: article (official docs)
**URL**: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices
**Date Added**: 2026-07-24

## Summary

Anthropic's canonical, product-agnostic guide to authoring Agent Skills. It carries the authoritative numeric limits (name/description caps, SKILL.md body length, reference-file TOC threshold) and the core authoring principles: keep content concise because "the context window is a public good," assume "Claude is already very smart," and match a skill's degrees of freedom to the task's fragility. Where it overlaps the agentskills.io open-standard docs, this is the authoritative source. It doubles as a judging rubric: it ships an explicit quality checklist and an evaluation-driven-development method.

## Key Extracts

> Hard limits: `name` **maximum 64 characters** (lowercase/numbers/hyphens, no "anthropic"/"claude"); `description` **non-empty, maximum 1,024 characters**, third person; SKILL.md body **under 500 lines** "for optimal performance"; reference files longer than **100 lines** must include a **table of contents** at the top (so partial reads like `head -100` still show full scope).

> "Claude is already very smart." Only add context Claude doesn't already have. Three challenge questions: "Does Claude really need this explanation?" / "Can I assume Claude knows this?" / "Does this paragraph justify its token cost?"

> Degrees of freedom (robot-on-a-path analogy): high freedom = text instructions (open field), medium = pseudocode/parameterized scripts, low freedom = specific scripts with few/no parameters (narrow bridge with cliffs). Match specificity to how fragile and variable the task is.

> Naming: prefer **gerund form** (`processing-pdfs`, `analyzing-spreadsheets`). Avoid vague (`helper`, `utils`), generic (`documents`, `data`), reserved words.

> Descriptions: always third person ("inconsistent point-of-view can cause discovery problems"); include what it does AND specific triggers for when to use it; format `<what it does>. Use when <triggers/contexts>.`

> Progressive disclosure: keep references **one level deep** from SKILL.md — deeply nested references risk partial reads (`head -100`) and incomplete information.

> Named anti-patterns: time-sensitive info (dates that "will become wrong"); inconsistent terminology; Windows-style paths; too many options; assuming tools installed; "voodoo constants" (Ousterhout — "If you don't know the right value, how will Claude determine it?").

> "Build evaluations first" — create evals BEFORE extensive documentation; **three** scenarios testing identified gaps, establish baseline, write minimal instructions to pass, iterate. Claude A / Claude B loop: refine with one instance, test with a fresh one.

## Related Concepts

- [[concept-skill-review-criteria]] — this source supplies most of the rubric's hard limits and axes
- [[concept-progressive-disclosure]] — authoritative 500-line body, one-level-deep, TOC>100-line numbers
- [[concept-context-as-scarce-resource]] — "the context window is a public good"

## Impact

Establishes the authoritative numeric limits and the "Claude is already smart" test used to build [[concept-skill-review-criteria]]. Its 1,024-char `description` frontmatter validation limit is distinct from Claude Code's 1,536-char runtime listing cap ([[source-claude-code-skills-authoring]]).
