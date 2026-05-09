---
name: claude-md
description: Generate or audit CLAUDE.md files for Claude Code — project-level (repo) or user-level (~/.claude/). Triggers when the user asks to write, create, refactor, review, trim, or audit a CLAUDE.md, or sets up Claude Code for the first time.
---

# CLAUDE.md Generator

Produce CLAUDE.md files that are short, executable, and router-style. Two modes: project-level and user-level.

## Operating principles

These shape how this skill behaves AND what it produces:

- **Think before writing.** State what you inferred. If multiple structures fit, present options — don't pick silently. If unclear, ask.
- **Simplicity first.** Minimum CLAUDE.md that solves the problem. No speculative sections.
- **Surgical when auditing.** Change only what's broken. Match the user's voice.
- **Goal-driven.** Every output ends with a verifiable check.

## Step 0: choose scope

Always ask first:

- **Project-level** → `<repo>/CLAUDE.md` or `<repo>/<module>/CLAUDE.md`. About this codebase.
- **User-level** → `~/.claude/CLAUDE.md`. About the user. Applies across every project.

**Fallback:** If the user didn't specify and the agent didn't ask, default to project-level.

Workflow branches from here.

---

## PROJECT-LEVEL workflow

### Hard constraints

1. **≤ 200 lines.** Overflow → `docs/*.md`, never inline.
2. **5-second test on every rule.**
3. **`Do NOT introduce` block mandatory.** ≥3 entries with one-line reasons.
4. **No prose, history, marketing, values.**
5. **Sensitive dirs (`auth/`, `billing/`, `infra/`, `migrations/`) get their own CLAUDE.md.**
6. **Single language throughout.** Don't mix languages in the file. Choose based on the user's language or existing CLAUDE.md. Technical terms (library names, CLI commands, file paths) are not considered mixing.

### Pre-generation scan

Inspect first, then ask:
- Manifest: `package.json` / `pyproject.toml` / `Cargo.toml` / `go.mod`
- Top-level dirs → flag sensitive-module candidates
- Existing `CLAUDE.md` / `MEMORY.md` / `docs/` / `.claude/hooks/`

State what you found. Ask only what you couldn't infer (priority order, working-style tweaks, additions to `Do NOT introduce`).

### Skeleton

```markdown
# <Project>

## Overview
<one line: what + for whom>
<one line: priority order, e.g. "speed > features > polish">

## Tech Stack
- <lib>@<major version>

## Do NOT introduce unless explicitly requested
- <lib> — <one-line reason>

## Coding Rules
- <imperative, verifiable rule>

## Working Style
- Surface assumptions; ask when unclear instead of guessing
- For ambiguous requests, present interpretations — don't pick silently
- Smallest change that solves it; no speculative abstractions
- Match existing code style, even if you'd write it differently
- For multi-step tasks, state plan with verify steps
- No "Great question!" / "Happy to help!" filler

## Pointers (read on demand)
- Architecture: `docs/architecture.md`
- ADRs: `docs/adrs/`

## Hooks (enforced, not remembered)
Defined in `.claude/hooks/`. Listed for visibility:
- <hook>

## Memory
Read `MEMORY.md` before each task. Append findings after.
```

The Working Style defaults are non-negotiable starting points. To remove one, name what's removed and why.

---

## USER-LEVEL workflow

User-level CLAUDE.md is about *who the user is* and *how Claude should default*. Input mode: **interview**, not scan.

### Interview

Conduct in 3–4 batched rounds. Don't ask every question; infer where you can. Adapt based on prior answers. Keep it ≤ 12 questions total.

**Round 1 — Role & domain**
- What do you primarily build? (backend / frontend / fullstack / ML / infra / mobile / mixed)
- Default languages when unspecified?

**Round 2 — Stack defaults**
- Frameworks/tools you reach for first?
- Anything you refuse globally? (libraries, patterns, paradigms)

**Round 3 — Working style**
- Ambiguity: ask vs make best guess?
- Plans: upfront vs as-needed?
- Code commentary: terse vs documented?

**Round 4 — Communication**
- Reply language? Code-comment language?
- Tone / formality?
- Pet peeves? (filler phrases, emoji, over-apologizing, hedging)

### Skeleton

```markdown
# Personal Defaults

## About me
- Role: <e.g. "backend engineer; infra & DX focus">
- Primary domains: <list>

## Default Stack
- Language priority when unspecified: <e.g. "Go > Python > TS">
- Default frameworks: <e.g. "FastAPI for Python web, Next.js for frontend">
- Avoid globally: <list with one-line reasons>

## Working Style
[Read `references/karpathy-claude.md` and adapt to interview answers.
Keep the 4 principles: Think Before Coding / Simplicity First / Surgical
Changes / Goal-Driven. Paraphrase tone, don't drop principles silently.]

## Communication
- Reply language: <e.g. "中文">
- Code comments: <e.g. "English">
- Tone: <one line>
- Don't: <pet peeves>

## Tradeoffs I prefer
- <e.g. "readability > cleverness">
- <e.g. "boring tech > novel tech">
- <e.g. "explicit > implicit">
```

### Hard constraints (user-level)

1. **≤ 150 lines.** Tighter than project-level — applies to every project.
2. **No project-specific content.** If a rule applies only to one repo, it belongs in that repo's CLAUDE.md.
3. **Every preference must be actionable.** "I like clean code" → out. "Prefer named exports" → in.
4. **Single language throughout.** Same rule as project-level — don't mix languages.

---

## Rule rewriting (both modes)

| ❌ Vague | ✅ Verifiable |
|---|---|
| Write clean code | Use named exports except in route files |
| Be type-safe | No `any`; use generics or interfaces |
| Keep components small | Components ≤ 200 lines unless justified |
| Modern async | `async/await`, no `.then()` chains |
| Good naming | Full words, except `id`, `url`, `ctx` |

## Audit existing file (surgical mode, both)

1. Count lines. Project >200 / user >150 → flag sections to trim.
2. Mark rules failing 5-second test, propose targeted rewrites.
3. Mark inlined content that belongs elsewhere (`docs/` for project; project-specific rules out of user-level).
4. Check for missing `Do NOT introduce` block (project) or `Don't` list (user).
5. Propose deletions BEFORE additions. Preserve voice and section ordering.

## Output check (both)

Print line count + 30-second test:
- Project-level: can a stranger answer "what is this / stack / where does new code go"?
- User-level: can a stranger answer "what does this person build / what do they default to / how do they communicate"?

If no, fix before declaring done.

## Omit, don't placeholder

If a skeleton section has no real content for this project/user, **drop the entire section**. Never write filler entries such as:
- "Hooks: none" / "Memory: N/A" / "No hooks configured"
- "Not a Python project" / "No framework" / "None applicable"
- Any entry whose value is "无", "none", "N/A", "—", or semantically equivalent

A shorter file with only relevant sections beats a "complete" file padded with empties.

## Refuse to add

- Project: "Project History" / "Mission" / "Values" sections
- User: personal manifestos, philosophy essays, biographical detail
- Both: architecture diagrams or ADR content inline; rules starting with "try to / consider / preferably / ideally"; tone instructions without a concrete example
