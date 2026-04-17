# Quickstart — agentskills.io

**URL**: https://agentskills.io/skill-creation/quickstart.md
**Fetched**: 2026-04-17

---

> Create your first Agent Skill and see it work in VS Code.

## Create the skill

A skill is a folder containing a `SKILL.md` file. VS Code looks for skills in `.agents/skills/` by default. Create `.agents/skills/roll-dice/SKILL.md`:

```markdown
---
name: roll-dice
description: Roll dice using a random number generator. Use when asked to roll a die (d6, d20, etc.), roll dice, or generate a random dice roll.
---

To roll a die, use the following command that generates a random number from 1 to the given number of sides:

```bash
echo $((RANDOM % <sides> + 1))
```

Replace `<sides>` with the number of sides on the die (e.g., 6 for a standard die, 20 for a d20).
```

## How it works (progressive disclosure)

1. **Discovery** — Agent scans skill directories and reads only `name` and `description` at startup.
2. **Activation** — When you ask about rolling dice, the agent matches your question to the description and loads the full `SKILL.md` body.
3. **Execution** — The agent follows the instructions, adapting the terminal command to the number of sides requested.
