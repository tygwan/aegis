# AGENTS.md — {project name}

> Conventions for any AI agent (Claude Code, Codex CLI, future agents) and any human contributor working on this project. **Remove this blockquote after filling in.**
>
> See [`aegis/standards/01-conventions.md`](https://github.com/tygwan/aegis) for what this file must contain.

## Project context

{One paragraph: what this project is, who it serves, current stage. State enough that a fresh agent session can orient itself in 30 seconds.}

## Stack

- **Language(s):** {e.g. TypeScript, Python}
- **Runtime / framework:** {e.g. Node 20, Next.js 15, Django 5}
- **Package manager:** {e.g. pnpm 10, uv}
- **Persistence / infra:** {e.g. Postgres 16, Redis, AWS Seoul}
- **Test framework:** {e.g. vitest, pytest}

## Reading order

When starting a fresh session, read in this order:

1. This file (`AGENTS.md`)
2. `README.md`
3. `docs/decisions/` — the active D-records, especially the most recent
4. {project-specific: e.g. the architecture spec, the current milestone README}

## Active conventions

### Language

- Chat collaboration: {e.g. Korean, English}
- Authored documents (markdown, code comments): {e.g. English}

### Records

This project follows aegis standards:

- Structural decisions → `docs/decisions/D-{NNN}-{slug}.md` (per [aegis 02-decisions](https://github.com/tygwan/aegis))
- Non-trivial work units → plan record at `docs/plans/P-{NNN}-{slug}.md` (per [aegis 03-plans](https://github.com/tygwan/aegis))
- Verified work → `docs/verifications/V-{NNN}-{slug}.md` (per [aegis 04-verification](https://github.com/tygwan/aegis))
- Milestone closures → retrospective at `docs/milestones/M{N}-{slug}/retrospective.md` (per [aegis 07-learn-from-friction](https://github.com/tygwan/aegis))

### Commits

Per [aegis 05-changes](https://github.com/tygwan/aegis):

- Format: `type(scope): summary`
- Imperative mood ("add", not "added")
- One logical change per commit
- Body explains *why*, references related D/P/V records

## Working style

- {Pacing preference: e.g. "for foundation decisions, take time and ground choices in business constraints; for execution after rules are set, work briskly without per-step option prompts"}
- {Auto-OK actions: e.g. "editing files, running tests, refactoring within a file"}
- {Confirmation-required actions: e.g. "destructive git operations, dependency upgrades, deploys"}

## Out of scope (current milestone)

{List of items deliberately deferred. Update this list at every milestone open.}

- {item 1}
- {item 2}

## Memory

{Optional: pointer to project-specific persistent memory if used.}

- {e.g. `~/.claude/projects/-path-to-project/memory/` for Claude Code auto-memory}
