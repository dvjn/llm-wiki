# LLM Wiki

Build and maintain a knowledge wiki using LLMs. Three-layer architecture: raw sources, LLM-generated pages, and schema documentation.

## Install

### Claude Code (plugin)

```
/plugin marketplace add dvjn/llm-wiki
/plugin install llm-wiki@dvjn
```

Workflows become directly callable: `/llm-wiki:setup`, `/llm-wiki:ingest`, `/llm-wiki:query`, `/llm-wiki:lint`.

The skill ships three subagent role prompts (`skills/llm-wiki/references/agents/`), one per stage of the knowledge lifecycle: researcher (acquire — web research distilled into `wiki/raw/` source notes), librarian (retrieve — all wiki reading, from lookups to lint audits and source analysis), and scribe (write — pages and index/log updates from a brief). Each wiki operation costs the main agent roughly one subagent call plus a compact report — wiki file content never enters the main context. Any harness that can spawn subagents with a custom prompt gets this on every install path; without subagents the workflows run directly and identically.

### OpenAI Codex (plugin)

```bash
codex plugin marketplace add dvjn/llm-wiki
codex plugin add llm-wiki@dvjn
```

Then invoke with `$llm-wiki` or let it trigger implicitly.

### Any agent (skill only)

```bash
npx skills add dvjn/llm-wiki
```

## Quick Start

See [SKILL.md](skills/llm-wiki/SKILL.md) for full documentation.

## Credit

Based on [Karpathy's LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)