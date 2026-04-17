# Wiki Schema — llm-wiki-skill

This wiki documents the `llm-wiki` skill itself: its architecture, design decisions, workflows, and conventions. It serves as a meta-example of a wiki built and maintained using the skill.

## Purpose

Track accumulated understanding of the skill's design so that:
- Design rationale is preserved alongside the code
- Decisions are documented as they are made, not reconstructed later
- The wiki itself demonstrates the skill's value proposition

## Directory Structure

```text
wiki/
├── raw/                    # Source documents (skill files, spec notes)
│   ├── specs/             # Formal design specs or requirements
│   ├── papers/            # Research references (Karpathy LLM wiki concept, etc.)
│   ├── articles/          # Blog posts and informal references
│   └── assets/            # Supporting files
│
├── pages/                  # LLM-generated wiki pages
│   ├── concepts/          # Core ideas: architecture, workflow types, conventions
│   ├── entities/          # Concrete components: schema document, index.md, log.md
│   ├── decisions/         # Design decisions (ADR-style)
│   ├── comparisons/       # Trade-off analyses (e.g. wiki vs RAG)
│   ├── synthesis/         # Evolving thesis, open questions
│   └── sources/           # Per-source summaries
│
├── index.md               # Content catalog
├── log.md                 # Append-only operation log
└── SCHEMA.md              # This file
```

## Page Types in Use

| Type | Path | When to create |
|------|------|----------------|
| concept | `pages/concepts/` | Core ideas that appear across multiple files or design discussions |
| entity | `pages/entities/` | Concrete components: files, objects, structures the skill defines |
| decision | `pages/decisions/` | Recorded design choices with rationale |
| comparison | `pages/comparisons/` | Trade-off analyses between approaches |
| synthesis | `pages/synthesis/` | Evolving thesis, open questions, design tensions |
| source | `pages/sources/` | Summaries of raw source documents added to `wiki/raw/` |

## Domain-Specific Conventions

- Entity pages for files should link to their path in the repo (e.g. `skills/llm-wiki/SKILL.md`).
- Decision pages should reference the relevant concept pages they affect.
- The wiki itself is a live demonstration of the skill — keep quality high.

## Ingest Scope

Ingestible sources for this project:
- The skill's own markdown files (`SKILL.md` and `references/*.md`)
- Design discussions, eval files, or planning documents in `.sisyphus/`
- External references (Karpathy's wiki concept, related tools)
