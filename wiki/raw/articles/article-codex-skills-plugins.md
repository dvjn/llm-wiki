# OpenAI Codex Skills and Plugins (research notes)

Compiled 2026-07-23 from official Codex documentation and ecosystem sources:
- https://developers.openai.com/codex/skills (→ https://learn.chatgpt.com/docs/build-skills)
- https://learn.chatgpt.com/docs/build-plugins
- https://learn.chatgpt.com/docs/custom-prompts
- https://github.com/vercel-labs/skills
- https://blog.fsck.com/2025/12/19/codex-skills/

## Codex adopted the Agent Skills spec (Dec 2025)

Codex CLI supports the open agentskills.io standard: a skill is a folder with `SKILL.md` (YAML frontmatter requiring `name` + `description`). A Claude-style skill works as-is — Codex loads SKILL.md up front and reads references lazily, same progressive-disclosure model.

## Skill discovery directories (precedence high→low)

1. Repo-scoped: `.agents/skills` in `$CWD` up to `$REPO_ROOT/.agents/skills`
2. User-scoped: `~/.agents/skills`
3. Admin-scoped: `/etc/codex/skills`
4. Bundled system skills (`$skill-creator`, `$plan`, `$skill-installer`)

`~/.codex/skills/` is the legacy (Dec 2025) global path; still works, but `~/.agents/skills` is canonical per current docs. No official deprecation statement found.

## Invocation and differences from Claude Code

- Explicit: `$skill-name`; browse with `/skills`; implicit via description matching.
- Skills list is budgeted at max 2% of context window or 8,000 chars — descriptions are the routing signal.
- Codex resets skill activation per turn unless re-mentioned (Claude Code keeps it loaded).
- Codex-specific optional metadata lives in `agents/openai.yaml` inside the skill folder (display name, icons, `policy.allow_implicit_invocation`, tool dependencies). Ignored by Claude Code — safe in a shared skill.
- Claude-specific frontmatter (`allowed-tools`, etc.) is ignored by Codex.

## Plugins (2026)

A Codex plugin bundles skills, hooks, apps, and MCP servers. **Empirically verified with Codex CLI 0.144.4 (2026-07-23): Codex consumes the Claude plugin format directly** — `codex plugin marketplace add <repo-or-path>` reads `.claude-plugin/marketplace.json`, and `codex plugin add name@marketplace` installs the plugin to `~/.codex/plugins/cache/<marketplace>/<name>/<version>/`, recording it in `~/.codex/config.toml` under `[marketplaces.*]` and `[plugins."name@marketplace"]`. No separate `.codex-plugin/` manifest is needed (a repo with only `.claude-plugin/` installs cleanly).

Install: `codex plugin marketplace add owner/repo` (or local path), then `codex plugin add name@marketplace`. The old `github.com/openai/skills` catalog is deprecated in favor of plugins. Fallback: `$skill-installer install https://github.com/owner/repo/tree/main/skills/<name>`.

Custom prompts (`~/.codex/prompts/*.md`, invoked `/prompts:name`) are officially deprecated in favor of skills.

## Cross-tool installer

Vercel's `npx skills` (github.com/vercel-labs/skills, directory at skills.sh) installs SKILL.md skills across 70+ agents:

```bash
npx skills add owner/repo
npx skills add owner/repo --skill name -a claude-code -a codex
```

Mapping: Claude Code → `.claude/skills/` / `~/.claude/skills/`; Codex → `.agents/skills/` (project) / `~/.codex/skills/` (global). A repo with `skills/<name>/SKILL.md` layout is one-command installable in both ecosystems.
