# Raw: Claude Code / Agent SDK — Subagents Documentation

**Source URLs**:
- https://code.claude.com/docs/en/sub-agents (Claude Code "Create custom subagents")
- https://code.claude.com/docs/en/agent-sdk/subagents (Agent SDK "Subagents in the SDK")
**Retrieved**: 2026-07-24
**Prompted by**: Research on when to use subagents vs inline work; context isolation benefits and costs (cold-start context, single-call vs multi-turn); evaluating the delegate-every-wiki-operation design.

## When to use a subagent (the inline-vs-delegate line — key quote)

- "Use one when a side task would flood your main conversation with search results, logs, or file contents you won't reference again: the subagent does that work in its own context and returns only the summary."
- "Define a custom subagent when you keep spawning the same kind of worker with the same instructions."
- Stated benefits: **Preserve context** ("keeping exploration and implementation out of your main conversation"), **Enforce constraints** (tool limits), **Reuse configurations**, **Specialize behavior**, **Control costs** ("routing tasks to faster, cheaper models like Haiku").

## Context isolation mechanics

- "Each subagent runs in its own context window with a custom system prompt, specific tool access, and independent permissions."
- SDK: "Each subagent runs in its own fresh conversation. Intermediate tool calls and results stay inside the subagent; only its final message returns to the parent."
- SDK example: "a research-assistant subagent can explore dozens of files without any of that content accumulating in the main conversation. The parent receives a concise summary, not every file the subagent read."

## Cold-start context — what a subagent does and does NOT inherit

- "A subagent's context window starts fresh, with no parent conversation, but isn't empty. The only content you pass from parent to subagent is the Agent tool's prompt string, so include any file paths, error messages, or decisions the subagent needs directly in that prompt."
- Subagent RECEIVES: its own system prompt + the Agent tool prompt; project CLAUDE.md (when settingSources loaded); tool definitions.
- Subagent does NOT receive: parent's conversation history or tool results; parent's system prompt; preloaded skill content (unless listed in `skills`).
- Direct implication for the wiki design: a delegated lookup pays to rebuild context from scratch (read index.md + relevant page) on every call — the subagent cannot reuse anything the main thread already loaded. This is the concrete "cold-start" cost. The wiki scribe brief must be fully standalone (matches the decision's "give the scribe a complete standalone brief").

## Built-in subagents and the fast/cheap pattern

- Explore and Plan "skip your CLAUDE.md files and the parent session's git status to keep research fast and inexpensive." Every other subagent loads both.
- Explore: read/search only, thoroughness levels quick/medium/very thorough; "keeps exploration results out of your main conversation context." Can be pinned to `model: haiku`.
- general-purpose: used "when the task requires both exploration and modification, complex reasoning to interpret results, or multiple dependent steps."
- Cost control via model routing (Haiku for cheap exploration) is a first-class recommendation.

## Single-call vs multi-turn / resume

- Built-in Explore and Plan are "one-shot" and don't return an `agentId` (cannot be resumed).
- Custom / general-purpose subagents CAN be resumed: they "retain full conversation history." Resuming avoids re-paying cold-start context but requires resuming the same session.
- SDK: subagent transcripts persist in separate files and survive main-conversation compaction.

## Scaling limits

- "Subagents work well for a few delegated tasks per turn." For "dozens to hundreds of agents," use the `Workflow` tool, which "moves the orchestration into a script the runtime executes outside the conversation context."
- Background execution is now the default (v2.1.198+): Claude sets `run_in_background: false` only when it needs the result before continuing.

## Report/output handling and safety

- "The parent receives the subagent's final message as the Agent tool result, but may summarize it in its own response." To preserve verbatim, instruct so in the prompt.
- Output scanning (v2.1.210+): the harness neutralizes control-tag imitation, turn markers, and flags permission-config mentions in a subagent's final message before the parent reads it — relevant because the wiki researcher note says subagent reports feed back to a main agent.

## Relevance to the wiki subagent-delegation design

- SUPPORTS: the "returns only the summary / keep file contents out of main conversation" framing is almost exactly the wiki decision's governing principle. The read-only Explore/doc-reviewer tool-restriction pattern matches librarian (read-only) and researcher (raw-only) roles — though the docs make these ENFORCED restrictions, whereas the wiki decision downgraded them to instructions after dropping static agent definitions.
- AGAINST delegating trivial lookups: the trigger is "a side task would FLOOD your main conversation with... content you won't reference again." A single-fact lookup that returns one line does not flood anything; the cold-start cost (re-reading index + page from scratch, no cache reuse) can exceed the content it keeps out. The docs frame subagents around large/repeated side tasks, not atomic reads.
- Cost-control lever the wiki design could adopt: pin the librarian to a cheaper model (Haiku) for lookups, per the built-in Explore/`model: haiku` guidance.
