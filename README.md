# LLM Wiki Skill

A skill for coding agents (OpenCode, Claude Code, Codex, etc.) that makes your LLM build and maintain a wiki from your documents. Based on [Karpathy's LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

## What

Most RAG setups re-read your documents from scratch on every query. This works differently: the LLM builds a persistent wiki of markdown files that grows and cross-references itself as you add sources. Drop in a paper, and the LLM reads it, updates relevant pages, flags contradictions with existing content, and files everything properly. The wiki accumulates knowledge rather than re-deriving it each time.

You handle sourcing and questions. The LLM handles the rest.

## How it works

Three layers:

- **Raw sources**: your documents, never modified by the LLM
- **Wiki pages**: markdown files the LLM creates and maintains (concepts, entities, comparisons, source summaries)
- **Schema**: the skill file, which tells the LLM the conventions and structure to follow

Three main workflows:

- **Ingest**: drop a source. The LLM reads it and updates roughly 10-15 wiki pages (summary, related concepts, entities, index, log).
- **Query**: ask a question. The LLM searches the wiki, synthesizes an answer with citations, and offers to file it as a new page if it's useful.
- **Lint**: periodic health-check for contradictions, stale claims, orphan pages, and missing links.

## Install

```bash
npx skills install dvjn/llm-wiki-skill
```

## What's in the skill

```
skills/llm-wiki/
├── SKILL.md          # Loaded into the LLM's context
├── assets/           # Reference materials
├── evals/            # Evaluation prompts
└── references/       # Extra docs
```

## Credit

[Karpathy's gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) describes the pattern. This skill is one working instantiation of it with schema conventions, page templates, cross-reference rules, and workflow definitions ready to drop into an agent session.

## License

MIT

