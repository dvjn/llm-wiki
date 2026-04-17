# Ingest Workflow

New source in `wiki/raw/` or existing source needs folding into wiki.

## Workflow

1. Read source thoroughly
2. Discuss key takeaways with user (especially early sources in new wiki)
3. Create source summary: `scripts/wiki-new-page source <source-name>`
4. Update affected pages
5. Create new pages using `scripts/wiki-new-page <type> <name>`
6. Update index: `scripts/wiki-update-index add <type> <name> <description>`
7. Log: `scripts/wiki-log ingest "<title>" "<summary>"`

## Outputs (minimum)

- Source summary page: `scripts/wiki-new-page source <source-name>`
- Updates to affected pages
- Index entries added: `scripts/wiki-update-index add <type> <name> <desc>`
- Log entry appended: `scripts/wiki-log ingest "<title>" "<summary>"`

## Log Format

```markdown
## [YYYY-MM-DD] ingest | [Source Title]

Added `raw/...`. Created source summary. Updated N concept pages. Created N new pages. Updated index.
```

## Creation Heuristics

Create pages when:
- Concept/entity appears across multiple sources and merits synthesis
- Source introduces reusable comparison
- Source motivates design decision
- Source fills out a stub enough to convert

For important concepts not yet fully developed, create a stub page.
