# llm-wiki

Build and maintain a knowledge wiki using LLMs.

## Project Structure

- `skills/llm-wiki/` — the skill (SKILL.md, references/)
- `wiki/` — the knowledge base (raw/, pages/, index.md, log.md, SCHEMA.md)

## Knowledge Base - Wiki

The wiki at `wiki/` is the project knowledge base. Check it before answering questions; update it when you learn something significant.

- Use the llm-wiki skill for all wiki operations
- Query the wiki before working on any project task or answering project questions
- Ingest new information when it adds value to the knowledge base (papers, docs, decisions)
- Create pages when documenting decisions, concepts, entities, or comparisons
- Lint the wiki periodically or after major project changes