# Adapter — Codex CLI

How OpenAI's Codex CLI reads and applies aegis standards.

## Entry points

Codex CLI reads `AGENTS.md` at the project root as its native convention file. There is no separate `CODEX.md` equivalent — `AGENTS.md` is canonical.

This makes Codex naturally aligned with aegis: the canonical aegis entry-point file is the file Codex already reads.

## Versioning

Codex CLI is a fast-moving target. aegis does not pin a Codex version — instead, we depend only on:

- `AGENTS.md` being read at session start (supported in all current Codex CLI versions known to us).
- MCP server invocation (supported in Codex CLI versions with MCP support enabled).

If you find a Codex feature missing in your environment, update Codex CLI; aegis assumes the most recent stable release.

## Reading order

Codex's session bootstrap:

1. `AGENTS.md` is auto-loaded.
2. Whatever else the user references or the task requires.

`AGENTS.md` § Reading order should explicitly list the next files to read (D-records, current milestone README, etc.) so Codex can follow the trail.

## Custom prompts and instructions

Codex supports custom prompts via its configuration directory (typically `~/.codex/` for user-level, project-local equivalents where supported).

In M1, aegis does not ship Codex prompts. M5 will add a `codex/` directory with prompt files for the same workflows that Claude Code gets as skills:

- `decision-draft` — invokes the MCP `draft_decision_record` tool.
- `plan-draft` — invokes `draft_plan_record`.
- `verification-draft` — invokes `draft_verification_record`.
- `retrospective-draft` — invokes `draft_retrospective`.

These are thin wrappers around MCP tools, identical in semantics to the Claude Code skills.

## MCP integration

Codex supports MCP servers as tool providers. The aegis MCP server (when M3 lands) will be configured in Codex's MCP config the same way as in Claude Code:

```json
{
  "mcpServers": {
    "aegis": {
      "command": "node",
      "args": ["{path-to-aegis}/packages/mcp-server/dist/server.js"]
    }
  }
}
```

The exact configuration syntax is whatever each agent uses; the MCP server itself is identical.

## Asymmetries with Claude Code

Codex's ergonomic layer (custom prompts, instructions) is less mature than Claude Code's skill system. Where Claude Code has a polished slash-command ergonomics, Codex may rely on direct MCP tool invocation or a longer prompt.

aegis's strategy:

- The **functional** workflow is the same in both agents (MCP server + AGENTS.md + records).
- The **ergonomic** wrapping may differ (slash command vs. saved prompt vs. direct tool call).

A project that wants identical UX in both agents should test each workflow in both before declaring it adopted.

## When Codex falls behind a Claude Code feature

If a workflow lands in Claude Code (skill or sub-agent) before Codex has an equivalent:

1. Document the gap in [`claude-code.md`](claude-code.md) and here.
2. Provide a Codex fallback (direct MCP call, copy-pasted prompt) so the workflow is still achievable, just less ergonomic.
3. Track the parity gap as a follow-up in the relevant milestone retrospective.
