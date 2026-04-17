# Query Workflow

Answer from existing wiki.

## Query Types

- **Lookup** — specific fact. Read index + 1-2 pages. Answer directly.
- **Synthesis** — connect concepts. Read index + relevant pages. Answer with citations.
- **Gap/contradiction** — health check. Read index + affected pages. Surface issues.

## Workflow

1. Read `wiki/index.md` to locate pages
2. Read relevant pages in parallel
3. Read source summaries if provenance/conflict resolution needed
4. Synthesize with section-level citations: `[[concept-x]] §Section`
5. Surface gaps noticed while reading
6. For synthesis queries, suggest 2-3 follow-up questions
7. Offer to file durable synthesis back into wiki

## Principles

- Start narrow from index
- Prefer synthesized answers over raw excerpts
- Cite at section level (`§`)
- File good answers — they compound
- Note missing concepts while reading (potential lint pass)

## Filing Answers

When filing durable synthesis back into wiki:

- Create page: `scripts/wiki-new-page <type> <name>`
- Update index: `scripts/wiki-update-index add <type> <name> <description>`
- Log: `scripts/wiki-log synthesis <type> <name> "<summary>"`
