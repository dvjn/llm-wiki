# Decision: Dual Plugin Packaging (Claude Code + Codex)

**Status**: Accepted
**Date**: 2026-07-23
**Context**: The skill was only installable via `npx skills install dvjn/llm-wiki`. Both major agent ecosystems now have first-class plugin distribution, and users expect native install flows (`/plugin install` in Claude Code, `codex plugin marketplace add` in Codex).

## Decision

Package the existing repo as both a Claude Code plugin and a Codex plugin, keeping `skills/llm-wiki/` as the untouched portable core:

- `.claude-plugin/plugin.json` + `.claude-plugin/marketplace.json` — the repo is its own single-plugin marketplace; install via `/plugin marketplace add dvjn/llm-wiki` then `/plugin install llm-wiki@dvjn`.
- The same Claude-format manifests serve Codex — verified locally (Codex CLI 0.144.4) that `codex plugin marketplace add dvjn/llm-wiki` + `codex plugin add llm-wiki@dvjn` install from `.claude-plugin/marketplace.json` with no Codex-specific manifest.
- `commands/` (Claude Code only): thin slash commands `/llm-wiki:setup`, `/llm-wiki:ingest`, `/llm-wiki:query`, `/llm-wiki:lint`, each loading only the matching workflow reference — preserving the skill's lazy-loading discipline while making workflows directly callable.
- `npx skills add dvjn/llm-wiki` continues to work unchanged (same `skills/<name>/SKILL.md` layout).

## Rationale

- The Agent Skills standard is shared by both ecosystems ([[concept-cross-agent-skill-portability]]), so packaging is purely additive — no fork, no content drift.
- Alternative rejected: separate distribution repos per ecosystem — breaks the existing `npx skills` path and creates three copies of the skill to keep in sync.
- Alternative rejected: converting workflows into separate skills per workflow — Codex would list four skills competing for the 8,000-char listing budget, and Claude Code routing already works through the single skill description; commands give direct invocation without fragmenting the skill.
- Self-hosted marketplace over a separate marketplace repo: one repo to maintain, and the marketplace `source: "./"` mechanism exists precisely for this.

## Consequences

- Positive: three install paths from one repo; zero changes to skill content; commands make workflows one keystroke away in Claude Code.
- Negative: plugin installs clone the whole repo including the dogfood `wiki/` (~40 files) — disk cost only, since agents load just the skill.
- Risks: Codex's compatibility with the Claude plugin format is observed behavior (CLI 0.144.4), not a documented guarantee — it may diverge; `~/.codex/skills` vs `~/.agents/skills` path transition could confuse manual installers. Mitigated by documenting all install paths in README.

## Related

- [[concept-cross-agent-skill-portability]], [[decision-schema-in-wiki-dir]], [[entity-skill-md]]
- [[source-claude-code-plugins]], [[source-codex-skills-plugins]]
