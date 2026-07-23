# Source: Claude Code — Skills Authoring & Invocation (official docs)

**Type**: article (official docs)
**URL**: https://code.claude.com/docs/en/skills
**Date Added**: 2026-07-24

## Summary

The Claude Code-specific layer on top of the Agent Skills open standard. It covers the invocation model, Claude Code frontmatter extensions, and the runtime budgets/lifecycle a skill author must design around. Key runtime facts: the skill listing truncates combined `description` + `when_to_use` at 1,536 characters (distinct from the 1,024-char frontmatter validation limit), invoked SKILL.md content stays in context for the rest of the session, and skills, subagents, and commands are now unified (commands are skills). It also documents baseline-comparison evaluation via the `skill-creator` plugin.

## Key Extracts

> **Description listing cap: combined `description` + `when_to_use` truncated at 1,536 characters** in the skill listing "to reduce context usage." "Put the key use case first." (Differs from the 1,024-char frontmatter validation limit in [[source-anthropic-skill-best-practices]] — two different numbers for related things.)

> Listing budget scales at 1% of the model's context window; on overflow "Claude Code drops descriptions starting with the skills you invoke least" — too many skills strips keywords from rarely-used ones.

> Skill content lifecycle: invoked SKILL.md "enters the conversation as a single message and stays there for the rest of the session." Claude Code does not re-read the file — "write guidance that should apply throughout a task as standing instructions rather than one-time steps."

> "Commands are now skills." `.claude/commands/deploy.md` and `.claude/skills/deploy/SKILL.md` both create `/deploy`; if a skill and command share a name, the skill wins.

> `context: fork` runs a skill in an isolated subagent context — but "only makes sense for skills with explicit instructions"; a pure-guidelines skill forked as a subagent "receives the guidelines but no actionable prompt, and returns without meaningful output."

> Invocation control: `disable-model-invocation: true` (user-only, for side-effecting workflows), `user-invocable: false` (Claude-only, for background knowledge).

> Evaluate: "Seeing a skill trigger tells you Claude found it, not that it did what you intended." Method is **baseline comparison** — run realistic prompts in a fresh session with the skill and with it disabled, compare. "A fresh session matters because leftover context from authoring the skill will mask gaps."

## Related Concepts

- [[concept-skill-review-criteria]] — supplies the runtime-lifecycle and baseline-comparison eval axes
- [[concept-progressive-disclosure]] — 1,536-char listing cap and body-stays-in-context lifecycle
- [[decision-subagent-delegation]] — `context: fork` warning is directly relevant to the wiki's forked-skill design

## Impact

Clarifies the 1,536-char runtime listing cap vs. the 1,024-char frontmatter limit, and the `context: fork` "needs explicit instructions" warning that bears on the wiki's delegation design. Feeds the baseline-comparison eval criterion in [[concept-skill-review-criteria]].
