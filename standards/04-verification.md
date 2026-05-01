# 04 — Verification (V-records)

A **V-record** is a written record that a piece of work was finished and verified — and equally, a record of what *failed* verification or *deviated* from the plan. V-records turn ambiguous "I think it works" into durable, citable evidence.

## Trigger

Write a V-record at the end of every meaningful unit of work. "Meaningful" means it had a [P-record](03-plans.md), or it stands alone as a deliverable (a feature, a verified performance gain, a security review, a major refactor).

For routine commits, a V-record is overkill — the commit message and the code review are sufficient.

## Rationale

V-records solve two problems:

1. **"Did it actually work?"** Without a written verification, "done" is a feeling, not a fact. Six weeks later, when something regresses, no one remembers what was verified vs. what was assumed.
2. **"What did we learn?"** Deviations from the plan and unexpected findings are the most valuable knowledge a project produces. They are routinely lost to chat scrollback and head-knowledge. V-records capture them.

## Format

V-records are stored at `docs/verifications/V-{NNN}-{slug}.md`, numbered monotonically. Required sections:

1. **Title and metadata** — `V-{NNN}: {short title}`, date, related plan (P-record), related milestone, related D-records.
2. **Hypothesis** — what was expected to be true if the work succeeded. One paragraph. (Often: "the plan's done criterion is met".)
3. **Method** — how verification was performed. List the specific tests run, manual steps taken, environments checked. Be reproducible — a future contributor with the same access should be able to repeat the verification.
4. **Result** — what the method produced. Include numeric evidence where applicable: "auth flow round-trip 280ms", "all 47 tests pass", "RLS policy blocks cross-tenant read in psql".
5. **Conclusion** — verified / partially verified / not verified. One sentence.
6. **Deviations** — every place the executed work differed from the plan. Each deviation: what was planned, what was actually done, why. This section is often the most useful part of the record.
7. **Findings** — anything noticed during verification that was not the primary subject. Performance observations, surprising behaviour, latent issues, opportunities. Tagged with severity if it matters.
8. **Follow-ups** — items extracted from deviations or findings that need future work. Each follow-up: short description, rough owner, target milestone or P-record.

See [`templates/V-record.template.md`](../templates/V-record.template.md).

## V-records vs other records

| Concern | Where to record |
|---|---|
| What we will build / why | D-record |
| How we will build it | P-record |
| Whether we built what we said | V-record |
| What we learned doing it | V-record (Findings + Deviations) |
| Recurring lessons across milestones | Retrospective ([07-learn-from-friction](07-learn-from-friction.md)) |

## Anti-patterns

- **V-records that only say "verified".** Skipping Deviations and Findings turns a V-record into a checkbox; the point is the unexpected stuff that came out of doing the work.
- **Hiding partial failure.** If verification was partial — three of four browsers tested, two of three environments — say so. The point of the record is durable truth, not optimistic narrative.
- **Conflating V-record with PR description.** PRs explain a code change. V-records explain whether a *piece of work* (often spanning several PRs) is verified. They serve different audiences and different time horizons.
- **Skipping the V-record when the work succeeds.** Successful work is often the most valuable to record — its findings are what future-you cites.

## Closing the loop

When a V-record's Follow-ups section produces work for a future milestone, those follow-ups should appear in the next milestone's README ("Inherited from V-{NNN}") and ideally in its P-records. If they do not, the loop is broken — the lesson dies with the V-record.

[`06-milestones.md`](06-milestones.md) and [`07-learn-from-friction.md`](07-learn-from-friction.md) describe the closed-loop discipline in detail.
