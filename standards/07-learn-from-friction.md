# 07 — Learn from Friction

Friction — work that is slower, more confusing, or more painful than expected — is the most valuable signal a project produces. This rule defines how friction becomes durable knowledge instead of forgotten complaints.

## Trigger

Two triggers, both must be honoured:

1. **Acute trigger.** When something hurts twice in the same project, capture it. Once is a fluke; twice is a pattern that will hurt a third time.
2. **Periodic trigger.** At every [milestone](06-milestones.md) close, write a **retrospective**. The retrospective aggregates the milestone's frictions and decides which became durable lessons.

## Rationale

Friction signals are easy to lose. The painful debugging session that ate Tuesday afternoon ends with the bug fixed and an exhale; the underlying lesson — "we should test middleware paths" or "JWKS migration broke our test fixtures" — evaporates by Thursday.

This rule is the discipline for catching the lesson before it evaporates.

## The friction loop

```
Friction occurs (debugging, surprise, retrofit)
       ↓
Recorded somewhere (V-record § Findings, retrospective, or memory)
       ↓
Decision: is this a durable lesson?
       ↓                           ↓
   Yes → standard / convention   No  → leave in the V-record / memory file as
   refinement, possibly via a D-       a single-instance note. If it recurs,
   record. Update AGENTS.md or          it becomes "twice", which triggers
   a standards/ file.                  promotion to a durable lesson.
```

## The retrospective

Written at every milestone close, stored at `docs/milestones/M{N}-{slug}/retrospective.md` (or a portfolio entry per project convention). Three core sections:

1. **What worked** — three or so items. Each: what was done, why it was the right call, what would have happened if we had not done it.
2. **What to improve** — three or so items. Tone is not blame, it is *if-we-knew-then*. Each: what was done, why we now think differently, what we will do next time.
3. **Next-milestone follow-ups** — three or so items. Each: what carries forward, why it belongs in the next milestone, what is decided vs. still undecided.

Each item is short — a line of title, two to four lines of body. Resist the urge to make retrospectives long; longer retrospectives are less re-readable.

See [`templates/Retrospective.template.md`](../templates/Retrospective.template.md).

## Inputs to the retrospective

The retrospective's raw material is already written down by the time you sit to compose it:

- All V-records of the milestone (§ Deviations, § Findings, § Follow-ups).
- All P-records of the milestone (any "abandoned" steps, any plan revisions).
- The milestone's open questions (now resolved by D-records, or explicitly carried forward).
- Memory entries created during the milestone (if the project uses persistent memory).

A retrospective written without consulting these inputs is impressionistic and weaker.

## Closed-loop with the next milestone

Every "next-milestone follow-up" must be either:

- **Absorbed** into the next milestone's README `§ Inherited from M{N-1}`, or
- **Explicitly dropped** in the retrospective itself (with reasoning).

Items that quietly disappear are the failure mode this rule exists to prevent. See [06-milestones.md](06-milestones.md) for the structural enforcement.

## Memory (when the project uses it)

Some projects use a persistent memory file (Claude Code's auto-memory, a hand-curated `MEMORY.md`, etc.) to hold facts that span sessions: user preferences, project-specific gotchas, working agreements.

Memory entries record durable facts; retrospectives record durable *lessons*. They are complementary:

- **Memory** answers: "what should I know to start work today?"
- **Retrospective + standards refinements** answer: "what should the project's discipline be next time?"

If a friction-derived lesson is project-specific (e.g. "this codebase's middleware uses a particular pattern"), memory is its home. If it generalises (e.g. "we should always test middleware path skipping"), it belongs in `standards/` or a project-level convention.

## Anti-patterns

- **Retrospectives as venting.** A retrospective that lists complaints without proposing changes produces no durable improvement. Each item must end with a future-action.
- **Lessons without a home.** A retrospective that identifies "we should always X" but never updates a standard or convention is a lesson destined to be forgotten.
- **Skipping retrospectives on bad milestones.** The bad milestones are the ones with the most learning. The temptation to skip them is the strongest sign that you should not.
- **Retrospectives written months late.** The friction signals decay quickly. Write the retrospective within a week of milestone close, ideally same week.

## Cadence

One retrospective per milestone close. Memory entries created opportunistically during work, not in batches. Standards refinements are rare events triggered by retrospectives — most retrospectives produce zero standards changes; that is fine.
