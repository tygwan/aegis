# Decision records (D-records)

Structural decisions made for the aegis project itself. aegis follows aegis: every structural choice has a D-record.

## Index

| # | Title | Status | Date |
|---|---|---|---|
| [D-001](D-001-project-charter.md) | Project charter | Accepted | 2026-05-02 |
| [D-002](D-002-architecture-monolith-dual-agent-mcp.md) | Architecture: monolith repo, dual-agent, MCP for automation | Accepted | 2026-05-02 |
| [D-003](D-003-five-to-seven-standards.md) | Standards scope: refine dev-standards 14 → 7 | Accepted | 2026-05-02 |

## How D-records are managed

- Numbered monotonically — never reused, never re-ordered.
- Status moves Proposed → Accepted → Superseded (never deleted).
- When superseded, the older record gains a `> **SUPERSEDED YYYY-MM-DD** by [D-NNN](...)` block at the top.
- See [`standards/02-decisions.md`](../../standards/02-decisions.md) for the full discipline.
