# AGENTS.md — aegis

Conventions for any AI agent (Claude Code, Codex CLI, future agents) and any human contributor working on the **aegis repository itself**. aegis follows aegis — this file is the dogfood entry point.

> **This file is mirrored to `CLAUDE.md` with identical content** (per [D-004](docs/decisions/D-004-agents-claude-md-full-sync.md)). When you edit one, mirror the change to the other in the same commit.

If you are reading this and you are an AI agent: this is your project context. Treat it as authoritative.

## What aegis is

A markdown-first standards bundle that lets any software project bootstrap consistent AI-assisted development. The seven [`standards/`](standards/) files define the rules; the seven [`templates/`](templates/) define what users copy into their own projects.

aegis is intentionally small. Adding a rule, a template, or a tool requires a [decision record](docs/decisions/) explaining why. Removing one requires the same.

## Reading order

When starting a fresh session, read in this order:

1. `README.md` — vision, layout
2. This file (`AGENTS.md` or `CLAUDE.md` — they are identical)
3. `standards/README.md` — index of the seven standards
4. `docs/decisions/` — what was decided and why (read the most recent first)
5. The specific standard you need (e.g. `standards/02-decisions.md` before drafting a D-record)

<!-- aegis:orchestration:begin -->

## Orchestration — when to do what

Use this lookup before reaching for a standard or template. When in doubt, fall back to the linked standard.

| Situation / what user says | Read | Use template | Produce |
|---|---|---|---|
| Day 1 / convention setup | [01](standards/01-conventions.md) | `templates/AGENTS.template.md` | `AGENTS.md` (= `CLAUDE.md`) |
| "X vs Y" / structural choice with non-trivial reversal cost | [02](standards/02-decisions.md) | `templates/D-record.template.md` | `docs/decisions/D-{NNN}-{slug}.md` |
| Before non-trivial work (>1 day or >3 sub-tasks) | [03](standards/03-plans.md) | `templates/Plan-record.template.md` | `docs/plans/P-{NNN}-{slug}.md` |
| End of meaningful work / "it's done" | [04](standards/04-verification.md) | `templates/V-record.template.md` | `docs/verifications/V-{NNN}-{slug}.md` |
| Routine commit | [05](standards/05-changes.md) | (commit format) | git commit |
| Phase opens / "let's start M{N}" | [06](standards/06-milestones.md) | `templates/Milestone-README.template.md` | `docs/milestones/M{N}-{slug}/README.md` |
| Phase closes (Closed or Abandoned) | [06](standards/06-milestones.md) + [07](standards/07-learn-from-friction.md) | `templates/Retrospective.template.md` | `docs/milestones/M{N}-{slug}/retrospective.md` |
| External narrative needed (portfolio) | [07](standards/07-learn-from-friction.md) | `templates/Portfolio-entry.template.md` | (project-specific location) |
| Same friction recurred | [07](standards/07-learn-from-friction.md) | (memory entry or standards refinement) | memory file or D-record |

<!-- aegis:orchestration:end -->

## Active conventions

### Language

- All authored documents (markdown, code comments) are in **English** — aegis is a universal-adoption project and English is its lingua franca.
- Chat language follows the human collaborator's lead. Agents should match whichever language the user is conversing in; no preference is encoded into aegis itself.

### Records

- **Every structural decision** → `docs/decisions/D-{NNN}-{slug}.md`, per [`standards/02-decisions.md`](standards/02-decisions.md). No exceptions, even when the choice feels obvious.
- **Every non-trivial work unit** (> 1 day or > 3 sub-tasks) starts with a plan record per [`standards/03-plans.md`](standards/03-plans.md), and ends with a verification record per [`standards/04-verification.md`](standards/04-verification.md).
- **Milestone closure** produces a retrospective per [`standards/07-learn-from-friction.md`](standards/07-learn-from-friction.md).

### Commits and changes

Per [`standards/05-changes.md`](standards/05-changes.md):

- Commit message format: `type(scope): summary` — type ∈ {feat, fix, docs, refactor, chore, test}, scope is the area touched.
- Imperative mood ("add", not "added"). Body explains *why*, not *what*.
- One logical change per commit. Pre-existing structural decisions reference their D-record in the body.
- No mass refactors mixed with feature work. Either-or, not both.

### Working style

- **Markdown is the primary product.** Prose quality matters. Short, dense sentences. No filler. No emoji unless asked.
- **Universal applicability.** Every standard, every template must work for projects of any kind: web apps, CLIs, libraries, research code, design systems. Run a mental check before merging: "would this make sense for a Rust CLI? for a Jupyter notebook?" If not, refine until yes, or move it to `languages/` or a future profile.
- **Dual-agent parity.** A convention or automation that only works in Claude Code and not in Codex (or vice versa) is incomplete. Either bring both up to parity or document the asymmetry explicitly in [`adapters/`](adapters/).
- **No automation before validation.** Markdown layer first. Add an MCP tool, an agent skill, or a hook only after the manual workflow has been exercised at least once and the friction is concrete.

<!-- aegis:discipline:begin -->

## Discipline reminders

- A structural decision without a D-record is a future bug. Future-you, future contributors, and future agent sessions will reopen the same debate without context.
- A V-record's findings must either feed the next milestone or be explicitly dropped — never silently disappear. The closed-loop guarantee is what makes friction productive.
- `AGENTS.md` and `CLAUDE.md` must stay identical. Edit one, mirror to the other, in the same commit. See [D-004](docs/decisions/D-004-agents-claude-md-full-sync.md).
- When two standards seem to conflict, propose a refinement via D-record; do not silently work around. The standards are short by design — conflicts are a signal one of them is wrong, not that one should be ignored.

<!-- aegis:discipline:end -->

## When in doubt

- Decision: open [`standards/02-decisions.md`](standards/02-decisions.md), read the "when to write a D-record" section.
- Plan: same for [`standards/03-plans.md`](standards/03-plans.md).
- Verification: [`standards/04-verification.md`](standards/04-verification.md).
- Convention conflict (the rule contradicts itself or reality): write a D-record proposing the refinement; do not silently work around it.

## Out of scope (M1)

The following are intentionally absent from this milestone. Do not add them without a D-record promoting them in scope:

- MCP server implementation (deferred to M3).
- Claude Code skills, sub-agents, hooks (deferred to M5).
- Codex prompts, custom commands (deferred to M5).
- Language-specific guides beyond stub READMEs (deferred to M3+).
- Enforcement (commit-blocking hooks, etc.) — manual discipline only in M1-M2.
