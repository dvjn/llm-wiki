---
description: Ingest a source into the wiki
argument-hint: [path or URL of the source]
---

Ingest the following source into the project wiki: $ARGUMENTS

Read `${CLAUDE_PLUGIN_ROOT}/skills/llm-wiki/references/ingest.md` and follow the workflow. Load page templates from `${CLAUDE_PLUGIN_ROOT}/skills/llm-wiki/references/templates/` only as needed — one template per page type you create. Update `wiki/index.md` and append to `wiki/log.md`.

If no source was given, ask which source to ingest.
