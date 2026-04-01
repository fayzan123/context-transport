---
name: context-transport
description: Use when exporting project context from a source repo to share with AI agents in adjacent repos, OR when a PROJECT_CONTEXT.md is present and you need to ground your work in it before starting a task in a separate workspace.
---

# Context Transport

## Overview

Moves project understanding across repo boundaries. When building adjacent tools (outreach agents, integrations, companion apps), the AI in the other repo needs the kind of briefing a human PM gives a new contractor — not a code tour, but product intent, audience, vocabulary, and constraints.

Two modes: **Export** (source repo → `PROJECT_CONTEXT.md`) and **Consume** (read `PROJECT_CONTEXT.md` before any task).

---

## Mode 1: Export

**Trigger:** User asks to "export context", "generate a context document", "create a PROJECT_CONTEXT.md", or "prepare context for [adjacent project]."

### Step 1 — Gather

Read these in parallel before writing anything:
- `README.md` (or `README`)
- `CLAUDE.md` / `AGENTS.md` / `GEMINI.md`
- `package.json` / `pyproject.toml` / `go.mod` / `Cargo.toml` (pick what's present)
- Main entry point (`src/index.*`, `main.*`, `app.*`, `server.*`)
- Any `docs/` directory
- `.env.example` or config schemas (for integration points)
- Key domain model files (types, schemas, models)

If any of those are missing, proceed with what exists. Don't ask before reading.

### Step 2 — Synthesize, Don't Summarize

The goal is **contractor briefing**, not code documentation. Write what an experienced PM would tell a consultant on day one:
- What problem this solves and why it matters
- Who the users are and what they care about
- What the product actually does (user perspective)
- How it's structured (just enough for adjacent work to make coherent decisions)
- The language of the domain (terms that must be used consistently)
- Non-obvious decisions and constraints

**Avoid:** file-by-file summaries, exhaustive API listings, anything derivable by reading the code.

### Step 3 — Write

Save to `PROJECT_CONTEXT.md` at the repo root.

Use the template in `CONTEXT_TEMPLATE.md` (in this skill's directory) as the output structure.

Sections marked `[OPTIONAL]` — include only if content is meaningful. A missing section is better than a placeholder.

---

## Mode 2: Consume

**Trigger:** You're starting a task in a repo that contains `PROJECT_CONTEXT.md`. Load it before doing anything else.

### Protocol

1. Read `PROJECT_CONTEXT.md` in full before any other action.
2. Treat it as your product brief — equivalent to what the client told you before you started.
3. Anchor decisions in it: if the brief says the tone is "direct and unsentimental", don't write warm copy. If it says the audience is "senior engineers", don't over-explain basics.
4. When the brief and the code disagree, flag it — don't silently pick one.
5. If critical context is missing (e.g., you're writing outreach but tone/voice isn't in the doc), ask one targeted question before proceeding.

### What "using" the context means

- Reference the **domain terminology** — use the product's own words for its concepts
- Match the **tone/voice** in any user-facing output
- Respect **integration constraints** — don't assume APIs or services not listed
- Apply **audience knowledge** — write at the right level of sophistication
- Respect **business context** — don't propose directions the brief rules out

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Exporting a code tour ("File X does Y") | Write what a user/PM would say, not what a dev would say |
| Leaving sections blank with "N/A" | Omit sections with no meaningful content |
| Ignoring the context doc while working | Read it first, cite it when making decisions |
| Treating the doc as optional background | It's the brief — it's the starting point, not a reference |
| Writing tone/voice from scratch when the brief has it | Copy the brief's exact framing and vocabulary |

---

## Quick Reference

| Signal | Action |
|--------|--------|
| "export context" / "generate PROJECT_CONTEXT.md" | Export mode |
| Adjacent repo has `PROJECT_CONTEXT.md` | Consume mode — read before any task |
| Writing outreach/comms for a product | Check for `PROJECT_CONTEXT.md` first |
| Building an integration or plugin | Check for `PROJECT_CONTEXT.md` first |
| Context doc and code disagree | Flag the discrepancy, don't silently resolve |