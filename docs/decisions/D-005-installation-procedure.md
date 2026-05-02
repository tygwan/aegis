# D-005: Installation procedure — agent-driven, idempotent, never destructive

| | |
|---|---|
| **Status** | Accepted |
| **Date** | 2026-05-02 |
| **Deciders** | coffin |
| **Supersedes** | (refines) D-001 quick-start guidance, where install was implied as direct cp |
| **Superseded by** | — |

## Context

Initial M1 documentation described aegis installation as a sequence of `cp` operations from `templates/` into the user's project root. For empty projects this is safe; for projects already in flight, it is catastrophic. Existing `AGENTS.md`, `CLAUDE.md`, and `docs/decisions/` files may already contain weeks or months of project content that direct `cp` would destroy.

For aegis to be safely adoptable on any project — the universal-applicability constraint of [D-001](D-001-project-charter.md) — the installation must satisfy three properties:

1. **Detection-first.** Know what already exists before acting.
2. **Confirmation-gated.** No destructive action without explicit user OK.
3. **Idempotent.** Running install twice produces the same end state, not duplicate sections.

## Options considered

### Option A — Shell install script

Ship `install.sh` (or `install.ps1`) that performs `cp` and merges.

**Pros**
- Familiar UX.

**Cons**
- Cross-platform fragility (POSIX shell vs PowerShell vs Windows Git Bash).
- Limited UX for showing diffs and asking interactive questions.
- Adds tooling dependency to a project that explicitly chose markdown-only foundation in M1.

### Option B — Agent-driven markdown procedure

aegis ships an `INSTALLING.md` file that an AI agent (Claude Code, Codex CLI, etc.) reads and executes. The procedure has explicit phases: Detect → Classify → Plan → Confirm → Execute.

**Pros**
- Cross-platform — the agent abstracts the platform.
- Markdown renders nicely; the install procedure is also human-readable documentation.
- Confirmation gates are natural in a chat interface.
- Reuses the dual-agent foundation (D-002): both Claude Code and Codex can execute the same procedure.
- Zero tooling. Just a markdown file.

**Cons**
- Requires an AI agent (which aegis already targets, so this is not a new dependency).
- Users without an agent need a manual fallback (which the procedure document includes).

### Option C — Submodule / dependency

aegis lives as a subdirectory of the user's project; their `AGENTS.md` references aegis files via relative paths. Install = `git submodule add`.

**Pros**
- No content copy. Updates via `git submodule update`.

**Cons**
- Submodule UX is notoriously rough, especially for non-experts.
- The user's `AGENTS.md` still needs to be created somehow.
- Couples the project to the aegis repo at runtime; loses independence.

### Option D — Option B + idempotent merge via HTML-comment markers

Same as B, plus an explicit marker convention around aegis-managed sections in the user's `AGENTS.md`/`CLAUDE.md`:

```markdown
<!-- aegis:orchestration:begin -->
... aegis-canonical content ...
<!-- aegis:orchestration:end -->
```

Markers enable:
- **Idempotent updates**: re-running install replaces only the marker regions, never project-specific content outside.
- **Mechanical detection**: tooling (and careful agents) can find aegis sections programmatically.
- **Visual invisibility**: HTML comments do not render in markdown viewers.

**Pros**
- All of B, plus update-safety and tooling-friendliness.

**Cons**
- Slightly more design upfront. Marker syntax must be specified and agents must respect it.

## Decision

**Option D.** aegis ships `INSTALLING.md` as the canonical install procedure. AI agents read and follow it. The procedure injects aegis-managed sections wrapped in HTML-comment markers; subsequent installs and updates touch only marker regions. A manual fallback procedure for users without an agent is included in the same document.

## Consequences

- **Unblocks:**
  - "Just give me the GitHub link" UX: a user prompts their agent with the aegis URL, the agent reads `INSTALLING.md`, performs safe install.
  - Brownfield adoption is safe by default — preserving project-specific content is the procedure's responsibility, not the user's.
  - Updates are idempotent; aegis content can evolve without disturbing user content.
- **Locks in:**
  - `INSTALLING.md` as the long-lived install specification.
  - Marker syntax (`<!-- aegis:NAME:begin -->` / `<!-- aegis:NAME:end -->`). Stable for the foreseeable future; future evolution may add a version field but the syntax shape stays.
- **Cost:**
  - `INSTALLING.md` must be maintained alongside template and standard changes — they stay synchronized.
  - Marker discipline: every aegis-injected section must be marker-wrapped; aegis's own `AGENTS.md`/`CLAUDE.md` and templates must contain markers too (dogfood).
- **Open questions:**
  - Whether to ship an MCP tool for install in M3+. Likely yes — `aegis_install` would invoke the same procedure programmatically. Deferred until M3.
  - Whether markers should carry a version field (`v=1`). Deferred — single-version assumption is sufficient for M1-M2.

## References

- Charter: [D-001](D-001-project-charter.md)
- Architecture: [D-002](D-002-architecture-monolith-dual-agent-mcp.md)
- Two-file sync: [D-004](D-004-agents-claude-md-full-sync.md)
- Procedure document: [`INSTALLING.md`](../../INSTALLING.md)
- Marker semantics: [`standards/01-conventions.md`](../../standards/01-conventions.md) § Marker semantics
