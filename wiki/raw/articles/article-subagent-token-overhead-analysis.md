# Raw: Independent analysis — Subagent token overhead and when NOT to delegate

**Source URLs**:
- https://youcanbuildthings.com/articles/claude-code-subagents-token-usage/ ("Why Claude Code Subagents Burn So Many Tokens")
- Corroborating community sources surfaced via web search (2026-07-24): github.com/anthropics/claude-code issues #27645 ("Subagent spawning wastes tokens on tasks that should be done directly"), #68619; hidekazu-konishi.com orchestration guide; ofox.ai nested-subagents token math.
**Retrieved**: 2026-07-24
**Prompted by**: Need concrete per-spawn overhead numbers and a "too small to delegate" threshold to evaluate the delegate-every-wiki-operation design.
**Confidence note**: These are third-party/community figures, NOT official Anthropic numbers. Specific token/second values (e.g. "~20k tokens", "3–5 seconds") come from independent blog analyses and web-search summaries and should be treated as order-of-magnitude estimates, not authoritative constants. The structural claims (cold start exists, fresh subagents don't reuse parent cache, direct edits cheaper for small fixes) are broadly corroborated and align with official docs in [[article-claude-code-subagents-docs]].

## Structural overhead of a spawn

- "A subagent is an isolated delegation: it does not inherit your session, so it re-reads what it needs and bills its own calls." (matches official "starts fresh" docs)
- "Three subagents on one task is roughly four times the token spend of a single-thread session." (per-spawn fixed overhead + duplicated context reads)
- Community estimate: each subagent starts with a cold-start overhead on the order of ~20k tokens (context rebuild); spawn setup latency ~3–5 seconds plus a few hundred tokens. LOW CONFIDENCE on exact figures.
- Rule of thumb from the sources: "For a 30-second task, just do it inline." Delegation is inefficient below some minimum task size because fixed spawn overhead dominates.

## Cache reuse: forks vs fresh subagents

- Fresh subagents pay to build context from scratch and do NOT reuse the parent's prompt cache on their first request.
- A **fork** (inherits the parent conversation) reuses the parent's prompt cache on its first request — cheaper cold start than a fresh subagent, but no context isolation benefit.
- Implication: the isolation benefit (clean context) and the cost (no cache reuse, full rebuild) are two sides of the same coin.

## When NOT to delegate (against over-delegation)

- "For straightforward find-and-replace style fixes across test files, direct edits are 5-10x more token-efficient" — apply the fix directly rather than delegating.
- A poorly briefed subagent "will waste its first several turns rediscovering context you already had."
- The github issue framing ("Subagent spawning wastes tokens on tasks that should be done directly") shows this is a recognized real-world failure mode, not hypothetical.

## Throughput vs cost distinction

- Parallel subagents buy THROUGHPUT (wall-clock speed), not necessarily lower total cost: e.g. "5x cheaper per call [Haiku] times 3x parallel throughput equals roughly 2x billable work per dollar" — explicitly "a throughput gain, not cost reduction on the same workload."
- Realistic savings from routing subagents to cheaper models: "a 30 to 50% drop on the subagent line" — savings come from model routing, not from delegation itself.

## Relevance to the wiki subagent-delegation design

- STRONGEST evidence AGAINST the "delegate even a one-fact lookup" rule: for a small operation, direct handling is cited as 5–10× more token-efficient, and fixed cold-start overhead (context rebuild, no cache reuse) is exactly what a one-line lookup cannot amortize. The wiki decision's own "Negative: higher latency per operation... even for trivial lookups" acknowledges the latency but not the token-multiplier cost; this source quantifies the token side.
- The wiki decision's counter-argument stands only if main-thread context is genuinely the binding constraint AND the alternative (index + page content inline) would be large or repeatedly re-read. For a truly atomic fact used once, that condition is weak.
- Actionable mitigations these sources support (without abandoning the design): (1) route the librarian to a cheaper model for lookups; (2) allow direct inline handling for lookups small enough that inline cost < spawn overhead (a size threshold rather than a blanket rule); (3) prefer resuming/forking over fresh spawns where continuity matters.
