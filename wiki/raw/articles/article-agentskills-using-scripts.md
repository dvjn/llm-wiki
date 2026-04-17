# Using scripts in skills — agentskills.io

**URL**: https://agentskills.io/skill-creation/using-scripts.md
**Fetched**: 2026-04-17

---

> How to run commands and bundle executable scripts in your skills.

## One-off commands

When an existing package already does what you need, reference it directly in `SKILL.md` without a `scripts/` directory. Use runtime tools like `uvx`, `npx`, `bunx`, `deno run`, or `go run` that auto-resolve dependencies.

Tips:
* Pin versions for reproducibility
* State prerequisites in `SKILL.md` or use the `compatibility` frontmatter field
* Move complex commands into scripts when they grow hard to get right

## Referencing scripts from `SKILL.md`

Use relative paths from the skill directory root. List available scripts in `SKILL.md` so the agent knows they exist, then instruct the agent to run them.

## Self-contained scripts

Bundle scripts in `scripts/` that declare their own dependencies inline:

* **Python**: PEP 723 inline script metadata, run with `uv run`
* **Deno**: `npm:` and `jsr:` import specifiers
* **Bun**: auto-installs missing packages at runtime
* **Ruby**: `bundler/inline` for inline gem declarations

## Designing scripts for agentic use

### Avoid interactive prompts (hard requirement)

Agents operate in non-interactive shells. A script that blocks on interactive input will hang indefinitely. Accept all input via command-line flags, environment variables, or stdin.

### Document usage with `--help`

`--help` output is the primary way an agent learns your script's interface. Include description, available flags, and usage examples. Keep it concise — output enters the agent's context window.

### Write helpful error messages

Say what went wrong, what was expected, and what to try:

```
Error: --format must be one of: json, csv, table.
       Received: "xml"
```

### Use structured output

Prefer JSON, CSV, TSV over free-form text. Send structured data to stdout and diagnostics to stderr.

### Further considerations

* **Idempotency**: agents may retry commands
* **Input constraints**: reject ambiguous input with a clear error
* **Dry-run support**: `--dry-run` flag for destructive operations
* **Meaningful exit codes**: document them in `--help`
* **Predictable output size**: default to a summary or limit; support `--offset` for pagination
