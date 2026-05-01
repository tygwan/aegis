# Adapter — Claude Code

How Claude Code (Anthropic's CLI and IDE extensions) reads and applies aegis standards.

## Entry points

Claude Code automatically reads two files at the project root:

- **`CLAUDE.md`** — Claude Code's native convention file.
- **`AGENTS.md`** — read if `CLAUDE.md` references it (Claude Code does not load `AGENTS.md` natively).

aegis convention: keep `AGENTS.md` as the single source of truth and let `CLAUDE.md` be a one-line pointer:

```markdown
# CLAUDE.md

This project's conventions live in [`AGENTS.md`](AGENTS.md).
```

Why this pattern: `AGENTS.md` is also the convention file for Codex CLI. By making it the canonical, both agents see the same rules. Without this, `CLAUDE.md` becomes the de facto canonical and Codex misses the conventions.

## Reading order

When Claude Code starts a session, it pulls:

1. `CLAUDE.md` (auto-loaded as system context)
2. `AGENTS.md` via the redirect
3. Whatever the agent then chooses to read based on the task

aegis-managed projects should ensure `AGENTS.md` § Reading order tells the agent what to read next (e.g. "read the most recent D-record before proposing structural changes").

## Auto-memory

Claude Code maintains a per-project persistent memory at `~/.claude/projects/{path-encoded-project-root}/memory/`. The agent automatically writes to this directory across sessions.

aegis's view of memory:

- Memory holds **durable facts** (user preferences, project gotchas).
- Memory does **not** hold lessons that should propagate (those go in standards / AGENTS.md / retrospectives).
- See [`standards/07-learn-from-friction.md`](../standards/07-learn-from-friction.md) for the memory-vs-retrospective distinction.

If the project uses Claude Code memory, `AGENTS.md` should explicitly point to its location so other agents know it exists (even if they cannot read it).

## Slash commands and skills (M5+)

Not used in M1. Reserved for the M5 milestone where each MCP tool gets a Claude Code slash command wrapper.

When that milestone arrives, slash commands will live at `~/.claude/commands/aegis/` (user-level) or `<project>/.claude/commands/` (project-level). Each will be a thin wrapper around an MCP tool from the aegis MCP server.

## Sub-agents (M5+)

Reserved. Likely candidates:

- `aegis-decision-drafter` — read project context and propose a D-record draft for a stated choice.
- `aegis-verifier` — execute a P-record's done criterion and produce a V-record draft.
- `aegis-retrospective-drafter` — aggregate a milestone's V-records into a retrospective draft.

Not before M5. Skill-level wrappers come first.

## Hooks (M5+, optional)

Reserved. Possible enforcement hooks:

- Pre-commit: warn if a structural-looking change has no associated D-record.
- Pre-merge: block if a milestone's `Closing checklist` (in its README) is incomplete.

aegis is intentionally cautious about hooks. They are added only after manual discipline has been validated, and only with an explicit D-record approving each one.

## Asymmetries with Codex

Where Claude Code can do something Codex cannot (e.g. richer skill ecosystem), the asymmetry is documented in [`codex.md`](codex.md) with the equivalent Codex workflow (often: invoke the MCP tool directly).

aegis-managed projects should not rely on Claude-Code-specific workflows for actions that need to work in both agents. When in doubt, keep the action driven from `AGENTS.md` + the MCP server, not from a Claude Code skill alone.
