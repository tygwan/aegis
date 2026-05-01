# Adapter — MCP

> **Status: reserved.** This adapter describes the planned MCP server for aegis. Implementation lands in M3.

## Why MCP

aegis targets multiple AI agents (Claude Code, Codex, future). Each agent has its own ergonomic surface (skills, prompts, sub-agents). Re-implementing the same automation in each agent's native format would mean writing every workflow twice (or more, as new agents arrive).

MCP solves this. An MCP server exposes tools that *any* MCP-capable agent can invoke. aegis's automation lives in one place; each agent gets a thin ergonomic wrapper that invokes those tools.

```
                        aegis MCP server
                              │
              ┌───────────────┼───────────────┐
        Claude Code        Codex CLI       future agent
        (slash command)    (custom prompt) (whatever)
```

## Planned tools (M3 onwards)

| Tool | Inputs | Output | Standard |
|---|---|---|---|
| `draft_decision_record` | context paragraph, optional list of options, project root path | D-record markdown draft | [02-decisions](../standards/02-decisions.md) |
| `draft_plan_record` | goal, scope, milestone, project root | P-record draft | [03-plans](../standards/03-plans.md) |
| `draft_verification_record` | plan number, observations, project root | V-record draft | [04-verification](../standards/04-verification.md) |
| `draft_retrospective` | milestone path, project root | retrospective draft | [07-learn-from-friction](../standards/07-learn-from-friction.md) |
| `scan_undocumented_decisions` | project root | list of recent commits with structural-looking changes that lack a D-record reference | [02-decisions](../standards/02-decisions.md) |
| `verify_milestone_closed_loop` | M{N} root path, project root | report on whether M{N+1} README absorbed M{N}'s next-milestone follow-ups | [06-milestones](../standards/06-milestones.md) |
| `milestone_status` | milestone path | summary: completed scope items, pending V-records, open D-records | [06-milestones](../standards/06-milestones.md) |

The exact list will likely shift during M3-M4 as we dogfood; the priority order is roughly the order above.

## Architecture

- **Language:** TypeScript.
- **Runtime:** Node 20+.
- **Location:** `packages/mcp-server/` in this repository.
- **Distribution:** invoked locally via `node packages/mcp-server/dist/server.js`. No remote server, no auth, no persistent state — the project's filesystem is the database.
- **Per-tool design:** each tool reads project files (markdown, git history) and emits structured output. Stateless, idempotent.

## Configuration in agents

Each agent registers the MCP server via its native MCP config. The server is the same; only the wrapper differs.

Claude Code:
```json
{
  "mcpServers": {
    "aegis": { "command": "node", "args": ["{path}/packages/mcp-server/dist/server.js"] }
  }
}
```

Codex CLI: same shape, in Codex's MCP config.

## Open questions (deferred to M3)

- Whether the server runs per-project or globally (one instance pointed at multiple projects).
- Whether to expose tools as a CLI in addition to MCP (e.g. `aegis decision draft` for shell-only workflows).
- Whether to pre-fetch git history in tool calls or rely on the agent to provide context.

These are deliberately deferred — answers should emerge from M3 implementation, not be decided up front.
