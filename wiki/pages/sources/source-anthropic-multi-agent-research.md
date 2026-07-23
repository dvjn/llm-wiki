# Source: Anthropic — How We Built Our Multi-Agent Research System

**Type**: article (engineering blog)
**URL**: https://www.anthropic.com/engineering/multi-agent-research-system
**Date Added**: 2026-07-24

## Summary

Anthropic's account of its orchestrator-worker (lead-subagent) research system. It supplies the token-economics math for delegation and names over-delegation of trivial work as an early failure mode. The load-bearing numbers: multi-agent systems use about 15x more tokens as chats (agents about 4x), and effort must be scaled to query complexity — simple fact-finding needs just 1 agent with 3-10 tool calls, not a subagent spawned per fact. This is the primary against-side evidence for the wiki's "delegate even lookups" rule.

## Key Extracts

> "agents typically use about 4× more tokens than chat interactions, and multi-agent systems use about 15× more tokens as chats." "token usage by itself explains 80% of the variance" in performance. Multi-agent is viable only "where the value of the task is high enough to pay for the increased performance."

> Gains when it fits: "multi-agent system with Claude Opus 4 as the lead agent and Claude Sonnet 4 subagents outperformed single-agent Claude Opus 4 by 90.2%" on breadth-first research.

> Anti-pattern (named failure mode): early on they saw "spawning 50 subagents for simple queries." Fix — scale effort to complexity: "Simple fact-finding requires just 1 agent with 3-10 tool calls, direct comparisons might need 2-4 subagents with 10-15 calls each." The unit of delegation is a whole research task, not an individual lookup.

> Poor fit when tasks require "all agents to share the same context or involve many dependencies between agents." Coding tasks don't parallelize well. Heavy output goes to external storage; only lightweight references return to the orchestrator.

## Related Concepts

- [[concept-context-as-scarce-resource]] — the 15x multiplier is the cost counterweight to context savings
- [[comparison-delegate-vs-inline]] — supplies the anti-pattern and the scale-effort-to-complexity threshold
- [[decision-subagent-delegation]] — challenges the blanket "even lookups delegate" rule

## Impact

Provides the ~15x token multiplier and the "spawning subagents for simple queries" failure mode that count against delegating trivial lookups. The wiki design's motivation differs (context economy, not parallel performance), but the multiplier still applies at the system level, quantifying the cost side of that trade.
