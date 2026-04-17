# Page Templates

**Only load the template for the page type you're creating.**

## Concept Page (`pages/concepts/`)

```markdown
# [Concept Name]

> Brief one-line definition

## Core Definition
[2-3 sentences]

## Key Properties
- Property: explanation

## In [Domain]
[How this manifests]

## Related Concepts
- [[concept-x]] - relationship

## Sources
- [[source-name]] § section
```

## Entity Page (`pages/entities/`)

```markdown
# [Entity Name]

> One-line role description

## Purpose
[What it does]

## Structure
[Fields, properties]

## Lifecycle
[Creation, updates, destruction]

## Related
- [[entity-x]] - relationship
```

## Decision Page (`pages/decisions/`)

```markdown
# Decision: [Title]

**Status**: Accepted | Proposed | Superseded
**Date**: YYYY-MM-DD
**Context**: [What triggered this]

## Decision
[The choice]

## Rationale
[Why - alternatives, trade-offs]

## Consequences
- Positive: ...
- Negative: ...
- Risks: ...

## Related
- [[decision-x]], [[concept-x]]
```

## Comparison Page (`pages/comparisons/`)

```markdown
# [A] vs [B]

> Brief summary

## Dimensions
| Dimension | [A] | [B] |
|-----------|-----|-----|
| X         | ... | ... |

## Analysis
[Trade-offs, when each fits]

## Sources
- [[source-a]], [[source-b]]
```

## Source Summary (`pages/sources/`)

```markdown
# Source: [Title]

**Type**: spec | paper | article
**URL**: [if web source]
**Date Added**: YYYY-MM-DD

## Summary
[3-5 sentences]

## Key Extracts
> [Direct quotes or key points]

## Related Concepts
- [[concept-x]] - how this informs it

## Impact
[How this changed understanding]
```

## Stub Page

Use when concept is important but underdeveloped. See `references/conventions.md` §Stub Pages.
