---
description: Ingest a source into the wiki
argument-hint: [path or URL of the source]
---

Ingest the following source into the project wiki: $ARGUMENTS

Read `${CLAUDE_PLUGIN_ROOT}/skills/llm-wiki/references/ingest.md` and follow it. When it says to spawn a subagent, the role files live at `${CLAUDE_PLUGIN_ROOT}/skills/llm-wiki/references/agents/` and the templates directory is `${CLAUDE_PLUGIN_ROOT}/skills/llm-wiki/references/templates/` — use these absolute paths in spawn prompts; do not read the role files yourself.

If no source was given, ask which source to ingest.
