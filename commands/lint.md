---
description: Run a wiki health check
---

Run a health check on the project wiki.

Read `${CLAUDE_PLUGIN_ROOT}/skills/llm-wiki/references/lint.md` and follow it. When it says to spawn a subagent, the role files live at `${CLAUDE_PLUGIN_ROOT}/skills/llm-wiki/references/agents/` and the templates directory is `${CLAUDE_PLUGIN_ROOT}/skills/llm-wiki/references/templates/` — use these absolute paths in spawn prompts; do not read the role files yourself. The AGENTS.md section template is at `${CLAUDE_PLUGIN_ROOT}/skills/llm-wiki/references/templates/agents-md-wiki-section.md`.
