# Cross-Agent Skill Portability

> One SKILL.md folder serving multiple agent ecosystems through the open Agent Skills standard, wrapped by thin per-agent packaging manifests.

## Core Definition

Because the Agent Skills spec (agentskills.io) is an open standard adopted by both Anthropic (Claude Code) and OpenAI (Codex, Dec 2025), a single skill folder — SKILL.md plus lazily-loaded references — can be distributed to both ecosystems without content changes. Packaging differences are confined to additive manifest files and optional per-agent metadata (`agents/openai.yaml`), never to the skill body — and in practice Codex consumes the Claude plugin manifests (`.claude-plugin/`) directly, so one set of manifests serves both.

## Key Properties

- **Single source of truth**: SKILL.md and references are shared; forking per agent would drift.
- **Additive packaging**: each ecosystem's manifest sits beside the skill, not inside it; unknown files/frontmatter are ignored by the other agent.
- **Description as routing signal everywhere**: both ecosystems trigger skills off the frontmatter description; Codex additionally caps the skills listing at 2% of context or 8,000 chars.
- **Behavioral deltas remain**: Codex resets skill activation per turn and uses `$name` invocation; Claude Code keeps skills loaded and namespaces plugin skills as `plugin:skill`. The skill text must not assume either behavior.
- **Cross-tool installers exist**: `npx skills add owner/repo` (Vercel) installs a `skills/<name>/SKILL.md` repo into 70+ agents' directories.

## In This Skill

The llm-wiki repo keeps `skills/llm-wiki/` as the portable core and layers `.claude-plugin/` (plugin + self-hosted marketplace, consumed by both Claude Code and Codex) on top, per [[decision-dual-plugin-packaging]]. Claude-only conveniences (slash commands wrapping the lint/ingest/query/setup references) live in `commands/`, outside the skill folder.

## Related Concepts

- [[concept-progressive-disclosure]] - the shared loading model both ecosystems implement
- [[concept-type-prefixed-links]] - wiki-internal convention unaffected by packaging

## Sources

- [[source-claude-code-plugins]] § Plugin structure, Namespacing
- [[source-codex-skills-plugins]] § Skill discovery, Invocation differences
- [[source-agentskills-specification]] § directory conventions
