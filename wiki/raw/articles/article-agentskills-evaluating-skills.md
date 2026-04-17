# Evaluating skill output quality — agentskills.io

**URL**: https://agentskills.io/skill-creation/evaluating-skills.md
**Fetched**: 2026-04-17

---

> How to test whether your skill produces good outputs using eval-driven iteration.

## Designing test cases

A test case has three parts:
* **Prompt**: a realistic user message
* **Expected output**: a human-readable description of what success looks like
* **Input files** (optional): files the skill needs to work with

Store test cases in `evals/evals.json` inside the skill directory.

Tips for writing good test prompts:
* Start with 2-3 test cases. Don't over-invest before you've seen your first round of results.
* Vary the prompts: different phrasings, levels of detail, formality.
* Cover edge cases: at least one boundary condition.
* Use realistic context: file paths, column names, personal context.

## Running evals

Run each test case twice: once **with the skill** and once **without it** (baseline). Each pass gets its own `iteration-N/` directory in a workspace.

Each eval run should start with a clean context — no leftover state from previous runs.

### Capturing timing data

Record `total_tokens` and `duration_ms` for each run. The delta tells you what the skill costs (time, tokens) and what it buys (higher pass rate).

## Writing assertions

Assertions are verifiable statements about what the output should contain. Add them after you see your first round of outputs.

Good assertions:
* "The output file is valid JSON" — programmatically verifiable
* "The bar chart has labeled axes" — specific and observable
* "The report includes at least 3 recommendations" — countable

Weak assertions:
* "The output is good" — too vague
* "The output uses exactly the phrase 'Total Revenue: $X'" — too brittle

## Grading outputs

Evaluate each assertion against actual outputs and record PASS or FAIL with specific evidence quoting the output.

**Require concrete evidence for a PASS.** Don't give the benefit of the doubt.

Blind comparison: present both outputs to an LLM judge without revealing which came from which version. This complements assertion grading by evaluating holistic qualities.

## Aggregating results

Compute summary statistics per configuration: mean pass rate, time, tokens, and delta (skill vs. baseline).

## Analyzing patterns

* Remove assertions that always pass in both configurations — they don't tell you anything useful.
* Investigate assertions that always fail in both — the assertion may be broken.
* Study assertions that pass with the skill but fail without — this is where the skill adds value.
* Tighten instructions when results are inconsistent across runs.

## Iterating on the skill

Signals: failed assertions (specific gaps), human feedback (broader quality issues), execution transcripts (why things went wrong).

Give all three signals plus the current `SKILL.md` to an LLM and ask it to propose improvements. Guidelines:
* **Generalize from feedback.** Fixes should address underlying issues broadly.
* **Keep the skill lean.** Fewer, better instructions often outperform exhaustive rules.
* **Explain the why.** Reasoning-based instructions work better than rigid directives.
* **Bundle repeated work.** If every test run independently wrote a similar helper script, bundle it in `scripts/`.

The loop: propose improvements → apply → rerun in `iteration-<N+1>/` → grade and aggregate → human review → repeat.
