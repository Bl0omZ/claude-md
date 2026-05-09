# CLAUDE.md Generator

A [Claude Code](https://claude.ai/code) skill for generating project-level and user-level `CLAUDE.md` files.

[中文版本](./README-zh.md)

## What it does

Generates or audits `CLAUDE.md` files at two scopes:

- **Project-level** (≤200 lines) — tech stack, coding rules, forbidden libraries, and working style defaults for a specific codebase
- **User-level** (≤150 lines) — personal defaults, tradeoff preferences, and communication style applied across all projects

## Principles

The generator follows four principles adapted from [Karpathy's Claude working style](https://karpathy.ai):

1. **Think Before Coding** — surface assumptions, present options, ask when unclear
2. **Simplicity First** — minimum code that solves the problem, no speculative abstractions
3. **Surgical Changes** — touch only what's needed, match existing style
4. **Goal-Driven Execution** — define success criteria and loop until verified

User-level generation references the Karpathy behavioral baseline (`references/karpathy-claude.md`), paraphrased to match the user's confirmed preferences.

## Structure

```
.
├── claude-md/              # English version
│   ├── SKILL.md
│   └── references/
│       └── karpathy-claude.md
├── claude-md-zh/           # Chinese version
│   ├── SKILL.md
│   └── references/
│       └── karpathy-claude.md
├── README.md
└── README-zh.md
```

## Usage

Invoke `claude-md` (English) or `claude-md-zh` (Chinese) in Claude Code to generate or audit a `CLAUDE.md` file.