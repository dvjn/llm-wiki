# Query Workflow

Answer from existing wiki.

## With subagents (preferred)

One call, any query type: spawn a subagent with a short prompt — "First read `<skill path>/references/agents/librarian.md` and follow everything below its `---` separator, then: <the question verbatim>." Do not read the role file or wiki files in the main thread. The librarian reads index and pages in its own thread and returns a cited answer, pages consulted, and gaps noticed — relay and refine the report.

Exception — inline lookups are fine when `wiki/index.md` is already in your context and the answer is a specific fact in 1-2 known pages: a run of small lookups is one agent's few tool calls, not one spawn per fact. Delegate whenever the reading would exceed a couple of pages or is exploratory.

Then:
- Surface the gaps the librarian noticed
- For synthesis queries, suggest 2-3 follow-up questions
- Offer to file durable synthesis back — do this INLINE (the answer is already in your context; see Filing Answers below). Reserve the scribe for multi-page writes.
- If the wiki cannot answer, offer external research: one subagent call told to read `references/agents/researcher.md` the same way, to capture sources into `wiki/raw/` for ingest

## Without subagents

- **Lookup** — specific fact. Read index + 1-2 pages. Answer directly.
- **Synthesis** — connect concepts. Read index + all relevant pages in parallel.
- **Gap/contradiction** — health check. Read index + affected pages.

1. Read `wiki/index.md` to locate pages
2. Read relevant pages; read source summaries if provenance/conflict resolution needed
3. Synthesize with section-level citations: `[[concept-x]] §Section`
4. Surface gaps noticed while reading
5. For synthesis queries, suggest 2-3 follow-up questions
6. Offer to file durable synthesis back into wiki

## Principles

- Start narrow from index
- Prefer synthesized answers over raw excerpts
- Cite at section level (`§`)
- File good answers — they compound
- Note missing concepts while reading (potential lint pass)

## Filing Answers

Filing one synthesized answer happens inline (with or without subagents — the content is already in context):

- Create page: Load template `references/templates/<type>.md` and create page directly
- Update index: Add entry to index.md manually
- Log: Append to log.md
