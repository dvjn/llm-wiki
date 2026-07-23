# Raw: Anthropic — How We Built Our Multi-Agent Research System

**Source URL**: https://www.anthropic.com/engineering/multi-agent-research-system
**Retrieved**: 2026-07-24
**Prompted by**: Research on subagent context-engineering trade-offs; evaluating a design that delegates every wiki operation (even one-fact lookups) as one subagent call.

## Architecture: orchestrator-worker (lead-subagent)

- "a lead agent coordinates the process while delegating to specialized subagents that operate in parallel."

## Token economics (the trade-off math)

- **Token multipliers vs. chat (key quote)**: "agents typically use about 4× more tokens than chat interactions, and multi-agent systems use about 15× more tokens as chats."
- "token usage by itself explains 80% of the variance" in evaluation performance results.
- Multi-agent is economically viable only "where the value of the task is high enough to pay for the increased performance."

## Performance gains (when it does pay off)

- "multi-agent system with Claude Opus 4 as the lead agent and Claude Sonnet 4 subagents outperformed single-agent Claude Opus 4 by 90.2%" — on breadth-first research tasks.

## Context isolation benefits

- "Subagents facilitate compression by operating in parallel with their own context windows, exploring different aspects simultaneously before condensing tokens for the lead agent."

## When NOT to use multi-agent

- Poor fit for tasks requiring "all agents to share the same context or involve many dependencies between agents."
- Coding tasks don't parallelize well: "LLM agents are not yet great at coordinating and delegating to other agents in real time."

## Anti-pattern: over-delegation of trivial work (DIRECTLY relevant)

- Early failure mode: "spawning 50 subagents for simple queries."
- Fix — **scale effort to query complexity**: "Simple fact-finding requires just 1 agent with 3-10 tool calls, direct comparisons might need 2-4 subagents with 10-15 calls each."
- Key reading: even in Anthropic's own multi-agent system, simple fact-finding uses ONE agent doing 3–10 tool calls — NOT a fresh subagent spawned per fact. The unit of delegation is a whole research task, not an individual lookup.

## Report compression / output handling

- Rather than forcing all communication through the lead agent's context: "Subagents call tools to store their work in external systems, then pass lightweight references back to the coordinator...prevents information loss during multi-stage processing."
  - Pattern: heavy output goes to external storage (files); only lightweight references return to the orchestrator.

## Relevance to the wiki subagent-delegation design

- SUPPORTS: orchestrator-worker mapping (main agent = lead, librarian/researcher/scribe = workers); compact-report principle; parallel fan-out for independent subtasks (matches lint's opt-in parallel fan-out).
- AGAINST the "delegate even one-fact lookups" rule: the 15× token multiplier and the explicit "spawning N subagents for simple queries" anti-pattern argue that a single fact lookup is below the delegation threshold. Anthropic's own guidance keeps simple fact-finding as one agent's few tool calls, and treats over-spawning on simple queries as a bug to fix — not a design to adopt.
- Nuance for the wiki case: the wiki design's motivation is DIFFERENT from raw performance — it delegates to keep file *content* out of the main context, not to parallelize. The token multiplier still applies (you pay ~15× at the system level), so the design trades total token spend for main-thread context economy. That is a defensible choice ONLY if main-thread context scarcity is the binding constraint, which the wiki decision explicitly asserts. The math here quantifies the cost side of that trade.
