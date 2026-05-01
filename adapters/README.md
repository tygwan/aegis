# Adapters

Per-agent application notes. The [`standards/`](../standards/) are agent-agnostic; adapters describe how a specific agent or tool reads and applies them.

## Index

| Adapter | Audience | Status |
|---|---|---|
| [`claude-code.md`](claude-code.md) | Claude Code (Anthropic CLI / IDE extensions) | M1 — markdown-only |
| [`codex.md`](codex.md) | Codex CLI (OpenAI) | M1 — markdown-only |
| [`mcp.md`](mcp.md) | Any MCP-capable agent | reserved (M3) |

## Authoring rules

- An adapter describes **how** an agent applies aegis. It must not introduce new conventions; new rules go in `standards/`.
- When two adapters describe the same workflow differently, the asymmetry must be documented (which agent supports the feature, which does not, and what the workaround is).
- Adapters are short. If an adapter grows beyond ~200 lines, factor parts into a profile or a separate guide.
