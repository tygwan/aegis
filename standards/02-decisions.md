# 02 — Decisions (D-records)

A **D-record** captures a structural choice between viable alternatives. It explains what was decided, when, why, and what the alternatives were. Once written, it is durable: future changes either follow it or formally supersede it.

## Trigger

Write a D-record when a choice has all three of these properties:

1. **Multiple viable options exist.** "What database?" yes; "what variable name?" no.
2. **Reversal cost is non-trivial.** Reversing later requires migration, refactoring, or stakeholder re-alignment.
3. **The choice constrains future work.** Other people or future-you will revisit it.

If any one is missing, you are making a tactical decision, not a structural one. Tactical decisions live in code and commit messages, not D-records.

## Rationale

Six weeks from now, neither the human nor any AI agent will remember why the project chose X over Y. Without that context, every revisit of the choice reopens the debate from zero. D-records freeze the reasoning into the repo so that the next session, the next contributor, or the next agent can pick up where the original decision left off.

## Format

D-records are stored at `docs/decisions/D-{NNN}-{slug}.md`. Numbering is monotonic — once `D-007` exists, the next is `D-008`, never `D-007a` or reuse.

Required sections:

1. **Title and metadata** — `D-{NNN}: {short title}` plus status (`Proposed | Accepted | Superseded`), date, deciders.
2. **Context** — what is forcing the decision. One paragraph. State the constraint (deadline, requirement, problem) that makes a choice necessary.
3. **Options** — every option seriously considered. For each: one-paragraph description plus pros and cons. Two options minimum (if there is only one option, you are not deciding, you are documenting).
4. **Decision** — the choice, in one sentence. Reference an option by letter or name.
5. **Consequences** — what changes because of this decision. What gets unblocked, what gets locked in, what becomes harder. Both upside and downside.
6. **Supersession** (when applicable) — if this decision replaces or limits an earlier one, state which D-record and how.

See [`templates/D-record.template.md`](../templates/D-record.template.md).

## Status lifecycle

- **Proposed** — written but not yet committed to. Used briefly when discussion is in flight; should not stay Proposed for long.
- **Accepted** — the active choice. Most D-records sit here for their entire life.
- **Superseded** — replaced by a newer D-record. Add a `> **SUPERSEDED YYYY-MM-DD** by [D-{NNN}](...)` block at the top in the same commit that introduces the replacement. Do not delete superseded records — their reasoning is part of the project's history.

## Where to put structural reasoning that does not warrant a full D-record

- Tactical implementation notes → code comments (rarely, only when WHY is non-obvious).
- Stack-level patterns that apply project-wide → `AGENTS.md`.
- Cross-cutting rules with rationale → propose a refinement to a `standards/` file via D-record.

## Anti-patterns

- **Retroactive D-records to look organized.** D-records written months after the decision are weak — the reasoning has decayed. Better to write a "current state" record honestly: "this is what we do; the original reasoning is partial."
- **One D-record per code change.** If a decision is just "implement the plan", that is not a structural decision. The plan record (P-record) covers it.
- **Skipping options.** If the D-record only describes the chosen path and dismisses alternatives in one line, future-you cannot tell whether the alternatives were genuinely considered.
- **Burying supersession.** When you replace an old D-record, mark the old one explicitly. Otherwise, agents reading older records will follow stale reasoning.

## Cadence

A healthy project produces D-records irregularly: a cluster at project start (foundation choices), then sporadically when new structural questions arise. A six-month period with no D-records on a non-trivial project is suspicious — either decisions are being made silently, or growth has stopped.
