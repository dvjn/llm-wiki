# How to add skills support to your agent — agentskills.io

**URL**: https://agentskills.io/client-implementation/adding-skills-support.md
**Fetched**: 2026-04-17

---

> A guide for adding Agent Skills support to an AI agent or development tool.

## The core principle: progressive disclosure

Three-tier loading:

| Tier            | What's loaded               | When                        | Token cost              |
| --------------- | --------------------------- | --------------------------- | ----------------------- |
| 1. Catalog      | Name + description          | Session start               | ~50-100 tokens per skill |
| 2. Instructions | Full `SKILL.md` body        | When the skill is activated | <5000 tokens (recommended) |
| 3. Resources    | Scripts, references, assets | When instructions reference them | Varies             |

## Step 1: Discover skills

### Where to scan

Scan both client-specific and cross-client directories at project and user scope:

| Scope   | Path                               | Purpose                       |
| ------- | ---------------------------------- | ----------------------------- |
| Project | `<project>/.<your-client>/skills/` | Client's native location |
| Project | `<project>/.agents/skills/`        | Cross-client interoperability |
| User    | `~/.<your-client>/skills/`         | Client's native location |
| User    | `~/.agents/skills/`                | Cross-client interoperability |

`.agents/skills/` has emerged as the widely-adopted convention for cross-client skill sharing. Some implementations also scan `.claude/skills/` for pragmatic compatibility.

### What to scan for

Look for subdirectories containing a file named exactly `SKILL.md`. Skip `.git/` and `node_modules/`. Set reasonable bounds (max depth 4-6, max 2000 directories).

### Handling name collisions

**Project-level skills override user-level skills.** Log a warning when a collision occurs.

### Trust considerations

Consider gating project-level skill loading on a trust check — prevents untrusted repositories from silently injecting instructions.

## Step 2: Parse `SKILL.md` files

Extract YAML frontmatter (between `---` delimiters) and markdown body. Handle malformed YAML gracefully — consider a fallback for unquoted values containing colons.

**Lenient validation**: warn on issues but load the skill when possible (except when description is missing or YAML is completely unparseable).

## Step 3: Disclose available skills to the model

Include `name`, `description`, and optionally `location` in a skill catalog. Each skill adds ~50-100 tokens. Place in system prompt or tool description.

Include behavioral instructions telling the model how and when to use skills.

## Step 4: Activate skills

### Model-driven activation

Two patterns:
* **File-read activation**: model calls its standard file-read tool with the `SKILL.md` path
* **Dedicated tool activation**: register a tool (e.g., `activate_skill`) that returns content; allows content control, structured wrapping, resource listing, permission enforcement

### User-explicit activation

Slash command or mention syntax (`/skill-name`) that the harness intercepts.

### Structured wrapping (for dedicated tools)

Wrap skill content in identifying tags, include the skill directory path, and list bundled resources (without eagerly reading them).

### Permission allowlisting

Allowlist skill directories so the model can read bundled resources without triggering permission prompts.

## Step 5: Manage skill context over time

* **Protect skill content from context compaction**: skill instructions are durable behavioral guidance — losing them mid-conversation silently degrades performance.
* **Deduplicate activations**: skip re-injection if a skill is already in context.
* **Subagent delegation** (advanced): run the skill in a separate subagent session for complex workflows.
