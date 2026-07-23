# Ingest Workflow

New source in `wiki/raw/` or existing source needs folding into wiki.

If the source is a URL or topic rather than a local file: one researcher call first — it distills the material into raw note(s) under `wiki/raw/` with provenance. Without subagents, fetch and write the raw note yourself.

## With subagents (preferred)

Spawn each subagent with a short prompt — "First read `<skill path>/references/agents/<role>.md` and follow everything below its `---` separator, then: <task>" — never read the role files yourself. Do not read the source or wiki files in the main thread — work from reports:

1. **Analyze**: one librarian call, task = source analysis of the raw file. Returns key takeaways, affected pages, and a proposed page plan.
2. **Discuss** the takeaways and plan with the user (especially early sources in a new wiki); adjust the plan.
3. **Execute**: one scribe call, passing the approved plan, decisions from discussion, the source path (the scribe reads it for details), and the templates directory path. The scribe creates the source summary, creates/updates pages, updates `wiki/index.md`, and appends to `wiki/log.md`.
4. Review the scribe's report and relay the changes.

## Without subagents

1. Read source thoroughly
2. Discuss key takeaways with user (especially early sources in new wiki)
3. Create source summary: Load template `references/templates/source.md` and create `wiki/pages/sources/source-<name>.md`
4. Update affected pages; create new pages: Load template `references/templates/<type>.md` and create `wiki/pages/<type>s/<type>-<name>.md`
5. Update index: Manually add entry to `wiki/index.md` with type, name, and description
6. Log: Append entry to `wiki/log.md`

## Outputs (minimum)

- Source summary page in `wiki/pages/sources/`
- Updates to affected pages
- Index entries added to `wiki/index.md`
- Log entry appended to `wiki/log.md`

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
