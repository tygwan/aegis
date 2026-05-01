# 01 — Conventions

Every aegis-managed project has a single canonical conventions file at the repository root: **`AGENTS.md`**. This file is the entry point both AI agents and human contributors read first.

## Trigger

Day 1 of any project. Before any other aegis artifact is written, `AGENTS.md` exists.

## Rationale

Without one canonical file, conventions live in tribal knowledge: in chat scrollback, in inconsistent README sections, in the heads of one or two people. AI agents starting fresh sessions cannot recover that. A new contributor cannot recover that. Six weeks from now, the original author cannot recover that.

`AGENTS.md` is the durable, single-source artifact that solves this. Everything else in aegis assumes it exists.

## Required sections

An aegis-conformant `AGENTS.md` has at minimum:

1. **Project context** — one paragraph: what this project is, who it serves, current stage.
2. **Stack** — languages, frameworks, package manager, runtime versions. One line per layer.
3. **Reading order** — the first 3-5 files a new agent or contributor should read, in order.
4. **Active conventions** — language for chat vs documents; commit format reference; record disciplines (link to `standards/02-decisions.md`, `04-verification.md`, etc.).
5. **Working style** — pacing preferences, what the agent should ask before doing, what is auto-OK to do.
6. **Out of scope** — explicit non-goals for the current milestone or version. Equally important to in-scope.

## Optional but encouraged

- **Pacing rules** — when to ask, when to proceed.
- **Memory pointers** — paths to project-specific persistent memory (e.g. `~/.claude/projects/.../memory/`), if used.
- **Decision discipline summary** — a one-paragraph reminder that structural choices need a D-record.
- **Forthcoming spec list** — work that is required before specific future milestones can start.

## Format

`AGENTS.md` is markdown. It is **agent-agnostic**: Claude Code, Codex CLI, and any other AI agent reading it must derive the same behaviour. If a section depends on a specific agent feature, mark it with `[claude-code]` or `[codex]` and document the equivalent in the other agent's section.

A `CLAUDE.md` file may exist at root. Its only purpose is to point at `AGENTS.md` (Claude Code reads `CLAUDE.md` by default; Codex reads `AGENTS.md` natively). It must not contain conventions of its own — that creates drift.

## Anti-patterns

- **Treating `AGENTS.md` as a README.** README is for humans evaluating the project; AGENTS.md is for contributors (human or AI) actually working on it. Different audience, different content.
- **Letting conventions live in CLAUDE.md alone.** Codex agents will not see them, and the project loses dual-agent parity.
- **Listing every rule explicitly.** Reference the standards by link instead. AGENTS.md states *what applies*, not *what each rule says*.
- **Out-of-date out-of-scope.** When a milestone closes and the next begins, the "Out of scope" list usually changes. Update it in the same commit that opens the new milestone.

## Maintenance

`AGENTS.md` and aegis's records (`docs/decisions/`, `docs/verifications/`) must stay in sync. When a structural decision changes the project's working style, update `AGENTS.md` in the same commit as the D-record. The decision record explains why; AGENTS.md propagates the consequence.
