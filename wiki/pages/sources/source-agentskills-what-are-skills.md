# Source: What Are Skills? — agentskills.io

**Type**: article
**URL**: https://agentskills.io/what-are-skills.md
**Date Added**: 2026-04-17
**Key Sections**: How skills work, The SKILL.md file

## Summary

Introduction to the Agent Skills open format. A skill is a directory containing a `SKILL.md` file with YAML frontmatter (name + description required) and Markdown instructions. Skills use progressive disclosure: agents load only name/description at startup, load the full `SKILL.md` when a task matches, and load referenced files only when needed. The format is intentionally minimal and portable — skills are just files, easy to edit, version, and share.

## Key Extracts

> "Agent Skills are a lightweight, open format for extending AI agent capabilities with specialized knowledge and workflows."

> "Skills use **progressive disclosure** to manage context efficiently."

> "**Self-documenting**: A skill author or user can read a `SKILL.md` and understand what it does, making skills easy to audit and improve."

> "**Portable**: Skills are just files, so they're easy to edit, version, and share."

## Key Insights

- **Progressive disclosure as the core design principle**: Three stages — discovery (name+desc only), activation (full SKILL.md), execution (referenced files on demand). This is why skills stay fast even in large collections.
- **Minimal required surface**: Only `name` and `description` are required. Everything else is optional.
- **Self-documenting by design**: The `SKILL.md` format is meant to be human-readable, making it auditable and improvable without tooling.

## Related Concepts

- [[concept-progressive-disclosure]] — the loading strategy described here
- [[concept-three-layer-architecture]] — parallel structure (raw/pages/schema vs metadata/instructions/resources)

## Related Entities

- [[entity-skill-md]] — this article defines what SKILL.md must contain
