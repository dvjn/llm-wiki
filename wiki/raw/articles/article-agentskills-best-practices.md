# Best practices for skill creators — agentskills.io

**URL**: https://agentskills.io/skill-creation/best-practices.md
**Fetched**: 2026-04-17

---

> How to write skills that are well-scoped and calibrated to the task.

## Start from real expertise

A common pitfall in skill creation is asking an LLM to generate a skill without providing domain-specific context — relying solely on the LLM's general training knowledge. The result is vague, generic procedures ("handle errors appropriately," "follow best practices for authentication") rather than the specific API patterns, edge cases, and project conventions that make a skill valuable.

Effective skills are grounded in real expertise. The key is feeding domain-specific context into the creation process.

### Extract from a hands-on task

Complete a real task in conversation with an agent, providing context, corrections, and preferences along the way. Then extract the reusable pattern into a skill. Pay attention to:

* **Steps that worked** — the sequence of actions that led to success
* **Corrections you made** — places where you steered the agent's approach
* **Input/output formats** — what the data looked like going in and coming out
* **Context you provided** — project-specific facts, conventions, or constraints the agent didn't already know

### Synthesize from existing project artifacts

Good source material includes: internal documentation, runbooks, style guides, API specs, schemas, code review comments, issue trackers, version control history, and real-world failure cases.

## Refine with real execution

The first draft of a skill usually needs refinement. Run the skill against real tasks, then feed the results back into the creation process. Ask: what triggered false positives? What was missed? What could be cut?

Read agent execution traces, not just final outputs. Common causes of waste: instructions too vague (agent tries several approaches), instructions that don't apply (agent follows them anyway), or too many options without a clear default.

## Spending context wisely

### Add what the agent lacks, omit what it knows

Focus on what the agent *wouldn't* know without your skill: project-specific conventions, domain-specific procedures, non-obvious edge cases, and the particular tools or APIs to use. Don't explain things the agent already knows (what a PDF is, how HTTP works).

### Design coherent units

Deciding what a skill should cover is like deciding what a function should do: encapsulate a coherent unit of work that composes well with other skills. Skills scoped too narrowly force multiple skills to load for a single task. Skills scoped too broadly become hard to activate precisely.

### Aim for moderate detail

Overly comprehensive skills can hurt more than they help. Concise, stepwise guidance with a working example tends to outperform exhaustive documentation.

### Structure large skills with progressive disclosure

Keep `SKILL.md` under 500 lines / 5000 tokens. When a skill legitimately needs more content, move detailed reference material to `references/`. Tell the agent *when* to load each file: "Read `references/api-errors.md` if the API returns a non-200 status code."

## Calibrating control

### Match specificity to fragility

**Give the agent freedom** when multiple approaches are valid and the task tolerates variation. Explaining *why* can be more effective than rigid directives.

**Be prescriptive** when operations are fragile, consistency matters, or a specific sequence must be followed.

### Provide defaults, not menus

When multiple tools or approaches could work, pick a default and mention alternatives briefly:

```markdown
Use pdfplumber for text extraction. For scanned PDFs requiring OCR, use pdf2image with pytesseract instead.
```

### Favor procedures over declarations

A skill should teach the agent *how to approach* a class of problems, not *what to produce* for a specific instance.

## Patterns for effective instructions

### Gotchas sections

The highest-value content in many skills is a list of gotchas — environment-specific facts that defy reasonable assumptions:

```markdown
## Gotchas

- The `users` table uses soft deletes. Queries must include `WHERE deleted_at IS NULL`.
- The user ID is `user_id` in the database, `uid` in the auth service, and `accountId` in the billing API.
```

Keep gotchas in `SKILL.md` where the agent reads them before encountering the situation.

### Templates for output format

Provide a template when you need the agent to produce output in a specific format. Short templates can live inline; longer ones belong in `assets/`.

### Checklists for multi-step workflows

An explicit checklist helps the agent track progress and avoid skipping steps:

```markdown
Progress:
- [ ] Step 1: Analyze the form (run `scripts/analyze_form.py`)
- [ ] Step 2: Create field mapping (edit `fields.json`)
- [ ] Step 3: Validate mapping (run `scripts/validate_fields.py`)
```

### Validation loops

Instruct the agent to validate its own work before moving on: do the work, run a validator, fix any issues, repeat until passing.

### Plan-validate-execute

For batch or destructive operations: create an intermediate plan, validate it against a source of truth, then execute.

### Bundling reusable scripts

If you notice the agent independently reinventing the same logic each run, that's a signal to write a tested script once and bundle it in `scripts/`.
