# Lint Workflow

Wiki health check.

## Checks

1. Contradictions across related pages
2. Stale claims (newer sources supersede)
3. Orphan pages (no inbound links)
4. Frequently mentioned terms without pages
5. Missing cross-references
6. Research gaps
7. AGENTS.md has the "Knowledge Base - Wiki" section. If missing, insist on adding it using ONLY the template (references/templates/agents-md-wiki-section.md) — add nothing else.

## With subagents (preferred)

Spawn each subagent with a short prompt — "First read `<skill path>/references/agents/<role>.md` and follow everything below its `---` separator, then: <task>" — never read the role files yourself.

- **Audit** (checks 1-6): one librarian call, task = "audit". It runs the full checklist in its own thread and returns findings grouped by check, with evidence and proposed fixes. Holds to roughly a 150-page wiki; beyond that, run one audit call per page type. Fan out parallel librarians (one per check) only when speed matters more than main-context economy.
- Check 7 is a single file check — do it in the main thread.
- Propose fixes from the report; ask confirmation before applying non-trivial changes.
- **Fixes**: one scribe call (never more than one scribe at a time) carrying the approved fixes and the templates directory path; it applies them and appends the lint report to `wiki/log.md`. A trivial single-file fix is cheaper applied inline. If no fixes are applied (clean audit, or approval pending), append the lint report to `wiki/log.md` inline — don't spawn a scribe for one line.
- **Research gaps** the user wants filled: one researcher call per gap to capture sources into `wiki/raw/`, then ingest in a follow-up pass.

## Without subagents

Perform checks 1-7 directly, propose fixes, apply approved ones, and append the lint report to `wiki/log.md`. Suggest research gaps for the user to curate.

## Typical Fixes

- Repair broken/missing links
- Create stub pages for missing concepts
- Update stale summaries
- Flag contradictions for user review

## Log Format

Append to log.md: `## [YYYY-MM-DD] lint | Wiki health check`

```markdown
## [YYYY-MM-DD] lint | Wiki health check

Fixed N broken links. Created N stub pages. Noted N contradictions. Suggested N research gaps. AGENTS.md wiki section: present/missing.
```
