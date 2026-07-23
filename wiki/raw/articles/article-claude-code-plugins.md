# Claude Code Plugins and Marketplaces (research notes)

Compiled 2026-07-23 from official Claude Code documentation:
- https://code.claude.com/docs/en/plugins.md
- https://code.claude.com/docs/en/plugin-marketplaces.md
- https://code.claude.com/docs/en/skills.md

## Plugin structure

A plugin is a directory (typically a git repo) with a manifest at `.claude-plugin/plugin.json`. Components are auto-discovered from conventional directories at the plugin root:

```
plugin-root/
├── .claude-plugin/
│   └── plugin.json        # REQUIRED manifest
├── skills/<name>/SKILL.md # agent skills
├── commands/*.md          # slash commands
├── agents/                # custom agent definitions
├── hooks/hooks.json       # event handlers
├── .mcp.json              # MCP server configs
└── settings.json          # default settings
```

## Manifest schema (plugin.json)

Required: `name`, `description`, `version`, `author.name`.
Optional: `author.email`, `homepage`, `repository`, `license`, `keywords`.

```json
{
  "name": "llm-wiki",
  "description": "...",
  "version": "1.0.0",
  "author": { "name": "..." },
  "homepage": "https://github.com/...",
  "repository": "https://github.com/...",
  "license": "MIT",
  "keywords": ["..."]
}
```

## Marketplaces

A marketplace is a repo with `.claude-plugin/marketplace.json`. A single repo can be both a plugin and its own marketplace (manifest + marketplace catalog side by side in `.claude-plugin/`).

```json
{
  "name": "marketplace-name",
  "owner": { "name": "..." },
  "plugins": [
    { "name": "plugin-name", "source": "./", "description": "..." }
  ]
}
```

Source types: relative path (`"./path"`), GitHub (`{"source": "github", "repo": "owner/repo"}`), git URL, git-subdir, npm.

## Install commands

```
/plugin marketplace add owner/repo        # GitHub shorthand
/plugin install plugin-name@marketplace-name
/reload-plugins
claude plugin marketplace add owner/repo --scope project   # non-interactive
```

## Namespacing

Skills and commands inside plugins are namespaced as `plugin-name:item-name`. A plugin `llm-wiki` with skill `skills/llm-wiki/SKILL.md` is invoked as `llm-wiki:llm-wiki`; a command `commands/lint.md` becomes `/llm-wiki:lint`.

## Validation and local testing

- `claude plugin validate .` — checks JSON syntax, required fields, duplicate names, path traversal, frontmatter, hooks.json.
- `claude --plugin-dir ./plugin` — load without installing (dev loop).
- `/plugin marketplace add ./local/path` — local marketplace testing.
