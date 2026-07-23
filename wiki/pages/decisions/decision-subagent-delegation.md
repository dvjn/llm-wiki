# Decision: Role-Based Delegation via Dynamic Subagents

**Status**: Accepted (revised 2026-07-24 after evidence-based review; prior revisions 2026-07-23; original 2026-04-18)
**Date**: 2026-07-24
**Context**: The wiki workflows include operations that benefit from delegation: search-heavy reading (synthesis queries, gap/contradiction checks, lint), external research (filling gaps, capturing URL sources), and convention-heavy writing (pages, index, log). The original decision delegated only the search-heavy reading, generically ("if subagents available"), leaving external research unowned, writing always in the main context, and the delegation prompt improvised each time.

## Decision

**Governing principle: subagents exist to reduce main-context usage by grouping a wiki operation's tool calls into a separate thread — each operation is roughly one subagent call from the main agent's perspective, returning a compact report.** When subagents are available, the main agent prefers delegation over reading or writing wiki files; judgment (discussing takeaways, approving plans and fixes) happens in the main thread over reports, not raw material.

**Spawn mechanism (revised 2026-07-24): the subagent self-loads its role file.** The spawn prompt is ~120 tokens — "first read `<path>/references/agents/<role>.md`, follow everything below its `---` separator, then: <task>" — instead of the main agent reading the ~770-token role file and re-sending its body verbatim (~1,470 main-thread tokens per spawn). This is the thin-loader pattern from the dropped static agents, recovered for dynamic spawning; it made delegation cheap enough that it wins the context ledger for every operation type, including lookups ([[comparison-delegate-vs-inline]]).

Evidence-based exceptions to "always delegate" (2026-07-24):
- **Inline lookups** are allowed when `wiki/index.md` is already resident and the answer is a specific fact in 1-2 known pages — a run of small lookups is one agent's few tool calls, not one spawn per fact ([[source-anthropic-multi-agent-research]] names per-fact spawning a failure mode).
- **Filing a single synthesized answer is inline by default** — the content is already in the main context; delegating re-passes it and loses ([[source-subagent-token-overhead-analysis]]). The scribe is reserved for multi-page writes (ingest, lint fixes).
- **Single-call lint audit holds to ~150 pages** (~95k-token corpus in one subagent thread); beyond that, chunk the audit by page type.

Delegation is defined by **role**, mapped to the knowledge lifecycle, with canonical role prompts shipped inside the skill at `references/agents/{librarian,researcher,scribe}.md` — spawned as dynamic subagents by any harness that supports ad-hoc subagent prompts:

| Stage | Role | Role prompt | Access (by instruction) |
|-------|------|-------------|-------------------------|
| Acquire | Researcher | `references/agents/researcher.md` | Web + write to `wiki/raw/` only |
| Retrieve | Librarian | `references/agents/librarian.md` | Read-only |
| Write | Scribe | `references/agents/scribe.md` | Full write; templates path passed in the brief |

Calls per operation: query (any type, lookups included) = 1 librarian call; lint = 1 librarian audit call (full checklist in one thread) + 1 scribe call for fixes; ingest = 1 librarian source-analysis call + 1 scribe call executing the approved plan (+ 1 researcher call first for URL/topic sources). Parallel librarian fan-out for lint is an opt-in for speed, not the default — it multiplies spawns and reports in the main context. Audit needs no fourth role. Without subagent support, the same workflows run directly.

Each role-prompt file has a header for the spawning agent (required capabilities, review notes) and a verbatim prompt body.

Predefined static agents (Claude Code `agents/*.md` definitions) were built and then deliberately dropped: dynamic spawning from the shipped prompts is sufficient for both Claude Code and Codex, and one delegation mechanism on every install path beats two.

Operational rules:
- Give the scribe a complete standalone brief — it has no conversation context.
- Never run two scribes in parallel (`index.md`/`log.md` contention).
- The researcher never touches `wiki/pages/`, `index.md`, or `log.md`; its raw notes carry provenance and feed a later ingest.
- Judgment stays in the main context, exercised over subagent reports: ingest takeaway discussion works from the librarian's source-analysis report, lint fix approval from the audit report. The decisions are not delegated; the reading behind them is.

## Rationale

- The main context is the scarce resource: it accumulates for the whole session, while subagent threads are paid once and discarded. Moving every wiki read/write into subagent threads makes wiki cost to the main conversation proportional to the number of operations, not to wiki size or source length.
- This is why even lookups delegate (one call + terse answer beats index + page contents in the main thread) and why lint defaults to a single audit call (one spawn + one grouped report beats five spawns + five reports).
- Shipping the prompts makes delegation deterministic — defined workflows, output formats, and boundaries — instead of improvised per invocation.
- One mechanism everywhere preserves [[concept-cross-agent-skill-portability]]: Claude Code, Codex, and skill-only installs delegate identically.

Alternatives considered:
- **Prompt improvised by the main agent** (original decision): portable but vibes-based — no defined prompts, return formats, or boundaries.
- **Bundled static agent definitions** (interim revision): enforced tool restrictions, registry discoverability, and main-context savings (the role prompt loads in the subagent's context, not the spawner's). Dropped as not worth a second delegation mechanism and a Claude Code-only surface; boundary enforcement was judged unnecessary for this workload.
- **Keep lookups direct / parallel lint fan-out by default** (interim revision): optimized for latency over main-context economy. Reversed once the context-reduction principle was made governing — the fan-out survives as an explicit speed opt-in.
- **Main agent reads sources for ingest judgment** (interim revision): floods the main context with source material; replaced by the librarian's source-analysis call.
- **Separate audit role**: rejected — the audit is a librarian task type, not a fourth role.

## Consequences

- Positive: no wiki file content ever enters the main context — queries, audits, and ingests cost the main thread one tool call plus a compact report each; research gaps become captured sources instead of suggestions.
- Negative: higher latency per operation (subagent spawn + cold read of index) even for trivial lookups, and lint trades the fan-out's parallel speed for a sequential single thread by default. Accepted: context is the scarcer resource.
- Negative (token cost, quantified after the fact): delegation is not free at the system level. Multi-agent work uses about **15x more tokens** than chat ([[source-anthropic-multi-agent-research]]), and a fresh subagent rebuilds context from scratch with **no prompt-cache reuse** on its first request ([[source-subagent-token-overhead-analysis]], [[source-claude-code-subagents-docs]]). The decision trades higher total token spend for main-thread context economy — defensible only while main-thread context is genuinely the binding constraint. See [[concept-context-as-scarce-resource]] and [[comparison-delegate-vs-inline]].
- Positive: single source of truth for each role's instructions, shipped on every install path.
- Negative: role boundaries (librarian read-only, researcher raw-only) are instructions, not guarantees — the spawner grants capabilities per the prompt header and reviews reports for out-of-scope writes. Accepted deliberately.
- Resolved (2026-07-24): the spawning agent no longer reads role prompts into its own context — the self-load spawn mechanism recovers the main-context economy the static definitions had, without reintroducing a second delegation surface.
- Risk: `wiki-researcher` writing raw notes stretches [[decision-llm-writes-wiki]]'s "humans curate sources" — mitigated by keeping raw notes provenance-stamped drafts that the user reviews at ingest, and by the immutability rule ([[decision-immutable-raw-sources]]) applying to existing files.
- Resolved (2026-07-24): the "even lookups delegate" challenge was adjudicated with a measured cost model ([[synthesis-2026-07-skill-review]]). Under the old verbatim-spawn mechanism the critique held (delegated lookup ~1,870 main-thread tokens vs ~1,200 inline); the self-load mechanism inverts it (~520 vs ~1,200), so delegation stays the default with the inline-lookup exception above for the index-resident case. Cheaper-model routing for read-only calls adopted as a hint in SKILL.md.

## Related

- [[entity-wiki-librarian]], [[entity-wiki-researcher]], [[entity-wiki-scribe]] — the three roles
- [[concept-query-workflow]], [[concept-lint-workflow]], [[concept-ingest-workflow]] — where each role plugs in
- [[concept-cross-agent-skill-portability]] — why one mechanism everywhere matters
- [[decision-llm-writes-wiki]], [[decision-immutable-raw-sources]] — boundaries the researcher must respect
