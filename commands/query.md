---
description: Answer a question from the wiki
argument-hint: [question]
---

Answer this question from the project wiki: $ARGUMENTS

Read `${CLAUDE_PLUGIN_ROOT}/skills/llm-wiki/references/query.md` and follow it. When it says to spawn a subagent, the role files live at `${CLAUDE_PLUGIN_ROOT}/skills/llm-wiki/references/agents/` and the templates directory is `${CLAUDE_PLUGIN_ROOT}/skills/llm-wiki/references/templates/` — use these absolute paths in spawn prompts; do not read the role files yourself.

If no question was given, ask what to look up.
