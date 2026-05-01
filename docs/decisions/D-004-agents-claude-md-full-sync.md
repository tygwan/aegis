# D-004: AGENTS.md and CLAUDE.md kept fully synchronized (not pointer-redirected)

| | |
|---|---|
| **Status** | Accepted |
| **Date** | 2026-05-02 |
| **Deciders** | coffin |
| **Supersedes** | (revises) D-002 § per-agent ergonomics, where the pointer pattern was implied |
| **Superseded by** | — |

## Context

aegis targets dual-agent operation: Claude Code reads `CLAUDE.md`, Codex CLI reads `AGENTS.md`. Both are auto-loaded into the agent's session context.

The initial M1 design implied a "thin pointer" pattern — `CLAUDE.md` containing only a one-line redirect to `AGENTS.md`. The reasoning was DRY: a single source of truth avoids drift between two synchronized files.

Scenario review surfaced a reliability concern. When Claude Code auto-loads `CLAUDE.md` and that file contains only a pointer, **the agent must explicitly choose to read `AGENTS.md` to get the actual conventions**. In practice, well-instructed agents do this, but it is not a guarantee. The conventions live behind an indirection.

For aegis to be reliable across PCs, sessions, and operators (who may not be present to verify the agent's behavior), conventions must be visible to both agents directly, not via redirection.

## Options considered

### Option A — Thin pointer (`CLAUDE.md` redirects to `AGENTS.md`)

**Pros**
- Single source of truth; zero drift risk.
- Smaller `CLAUDE.md`.

**Cons**
- Relies on the agent following the pointer. Not formally guaranteed.
- A new contributor opening `CLAUDE.md` sees nothing useful — must navigate.
- For Claude Code specifically, the auto-loaded context is just "see other file" — the conventions are absent from the system prompt at session start.

### Option B — Full synchronization (`CLAUDE.md` and `AGENTS.md` are identical)

**Pros**
- Both agents auto-load the **full** conventions directly.
- No reliance on agent-specific redirect-following behavior.
- Resilient to future changes in either CLI's auto-load semantics.

**Cons**
- Two files to keep in sync; drift risk if the discipline lapses.

### Option C — One file, the other is a symlink

**Pros**
- True single source.

**Cons**
- Cross-platform fragility: Windows symlinks require admin rights or developer mode; git's `core.symlinks` is platform-dependent. aegis must work on any PC out of the box.

### Option D — Full sync via build/copy script

Two files, but `CLAUDE.md` is generated from `AGENTS.md` by a script (e.g. `scripts/sync-agents.sh`).

**Pros**
- Sync guaranteed when the script runs.
- Cross-platform (a small Node.js script works everywhere).

**Cons**
- Extra moving piece; users must remember to run it. Adds tooling for an M1 project that has chosen markdown-only foundation.

## Decision

**Option B.** `AGENTS.md` and `CLAUDE.md` carry identical content. Both files are committed; one of them — `AGENTS.md` — is the authoring file by convention, and any change is reflected in both files in the same commit.

For M1, sync is a manual discipline. For M3+, a sync script (Option D mechanic) may be added as a convenience, but the convention-of-record stays "both files contain identical content".

## Consequences

- **Unblocks:**
  - Claude Code's auto-loaded session context contains the full aegis conventions, not a pointer to them.
  - Codex's auto-loaded context contains the same.
  - Both agents are demonstrably equivalent — no per-agent instruction asymmetry baked into the file structure.
- **Locks in:**
  - Two-file maintenance discipline. Every change to conventions touches both files.
  - The `AGENTS.md` is canonical-by-convention; `CLAUDE.md` is the synchronized copy. Any tooling treats `AGENTS.md` as the source.
- **Cost:**
  - Drift risk if the discipline is forgotten. Mitigated by:
    - Stating the convention in [`standards/01-conventions.md`](../../standards/01-conventions.md).
    - Noting it in `AGENTS.md` itself (so any agent reading either file is reminded).
    - Optional: a future pre-commit hook or sync script (M3+).
- **Open questions:**
  - Whether to ship a sync script in M3 — likely yes, but M1 ships without.
  - Whether other agent CLIs that emerge later use yet another filename. If so, this decision generalizes: every native-load file at root has identical content.

## References

- Charter: [D-001](D-001-project-charter.md)
- Architecture: [D-002](D-002-architecture-monolith-dual-agent-mcp.md)
- Standard touched: [`standards/01-conventions.md`](../../standards/01-conventions.md)
- Adapter touched: [`adapters/claude-code.md`](../../adapters/claude-code.md)
