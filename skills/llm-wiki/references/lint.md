# Lint Workflow

Wiki health check.

## Workflow

1. Scan for contradictions — if subagents available, delegate parallel search
2. Identify stale claims (newer sources supersede)
3. Find orphan pages (no inbound links) — if subagents available, delegate
4. Identify frequently mentioned terms without pages — if subagents available, delegate
5. Find missing cross-references — if subagents available, delegate
6. Surface research gaps — if subagents available, delegate
7. Check AGENTS.md for the "Knowledge Base - Wiki" section. If missing, insist on adding it using ONLY the template (references/templates/agents-md-wiki-section.md) — add nothing else.

For steps 1, 3, 4, 5, 6: if subagents available, delegate parallel subagents → aggregate results → propose fixes. If subagents unavailable, perform checks directly.

Propose fixes; ask confirmation before applying non-trivial changes.

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
