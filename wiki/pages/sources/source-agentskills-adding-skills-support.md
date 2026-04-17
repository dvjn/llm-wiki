# Source: How to Add Skills Support to Your Agent — agentskills.io

**Type**: article
**URL**: https://agentskills.io/client-implementation/adding-skills-support.md
**Date Added**: 2026-04-17
**Key Sections**: Progressive disclosure tiers, Step 1 Discover, Step 2 Parse, Step 3 Disclose, Step 4 Activate, Step 5 Manage context

## Summary

The client implementation guide for agents that want to support the Agent Skills format. Covers skill discovery (which directories to scan, name collision rules, trust considerations), parsing (`SKILL.md` frontmatter extraction, lenient validation), catalog disclosure to the model (~50-100 tokens per skill in system prompt or tool description), activation (file-read vs. dedicated tool, user-explicit slash commands, structured wrapping), and context management (protecting skill content from compaction, deduplication, subagent delegation).

## Key Extracts

> "Protect skill content from context compaction: Skill instructions are durable behavioral guidance — losing them mid-conversation silently degrades the agent's performance without any visible error."

> "Consider gating project-level skill loading on a trust check — prevents untrusted repositories from silently injecting instructions."

> "Hide filtered skills entirely from the catalog rather than listing them and blocking at activation time."

> "Lenient validation: Warn on issues but still load the skill when possible."

## Key Insights for This Project

- **Cross-client standard directory**: `.agents/skills/` has emerged as the cross-client convention. The wiki skill is currently installed in an agent-specific location. Publishing to `.agents/skills/` would make it usable across any compliant agent.
- **Catalog token cost**: each skill adds ~50-100 tokens at startup. The wiki skill competes with other skills for context budget — a concise description pays dividends.
- **Context compaction risk**: if the agent compacts context mid-session, skill instructions may be dropped. This is the runtime equivalent of the wiki's "stale claim" lint check — silent quality degradation.
- **Lenient validation**: agents are expected to handle malformed frontmatter gracefully. The wiki skill's `SKILL.md` should be spec-conformant to work across all clients.
- **Trust model**: project-level skills can be silently injected by a repository. Relevant if the llm-wiki skill is ever distributed as part of a project template.

## Related Concepts

- [[concept-progressive-disclosure]] — this guide is the client-side implementation of the three-tier model

## Related Entities

- [[entity-skill-md]] — the `SKILL.md` format this guide explains how to parse and load

## Related Sources

- [[source-agentskills-llms-txt]] — index this page was discovered from
- [[source-agentskills-specification]] — the format spec this guide implements
