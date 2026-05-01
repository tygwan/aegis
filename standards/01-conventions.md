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

## Two files, identical content

aegis-conformant projects maintain **two** convention files at the repository root with **identical content**:

- `AGENTS.md` — Codex CLI's native convention file (and the canonical-by-convention source when editing).
- `CLAUDE.md` — Claude Code's native convention file.

Both files are committed. Both contain the full conventions verbatim. When one is edited, the other is updated in the same commit.

This pattern was chosen over a "thin pointer" approach (in which `CLAUDE.md` would redirect to `AGENTS.md`) because the pointer requires the agent to actively follow the redirect, which is not formally guaranteed for either CLI. Identical content means each agent's auto-loaded session context contains the full conventions directly, with no indirection. See [`docs/decisions/D-004`](../docs/decisions/D-004-agents-claude-md-full-sync.md) for the full reasoning.

Drift discipline:

- Edit `AGENTS.md` first by convention; immediately mirror to `CLAUDE.md`. (A future sync script or pre-commit hook may automate this — see [`adapters/mcp.md`](../adapters/mcp.md) for the M3+ plan.)
- A commit that modifies one without the other is a bug; either fix it before pushing or open a follow-up commit.
- For agents (human or AI) reading either file: assume the other contains identical content. If a discrepancy is detected, raise it as a problem to be reconciled, do not silently choose one.

## Orchestration — when to do what

Both `AGENTS.md` and `CLAUDE.md` should include this table inline, so an agent's auto-loaded session context contains the action map.

| Situation / what user says | Read | Use template | Produce |
|---|---|---|---|
| Day 1 / convention setup | [01](01-conventions.md) | `AGENTS.template.md` | `AGENTS.md` (= `CLAUDE.md`) |
| "X vs Y" / structural choice with non-trivial reversal cost | [02](02-decisions.md) | `D-record.template.md` | `docs/decisions/D-{NNN}-{slug}.md` |
| Before non-trivial work (>1 day or >3 sub-tasks) | [03](03-plans.md) | `Plan-record.template.md` | `docs/plans/P-{NNN}-{slug}.md` |
| End of meaningful work / "it's done" | [04](04-verification.md) | `V-record.template.md` | `docs/verifications/V-{NNN}-{slug}.md` |
| Routine commit | [05](05-changes.md) | (commit format) | git commit |
| Phase opens / "let's start M{N}" | [06](06-milestones.md) | `Milestone-README.template.md` | `docs/milestones/M{N}-{slug}/README.md` |
| Phase closes (Closed or Abandoned) | [06](06-milestones.md) + [07](07-learn-from-friction.md) | `Retrospective.template.md` | `docs/milestones/M{N}-{slug}/retrospective.md` |
| External narrative needed (portfolio) | [07](07-learn-from-friction.md) | `Portfolio-entry.template.md` | (project-specific location) |
| Same friction recurred | [07](07-learn-from-friction.md) | (memory entry or standards refinement) | memory file or D-record |

## Discipline reminders

Inlined in `AGENTS.md` / `CLAUDE.md` as the closing reminder block:

- A structural decision without a D-record is a future bug. Future-you, future contributors, and future agent sessions will reopen the same debate without context.
- A V-record's findings must either feed the next milestone or be explicitly dropped — never silently disappear. The closed-loop guarantee is what makes friction productive.
- `AGENTS.md` and `CLAUDE.md` must stay identical. Edit one, mirror to the other, in the same commit. See [D-004](../docs/decisions/D-004-agents-claude-md-full-sync.md).
- When two standards seem to conflict, propose a refinement via D-record; do not silently work around. The standards are short by design — conflicts are a signal one of them is wrong, not that one should be ignored.

## Anti-patterns

- **Treating `AGENTS.md` as a README.** README is for humans evaluating the project; AGENTS.md is for contributors (human or AI) actually working on it. Different audience, different content.
- **Letting conventions live in CLAUDE.md alone.** Codex agents will not see them, and the project loses dual-agent parity.
- **Listing every rule explicitly.** Reference the standards by link instead. AGENTS.md states *what applies*, not *what each rule says*.
- **Out-of-date out-of-scope.** When a milestone closes and the next begins, the "Out of scope" list usually changes. Update it in the same commit that opens the new milestone.

## Project-specific standards (extending aegis)

aegis's seven standards are deliberately universal — they apply to any project type. A given project will often need *additional* rules that go beyond aegis: API design conventions for a SaaS, library publication conventions for an npm package, ethical-AI checklists for a research project, GraphQL schema conventions, and so on.

These project-specific extensions live in **your project**, not in aegis:

- Place them at `docs/standards/local/{NN}-{slug}.md`. Number them however you like (`L1`, `local-01`, or simply `01-api-design.md`) — aegis is silent on the format, but consistency within your project matters.
- Reference them from `AGENTS.md` § Active conventions, alongside the aegis links. The reader should see one clean list.
- Apply the same authoring discipline aegis uses: state the rule's trigger, rationale, format, and anti-patterns. A standard worth writing is worth writing at the same quality bar as aegis's own.
- Maintain them with D-records. Adding, refining, or retiring a project-specific standard is itself a structural decision.

A project-specific standard that turns out to be useful elsewhere may eventually graduate — either by upstreaming a refined version into aegis (via PR), or by becoming a published profile that other projects can adopt independently. Most stay project-local forever; that is fine.

Anti-pattern: copying aegis's rules into your project's `local/` directory and editing them in place. That fork eventually drifts from the aegis canonical. Reference aegis's standards by link; only add *new* rules locally.

## Maintenance

`AGENTS.md` and aegis's records (`docs/decisions/`, `docs/verifications/`) must stay in sync. When a structural decision changes the project's working style, update `AGENTS.md` in the same commit as the D-record. The decision record explains why; AGENTS.md propagates the consequence.
