# Source: OpenAI Codex Skills and Plugins

**Type**: article
**URL**: https://developers.openai.com/codex/skills, https://learn.chatgpt.com/docs/build-plugins
**Date Added**: 2026-07-23

## Summary

Official OpenAI Codex documentation on skills and plugins. Codex adopted the open Agent Skills standard (agentskills.io) in December 2025, so Claude-style SKILL.md folders work unmodified — same frontmatter requirements (`name`, `description`) and same lazy-loading of references. Skills are discovered from `.agents/skills` (repo-scoped), `~/.agents/skills` (user), and legacy `~/.codex/skills`. For plugins, Codex consumes the Claude plugin format directly (verified with Codex CLI 0.144.4): `codex plugin marketplace add owner/repo` reads `.claude-plugin/marketplace.json` and installs plugins from it — no Codex-specific manifest needed. Custom prompts are deprecated in favor of skills.

## Key Extracts

> Skills "build on the open agent skills standard" — a Claude-style skill works as-is.

> Invocation: explicit `$skill-name`, browse via `/skills`, or implicit description matching. The skills list is budgeted at 2% of context or 8,000 chars, making descriptions the routing signal.

> Codex resets skill activation per turn unless re-mentioned, unlike Claude Code which keeps the skill loaded.

> Codex-only metadata goes in `agents/openai.yaml` inside the skill folder (display name, invocation policy, tool deps) — ignored by Claude Code, safe in shared skills.

> Vercel's `npx skills add owner/repo` installs a `skills/<name>/SKILL.md` repo across 70+ agents, mapping to each agent's directory.

## Related Concepts

- [[concept-cross-agent-skill-portability]] - Codex's spec adoption is what makes single-source dual distribution possible
- [[concept-progressive-disclosure]] - Codex enforces the same model with an explicit context budget for skill listings
- [[entity-skill-md]] - identical file serves both ecosystems

## Impact

Confirmed the skill needs zero content changes for Codex users and that per-turn activation reset plus the 8,000-char listing budget make the SKILL.md description even more load-bearing on Codex. Local testing showed Codex installs directly from the Claude-format marketplace, collapsing "dual packaging" into a single set of manifests. Motivated [[decision-dual-plugin-packaging]].
