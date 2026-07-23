# Source: Claude Code Plugins and Marketplaces

**Type**: article
**URL**: https://code.claude.com/docs/en/plugins.md, https://code.claude.com/docs/en/plugin-marketplaces.md
**Date Added**: 2026-07-23

## Summary

Official Claude Code documentation on packaging and distributing plugins. A plugin is a directory with a `.claude-plugin/plugin.json` manifest; skills, commands, agents, hooks, and MCP configs are auto-discovered from conventional directories at the plugin root. A single GitHub repo can serve as both a plugin and its own marketplace by adding `.claude-plugin/marketplace.json`, letting users install with `/plugin marketplace add owner/repo` followed by `/plugin install name@marketplace`. All plugin components are namespaced as `plugin-name:item-name`.

## Key Extracts

> Required manifest fields: `name`, `description`, `version`, `author.name`. Location: `<plugin-root>/.claude-plugin/plugin.json`.

> A marketplace entry's `source` can be a relative path (`"./"`) — so one repo is simultaneously plugin and marketplace.

> Skills in `skills/*/SKILL.md` and commands in `commands/*.md` are auto-discovered; a command `commands/lint.md` in plugin `llm-wiki` is invoked as `/llm-wiki:lint`.

> Validation: `claude plugin validate .`; local dev: `claude --plugin-dir ./plugin` and `/plugin marketplace add ./local/path`.

## Related Concepts

- [[concept-cross-agent-skill-portability]] - the plugin manifest wraps a portable skill without modifying it
- [[concept-progressive-disclosure]] - plugin skills keep the same lazy-loading behavior as standalone skills
- [[entity-skill-md]] - SKILL.md is unchanged when packaged inside a plugin

## Impact

Established that the existing `skills/llm-wiki/SKILL.md` layout is already plugin-compatible — packaging requires only additive manifest files, no restructuring. Motivated [[decision-dual-plugin-packaging]] and the addition of directly-callable commands (`/llm-wiki:lint` etc.) wrapping the skill's workflow references.
