# context-transport

A Claude Code skill that solves the **context transportation problem** — moving deep project understanding from one repository to an AI agent working in a separate, adjacent workspace.

When you build tools that serve a product (outreach agents, integrations, companion apps, marketing automation), the AI in that adjacent repo needs more than a code tour. It needs the kind of briefing a PM gives a contractor on day one: what the product does, who it's for, how to talk about it, and what's off-limits.

This skill gives Claude a structured way to generate that briefing and consume it.

---

## The Problem

You're building an outreach agent in `repo-b`. It needs to write emails, position the product correctly, use the right terminology, and stay within the constraints of what the product actually does.

But `repo-b` has no idea what the product in `repo-a` is.

You could paste in a README. But READMEs are for developers — they describe implementation, not intent. They don't capture audience, tone, domain vocabulary, or the non-obvious decisions that shape everything.

**context-transport** generates a structured product brief from `repo-a` and drops it into `repo-b` as `PROJECT_CONTEXT.md`. When Claude opens `repo-b`, it reads that brief first — and works from it.

---

## Two Modes

### Export — Source repo → `PROJECT_CONTEXT.md`

Run this in the product repo you want to describe. Claude reads the codebase, synthesizes (not summarizes) it into a structured contractor brief, and writes `PROJECT_CONTEXT.md`.

**Trigger phrases:**
- "export context"
- "generate a PROJECT_CONTEXT.md"
- "prepare context for [adjacent project]"
- "create a context document"

**What gets generated:**

| Section | What it captures |
|---------|-----------------|
| The Brief | One-paragraph summary written for a smart generalist, not a developer |
| Product Purpose | The problem, the approach, what success looks like |
| Target Audience | Who uses it, what they care about, what to avoid assuming |
| Key Features | User-facing capabilities, not implementation details |
| Architecture Overview | Just enough structure for adjacent work to make coherent decisions |
| Domain Terminology | Glossary of terms that must be used consistently |
| Tone & Voice | How the product communicates — with examples |
| Integration Points | External systems in play; adjacent agents shouldn't assume more |
| Business & Product Context | Stage, constraints, non-obvious decisions and why they were made |
| For Adjacent AI Agents | Explicit guidance: what to assume, what to verify, red lines |

The last section — **For Adjacent AI Agents** — is the most important. It's written specifically for the AI that will consume this document, not for humans. It tells the consuming agent what it's safe to assume, what it must never assume, and what's off-limits.

### Consume — Read `PROJECT_CONTEXT.md` before any task

When `PROJECT_CONTEXT.md` exists in a repo, Claude reads it in full before doing anything else and treats it as the foundational product brief throughout the session.

This means:
- Using the product's own terminology, not generic synonyms
- Matching the documented tone and voice in any user-facing output
- Respecting integration constraints — not inventing APIs or services
- Writing at the right level of sophistication for the documented audience
- Flagging (not silently resolving) any conflict between the brief and what's in the code

---

## Installation

### Option A — Personal skill (this Claude Code session only)

Copy the skill into your personal Claude skills directory:

```bash
cp -r skills/context-transport ~/.claude/skills/context-transport
```

Claude Code will pick it up automatically on next session start.

### Option B — Project skill (available to everyone working in a repo)

Copy into the target project's `.claude/skills/` directory:

```bash
cp -r skills/context-transport /path/to/your/project/.claude/skills/context-transport
```

### Option C — Manual

Copy `skills/context-transport/SKILL.md` wherever your agent runtime loads skills from.

---

## Usage

### Step 1 — Export context from your product repo

Open Claude Code in the repo you want to describe and say:

```
export context
```

Claude will read your README, CLAUDE.md, package manifests, entry points, docs, and domain models — then write `PROJECT_CONTEXT.md` at the repo root.

### Step 2 — Copy `PROJECT_CONTEXT.md` to the adjacent repo

```bash
cp PROJECT_CONTEXT.md /path/to/adjacent-repo/PROJECT_CONTEXT.md
```

Or commit it to the adjacent repo directly.

### Step 3 — Work in the adjacent repo

Open Claude Code in the adjacent repo. It will detect `PROJECT_CONTEXT.md`, read it before starting any task, and use it as the product brief throughout the session.

---

## Output Format

The generated `PROJECT_CONTEXT.md` follows the template in [`skills/context-transport/CONTEXT_TEMPLATE.md`](skills/context-transport/CONTEXT_TEMPLATE.md).

Sections marked `[OPTIONAL]` are omitted if there's no meaningful content — a missing section is better than a placeholder.

---

## What Makes This Different from a README

| README | PROJECT_CONTEXT.md |
|--------|-------------------|
| Written for developers | Written for adjacent AI agents (and humans onboarding them) |
| Describes implementation | Describes intent, audience, and constraints |
| Code-centric | Product-centric |
| Static documentation | Operational brief — consumed before every task |
| No guidance for adjacent work | Explicit "For Adjacent AI Agents" section |

---

## License

MIT
