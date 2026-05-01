# Templates

Files you copy into your own project. Each is a fillable scaffold matching one of the [`standards/`](../standards/) rules.

## How to use

1. Copy the relevant `*.template.md` into your project at the location indicated below.
2. Rename to drop the `.template` suffix (e.g. `D-record.template.md` → `D-001-{slug}.md`).
3. Fill in `{placeholders}` and remove instructional blockquotes (the `> ` lines at the top of each template).

## Index

| Template | Copies to | Standard | When to copy |
|---|---|---|---|
| [`AGENTS.template.md`](AGENTS.template.md) | `AGENTS.md` | [01](../standards/01-conventions.md) | Day 1 |
| [`D-record.template.md`](D-record.template.md) | `docs/decisions/D-{NNN}-{slug}.md` | [02](../standards/02-decisions.md) | Each structural choice |
| [`Plan-record.template.md`](Plan-record.template.md) | `docs/plans/P-{NNN}-{slug}.md` | [03](../standards/03-plans.md) | Before non-trivial work |
| [`V-record.template.md`](V-record.template.md) | `docs/verifications/V-{NNN}-{slug}.md` | [04](../standards/04-verification.md) | After meaningful work |
| [`Milestone-README.template.md`](Milestone-README.template.md) | `docs/milestones/M{N}-{slug}/README.md` | [06](../standards/06-milestones.md) | At milestone open |
| [`Retrospective.template.md`](Retrospective.template.md) | `docs/milestones/M{N}-{slug}/retrospective.md` | [07](../standards/07-learn-from-friction.md) | At milestone close |
| [`Portfolio-entry.template.md`](Portfolio-entry.template.md) | wherever your project keeps narrative artifacts | [07](../standards/07-learn-from-friction.md) | At milestone close (when external narrative wanted) |

## Conventions used in templates

- Lines starting with `> ` at the top of a template are **instructional blockquotes**. Remove them after copying.
- `{placeholders}` are required substitutions.
- `[bracketed text]` is optional guidance about what to put there.
- Section headers and structure are not optional — keep all required sections, omit only the optional ones.
