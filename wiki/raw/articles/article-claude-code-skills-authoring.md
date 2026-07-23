# Claude Code — Skills Authoring & Invocation (official docs)

**Source:** https://code.claude.com/docs/en/skills
**Retrieved:** 2026-07-24
**Prompted by:** research into Anthropic's official skill/plugin authoring guidance — specifically the skills-vs-subagents-vs-commands division of labor and Claude Code runtime behavior that constrains skill design.

Claude Code-specific layer on top of the Agent Skills open standard. Complements the product-agnostic [[article-anthropic-skill-best-practices]] and the existing plugin note. Focuses on invocation model, frontmatter fields, and runtime lifecycle that a skill author must design around.

## When to create a skill (division of labor)

"Create a skill when you keep pasting the same instructions, checklist, or multi-step procedure into chat, or when a section of CLAUDE.md has grown into a procedure rather than a fact." Key contrast: **"Unlike CLAUDE.md content, a skill's body loads only when it's used, so long reference material costs almost nothing until you need it."**

**Commands are now skills.** "Custom commands have been merged into skills." `.claude/commands/deploy.md` and `.claude/skills/deploy/SKILL.md` both create `/deploy`. Old `.claude/commands/` files keep working; skills add supporting-files directory, invocation-control frontmatter, and automatic model loading. If a skill and command share a name, the skill wins.

Two content types the docs name:
- **Reference content** — knowledge applied to current work (conventions, style guides, domain knowledge); runs inline.
- **Task content** — step-by-step instructions for an action (deploy, commit, codegen); "often actions you want to invoke directly with `/skill-name`", frequently paired with `disable-model-invocation: true`.

## Skills vs. subagents

`context: fork` runs a skill in an isolated subagent context (the SKILL.md content becomes the subagent's prompt; no access to conversation history). Two directions of composition:

| Approach | System prompt | Task | Also loads |
|---|---|---|---|
| Skill with `context: fork` | From agent type | SKILL.md content | CLAUDE.md (except Explore/Plan agents) |
| Subagent with `skills` field | Subagent's markdown body | Claude's delegation message | Preloaded skills + CLAUDE.md |

Warning: `context: fork` "only makes sense for skills with explicit instructions" — a pure-guidelines skill forked as a subagent "receives the guidelines but no actionable prompt, and returns without meaningful output." (Directly relevant to llm-wiki's [[decision-subagent-delegation]] design.)

## Invocation control (who triggers)

- `disable-model-invocation: true` — only the user can invoke (`/name`); use for side-effecting workflows (`/commit`, `/deploy`, `/send-slack-message`) — "You don't want Claude deciding to deploy because your code looks ready." Also removes the description from Claude's context and prevents preloading into subagents.
- `user-invocable: false` — only Claude can invoke; use for non-actionable background knowledge (e.g. a `legacy-system-context` skill).
- Default: both can invoke; description always in context, full body loads on invoke.

## Frontmatter fields (Claude Code extensions beyond name/description)

`when_to_use` (trigger phrases, appended to description), `argument-hint`, `arguments`, `allowed-tools` (per-turn pre-approval, grant clears next message), `disallowed-tools`, `model`, `effort`, `context: fork`, `agent`, `background`, `hooks`, `paths` (glob patterns limiting auto-activation), `shell`. All optional; only `description` recommended.

## Runtime budgets and lifecycle (constrains authoring)

- **Description listing cap: combined `description` + `when_to_use` truncated at 1,536 characters** in the skill listing "to reduce context usage." "Put the key use case first." Configurable via `skillListingMaxDescChars`. (Note this differs from the 1,024-char frontmatter validation limit in [[article-anthropic-skill-best-practices]].)
- **Listing budget scales at 1% of the model's context window** (`skillListingBudgetFraction`, or fixed `SLASH_COMMAND_TOOL_CHAR_BUDGET`). When it overflows, "Claude Code drops descriptions starting with the skills you invoke least" — so having too many skills actively strips keywords from rarely-used ones. `/doctor` estimates the listing's context cost.
- **Skill content lifecycle:** invoked SKILL.md "enters the conversation as a single message and stays there for the rest of the session." Claude Code does not re-read the file on later turns — "write guidance that should apply throughout a task as standing instructions rather than one-time steps." Re-invoking identical content adds only a short "already loaded" note.
- **Keep the body concise** — "once a skill loads, its content stays in context across turns, so every line is a recurring token cost. State what to do rather than narrating how or why." Same 500-line guidance: "Keep SKILL.md under 500 lines. Move detailed reference material to separate files."
- **Auto-compaction:** invoked skills carried forward keeping first 5,000 tokens each, combined budget 25,000 tokens, filled from most-recent; older skills can be dropped entirely.

## Triggering troubleshooting (judging discovery)
Not triggering: description should include keywords users naturally say; verify via "What skills are available?"; rephrase to match; invoke directly. Malformed YAML → body loads with empty metadata, so `/name` works but no description to match (use `--debug`). Triggers too often → make description more specific or add `disable-model-invocation: true`.

## Evaluate and iterate (Claude Code specifics)
"Seeing a skill trigger tells you Claude found it, not that it did what you intended." Measure two things separately: whether Claude invokes it when it should, and whether output matches expectations. Method: **baseline comparison** — run realistic prompts in a fresh session with the skill and with it disabled, compare. "A fresh session matters because leftover context from authoring the skill will mask gaps." The `skill-creator` plugin (github.com/anthropics/claude-plugins-official) automates this: `evals/evals.json` test cases, isolated subagent per case (records tokens + duration), `grading.json`, `benchmark.json` (with-skill vs without-skill pass rate / time / tokens), blind A/B version comparison, and description tuning (should-trigger vs should-not-trigger hit rate).

## Distribution
Scopes: project (`.claude/skills/` in VCS), plugin (`skills/` dir), managed (org-wide). Precedence: enterprise > personal > project; any level overrides a bundled skill of the same name; plugin skills use `plugin-name:skill-name` namespace so they never conflict. Nested `.claude/skills/` in subdirectories load on-demand for monorepos (directory-qualified names like `apps/web:deploy`).
