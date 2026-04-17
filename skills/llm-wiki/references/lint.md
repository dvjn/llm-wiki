# Lint Workflow

Wiki health check.

## Workflow

1. Scan for contradictions
2. Identify stale claims (newer sources supersede)
3. Find orphan pages (no inbound links)
4. Identify frequently mentioned terms without pages
5. Find missing cross-references
6. Surface research gaps

Propose fixes; ask confirmation before applying non-trivial changes.

## Typical Fixes

- Repair broken/missing links
- Create stub pages for missing concepts
- Update stale summaries
- Flag contradictions for user review

## Log Format

Use script: `scripts/wiki-log lint "<summary of fixes>"`

```markdown
## [YYYY-MM-DD] lint | Wiki health check

Fixed N broken links. Created N stub pages. Noted N contradictions. Suggested N research gaps.
```
