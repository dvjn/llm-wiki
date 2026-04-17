# Source: agentskills Quickstart

**Type**: article
**URL**: https://agentskills.io/skill-creation/quickstart.md
**Date Added**: 2026-04-17
**Key Sections**: §Create the skill, §How it works (progressive disclosure)

## Summary

This quickstart guide demonstrates how to create a basic Agent Skill by building a dice-rolling skill in VS Code. It introduces the fundamental skill structure—a folder containing a `SKILL.md` file located at `.agents/skills/`—and walks through the three-phase execution model known as progressive disclosure: discovery (where the agent scans skills and reads metadata), activation (where description matching triggers full SKILL.md loading), and execution (where the agent follows instructions). The guide emphasizes that the `description` field in frontmatter serves as the routing signal that determines when a skill activates.

## Key Extracts

> A skill is a folder containing a `SKILL.md` file. VS Code looks for skills in `.agents/skills/` by default.

> **Discovery** — Agent scans skill directories and reads only `name` and `description` at startup.

> When you ask about rolling dice, the agent matches your question to the description and loads the full `SKILL.md` body.

## Key Insights for This Project

The quickstart reinforces several core concepts for our llm-wiki skill:

- **Skill discovery architecture**: Skills live in a predictable directory structure (`.agents/skills/`) with each skill as a self-contained folder containing its own `SKILL.md`. This mirrors our own `wiki/` directory structure where source articles and generated pages live in predictable locations.

- **Progressive disclosure model**: The three-phase model (discovery → activation → execution) aligns with our wiki's layered architecture. Our raw sources correspond to "discovery" data, our generated wiki pages to "activation" content, and our lint/validate workflows to "execution" tasks.

- **Description as routing signal**: The `description` field is the activation trigger. This is analogous to how we use frontmatter `description` in our source summary pages to help LLMs understand when to reference a source.

- **Frontmatter convention**: Skills use YAML frontmatter with `name` and `description` fields. Our source summaries follow a similar pattern with `Type`, `URL`, and metadata fields that help route queries to relevant content.

## Related Concepts

- [[concept-progressive-disclosure]] - the three-phase execution model (discovery, activation, execution) that this guide demonstrates
- [[concept-ingest-workflow]] - our workflow for processing raw sources into wiki pages, analogous to how skills are loaded
- [[entity-skill-md]] - the SKILL.md specification this quickstart introduces

## Related Entities

- [[entity-skill-md]] - the skill file format with YAML frontmatter that defines name, description, and instructions

## Related Sources

- [[source-agentskills-best-practices]] - deeper guidance on skill creation patterns
- [[source-agentskills-using-scripts]] - companion guide on bundling executable scripts within skills
- [[source-agentskills-llms-txt]] - index resource for the agentskills documentation
