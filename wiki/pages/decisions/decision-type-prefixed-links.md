# Decision: Require Type Prefixes on All Wiki Links

**Status**: Accepted
**Date**: 2026-04-17
**Context**: Wiki pages for concepts and entities frequently share names (e.g. "authentication" as a concept vs. "authentication" as an entity/middleware). Without a disambiguation system, cross-references would be ambiguous.

## Decision

All wiki links use a mandatory type prefix: `[[concept-name]]`, `[[entity-name]]`, `[[decision-name]]`, `[[comparison-name]]`, `[[synthesis-name]]`, `[[source-name]]`. The prefix is the first hyphen-delimited segment and must be one of six allowed values.

## Rationale

Domain wikis inevitably develop naming collisions across types — a concept and an entity often have the same name. The prefix system resolves this without per-page disambiguation hacks. It also serves as a visual type hint, making links more readable at a glance.

Alternatives considered:
- **Directory-only disambiguation** (no prefix in link text): Requires knowing file paths; links become opaque.
- **Unique names without prefixes**: Fragile; requires active collision avoidance and breaks when the wiki grows.

## Consequences

- Positive: Zero ambiguity in cross-references across all page types.
- Positive: Links read as typed identifiers, which aids navigation.
- Positive: Lint can mechanically verify prefix validity.
- Negative: Link names are slightly more verbose.
- Risk: Authors forget the prefix and create bare links; these are caught by lint.

## Related

- [[concept-type-prefixed-links]] — the concept page documenting this convention
- [[concept-lint-workflow]] — lint detects bare or missing-prefix links
