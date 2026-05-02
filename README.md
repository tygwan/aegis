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

## Who this is for

Solo developers and small teams using AI agents (Claude Code, Codex CLI, others) on substantive software work — where decisions accumulate, milestones repeat, and "what did we decide last month?" needs an answer that doesn't depend on chat scrollback.

It works for projects of any kind: web apps, CLI tools, libraries, mobile apps, research code, design systems, internal tooling. The seven standards are written to the universal-applicability constraint — none assume a stack, runtime, or domain.

**It is probably not for you if:**

- Your project is a one-week throwaway. The discipline pays back over months, not days.
- You already have a strong project-management methodology (Scrum, Shape Up, etc.) that records decisions and verifications another way. aegis is record-keeping, not process — it overlaps awkwardly with prescribed processes.
- You want a code-quality enforcer (linter, type checker). aegis does not check code; it captures *reasoning about* code.

## Installation

aegis is installed by an AI agent (Claude Code, Codex CLI, or similar) following a safe, detection-first procedure that never overwrites existing project content. In your target project, give your agent a prompt like:

> "Read https://github.com/tygwan/aegis/blob/main/INSTALLING.md and follow the installation procedure for this project."

The agent will:

1. **Detect** what already exists at the project root (`AGENTS.md`, `CLAUDE.md`, `docs/` aegis structure).
2. **Classify** the install state (greenfield vs. several brownfield variants).
3. **Plan** the safe set of changes and present it to you in markdown.
4. **Confirm** with you before any modification.
5. **Execute** only after explicit approval, using HTML-comment markers around aegis-managed sections so future updates touch only those regions.

Greenfield (empty) projects resolve in roughly 1 minute. Brownfield (in-flight) projects preserve all existing content and inject only marked sections.

For the full procedure, see [`INSTALLING.md`](INSTALLING.md). For the manual fallback (no AI agent), see that document's § Manual procedure.

## Adopting aegis on an existing project

Most projects do not adopt aegis on day 1 — they adopt it sometime later, when the project has accumulated decisions, code, and possibly chaos. Here is the path for an in-flight project:

1. **Write `AGENTS.md` first**, even before any record. Capture the project's current state honestly: stack, entry points, conventions you actually follow, conventions you wish you followed. This file is the new single source of truth — everything downstream points back to it.
2. **Do not retroactively reconstruct old decisions.** D-records written months after the fact are weak — the reasoning has decayed. Instead, write a single "current state" record (D-001) that says: "this is what we do today; the historical reasoning is partial." From there, every *new* structural decision gets its own D-record.
3. **Start P-records with the next non-trivial work**, not the last one. Plans are forward-looking; reconstructing past plans yields nothing.
4. **First V-record** at the next meaningful work unit's completion. Older work that was never verified can be left as-is, or batch-verified in a single V-record marked as a current-state audit.
5. **Define the next milestone (M1) starting today.** Earlier milestones, if any existed informally, do not need retroactive READMEs.
6. **Don't try to migrate everything in one commit.** Adoption is a slow rolling change — each new artifact follows aegis, old artifacts coexist, and the project's discipline visibly tightens over the next few weeks.

The discipline at the end of brownfield adoption looks identical to a greenfield project; the difference is only that older work isn't retrofitted.

## Repository layout

```
aegis/
├── README.md                    you are here
├── INSTALLING.md                agent-readable install procedure (safe, idempotent)
├── AGENTS.md                    aegis follows aegis (dogfood)
├── CLAUDE.md                    byte-identical to AGENTS.md (per D-004)
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
