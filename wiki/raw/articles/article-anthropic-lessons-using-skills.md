# Anthropic Engineering — "Lessons from building Claude Code: How we use skills"

**Source:** https://claude.com/blog/lessons-from-building-claude-code-how-we-use-skills
**Retrieved:** 2026-07-24
**Prompted by:** research into Anthropic's official best practices and the parameters by which a skill should be judged — this post reflects how Anthropic judges skills internally at scale (hundreds in active use).

Anthropic's own account of running "hundreds" of internal skills. Higher-signal on *judging quality* and *maintainability at scale* than the reference docs. Extends [[article-anthropic-skill-best-practices]].

## How they judge a skill's value

- **Avoid obviousness:** "A skill that restates what Claude would do by default adds context without adding value." Effective skills teach things *outside* Claude's normal reasoning patterns, not basic coding knowledge. (This is the strongest single judging criterion — it operationalizes the "Claude is already very smart" principle from the best-practices doc.)
- **The Gotchas section matters most:** "The highest-signal content in any skill is the Gotchas section." Build it from real failure points Claude hits; it becomes institutional memory around edge cases.
- **One clean category, not several:** a taxonomy of nine skill categories (library reference, product verification, data fetching, business automation, scaffolding, code quality, CI/CD, runbooks, infrastructure). "The best skills fit cleanly into one; the ones that try to do too much straddle several and confuse the agent." (A scope/single-responsibility test.)

## Description & triggering

- "The description field is not a summary, it's a description of *when to trigger* this skill." Include trigger terms (their example: "babysit" for a monitoring task).
- Claude has "a measured tendency to under-trigger skills," so descriptions should be a little **"pushy"** — phrase as "use this skill when you need to X" rather than "this skill does X." (Matches the third-person "Use when..." format in the reference docs but adds the deliberate-pushiness rationale.)
- **Under-triggering vs over-specification:** avoid "railroading Claude" with overly rigid instructions — provide necessary info but allow flexibility to adapt to context. (Mirrors the degrees-of-freedom guidance.)

## Structure, config, memory

- Skills are folders, not markdown files — a "common misconception is that they are 'just markdown files'." Use progressive disclosure via subfolders (`references/`, `assets/`, `scripts/`).
- **Configuration:** include `config.json` for user setup info; prompt Claude to request missing config via the `AskUserQuestion` tool.
- **Memory:** persist state under the `${CLAUDE_PLUGIN_DATA}` env var — append-only text logs (their "standup" skill keeps a log of every post it wrote), JSON state files, or SQLite. Lets skills "remember past events" across sessions.
- Skills can reference other skills by name; the model will invoke them if installed. Dependencies are manually managed.

## Maintainability at scale

- Usage tracked via **PreToolUse hooks logging skill invocations** — identifies popular skills and ones under-triggering vs expectations.
- Skills "began simple (a few lines, one gotcha) and improved iteratively as Claude encountered edge cases." (Grow the skill from real usage, don't front-load.)
- **Distribution scaling:** small teams check skills into `./.claude/skills`; at scale, internal plugin marketplaces let teams choose what to install — "reducing context overhead while enabling organic skill discovery" (skills surface in Slack channels before marketplace promotion). This is the operational answer to the "too many skills strips the listing budget" problem noted in [[article-claude-code-skills-authoring]].
