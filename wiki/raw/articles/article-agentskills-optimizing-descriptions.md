# Optimizing skill descriptions — agentskills.io

**URL**: https://agentskills.io/skill-creation/optimizing-descriptions.md
**Fetched**: 2026-04-17

---

> How to improve your skill's description so it triggers reliably on relevant prompts.

A skill only helps if it gets activated. The `description` field in your `SKILL.md` frontmatter is the primary mechanism agents use to decide whether to load a skill for a given task. An under-specified description means the skill won't trigger when it should; an over-broad description means it triggers when it shouldn't.

## How skill triggering works

Agents use progressive disclosure to manage context. At startup, they load only the `name` and `description` of each available skill. When a user's task matches a description, the agent reads the full `SKILL.md` into context.

Important nuance: agents typically only consult skills for tasks that require knowledge or capabilities beyond what they can handle alone. A simple, one-step request may not trigger a skill even if the description matches perfectly. Tasks that involve specialized knowledge — an unfamiliar API, a domain-specific workflow, or an uncommon format — are where a well-written description makes the difference.

## Writing effective descriptions

* **Use imperative phrasing.** Frame the description as an instruction: "Use this skill when..." rather than "This skill does..."
* **Focus on user intent, not implementation.** Describe what the user is trying to achieve.
* **Err on the side of being pushy.** Explicitly list contexts where the skill applies, including cases where the user doesn't name the domain directly.
* **Keep it concise.** A few sentences to a short paragraph. Hard limit of 1024 characters.

## Designing trigger eval queries

To test triggering, create ~20 eval queries: 8-10 that should trigger and 8-10 that shouldn't.

### Should-trigger queries

Vary along axes: phrasing (formal/casual), explicitness (names domain vs. describes need), detail (terse vs. context-heavy), complexity (single-step vs. multi-step).

### Should-not-trigger queries

Most valuable: **near-misses** — queries that share keywords or concepts but actually need something different.

For a CSV analysis skill, weak negatives:
* "Write a fibonacci function" — obviously irrelevant
* "What's the weather today?" — no keyword overlap

Strong negatives:
* "I need to update the formulas in my Excel budget spreadsheet" — shares "spreadsheet" but needs Excel editing
* "can you write a python script that reads a csv and uploads each row to our postgres database" — involves CSV but task is ETL, not analysis

### Tips for realism

Include file paths, personal context, specific details, casual language, abbreviations, and occasional typos.

## Testing whether a description triggers

Run each query through your agent with the skill installed. Observe whether the agent invokes it. Model behavior is nondeterministic — run each query multiple times (3 is a reasonable starting point) and compute a **trigger rate**.

A should-trigger query passes if its trigger rate is above a threshold (0.5 is a reasonable default).

## Avoiding overfitting with train/validation splits

Split your query set:
* **Train set (~60%)**: queries used to identify failures and guide improvements.
* **Validation set (~40%)**: set aside to check whether improvements generalize.

## The optimization loop

1. Evaluate on both train and validation sets.
2. Identify failures in the train set only.
3. Revise the description. Generalize from failure patterns — don't add specific keywords from failed queries (overfitting). Find the general category those queries represent.
4. Repeat until train set passes or improvement stalls.
5. Select the best iteration by validation pass rate.

## Applying the result

Before and after:

```yaml
# Before
description: Process CSV files.

# After
description: >
  Analyze CSV and tabular data files — compute summary statistics,
  add derived columns, generate charts, and clean messy data. Use this
  skill when the user has a CSV, TSV, or Excel file and wants to
  explore, transform, or visualize the data, even if they don't
  explicitly mention "CSV" or "analysis."
```
