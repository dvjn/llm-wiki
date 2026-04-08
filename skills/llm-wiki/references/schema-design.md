# Schema Design Guide

This reference helps you design a SCHEMA.md file for your specific wiki domain.

## What is a Schema?

The schema (typically `AGENTS.md` or `SCHEMA.md` in your wiki root) tells the LLM:
- How your wiki is structured
- What conventions to follow
- What workflows to use for ingest, query, and lint

It is the key configuration file that makes the LLM a disciplined wiki maintainer rather than a generic chatbot.

## Schema Components

### 1. Wiki Purpose

Define what the wiki is for:

```markdown
## Wiki Purpose

The wiki sits between you (the human) and raw external sources (specs, papers, RFCs). It is not product documentation — that is in `docs/`. The wiki is the LLM's accumulated understanding, cross-linked and evolving. Every source you ingest and every question you ask compounds into this knowledge base.

**Key principle**: The LLM writes the wiki. You curate sources and ask questions. The tedious bookkeeping is handled automatically.
```

### 2. Directory Structure

Document your specific directory layout:

```markdown
## Directory Structure

```
wiki/
├── raw/
│   ├── specs/          # Formal standards
│   ├── papers/         # Academic papers
│   ├── rfcs/           # IETF documents
│   ├── comparisons/    # Other specs for comparison
│   ├── articles/       # Blog posts
│   └── assets/         # Downloaded images
│
├── pages/
│   ├── concepts/       # Domain concepts
│   ├── entities/       # System entities
│   ├── decisions/      # ADR-style decisions
│   ├── comparisons/    # Trade-off analyses
│   ├── synthesis/      # Evolving thesis
│   └── sources/        # Per-source summaries
│
├── index.md
└── log.md
```
```

### 3. Page Templates

Customize the templates for your domain:

```markdown
## Page Templates

### Concept Page

```markdown
# [Concept Name]

> Brief one-line definition

## Core Definition

[2-3 sentences]

## Key Properties

- Property 1: explanation

## In [Your Project]

[Implementation notes, code references]

## Related Concepts

- [[related]] — relationship

## Sources

- [[source:filename]] § section
```
```

### 4. Workflows

Document your specific workflows:

```markdown
## Workflows

### Ingest Workflow

1. Read the source
2. Discuss key takeaways
3. Create source summary
4. Update concept pages
5. Create new pages as needed
6. Update index
7. Append to log

**Log format**:
```
## [YYYY-MM-DD] ingest | [Title]

Added `raw/...`. Created source summary. Updated X concept pages. Created Y new pages.
```
```

### 5. Cross-Reference Rules

Define when to create links and new pages:

```markdown
## Cross-Reference Rules

### When to Create Links

- Always link when mentioning a concept with its own page
- Create a stub if concept appears 3+ times without a page
- Link to sources with `[[source:filename]]`

### When to Create New Pages

- Concept appears in 3+ sources
- Design decision made
- Comparison requested
- Synthesis of multiple sources
```

### 6. Integration with Code

If your wiki relates to a codebase:

```markdown
## Integration with Codebase

### Code References in Wiki

Entity pages should reference the codebase:

```markdown
## In [Project]

Implemented in `src/module/file.rs`. The `Type` struct maps to...
```

### Design Decisions from Code

When encountering architectural choices, create decision pages explaining rationale.
```

## Example Schemas by Domain

### Research Wiki

For academic research, papers, and synthesis:

- `raw/papers/` — Academic papers
- `raw/articles/` — Blog posts, news
- `pages/concepts/` — Theoretical concepts
- `pages/synthesis/` — Literature reviews, thesis chapters
- Focus on citation tracking and contradiction detection

### Product/Technical Wiki

For software projects, APIs, technical documentation:

- `raw/specs/` — API specs, RFCs
- `pages/entities/` — System components
- `pages/decisions/` — Architecture Decision Records
- Focus on code references and design rationale

### Book Notes Wiki

For reading notes, character tracking, themes:

- `raw/chapters/` — Chapter summaries
- `pages/concepts/` — Themes, motifs
- `pages/entities/` — Characters, places
- Focus on cross-references and thematic synthesis

## Schema Evolution

Your schema should evolve as you discover what works:

1. **Start minimal** — Use the defaults from this skill
2. **Add conventions** — As patterns emerge, document them
3. **Refine templates** — Adjust page structures to fit your content
4. **Update workflows** — Add steps as your process matures

The schema is itself a wiki page — maintain it like one.
