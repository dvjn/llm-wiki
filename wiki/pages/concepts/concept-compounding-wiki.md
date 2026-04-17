# Compounding Wiki

> A wiki where each ingest, query, and lint pass increases the value of all future operations — contrast with stateless RAG retrieval.

## Core Definition

A compounding wiki is a knowledge base that accumulates structure over time rather than retrieving from raw sources on every query. When the LLM processes a new source, it updates cross-references, flags contradictions, and integrates the new material with everything already in the wiki. The result compounds: later queries benefit from all prior synthesis, not just the relevant raw documents.

## Key Properties

- **Synthesis is preserved**: Answers are filed back into the wiki rather than lost in chat history.
- **Cross-references are maintained**: Every ingest links concepts to related concepts, so the graph of knowledge grows denser.
- **Contradictions are surfaced explicitly**: The LLM notes when new sources conflict with prior pages rather than silently overwriting.
- **Maintenance cost is near-zero**: The LLM handles bookkeeping — humans only need to add sources and ask questions.

## Contrast With RAG

| Dimension | Compounding Wiki | Traditional RAG |
|-----------|-----------------|-----------------|
| Query path | LLM reads pre-synthesized pages | LLM reads raw chunks |
| Cross-references | Pre-built during ingest | Reconstructed at query time |
| Contradiction detection | Explicit, during ingest | Implicit, varies by query |
| Maintenance burden | LLM handles it | Human or pipeline required |
| Value over time | Compounds | Flat |

Examples of RAG-style systems: NotebookLM, ChatGPT file uploads. The wiki approach compiles knowledge once and keeps it current rather than re-deriving it on every query.

## Analogy

> "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase." — [[source-karpathy-llm-wiki]]

The human curates sources and asks good questions. The LLM does all the bookkeeping — summarizing, cross-referencing, filing, and maintenance.

## Use Cases

The pattern applies wherever knowledge accumulates over time:

- **Personal**: goals, health, self-improvement — filing journal entries and articles
- **Research**: reading papers over weeks/months, building an evolving thesis
- **Book notes**: filing chapters as you read, building a companion wiki (analogous to fan wikis like Tolkien Gateway)
- **Business/team**: internal wiki fed by Slack threads, meeting transcripts, customer calls
- **Other**: competitive analysis, due diligence, course notes, hobby deep-dives

## Intellectual Lineage

The pattern is related to Vannevar Bush's Memex (1945) — a personal, curated knowledge store with associative trails between documents. Bush's vision was private, actively curated, with connections as valuable as the documents themselves. The unsolved problem was maintenance. The LLM handles that.

## Related Concepts

- [[concept-three-layer-architecture]] — the structure that makes compounding possible
- [[concept-ingest-workflow]] — the mechanism by which synthesis accumulates
- [[concept-query-workflow]] — how compounded knowledge is retrieved

## Related Decisions

- [[decision-llm-writes-wiki]] — why the LLM, not the human, writes wiki pages
