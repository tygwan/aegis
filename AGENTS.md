# AGENTS.md — aegis

Conventions for any AI agent (Claude Code, Codex CLI, future agents) working on the **aegis repository itself**. aegis follows aegis — this file is the dogfood entry point.

If you are reading this and you are an AI agent: this is your project context. Treat it as authoritative.

## What aegis is

A markdown-first standards bundle that lets any software project bootstrap consistent AI-assisted development. The seven [`standards/`](standards/) files define the rules; the seven [`templates/`](templates/) define what users copy into their own projects.

aegis is intentionally small. Adding a rule, a template, or a tool requires a [decision record](docs/decisions/) explaining why. Removing one requires the same.

## Reading order

When starting a fresh session, read in this order:

1. [`README.md`](README.md) — vision, layout
2. This file (`AGENTS.md`) — active conventions
3. [`standards/README.md`](standards/README.md) — index of the seven standards
4. [`docs/decisions/`](docs/decisions/) — what was decided and why
5. The specific standard you need (e.g. `standards/02-decisions.md` before drafting a D-record)

## Active conventions

### Language

- Internal collaboration in chat: **Korean** (project owner's preference).
- All authored documents (markdown, code comments): **English**, unless a document is explicitly marked Korean for portfolio/external-narrative purposes.

### Records

- **Every structural decision** → [`docs/decisions/D-{N}-{slug}.md`](docs/decisions/), per [`standards/02-decisions.md`](standards/02-decisions.md). No exceptions, even when the choice feels obvious.
- **Every non-trivial work unit** (> 1 day or > 3 sub-tasks) starts with a plan record, per [`standards/03-plans.md`](standards/03-plans.md), and ends with a verification record, per [`standards/04-verification.md`](standards/04-verification.md).
- **Milestone closure** produces a retrospective, per [`standards/07-learn-from-friction.md`](standards/07-learn-from-friction.md).

### Commits and changes

Per [`standards/05-changes.md`](standards/05-changes.md):

- Commit message format: `type(scope): summary` — type ∈ {feat, fix, docs, refactor, chore, test}, scope is the area touched.
- Imperative mood ("add", not "added"). Body explains _why_, not _what_.
- One logical change per commit. Pre-existing structural decisions reference their D-record in the body.
- No mass refactors mixed with feature work. Either-or, not both.

### Working style

- **Markdown is the primary product.** Prose quality matters. Short, dense sentences. No filler. No emoji unless asked.
- **Universal applicability.** Every standard, every template must work for projects of any kind: web apps, CLIs, libraries, research code, design systems. Run a mental check before merging: "would this make sense for a Rust CLI? for a Jupyter notebook?" If not, refine until yes, or move it to `languages/` or a future profile.
- **Dual-agent parity.** A convention or automation that only works in Claude Code and not in Codex (or vice versa) is incomplete. Either bring both up to parity or document the asymmetry explicitly in [`adapters/`](adapters/).
- **No automation before validation.** Markdown layer first. Add an MCP tool, an agent skill, or a hook only after the manual workflow has been exercised at least once and the friction is concrete.

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
