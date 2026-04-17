# Source: Using Scripts in Skills

**Type**: article
**URL**: https://agentskills.io/skill-creation/using-scripts.md
**Date Added**: 2026-04-17
**Key Sections**: §One-off commands, §Referencing scripts from SKILL.md, §Self-contained scripts, §Designing scripts for agentic use

## Summary

This guide covers how to integrate executable scripts into Agent Skills, addressing the decision between one-off commands (using runtime tools like `uvx`, `npx`, `deno run`) versus bundling self-contained scripts in a `scripts/` directory. It provides concrete guidance on making scripts work reliably in agentic contexts, with hard requirements around avoiding interactive prompts and detailed recommendations for structured output, helpful error messages, and design considerations like idempotency and dry-run support.

## Key Extracts

> Agents operate in non-interactive shells. A script that blocks on interactive input will hang indefinitely. Accept all input via command-line flags, environment variables, or stdin.

> `--help` output is the primary way an agent learns your script's interface.

> Prefer JSON, CSV, TSV over free-form text. Send structured data to stdout and diagnostics to stderr.

> Error messages should say what went wrong, what was expected, and what to try.

## Key Insights for This Project

This guide offers practical patterns we can apply to our llm-wiki tooling:

- **Self-contained scripts with inline dependencies**: The guide endorses modern dependency declaration patterns (Python PEP 723, Deno's `npm:`/`jsr:` specifiers, Bun's auto-install) that make scripts portable without separate setup steps. This is directly relevant if we add executable tooling to our skill—for example, scripts that validate wiki structure or generate cross-references.

- **Agentic design requirements**: The hard requirement against interactive prompts and the emphasis on `--help` documentation directly inform how any CLI tooling we build should be designed. Our lint workflows should be fully non-interactive and self-documenting.

- **Structured output preference**: Recommending JSON/CSV over free-form text aligns with our need for machine-readable wiki metadata. Structured output enables better programmatic consumption during lint and query operations.

- **Operational concerns**: The guide's points on idempotency (agents may retry), input validation, dry-run support, meaningful exit codes, and predictable output size collectively define what "production-ready" tooling looks like for agentic contexts. These same concerns apply to our wiki maintenance scripts.

## Related Concepts

- [[concept-ingest-workflow]] - the ingestion process could benefit from self-contained scripts with inline dependencies
- [[concept-lint-workflow]] - should follow agentic design principles: non-interactive, structured output, helpful errors
- [[concept-query-workflow]] - structured output recommendation supports better query parsing

## Related Entities

- [[entity-skill-md]] - scripts are referenced from within SKILL.md using relative paths

## Related Sources

- [[source-agentskills-quickstart]] - foundational guide that introduces the skill structure this guide builds upon
- [[source-agentskills-best-practices]] - broader best practices for skill creation
- [[source-agentskills-optimizing-descriptions]] - optimizing the description field that triggers skill activation
- [[source-agentskills-evaluating-skills]] - evaluating whether skills work correctly in agentic contexts
