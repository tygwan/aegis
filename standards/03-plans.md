# 03 — Plans (P-records)

A **P-record** is a written plan for a unit of work that is too large to hold in working memory or to execute as a single thought. It is written **before** the work starts, and the verification record (V-record) at the end of the work measures against it.

## Trigger

Write a P-record before starting a unit of work that has any of these properties:

- Estimated to take **more than ~1 day** of focused effort.
- Decomposes into **more than 3 distinct sub-tasks**.
- Touches **more than one subsystem** (frontend + backend, code + infra, etc.).
- Has **non-obvious failure modes** that benefit from being enumerated upfront.
- Has **a deadline** that requires sequencing.

For tasks below all of these thresholds, hold the plan in your head or in a TODO comment. Plans are a tool, not a tax.

## Rationale

Two failure modes plans exist to prevent:

1. **Scope drift.** Without an explicit plan, "fix the auth bug" turns into a four-day refactor. The plan freezes the scope so drift is visible.
2. **Hidden risks.** Risks identified during execution are crisis-mode risks. Risks identified during planning have time to be mitigated cheaply.

A plan is not a contract. It is a working hypothesis that can be revised, but each revision should be visible.

## Format

P-records are stored at `docs/plans/P-{NNN}-{slug}.md`, numbered monotonically. Required sections:

1. **Title and metadata** — `P-{NNN}: {short title}`, status (`Open | In progress | Done | Abandoned`), owner, related D-records, related milestone (if any).
2. **Goal** — one paragraph. What outcome would let us call this Done?
3. **Non-goals** — explicit list of what this plan does *not* attempt. Equally load-bearing as goals.
4. **Steps** — ordered sub-tasks, each with a one-line description plus an expected output (file, commit, test, decision). Ten or fewer at a single level — if more, group into phases.
5. **Risks and mitigations** — what could go wrong, and what will you do about each. Risks below "could plausibly cost half a day to recover from" are not worth listing.
6. **Done criterion** — concrete, falsifiable. "All tests pass" + "manual verification of X works" + "V-record written" is a typical shape.
7. **Verification plan** — pointer to the V-record number that will measure completion (e.g. "V-006 will verify"). Reserve the number now.

See [`templates/Plan-record.template.md`](../templates/Plan-record.template.md).

## Plans vs decisions

- **D-record** captures *what to build / how to architect*: "we use Postgres, not MySQL".
- **P-record** captures *how to get there*: "Step 1, set up local Postgres; Step 2, write migration ...".

A P-record may reference D-records as constraints. A D-record may spawn a P-record as its implementation route.

## Exploratory plans

Some work is not deterministic. In research, data science, design exploration, and feasibility spikes, you do not know in advance which approach will win — the plan is to *try several* and pick. The standard P-record format assumes linear steps; for exploratory work, adapt as follows:

- **Goal** stays as goal: "find the approach that meets accuracy ≥ 0.85 within 4 days."
- **Steps** become **branches** — not "step 1, step 2, step 3" but "approach A, approach B, approach C", each with its own quick sub-steps and a quick-eval criterion.
- **Done criterion** is the *selection* criterion ("one approach meets the threshold and is documented") plus a *budget* ("or 4 days elapsed, whichever first").
- **Verification plan** describes how the chosen approach will be measured against the threshold; the unchosen approaches are documented in the V-record `§ Findings` section as negative results.

Negative results — the approaches that did not win — are valuable. Do not delete them from the V-record. A future contributor weighing the same trade-off will benefit from knowing you already tried option C and why it lost.

If the exploration concludes "none of the approaches met the threshold," that is a legitimate verification result (`Conclusion: not verified`). Either the goal needs revision (D-record) or the time-box needs extension (revise the plan).

## Revision

Plans change. When a step turns out wrong:

- Edit the P-record in place. Mark the obsolete step with `~~strikethrough~~` and add the new step. Do not silently replace.
- If the change is large enough to invalidate the goal or done criterion, the plan needs a fresh number — close the old one as `Abandoned` and open a new P-record that references the old one in its context.

## Anti-patterns

- **Planning everything.** Trivial work does not benefit from a plan; the planning overhead exceeds the execution. Plans are for work that earns the overhead.
- **Plans without done criteria.** A plan that ends "and so on" is unfinished. If you cannot say what success looks like, you are not yet ready to start.
- **Plan-the-plan recursion.** If writing the plan is itself non-trivial, that is a signal the work is poorly understood. Spend the first hour exploring; come back to plan with material.
- **Hidden mid-execution rewrites.** If you abandoned the original plan halfway through and finished by improvising, the V-record must say so. Quietly diverging from the plan and then claiming success is the failure mode plans were meant to prevent.

## Cadence

On most projects, P-records appear in clusters: at the start of a milestone, at the start of each non-trivial feature within it. Solo developers may comfortably skip P-records for routine work; teams typically benefit from writing them more often, since they double as a coordination artifact.
