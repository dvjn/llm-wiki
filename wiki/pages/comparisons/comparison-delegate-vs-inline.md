# Delegate to Subagent vs. Handle Inline

> When to push a unit of work into a subagent thread versus doing it in the main conversation — the trade-off between main-thread context economy and per-spawn overhead.

## Dimensions

| Dimension | Delegate (subagent) | Inline (main thread) |
|-----------|---------------------|----------------------|
| Main-thread context | Isolated — only a compact 1,000-2,000 token report returns | Full search results, logs, and file contents accumulate |
| Report/output | Distilled summary; heavy output can go to external storage with references back | No handoff; everything stays visible |
| Parallelism | Independent subtasks explore in parallel (throughput gain) | Sequential |
| Spawn overhead | Fixed cold-start cost per spawn (~20k tokens, LOW CONFIDENCE); no parent prompt-cache reuse on a fresh subagent | None |
| Prompt-cache reuse | Fresh subagent reuses nothing; a fork reuses parent cache but loses isolation | Full reuse of already-loaded context |
| Total token spend | ~15x more tokens than chat at the system level | Baseline |
| Latency | Higher (spawn + cold read) | Lower |

## Analysis

**The threshold rule.** Scale effort to task complexity. Delegate when the work is large and its detail will not be reused inline — the ~1-2k-token compression ratio and context isolation then dominate the fixed spawn cost. Handle inline when the task is atomic: simple fact-finding is "just 1 agent with 3-10 tool calls," and for small find-and-replace-style fixes direct handling is 5-10x more token-efficient. Anthropic's own named failure mode was "spawning 50 subagents for simple queries"; the rule of thumb is "for a 30-second task, just do it inline."

**Delegation still viable for the wiki case.** The wiki's motivation differs from raw performance: it delegates to keep file *content* out of the main context, not to parallelize. That is defensible only if main-thread context is the binding constraint (which [[decision-subagent-delegation]] asserts). Three mitigations keep delegation viable without over-paying:

- **Cheaper model for light calls** — route the librarian to Haiku for lookups; realistic savings of 30-50% on the subagent line come from model routing, not delegation itself.
- **Size threshold** — allow inline handling for operations small enough that inline cost is below spawn overhead, rather than a blanket "delegate everything" rule.
- **Fork/resume over fresh spawns** — a fork reuses the parent's prompt cache; resuming a custom subagent avoids re-paying cold-start context.

## Sources

- [[source-anthropic-context-engineering]], [[source-anthropic-multi-agent-research]], [[source-claude-code-subagents-docs]], [[source-subagent-token-overhead-analysis]]

## Related

- [[concept-context-as-scarce-resource]] — the principle this trade-off operationalizes
- [[decision-subagent-delegation]] — the wiki's applied decision, where this tension is under review
