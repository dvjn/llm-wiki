# Source: Anthropic Engineering — Lessons from Building Claude Code: How We Use Skills

**Type**: article (engineering blog)
**URL**: https://claude.com/blog/lessons-from-building-claude-code-how-we-use-skills
**Date Added**: 2026-07-24

## Summary

Anthropic's own account of running "hundreds" of internal skills at scale. Higher-signal on how they *judge* a skill's value and maintain skills over time than the reference docs. Its strongest contribution is operationalizing the "Claude is already smart" principle into a sharp judging criterion: a skill that restates default behavior adds no value. It also names the Gotchas section as the highest-signal content, argues for single-category scope, and adds the "deliberately pushy" description rationale.

## Key Extracts

> "A skill that restates what Claude would do by default adds context without adding value." Effective skills teach things *outside* Claude's normal reasoning patterns.

> "The highest-signal content in any skill is the Gotchas section." Build it from real failure points; it becomes institutional memory around edge cases.

> Single-responsibility: "The best skills fit cleanly into one [category]; the ones that try to do too much straddle several and confuse the agent."

> "The description field is not a summary, it's a description of *when to trigger* this skill." Claude has "a measured tendency to under-trigger skills," so descriptions should be a little **"pushy"** — "use this skill when you need to X" rather than "this skill does X."

> Avoid "railroading Claude" with overly rigid instructions — provide necessary info but allow flexibility (mirrors degrees-of-freedom).

> Skills are folders, not markdown files. Use progressive disclosure via subfolders. Config via `config.json`; persist memory under `${CLAUDE_PLUGIN_DATA}`. Usage tracked via PreToolUse hooks. Skills "began simple (a few lines, one gotcha) and improved iteratively."

## Related Concepts

- [[concept-skill-review-criteria]] — supplies the "avoid obviousness" test, Gotchas-section, single-category, and pushy-description criteria
- [[concept-progressive-disclosure]] — skills-are-folders, subfolder disclosure

## Impact

Contributes the single sharpest judging criterion in [[concept-skill-review-criteria]] ("restates what Claude would do by default adds no value") and the deliberate-pushiness rationale for trigger-focused descriptions.
