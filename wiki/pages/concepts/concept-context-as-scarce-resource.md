# Concept: Context as a Scarce Resource

> The main agent's context window is the binding constraint; delegation trades total token spend for main-thread context economy, and only pays off when that context is genuinely the bottleneck.

## Core Definition

Context-as-a-scarce-resource treats the main conversation's context window — not total token spend — as the resource to budget. It accumulates for the whole session and degrades as it fills, so detailed work is pushed into subagent threads that are paid once and discarded. The counterweight is that delegation is not free at the system level: it multiplies total tokens and pays a fixed cold-start cost per spawn.

## Key Properties

- **Main context is the binding constraint.** The main thread should hold high-level synthesis and decisions, not detailed exploration state; subagents run in clean context windows. — [[source-anthropic-context-engineering]]
- **Compression ratio justifies delegation.** A subagent explores using tens of thousands of tokens but returns only a **1,000-2,000 token** distilled summary. The justification is strongest when the delegated work is large. — [[source-anthropic-context-engineering]]
- **The counterweight: a ~15x multiplier.** Multi-agent systems use about **15x more tokens** than chat (agents about 4x). Delegation reduces main-thread context at the cost of higher total token spend. — [[source-anthropic-multi-agent-research]]
- **Cold-start cost.** A fresh subagent rebuilds context from scratch (community estimate ~20k tokens, LOW CONFIDENCE) and does not reuse the parent's prompt cache; a fork does reuse the cache but gives no isolation. — [[source-subagent-token-overhead-analysis]], [[source-claude-code-subagents-docs]]

## The Delegate-vs-Inline Threshold

Delegate when the work is large and its detail will not be reused inline — the compression ratio and context savings then dominate. Handle atomic tasks inline: a one-fact lookup's exploration is small, so the fixed spawn overhead dominates and inline is 5-10x more token-efficient. The rule of thumb: "for a 30-second task, just do it inline." — [[source-subagent-token-overhead-analysis]], [[source-anthropic-multi-agent-research]]

## In This Skill

This is the governing principle behind [[decision-subagent-delegation]]: wiki operations delegate so file content never enters the main context. The decision asserts main-thread context is the binding constraint and accepts the token multiplier as the price. The threshold above is the live tension in that design — see [[comparison-delegate-vs-inline]] and the under-review risk in the decision.

## Related Concepts

- [[comparison-delegate-vs-inline]] — the trade-off applied as a decision rule
- [[concept-skill-review-criteria]] — conciseness criteria follow from context scarcity
- [[concept-progressive-disclosure]] — the same scarcity motivates staged loading

## Sources

- [[source-anthropic-context-engineering]] — context as finite resource; 1-2k-token summaries
- [[source-anthropic-multi-agent-research]] — 15x multiplier; scale effort to complexity
- [[source-subagent-token-overhead-analysis]] — cold-start cost; inline cheaper below a threshold
- [[source-claude-code-subagents-docs]] — cold-start inheritance mechanics
