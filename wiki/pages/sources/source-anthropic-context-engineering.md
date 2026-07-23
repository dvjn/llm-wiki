# Source: Anthropic — Effective Context Engineering for AI Agents

**Type**: article (engineering blog)
**URL**: https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
**Date Added**: 2026-07-24

## Summary

Frames the model's context window as a finite, degrading resource that must be budgeted, and positions sub-agent architecture as one of several context-management techniques (alongside compaction and note-taking/external memory). Its load-bearing quantified claim is the compression ratio that justifies delegation: a subagent explores using tens of thousands of tokens but returns only a 1,000-2,000 token distilled summary. This supports the wiki's governing principle that main context is the scarce resource — with the caveat that the compression math is strongest when the delegated work is large.

## Key Extracts

> The main thread should focus on "synthesizing and analyzing the results" rather than holding all detailed exploration state; sub-agents operate with **"clean context windows."**

> Context isolation (quantified): each subagent "explores extensively, using tens of thousands of tokens or more, but returns only a condensed, distilled summary of its work (often 1,000-2,000 tokens)."

> Sub-agent architecture is most effective "for complex research and analysis where parallel exploration pays dividends" — one technique among compaction (for extensive back-and-forth) and note-taking/external memory (for iterative development with milestones). It addresses "only specific scenarios requiring parallel, independent exploration," not a universal default.

## Related Concepts

- [[concept-context-as-scarce-resource]] — this is the primary source for that concept
- [[comparison-delegate-vs-inline]] — the 1-2k-token summary is the compact-report benefit on the delegation side
- [[decision-subagent-delegation]] — supports the "keep file content out of main context" principle

## Impact

Supplies the 1,000-2,000 token distilled-summary compression benchmark that the wiki's librarian/scribe reports aim for. Its caveat — the math holds when exploration is large, not for a trivial one-fact lookup — feeds the delegate-vs-inline threshold and the risk noted in [[decision-subagent-delegation]].
