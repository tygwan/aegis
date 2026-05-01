# 06 — Milestones

A **milestone** is a bounded unit of project work that opens explicitly, executes through plans and verifications, and closes with a retrospective. Milestones are the largest unit aegis treats as first-class.

## Trigger

You need milestones once your project's work groups naturally into phases — typically, anything beyond a one-week scope. Solo and small projects often delineate them by feature theme ("auth", "first vertical slice", "performance pass"); larger projects may align them with calendar weeks or release versions.

A project of one milestone is fine; a project of zero milestones (no boundaries) tends to lose track of what is supposedly being delivered when.

## Rationale

Without milestone boundaries, three things go wrong:

1. **Scope drifts forever.** No "done" → no checkpoint → no closure.
2. **Lessons do not aggregate.** Per-task V-records are too granular to drive future planning. Milestones are the natural aggregation level for retrospectives.
3. **Closed-loop breaks.** A V-record's follow-up has nowhere to land if there is no "next milestone" to inherit it.

## Lifecycle

A milestone has four phases:

### Open

Create `docs/milestones/M{N}-{slug}/README.md` with:

1. **Goal** — one paragraph. What outcome would let us close this milestone?
2. **Scope** — bullet list of in-scope deliverables.
3. **Out of scope** — bullet list of explicitly excluded items, including everything inherited from the previous milestone's "next milestone follow-ups" that did *not* make this milestone's cut.
4. **Inherited from M{N-1}** — items absorbed from the previous milestone's retrospective `§ Next-milestone follow-ups` (closed-loop guarantee).
5. **Plan / verification / decision pointers** — reserved P-record range, reserved V-record range, anticipated D-records.
6. **Open questions** — what is undecided at milestone open. Each gets resolved by a D-record before the milestone closes.

Update `AGENTS.md` "Out of scope" section to reflect this milestone.

### Execute

The body of the milestone:

- D-records resolve the open questions and any new structural choices.
- P-records decompose the work.
- V-records verify each unit.
- Commits link back to records via their bodies.

The milestone README is updated as records land — primarily, ticking off scope items as they verify.

### Close

A milestone closes in one of two ways: **Closed** (the goal was met) or **Abandoned** (the goal was wrong, infeasible, or invalidated by changed context). Both produce a retrospective; only the framing differs.

When closing **Closed** (goal met):

1. Write the **retrospective** — `docs/milestones/M{N}-{slug}/retrospective.md` (or a portfolio entry, depending on project conventions). Format per [`07-learn-from-friction.md`](07-learn-from-friction.md). Three sections: what worked, what to improve, next-milestone follow-ups.
2. Mark the milestone README's status as **Closed** with the date.
3. Confirm every V-record's follow-ups have been considered for the next milestone — either inherited or explicitly dropped with reasoning.

When closing **Abandoned** (goal wrong / infeasible / context changed):

1. Write the **retrospective** anyway. The framing shifts: instead of "what worked", focus on *what we learned about why the goal was wrong*; instead of "next-milestone follow-ups", focus on *what the next milestone should attempt instead*.
2. Write a **D-record** documenting the abandonment — what changed, what the new direction is. This D-record supersedes any milestone-scoped D-records that no longer apply.
3. Mark the milestone README's status as **Abandoned** with the date.
4. Keep all V-records produced before abandonment — partial verifications have long-term value, even when the surrounding work is discarded.

Abandoned milestones often produce the most valuable retrospectives. Skipping the retrospective on a "bad" milestone is the failure mode this discipline exists to prevent.

### Carry over

The next milestone's `Open` step (above) absorbs follow-ups from this one. **This is the closed-loop guarantee.** A milestone retrospective whose follow-ups are not absorbed (or explicitly dropped) is a broken loop, and the lesson dies.

## Numbering

Milestones are numbered monotonically: `M1`, `M2`, `M3`. The `{slug}` is short and descriptive. A milestone's records use the same prefix conceptually but are independently numbered (D-records, P-records, V-records all have project-wide numbering).

## What does NOT belong in a milestone

- **Maintenance work.** Routine bug fixes, dependency bumps, doc tweaks live outside milestone boundaries unless they are *the milestone*.
- **Spike / exploration.** When the goal is "find out whether X is feasible", that is a P-record, not a milestone. The milestone forms once a feasibility answer exists.
- **Open-ended quality work.** "Improve performance" is not a milestone; "reduce API p95 below 200ms by Friday" is.

## Anti-patterns

- **Milestones without close.** If a milestone is implicitly abandoned because the team moved on, write the retrospective anyway — what was learned is more valuable than the unfinished scope.
- **Sliding goals.** When the milestone's goal is rewritten halfway through to match what was actually achieved, the retrospective becomes vacuous. Better to honestly close as "partial" and open a new milestone with a corrected goal.
- **Boundaryless milestones.** Goals like "build the whole product" or "make it good" do not have a checkable close. Tighter scope, shorter horizon.
- **Inherited backlog dump.** "Next-milestone follow-ups" is not a graveyard for everything that did not get done. Items that should be permanently dropped are dropped explicitly, with reasoning, in the retrospective.

## Cadence

Healthy milestone size depends on project pace, but a useful sanity check: a milestone that produces 0 D-records, 0 V-records, or 0 deviations is suspiciously low-friction; one that produces > 20 of any of these is suspiciously sprawly. Adjust scope at the next open.
