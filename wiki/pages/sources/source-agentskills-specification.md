# Source: Specification — agentskills.io

**Type**: article
**URL**: https://agentskills.io/specification.md
**Date Added**: 2026-04-17
**Key Sections**: Directory structure, SKILL.md format, Frontmatter fields, Progressive disclosure, File references, Validation

## Summary

The normative specification for the Agent Skills format. Defines the full SKILL.md frontmatter schema (required: `name`, `description`; optional: `license`, `compatibility`, `metadata`, `allowed-tools`), directory conventions (`scripts/`, `references/`, `assets/`), and progressive disclosure budgets (metadata ~100 tokens, instructions <5000 tokens recommended / <500 lines, resources on demand). Also covers naming constraints for the `name` field (lowercase, hyphens, max 64 chars, must match directory name) and the experimental `allowed-tools` field for pre-approving tool use.

## Key Extracts

> "Skills should be structured for efficient use of context: Metadata (~100 tokens)... Instructions (< 5000 tokens recommended)... Resources (as needed)"

> "Keep your main `SKILL.md` under 500 lines. Move detailed reference material to separate files."

> "Keep file references one level deep from `SKILL.md`. Avoid deeply nested reference chains."

## Frontmatter Reference

| Field           | Required | Key Constraint |
| --------------- | -------- | -------------- |
| `name`          | Yes      | Max 64 chars, lowercase + hyphens only, must match directory name |
| `description`   | Yes      | Max 1024 chars; describe what + when |
| `license`       | No       | License name or bundled file ref |
| `compatibility` | No       | Max 500 chars; environment requirements |
| `metadata`      | No       | Arbitrary string key-value map |
| `allowed-tools` | No       | Space-separated pre-approved tools (experimental) |

## Key Insights

- **`name` must match directory name**: Enforced constraint — the skill folder name and the `name` field must be identical. Important for packaging and discovery.
- **`allowed-tools` is experimental**: Pre-approving tools reduces friction for users but support varies across agent implementations.
- **Token budgets are explicit**: The spec quantifies the progressive disclosure tiers (100 / 5000 tokens), giving skill authors a concrete target.
- **One level of reference depth**: File references should stay shallow — `SKILL.md` → `references/foo.md`, not `references/foo.md` → `references/bar.md`.

## Related Concepts

- [[concept-progressive-disclosure]] — the three-tier loading model defined here
- [[concept-three-layer-architecture]] — structural parallel to the wiki's own layers

## Related Entities

- [[entity-skill-md]] — this spec normatively defines what SKILL.md must contain
