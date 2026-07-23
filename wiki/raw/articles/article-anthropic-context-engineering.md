# Raw: Anthropic — Effective Context Engineering for AI Agents

**Source URL**: https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
**Retrieved**: 2026-07-24
**Prompted by**: Research on context-engineering best practices with subagents — when delegation reduces cost/context vs adds overhead; evaluating a design where every wiki operation (even one-fact lookups) is delegated to a subagent to keep file content out of the main context.

## Core framing: context is a finite, degrading resource

- The post treats the model's context window as the scarce resource to be budgeted — the reason to move detailed work out of the main thread is to preserve the main agent's limited attention budget for high-level synthesis and decision-making.
- Main thread should focus on "synthesizing and analyzing the results" rather than holding all detailed exploration state.

## Sub-agent architecture as a context-engineering tool

- Sub-agents operate with **"clean context windows"** while the main coordinator maintains strategic oversight.
- **Context isolation (key quantified claim)**: each subagent "explores extensively, using tens of thousands of tokens or more, but returns only a condensed, distilled summary of its work (often 1,000-2,000 tokens)."
  - This is the compression ratio that justifies delegation: ~10s of thousands of tokens of exploration in → ~1–2k tokens out to the main thread.
- "Clear separation of concerns" ensures "the detailed search context remains isolated within sub-agents."

## When multi-agent / sub-agent architecture fits

- Most effective "for complex research and analysis where parallel exploration pays dividends" — i.e., tasks decomposable into independent subtasks explored in parallel.
- The post positions sub-agent architecture as ONE of several context-management techniques, alongside:
  - **Compaction** — better "for tasks requiring extensive back-and-forth" where conversational flow matters.
  - **Note-taking / external memory** — excels "for iterative development with clear milestones."
- Implication: multi-agent addresses "only specific scenarios requiring parallel, independent exploration," not a universal default.

## Relevance to the wiki subagent-delegation design

- SUPPORTS the design's governing principle that main context is the scarce resource and that wiki reads/writes should be pushed into subagent threads returning compact reports.
- The 1–2k-token distilled-summary target is a concrete report-compression benchmark the wiki's librarian/scribe reports should aim for.
- CAVEAT / tension: the compression math is stated for subagents that "explore extensively, using tens of thousands of tokens." The justification is strongest when the delegated work is large. It does not, by itself, endorse delegating a trivial one-fact lookup (where exploration is small and the fixed spawn overhead dominates). See [[article-anthropic-multi-agent-research]] and [[article-subagent-token-overhead-analysis]] for the against side.
