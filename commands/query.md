---
description: Answer a question from the wiki
argument-hint: [question]
---

Answer this question from the project wiki: $ARGUMENTS

Read `${CLAUDE_PLUGIN_ROOT}/skills/llm-wiki/references/query.md` and follow the workflow: start from `wiki/index.md`, cite pages at the section level, and suggest filing durable synthesis back into the wiki.

If no question was given, ask what to look up.
