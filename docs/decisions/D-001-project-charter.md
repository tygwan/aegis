# D-001: Project charter

| | |
|---|---|
| **Status** | Accepted |
| **Date** | 2026-05-02 |
| **Deciders** | coffin |
| **Supersedes** | — |
| **Superseded by** | — |

## Context

aegis is a new project, evolving from the existing `dev-standards` repository. Before scope creep starts, the charter needs to be explicit so that future contributions have something to measure against.

The existing `dev-standards` repository is solid but has accumulated rules of varying universality (some assume web-stack, some assume mature SaaS, some are research-specific). It also predates the dual-agent (Claude Code + Codex) and MCP-based automation possibilities. A clean restart with explicit scope serves better than retrofitting.

## Options considered

### Option A — Refactor `dev-standards` in place

Keep working in the existing repo. Refine rules, drop or rewrite the universally-misapplied ones, layer MCP automation on top.

**Pros**
- No new repo, no migration overhead.
- Existing rules and history preserved.

**Cons**
- 14 core rules, 4 profiles, 7 adapters — many of which are not aligned with the new direction. Refactoring in place blurs what is "current intent" vs "legacy".
- Audience confusion: anyone watching the repo sees a long churn of moves and rewrites instead of a clean intent statement.
- "Refactor" framing implies preservation; clean break framing forces hard choices.

### Option B — New repository (`aegis`)

Start a fresh repository. Cherry-pick the rules that survive scrutiny. Build the agent-aware layer (dual-agent adapters, MCP server scaffold) from scratch.

**Pros**
- Clean intent — the repo's first commit states what it is.
- Forces hard decisions (drop vs. keep vs. refine) instead of accumulating.
- Naming opportunity: the new name ("aegis") frames the value proposition better than "dev-standards" did.

**Cons**
- New repo has zero adoption history. Existing dev-standards consumers need migration.
- Lose the literal git history of dev-standards (mitigated by linking from aegis README).

### Option C — `aegis` as a successor repo, dev-standards archived

Same as B, but explicitly archive `dev-standards` and point its README at aegis.

**Pros / Cons:** Inherits B; adds clear deprecation signal, costs one extra README change in dev-standards.

## Decision

**Option C.** New repository named `aegis`; existing `dev-standards` is archived (its README to point at aegis once aegis M1 is published). The new repo carries forward refined rules, adds dual-agent and MCP scaffolding from scratch, and is the canonical project standards system going forward.

## Consequences

- **Unblocks:**
  - Clean structure for the dual-agent + MCP architecture (see D-002).
  - Hard decisions on which rules survive (see D-003).
  - Distinct identity for this evolution; "aegis" is brandable in a way "dev-standards" is not.
- **Locks in:**
  - Project name `aegis`. Renaming later is expensive; we accept this.
  - MIT license, TypeScript-first automation (see D-002).
  - Universal-applicability constraint for every standard and template — projects of any type must be able to adopt.
- **Cost:**
  - Migration work for existing dev-standards consumers.
  - One-time content authoring of all M1 markdown.
- **Open questions** (to be resolved by later D-records):
  - Whether profiles ever reappear, and in what form.
  - Whether language-specific guides live in `languages/` or graduate to separate repos.

## Project charter

Captured here, not in a separate file, because charter elements are themselves subject to revision-by-D-record:

- **Mission.** Make AI-assisted software development consistent, durable, and dual-agent-capable through markdown-first conventions and an optional automation layer.
- **Target audience.** Solo developers and small teams using AI agents (Claude Code, Codex CLI, others) for substantive software work, on projects of any kind.
- **Success criteria for M1.** A new project can adopt aegis (copy templates, write `AGENTS.md`) in under 30 minutes and have both Claude Code and Codex follow the same conventions.
- **Non-goals.**
  - Not a project management methodology (Scrum, Shape Up, etc.) — aegis is record-keeping discipline, not process prescription.
  - Not a code quality enforcer (linters, formatters, type checkers handle that).
  - Not opinionated about stack, language, or architecture — universal applicability is a hard constraint.
  - Not a Claude-Code-only system — Codex parity is required.

## References

- Predecessor: dev-standards repository (to be archived).
- Related: [D-002](D-002-architecture-monolith-dual-agent-mcp.md) for architecture, [D-003](D-003-five-to-seven-standards.md) for standards scope.
