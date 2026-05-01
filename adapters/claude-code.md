# Adapter — Claude Code

How Claude Code (Anthropic's CLI and IDE extensions) reads and applies aegis standards.

## Entry points

Claude Code automatically reads `CLAUDE.md` at the project root and loads its content into the session's system context. It does not auto-load `AGENTS.md`.

aegis convention (per [D-004](../docs/decisions/D-004-agents-claude-md-full-sync.md)): both `AGENTS.md` and `CLAUDE.md` exist at the project root with **identical content**. This is *not* a pointer pattern — both files contain the full conventions verbatim.

Why identical sync rather than pointer:

- A pointer (`CLAUDE.md` containing only "see AGENTS.md") relies on Claude Code actively following the redirect. That behavior is not formally guaranteed and varies across CLI versions and modes.
- Identical content means Claude Code's auto-load gives Claude the full aegis conventions directly. No indirection, no version-dependent behavior.
- Codex CLI auto-loads `AGENTS.md`, so it sees the same conventions from its own native file. Both agents — both files — same content.

Drift discipline:

- Edit `AGENTS.md` first by convention. Immediately mirror to `CLAUDE.md` in the same commit.
- A commit that touches one and not the other is incomplete.
- M3+ may add a sync script or a pre-commit hook; until then, the discipline is manual.

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
