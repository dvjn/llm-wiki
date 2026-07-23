# Stub Pages

> Placeholder wiki pages for concepts that deserve their own page but can't yet be fully written — a way to make knowledge gaps visible instead of losing them.

## Core Definition

A stub is a skeleton page created when a term recurs across sources (or would produce a broken cross-reference) but there isn't enough material yet to synthesize a full page. A stub lives in the directory of the type it will graduate into (a stub concept is `pages/concepts/concept-<name>.md`) — so its `[[type-name]]` link resolves immediately — and carries an explicit status checklist so it can be rewritten in place as a full page later.

## Key Properties

- **Visible debt**: A stub turns an implicit gap into a tracked, linkable artifact. The `Status` checklist names what the stub still needs (core concept, properties, sources, cross-references).
- **Link target**: Stubs exist partly to keep cross-references unbroken — the [[concept-type-prefixed-links]] system assumes that if you link to something, the page exists.
- **Graduation path**: When a stub has enough material, it's rewritten in place as a full concept/entity/decision page — same file, same link.
- **Created by workflows, not humans**: [[concept-ingest-workflow]] creates stubs when a source references a concept it can't fully capture; [[concept-lint-workflow]] creates stubs for frequently mentioned but undocumented terms.

## When to Create a Stub

- During ingest, when a source repeatedly references a concept that can't be fully defined from that source alone.
- During lint, when a term shows up across multiple pages without a dedicated page of its own.
- During query filing, when answering a question surfaces a subtopic worth tracking but not fully exploring right now.

Do **not** create a stub for every unlinked term — only those likely to recur or to anchor future work.

## Template

The `stub` type in `references/templates/stub.md` defines the skeleton:
- One-line definition of what the page needs to become
- `Status` checklist (define core concept, add key properties, find sources, integrate with related concepts)
- `Notes` section for partial understanding or open questions

## Related Concepts

- [[concept-ingest-workflow]] — the workflow that most commonly creates stubs
- [[concept-lint-workflow]] — proposes stubs for gaps surfaced during lint
- [[concept-compounding-wiki]] — stubs are one mechanism by which the wiki converts unknowns into tracked future work

## Related Entities

- [[entity-skill-md]] — names stub creation as a global rule
