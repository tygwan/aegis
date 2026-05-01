# Standards

Seven rules. Each is universal — applicable to any project type, any language, any team size from solo to small group. Each is short.

## Index

| # | Standard | What it governs | When you need it |
|---|---|---|---|
| 01 | [conventions](01-conventions.md) | What an `AGENTS.md` must contain | Day 1 of any project |
| 02 | [decisions](02-decisions.md) | D-record discipline (structural choices) | When the project has more than one viable path |
| 03 | [plans](03-plans.md) | Plan record (P-record) discipline | When a unit of work spans more than ~1 day or 3 sub-tasks |
| 04 | [verification](04-verification.md) | V-record discipline (post-work) | At the end of every meaningful work unit |
| 05 | [changes](05-changes.md) | Commit / branch / migration discipline | Continuously |
| 06 | [milestones](06-milestones.md) | Milestone definition, lifecycle, closed-loop | When work groups into phases |
| 07 | [learn-from-friction](07-learn-from-friction.md) | Retrospective + memory + friction loop | At every milestone close, and whenever something hurts twice |

## Reading order on first contact

`01` first (it tells you what AGENTS.md needs), then skim the rest. Re-read each one in detail when its trigger fires.

## Authoring rules for the standards themselves

Each standard:

- Fits in one screen of dense markdown (50-150 lines).
- States its **trigger** (when this rule applies) up top.
- States its **rationale** in one paragraph.
- Provides a **format** that templates fill in.
- Lists **anti-patterns** — common ways to violate the rule while looking compliant.

If a standard grows beyond one screen, that is a signal to split it or move parts into a profile or adapter.

## Out of scope here

- Language-specific guidance (Python typing, TypeScript module resolution, etc.) → `languages/`.
- Tool-specific application (how Claude Code or Codex interprets these rules) → `adapters/`.
- Example records and starter content → `templates/`.
- aegis's own decisions and verifications → `docs/`.
