# Anthropic Official — Skill Authoring Best Practices

**Source:** https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices
**Retrieved:** 2026-07-24
**Prompted by:** "What are Anthropic's OFFICIAL best practices for authoring Agent Skills — the parameters by which an existing skill should be judged?" (to become review criteria for the llm-wiki skill)

This is Anthropic's canonical, product-agnostic authoring guide ("writing guidance that applies across Claude products"). It is distinct from the agentskills.io open-standard docs already captured; where they overlap, this one carries the authoritative numeric limits.

## Hard limits (verbatim)

Frontmatter validation (also restated in a "Technical notes" section):
- `name`: **Maximum 64 characters**, lowercase letters/numbers/hyphens only, no XML tags, cannot contain reserved words "anthropic" or "claude".
- `description`: **non-empty, maximum 1,024 characters**, no XML tags. "Should describe what the Skill does and when to use it."
- SKILL.md body: **"Keep SKILL.md body under 500 lines for optimal performance."** Split into separate files when approaching the limit.
- Reference files longer than 100 lines: **include a table of contents at the top** (so Claude sees full scope even when previewing with partial reads like `head -100`).

Note: Claude Code's own docs give a different, larger listing cap — the combined `description` + `when_to_use` is truncated at **1,536 characters** in the skill listing (see [[article-claude-code-skills-authoring]]). The 1,024 figure here is the frontmatter field validation limit; 1,536 is the runtime listing budget cap. These are two different numbers for related things.

## Core principles

**Concise is key.** "The context window is a public good." Startup cost is only metadata (name + description) from all skills; SKILL.md loads only when relevant; other files load only as needed. But once SKILL.md loads, "every token competes with conversation history and other context." Default assumption: **"Claude is already very smart"** — only add context Claude doesn't already have. Three challenge questions: "Does Claude really need this explanation?" / "Can I assume Claude knows this?" / "Does this paragraph justify its token cost?" (Concrete example: ~50-token concise PDF snippet vs. ~150-token verbose version that explains what a PDF is.)

**Set appropriate degrees of freedom.** Match specificity to the task's fragility and variability. The robot-on-a-path analogy:
- **High freedom** (text instructions): multiple valid approaches, decisions depend on context, heuristics guide. "Open field with no hazards." Example: code review.
- **Medium freedom** (pseudocode / scripts with parameters): a preferred pattern exists, some variation acceptable.
- **Low freedom** (specific scripts, few/no parameters): operations fragile and error-prone, consistency critical, a specific sequence must be followed. "Narrow bridge with cliffs on both sides." Example: database migration — "Run exactly this script... Do not modify the command or add additional flags."

**Test with all models you plan to use.** Skills are additions to the model, so effectiveness depends on the model. Haiku: "Does the Skill provide enough guidance?" Sonnet: "Is the Skill clear and efficient?" Opus: "Does the Skill avoid over-explaining?" "What works perfectly for Opus might need more detail for Haiku."

## Naming and description

**Naming:** prefer **gerund form** (verb + -ing): `processing-pdfs`, `analyzing-spreadsheets`, `writing-documentation`. Acceptable: noun phrases (`pdf-processing`) or action-oriented (`process-pdfs`). Avoid vague (`helper`, `utils`, `tools`), overly generic (`documents`, `data`, `files`), reserved words, and inconsistent patterns within a collection.

**Descriptions:**
- **Always write in third person** ("The description is injected into the system prompt; inconsistent point-of-view can cause discovery problems"). Good: "Processes Excel files and generates reports." Avoid: "I can help you..." / "You can use this to..."
- **Be specific and include key terms** — both *what* it does and *specific triggers/contexts for when* to use it. "Claude uses it to choose the right Skill from potentially 100+ available Skills."
- Format that recurs across examples: `<what it does>. Use when <triggers/contexts>.` e.g. "Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction."
- Avoid: "Helps with documents", "Processes data", "Does stuff with files".

## Progressive disclosure

SKILL.md is "an overview that points Claude to detailed materials as needed, like a table of contents in an onboarding guide." Three patterns: (1) high-level guide with references, (2) domain-specific organization (e.g. `reference/finance.md`, `reference/sales.md` so a sales question never loads finance context), (3) conditional details (show basic, link advanced).

**Avoid deeply nested references** — the anti-pattern is explicit. "Claude may partially read files when they're referenced from other referenced files... might use commands like `head -100` to preview rather than reading entire files, resulting in incomplete information." **Rule: keep references one level deep from SKILL.md.** All reference files should link directly from SKILL.md.

## Content guidelines / anti-patterns (explicit)

- **Avoid time-sensitive information** — anti-pattern is dates that "will become wrong" ("If you're doing this before August 2025, use the old API"). Fix: a collapsed `## Old patterns` / `<details>` section for historical/deprecated context.
- **Use consistent terminology** — pick one term and keep it (always "API endpoint", never mixing "URL"/"route"/"path"; always "field", never "box"/"element"/"control"). "Consistency helps Claude parse and follow instructions."
- **Avoid Windows-style paths** — always forward slashes even on Windows.
- **Avoid offering too many options** — don't present multiple approaches unless necessary ("You can use pypdf, or pdfplumber, or PyMuPDF..."). Provide a default with an escape hatch.
- **Avoid assuming tools are installed** — be explicit about dependencies / install commands.

## Workflows and feedback loops

Break complex operations into clear sequential steps; for particularly complex ones, provide a **checklist Claude copies into its response and checks off**. Applies to code and non-code skills alike. **Feedback loop pattern:** run validator → fix errors → repeat ("greatly improves output quality"); the "validator" can be a STYLE_GUIDE.md that Claude reads and compares against, not just a script.

Patterns catalog: template pattern (strict "ALWAYS use this exact template" vs. flexible "sensible default, use your best judgment"), examples pattern (input/output pairs), conditional workflow pattern (decision points).

## Evaluation and iteration (a judging axis)

**"Build evaluations first"** — "Create evaluations BEFORE writing extensive documentation. This ensures your Skill solves real problems rather than documenting imagined ones." Evaluation-driven development: (1) identify gaps by running Claude without the skill, (2) create **three** scenarios testing the gaps, (3) establish baseline, (4) write minimal instructions to pass, (5) iterate. Eval JSON structure: `skills`, `query`, `files`, `expected_behavior` (list of assertions). Note: "There is not currently a built-in way to run these evaluations. Users can create their own."

**Claude A / Claude B iteration loop:** develop with one instance ("Claude A" — the expert refining the skill) and test with a fresh instance ("Claude B" — using the skill on real tasks). Observe Claude B's behavior, bring specifics back to Claude A. "Iterate based on observation rather than assumptions." Watch for: unexpected exploration paths, missed connections (references not followed), overreliance on one section (maybe promote it into SKILL.md), ignored content (maybe unnecessary or poorly signaled). "The 'name' and 'description' in your Skill's metadata are particularly critical."

## Checklist (Anthropic's own, verbatim structure)

Core quality: description specific + includes key terms; description includes what + when; SKILL.md body under 500 lines; additional details in separate files; no time-sensitive info; consistent terminology; concrete not abstract examples; file references one level deep; progressive disclosure used appropriately; workflows have clear steps.
Code/scripts: scripts solve rather than defer; explicit error handling; no "voodoo constants" (Ousterhout's law — "If you don't know the right value, how will Claude determine it?"); packages listed and verified; scripts documented; no Windows paths; validation steps for critical ops; feedback loops for quality-critical tasks.
Testing: at least three evaluations; tested with Haiku, Sonnet, and Opus; tested with real usage; team feedback incorporated.

## Executable-code specifics
"Solve, don't defer" (handle errors in scripts, don't punt to Claude); provide utility scripts (more reliable, save tokens/time, ensure consistency); make execution-vs-read intent explicit ("Run X" vs "See X for the algorithm"); create verifiable intermediate outputs (plan-validate-execute); MCP tools need fully-qualified names (`ServerName:tool_name`). Package availability differs: claude.ai can install from npm/PyPI/GitHub; Claude API has no network access / no runtime install.
