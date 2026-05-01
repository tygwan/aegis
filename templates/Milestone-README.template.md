# M{N} — {short title}

> Milestone README per [aegis 06-milestones](https://github.com/tygwan/aegis). Written at milestone open; updated as work progresses; finalized at close. **Remove this blockquote after filling in.**

| | |
|---|---|
| **Status** | Open / In progress / Closed / Abandoned |
| **Opened** | YYYY-MM-DD |
| **Closed** | YYYY-MM-DD (fill at close or abandonment) |
| **Owner** | {name} |

> **Abandoned** is not failure — it is honest closure of a milestone whose goal was wrong, whose scope was infeasible, or whose context changed. Per [aegis 06-milestones](https://github.com/tygwan/aegis), abandoned milestones still get a retrospective; the lessons from a failed milestone are often the most valuable.

## Goal

{One paragraph. What outcome would let us call this milestone Closed?}

## Scope

{Bullet list of in-scope deliverables. Update with checkmarks as items verify.}

- [ ] {deliverable}
- [ ] {deliverable}
- [ ] {deliverable}

## Out of scope

{Bullet list of explicitly excluded items. Equally important as Scope.}

- {item}
- {item}

## Inherited from M{N-1}

{Items absorbed from the previous milestone's retrospective `§ Next-milestone follow-ups`. This is the closed-loop guarantee — every M{N-1} follow-up should appear here, in scope, or in out-of-scope with reasoning.}

- {follow-up} → see {M{N-1} retrospective § ...}
- {follow-up} → see {...}

## Open questions

{Things undecided at milestone open. Each is resolved by a D-record before close.}

| # | Question | Resolved by |
|---|---|---|
| 1 | {question} | (D-NNN, when written) |
| 2 | {question} | — |

## Reserved record numbers

| Type | Range | Notes |
|---|---|---|
| D-records | D-NNN ... D-NNN | {anticipated decisions} |
| P-records | P-NNN ... P-NNN | {anticipated plans} |
| V-records | V-NNN ... V-NNN | {anticipated verifications} |

## Plan summary

{Optional. Either inline a top-level plan here, or list the P-records that decompose the work.}

- P-{NNN}: {plan title}
- P-{NNN}: {plan title}

## References

- Previous milestone: M{N-1} → {link to retrospective}
- Architecture spec: {link, if any}
- Related D-records that constrain this milestone: {D-NNN, ...}

---

## Closing checklist (run at milestone close)

- [ ] All scope items verified (V-records exist for each)
- [ ] All open questions resolved by D-records
- [ ] Retrospective written at `retrospective.md`
- [ ] Each V-record's follow-ups: either listed in the retrospective `§ Next-milestone follow-ups` or explicitly dropped with reasoning
- [ ] `AGENTS.md` "Out of scope" updated for the next milestone
- [ ] Status above changed to **Closed**

## Abandonment checklist (when closing as Abandoned)

- [ ] Retrospective written, focused on *why this milestone was wrong* and *what the next milestone should look like instead*
- [ ] D-record written explaining the abandonment (what changed, what the new direction is — supersedes the milestone's open D-records if any)
- [ ] Status above changed to **Abandoned**
- [ ] Any V-records produced before abandonment are kept — partial verification has long-term value
