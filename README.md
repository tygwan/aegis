# aegis

> Universal project standards for AI-assisted software development. One markdown bundle, two AI agents (Claude Code + Codex CLI), any project type.

## What this is

A small, opinionated set of conventions and templates that lets any software project — web app, CLI tool, library, research code, anything — get consistent, high-quality help from AI agents (and humans). It produces durable records of what was decided, what was planned, what was verified, and what was learned, so that knowledge does not evaporate between sessions or between people.

The system is **markdown-first**. The 80% case is solved by templates and conventions you copy into your project. Automation (an MCP server, agent skills, hooks) is layered on top later, where it accelerates real workflows that have already been validated by hand.

## Why it exists

AI-assisted development hits the same six pains over and over:

1. Every new agent session relearns the project from scratch.
2. Past decisions vanish — the next change reopens the same debate.
3. Conventions drift; the agent does what feels reasonable, not what the project requires.
4. Failed work recurs because the failure was never recorded.
5. The git history is rich but the portfolio narrative is missing.
6. Two agents on the same project (Claude, Codex) behave differently.

aegis addresses all six with markdown that both agents read identically, plus a deferred automation layer.

## Quick start (5 minutes)

In your project root:

1. Copy `templates/AGENTS.template.md` → `AGENTS.md`. Fill in three fields: project name, stack, primary entry points.
2. Copy `templates/D-record.template.md` → `docs/decisions/D-001-{slug}.md` whenever you make a structural decision.
3. Copy `templates/V-record.template.md` → `docs/verifications/V-001-{slug}.md` after every meaningful unit of work.
4. Add a 1-line `CLAUDE.md` pointing to `AGENTS.md` (Claude Code reads CLAUDE.md by default; Codex reads AGENTS.md natively).

That is enough to bootstrap. Read the seven `standards/*.md` files when a question arises about how to apply the conventions; they are short and self-contained.

## Repository layout

```
aegis/
├── README.md                    you are here
├── AGENTS.md                    aegis follows aegis (dogfood)
├── CLAUDE.md                    1-line pointer to AGENTS.md
├── LICENSE                      MIT
│
├── standards/                   the seven rules
│   ├── 01-conventions.md
│   ├── 02-decisions.md
│   ├── 03-plans.md
│   ├── 04-verification.md
│   ├── 05-changes.md
│   ├── 06-milestones.md
│   └── 07-learn-from-friction.md
│
├── templates/                   what you copy into your project
│   ├── AGENTS.template.md
│   ├── D-record.template.md
│   ├── Plan-record.template.md
│   ├── V-record.template.md
│   ├── Milestone-README.template.md
│   ├── Retrospective.template.md
│   └── Portfolio-entry.template.md
│
├── adapters/                    per-agent application notes
│   ├── claude-code.md
│   ├── codex.md
│   └── mcp.md                   reserved for M3 (MCP server)
│
├── docs/                        aegis's own records
│   ├── decisions/
│   ├── verifications/
│   └── retrospectives/
│
├── languages/                   language-specific guides (M3+)
└── packages/                    workspace for MCP server (M3+)
```

## Status

This is **M1** (markdown foundation). No MCP server, no agent skills, no enforcement hooks yet. Markdown only.

The markdown layer alone delivers most of the value. M3 (MCP server) and M5 (agent-native ergonomics) extend it with automation but are not prerequisites.

See [`docs/decisions/`](docs/decisions/) for the architectural choices, and [`AGENTS.md`](AGENTS.md) for the active conventions on this repo.

## License

MIT. See [LICENSE](LICENSE).
