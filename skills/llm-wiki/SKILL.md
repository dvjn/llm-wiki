---
name: llm-wiki
description: Build and maintain a knowledge wiki using LLMs. This skill manages a three-layer architecture (raw sources, LLM-generated wiki pages, and schema documentation) with workflows for ingesting sources, querying knowledge, and linting for consistency. Use this skill whenever the user wants to create a knowledge base, manage research notes, build a wiki, ingest documents for synthesis, or maintain structured documentation that compounds over time. Essential for research projects, book notes, competitive analysis, course notes, or any domain where knowledge accumulates and cross-references matter.
---

# LLM Wiki Skill

Build and maintain a knowledge wiki using LLMs. The wiki is a persistent, compounding artifact that sits between you and your raw sources.

## Core Philosophy

Traditional RAG retrieves from raw documents on every query. A wiki is different: the LLM **incrementally builds and maintains a structured knowledge base** that accumulates understanding. Cross-references are already there. Contradictions are already flagged. The synthesis reflects everything you've read.

**Key principle**: The LLM writes the wiki. You curate sources and ask questions. The tedious bookkeeping (cross-references, updates, consistency) is handled automatically.

## Three-Layer Architecture

```
wiki/
├── raw/                    # Immutable source documents (never modified)
│   ├── specs/             # Formal standards, W3C specs, IETF drafts
│   ├── papers/            # Academic papers
│   ├── articles/          # Blog posts, tutorials, informal sources
│   └── assets/            # Images downloaded from web sources
│
├── pages/                  # LLM-generated wiki pages
│   ├── concepts/          # Domain concepts appearing across sources
│   ├── entities/          # Concrete things in your domain
│   ├── decisions/         # Design decisions (ADR-style)
│   ├── comparisons/       # Trade-off analyses
│   ├── synthesis/         # Evolving thesis, open questions
│   └── sources/           # Per-source summaries
│
├── index.md               # Content catalog (auto-updated)
└── log.md                 # Chronological activity log (append-only)
```

### Raw Sources (`wiki/raw/`)

Your curated collection of source documents. These are immutable — the LLM reads from them but never modifies them. This is your source of truth.

**Naming convention**: `{type}-{short-title}.md`
- Example: `spec-rest-api.md`, `paper-consensus-raft.md`, `article-react-hooks.md`

### Wiki Pages (`wiki/pages/`)

LLM-generated markdown files. The LLM owns this layer entirely — it creates pages, updates them, maintains cross-references, and keeps everything consistent.

## Workflows

### Ingest Workflow

When you add a new source to `wiki/raw/` and ask the LLM to process it:

1. **Read** the source document thoroughly
2. **Discuss** key takeaways with the user (optional but recommended for first few sources)
3. **Create** a source summary page in `wiki/pages/sources/`
4. **Update** relevant concept and entity pages across the wiki
5. **Create** new concept/entity pages if the source introduces new concepts
6. **Update** `wiki/index.md` to catalog the new pages
7. **Append** an entry to `wiki/log.md`

A single source might touch 10-15 wiki pages. This is the compounding effect.

**Log entry format**:
```markdown
## [YYYY-MM-DD] ingest | [Source Title]

Added `raw/papers/paper-consensus-raft.md`. Created source summary. Updated 8 concept pages (consensus, leader-election, log-replication). Created 2 new entity pages (node, log-entry). Updated index.
```

### Query Workflow

When the user asks a question requiring wiki knowledge:

1. **Read** `wiki/index.md` first to find relevant pages
2. **Read** the relevant pages (concepts, entities, comparisons)
3. **Read** relevant source summaries if deeper context needed
4. **Synthesize** an answer with wiki-style citations: `[[concept-x]] §2`
5. **Offer** to file the synthesized answer as a new wiki page if it's valuable

**Important**: Good answers compound. A comparison, analysis, or connection discovered — these are valuable and shouldn't disappear into chat history. Always offer to create a wiki page from substantial answers.

### Lint Workflow

Periodically (or when asked), the LLM health-checks the wiki:

1. **Contradictions** — scan for conflicting claims across pages
2. **Stale claims** — identify claims that newer sources supersede
3. **Orphan pages** — find pages with no inbound links from index or other pages
4. **Missing concepts** — identify frequently mentioned terms lacking their own page
5. **Missing cross-references** — find pages that should link but don't
6. **Data gaps** — identify questions that could be answered with a web search

The LLM proposes fixes and asks for confirmation before applying.

**Log entry format**:
```markdown
## [YYYY-MM-DD] lint | Wiki lint pass

Fixed 3 broken links. Created 2 stub concept pages for frequently mentioned terms. Noted 1 potential contradiction between [[concept-consistency]] and [[concept-availability]] pages regarding CAP theorem tradeoffs. Suggested web search for "distributed consensus patterns".
```

## Page Templates

Each page type has a consistent structure. Follow these templates when creating or updating pages.

### Concept Page (`pages/concepts/*.md`)

```markdown
# [Concept Name]

> Brief one-line definition

## Core Definition

[2-3 sentences defining the concept precisely]

## Key Properties

- Property 1: explanation
- Property 2: explanation

## In [Your Domain]

[How this concept manifests in your specific domain — implementation notes, examples]

## Related Concepts

- [[concept-related-1]] — relationship description
- [[concept-related-2]] — relationship description

## Sources

- [[source-spec-rest-api]] § section
- [[source-paper-consensus-raft]] § section
```

### Entity Page (`pages/entities/*.md`)

```markdown
# [Entity Name]

> One-line role description

## Purpose

[What this entity does in the system]

## Structure

[Fields, properties, data shape]

## Lifecycle

[How this entity is created, updated, destroyed]

## Related Entities

- [[entity-related-1]] — relationship
- [[entity-related-2]] — relationship
```

### Decision Page (`pages/decisions/*.md`)

```markdown
# Decision: [Short Title]

**Status**: Accepted | Proposed | Superseded
**Date**: YYYY-MM-DD
**Context**: [What triggered this decision]

## Decision

[The choice made]

## Rationale

[Why this choice — alternatives considered, trade-offs]

## Consequences

- Positive: ...
- Negative: ...
- Risks: ...

## Related

- [[decision-related-decision]] — related decision
- [[concept-related-concept]] — relevant concept
```

### Comparison Page (`pages/comparisons/*.md`)

```markdown
# [A] vs [B]

> Brief summary of the comparison

## Dimensions

| Dimension | [A] | [B] | Position |
|-----------|-----|-----|----------|
| X | ... | ... | ... |
| Y | ... | ... | ... |

## Analysis

[Synthesis of trade-offs, when each is appropriate]

## Sources

- [[source-spec-method-a]]
- [[source-spec-method-b]]
```

### Source Summary Page (`pages/sources/*.md`)

```markdown
# Source: [Title]

**Type**: spec | paper | article
**URL**: [original URL if web source]
**Date Added**: YYYY-MM-DD
**Key Sections**: §1, §2, §3

## Summary

[3-5 sentence summary of the source's main contribution]

## Key Extracts

> [Direct quotes or paraphrased key points]

## Related Concepts

- [[concept-related-1]] — how this source informs it
- [[concept-related-2]] — how this source informs it

## Impact

[How this source influenced understanding or decisions]
```

## Cross-Reference Rules

### When to Create Links

Use `[[page-name]]` syntax (Obsidian-compatible) for internal wiki links:

- **Always** link when mentioning a concept/entity that has its own page
- **Create** a new page (stub) if a concept appears 3+ times across sources but has no page
- **Link to sources** using `[[source-filename]]` syntax

### When to Create New Pages

Create a new page when:
- A concept/entity appears in 3+ sources and warrants its own synthesis
- A design decision is made that should be recorded
- A comparison is requested and is valuable enough to persist
- An answer synthesizes multiple sources and is reusable

### Stub Pages

For concepts that need a page but lack full content, create a stub:

```markdown
# [Concept Name]

> [Brief definition from first mention]

## Status

Stub — needs expansion from sources: [[source-spec-auth-flow]], [[source-paper-consensus-raft]]

## Initial Notes

[Brief notes from current understanding]
```

### Link Prefix System

All wiki links use a mandatory type prefix to prevent name collisions:

- **Format**: `{type}-{name}` where type is one of: `concept`, `entity`, `decision`, `comparison`, `synthesis`, `source`
- **Mandatory**: Every `[[...]]` link must have a type prefix — no exceptions
- **Casing**: All lowercase with hyphens (e.g., `[[concept-authentication]]`, not `[[concept-Authentication]]`)
- **Prefix position**: Always the first segment before the first hyphen
- **Source links**: Use `[[source-filename]]` (e.g., `[[source-spec-rest-api]]` — the `spec-` is part of the filename, `source-` is the type prefix)
- **Comparison links**: The `vs` is part of the name (e.g., `[[comparison-postgres-vs-mysql]]`)

**Why**: When multiple sources discuss overlapping topics, or when concepts and entities share names (e.g., "User" as a concept vs. entity), the prefix prevents the LLM from mixing up links. Each type has its own namespace.

## Index Structure

`index.md` is the content catalog. Update it on every ingest:

```markdown
# Wiki — Content Catalog

> Auto-updated by the LLM on every ingest, query filing, and lint pass.

## Concepts

Domain concepts that appear across multiple sources.

- [[concept-example-name]] — one-line description
- [[concept-another-example]] — one-line description

## Entities

Concrete things in the system.

- [[entity-example-name]] — one-line description

## Decisions

Design decisions with rationale.

- [[decision-example-name]] — one-line description

## Comparisons

Trade-off analyses.

- [[comparison-example-name]] — one-line description

## Sources

Per-source summaries.

- [[source-filename]] — one-line description

## Statistics

| Metric | Count |
|--------|-------|
| Raw sources | N |
| Wiki pages | N |
| Concepts | N |
| Entities | N |
```

## Tips and Tools

### Obsidian Integration

- **Obsidian Web Clipper**: Browser extension to convert web articles to markdown
- **Graph view**: See the shape of your wiki — connections, hubs, orphans
- **Dataview plugin**: Query page frontmatter for dynamic tables

### Optional CLI Tools

As the wiki grows, consider adding search:
- [qmd](https://github.com/tobi/qmd): Local search engine for markdown with hybrid BM25/vector search
- Or have the LLM vibe-code a simple search script

### Git Version Control

The wiki is just a git repo of markdown files. You get version history, branching, and collaboration for free.

## Why This Works

The tedious part of maintaining a knowledge base is not the reading or the thinking — it is the bookkeeping. Updating cross-references, keeping summaries current, noting when new data contradicts old claims, maintaining consistency across dozens of pages. Humans abandon wikis because the maintenance burden grows faster than the value. LLMs don't get bored, don't forget to update a cross-reference, and can touch 15 files in one pass. The wiki stays maintained because the cost of maintenance is near zero.

The human's job is to curate sources, direct the analysis, ask good questions, and think about what it all means. The LLM's job is everything else.
