# Source: Independent Analysis — Subagent Token Overhead and When Not to Delegate

**Type**: article (independent / community analysis)
**URL**: https://youcanbuildthings.com/articles/claude-code-subagents-token-usage/ (plus corroborating community sources: anthropics/claude-code issues #27645, #68619; hidekazu-konishi.com; ofox.ai)
**Date Added**: 2026-07-24

## Summary

Third-party analysis of per-spawn subagent overhead and a "too small to delegate" threshold. It supplies the strongest quantified against-side case for the wiki's delegate-every-operation design: for small operations, direct handling is cited as 5-10x more token-efficient, and fixed cold-start overhead (context rebuild, no prompt-cache reuse) is exactly what a one-line lookup cannot amortize.

**Confidence note**: These are third-party/community figures, NOT official Anthropic numbers. Specific token/second values (~20k cold-start tokens, 3-5s spawn latency, 5-10x direct-edit efficiency) are order-of-magnitude estimates, LOW CONFIDENCE. The structural claims (cold start exists, fresh subagents don't reuse parent cache, direct edits cheaper for small fixes) are broadly corroborated and align with the official docs in [[source-claude-code-subagents-docs]].

## Key Extracts

> "A subagent is an isolated delegation: it does not inherit your session, so it re-reads what it needs and bills its own calls." "Three subagents on one task is roughly four times the token spend of a single-thread session." Community estimate: ~20k-token cold-start overhead, ~3-5s spawn latency (LOW CONFIDENCE). Rule of thumb: "For a 30-second task, just do it inline."

> Cache: fresh subagents do NOT reuse the parent's prompt cache on their first request; a **fork** (inherits the parent conversation) does reuse it — cheaper cold start, but no isolation benefit. Isolation and no-cache-reuse are two sides of the same coin.

> When not to delegate: "For straightforward find-and-replace style fixes across test files, direct edits are 5-10x more token-efficient." A poorly briefed subagent "will waste its first several turns rediscovering context you already had." Github issue #27645 frames over-spawning as a recognized failure mode.

> Parallel subagents buy THROUGHPUT, not lower total cost. Realistic savings come from routing subagents to cheaper models ("30 to 50% drop on the subagent line"), not from delegation itself.

## Related Concepts

- [[comparison-delegate-vs-inline]] — supplies the fork-vs-fresh cache distinction and the size-threshold mitigations
- [[concept-context-as-scarce-resource]] — quantifies the cold-start cost counterweight
- [[decision-subagent-delegation]] — quantifies the token cost the decision acknowledged only as latency

## Impact

Quantifies the token side of the delegate-trivial-lookups cost that [[decision-subagent-delegation]] noted only as latency. Supports three mitigations without abandoning the design: route the librarian to a cheaper model, allow inline handling below a size threshold, and prefer fork/resume over fresh spawns.
