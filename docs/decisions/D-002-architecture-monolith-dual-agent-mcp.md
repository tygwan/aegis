# D-002: Architecture — monolith repo, dual-agent, MCP for automation

| | |
|---|---|
| **Status** | Accepted |
| **Date** | 2026-05-02 |
| **Deciders** | coffin |
| **Supersedes** | — |
| **Superseded by** | — |

## Context

[D-001](D-001-project-charter.md) sets aegis's mission and the universal-applicability constraint. This decision answers the architectural questions that follow:

1. Should the standards markdown, the templates, and the automation server live in one repository or multiple?
2. How does dual-agent (Claude Code + Codex CLI) support get implemented? Per-agent re-implementation, or shared mechanism?
3. What automation distribution mechanism is used?

These are interlocking questions and benefit from a single decision.

## Options considered

### Option A — Monolith repo, per-agent automation

Single repository containing standards markdown, templates, and per-agent automation (Claude Code skills + Codex prompts, each implementing the same workflows separately).

**Pros**
- Simple distribution (one repo).
- Each agent gets the most native ergonomics.

**Cons**
- Every workflow written twice (or more, as new agents arrive).
- Drift between agents inevitable as one side gets fixed and the other lags.
- Adding a new agent ecosystem requires reimplementing everything.

### Option B — Split repos, shared markdown, per-agent automation

`aegis-standards` (markdown only) + `aegis-claude` + `aegis-codex` repositories. Standards stay platform-agnostic; agents pull standards as a dependency.

**Pros**
- Clear separation; `aegis-standards` truly platform-agnostic.

**Cons**
- Three repos to maintain. Cross-cutting changes touch all three.
- Adoption is harder (users install three things).
- Duplication of automation across `aegis-claude` and `aegis-codex` — same problem as Option A, just in separate repos.

### Option C — Monolith repo, MCP-based automation

Single repository. Standards as markdown. Automation as an MCP server (Model Context Protocol — a cross-platform tool-calling standard supported by both Claude Code and Codex). Each agent gets a thin wrapper that invokes MCP tools.

**Pros**
- Each automation is written once. Both agents (and any future MCP-capable agent) get it.
- Single repo for distribution.
- Standards stay markdown — readable by any AI agent or human, even without MCP.
- Adding a new agent ecosystem: write a new thin wrapper, the underlying tools already work.

**Cons**
- MCP server is a TypeScript codebase to maintain — heavier than pure markdown.
- MCP requires specific agent versions (Codex CLI must support MCP, which it does in current versions).

### Option D — Monolith repo, MCP-based automation, agent-specific wrappers optional

Same as C, but with explicit room for per-agent ergonomics where MCP alone is too clunky. The MCP tools are the canonical workflow; per-agent skills (Claude) and prompts (Codex) are optional polish layered on top.

**Pros / Cons:** Same as C plus: degrades gracefully when an agent's wrapper is missing — direct MCP invocation still works.

## Decision

**Option D.** Single repository (`aegis`), markdown-first standards, MCP server for automation, per-agent ergonomic wrappers as optional polish.

Concrete implications:

- Repository structure: standards/, templates/, adapters/, docs/, and a single `packages/mcp-server/` for automation. No separate per-agent repos.
- Standards markdown is **agent-agnostic** — readable by any AI or human, no agent-specific terminology.
- Automation lives in **one MCP server**. Claude Code and Codex each invoke the same tools.
- Per-agent wrappers (`adapters/claude-code.md`, `adapters/codex.md`) are documentation in M1, executable artifacts (skills, prompts) in M5+.
- TypeScript / Node 20+ for the MCP server (consistent with other current ecosystem projects).

## Consequences

- **Unblocks:**
  - Both agents see the same conventions from day 1 of any aegis-managed project.
  - One implementation site for each automation — no per-agent drift.
  - New agents (a future Cursor mode, a different OpenAI tier) can adopt aegis cheaply by adding a wrapper.
- **Locks in:**
  - MCP as automation transport. If MCP support regresses in either agent, we have a problem (acceptable risk; both providers are committed to MCP).
  - TypeScript for the MCP server. Reasonable for the audience and ecosystem; revisit if it becomes a barrier.
  - Single-repo distribution. Splitting later (extracting `mcp-server/` into a separate package) is feasible but not free.
- **Cost:**
  - Higher upfront work for the MCP server compared to skill-only automation.
  - Documentation discipline in `adapters/` to maintain dual-agent parity.
- **Open questions:**
  - Whether the MCP server should also expose a CLI interface for shell-only use cases. Deferred to M3.
  - How language-specific automation (TypeScript-aware tools, Python-aware tools) is structured. Deferred until concrete need arises.

## References

- Charter: [D-001](D-001-project-charter.md)
- MCP details: [`adapters/mcp.md`](../../adapters/mcp.md)
- Per-agent details: [`adapters/claude-code.md`](../../adapters/claude-code.md), [`adapters/codex.md`](../../adapters/codex.md)
