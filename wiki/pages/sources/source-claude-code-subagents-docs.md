# Source: Claude Code / Agent SDK — Subagents Documentation

**Type**: article (official docs)
**URL**: https://code.claude.com/docs/en/sub-agents · https://code.claude.com/docs/en/agent-sdk/subagents
**Date Added**: 2026-07-24

## Summary

The official Claude Code and Agent SDK documentation on subagents: when to use one, context-isolation mechanics, what a subagent does and does not inherit at cold start, and single-call-vs-resume behavior. The stated trigger for delegation is "a side task would flood your main conversation with... content you won't reference again" — which supports the wiki's keep-content-out-of-main-context framing but frames subagents around large/repeated side tasks, arguing against delegating a one-line lookup that floods nothing.

## Key Extracts

> When to use: "a side task would flood your main conversation with search results, logs, or file contents you won't reference again: the subagent does that work in its own context and returns only the summary." Define a custom subagent "when you keep spawning the same kind of worker with the same instructions."

> Cold start: "A subagent's context window starts fresh, with no parent conversation, but isn't empty. The only content you pass from parent to subagent is the Agent tool's prompt string." Subagent does NOT receive parent conversation/tool results, parent system prompt, or preloaded skill content (unless in `skills`). A delegated lookup pays to rebuild context (read index + page) every call, reusing nothing the main thread already loaded.

> Cost control: route tasks "to faster, cheaper models like Haiku"; built-in Explore can be pinned to `model: haiku` and "keeps exploration results out of your main conversation context."

> Single-call vs resume: built-in Explore and Plan are "one-shot" (no `agentId`, cannot resume). Custom/general-purpose subagents "retain full conversation history" and can be resumed — avoiding re-paid cold-start context.

> Scaling: "Subagents work well for a few delegated tasks per turn"; for dozens-to-hundreds use the `Workflow` tool. Background execution is the default (v2.1.198+).

## Related Concepts

- [[comparison-delegate-vs-inline]] — the flood-vs-atomic trigger and cold-start mechanics are core to the trade-off
- [[concept-context-as-scarce-resource]] — cold-start rebuild cost
- [[decision-subagent-delegation]] — read-only/raw-only role restrictions match librarian/researcher (docs make these enforced; the wiki downgraded to instructions)

## Impact

Confirms the "returns only the summary" framing that mirrors the wiki's governing principle, while its flood-oriented trigger and concrete cold-start (no cache reuse, full rebuild) argue that atomic lookups fall below the delegation threshold. Suggests the adoptable mitigation of pinning the librarian to a cheaper model, and forking/resume over fresh spawns.
