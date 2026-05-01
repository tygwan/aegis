# V-{NNN}: {short title}

> Verification record per [aegis 04-verification](https://github.com/tygwan/aegis). Written *after* the work is complete. **Remove this blockquote after filling in.**

| | |
|---|---|
| **Date** | YYYY-MM-DD |
| **Verifier** | {name} |
| **Plan** | P-{NNN} (the plan being verified) |
| **Milestone** | {M-N-slug, if any} |
| **Related D-records** | {D-NNN, ...} |

## Hypothesis

{One paragraph. What was expected to be true if the work succeeded? Often: "the plan's done criterion is met."}

## Method

{How verification was performed. Be reproducible — list the specific tests, manual steps, environments. A future contributor with the same access should be able to repeat this.}

- {Test: e.g. "ran `pnpm test` against branch X, all 47 tests pass"}
- {Manual: e.g. "logged in as user-A, attempted cross-tenant read, blocked"}
- {Measurement: e.g. "API p95 over 100 sequential requests"}

## Result

{What the method produced. Include numeric evidence where applicable.}

- {observation}
- {observation}
- {observation}

## Conclusion

{One sentence: Verified / Partially verified / Not verified, plus a one-clause reason.}

## Deviations

{Every place the executed work differed from the plan. Each deviation: what was planned, what was actually done, why.}

| # | Planned | Actual | Reason |
|---|---|---|---|
| 1 | {...} | {...} | {...} |
| 2 | {...} | {...} | {...} |

## Findings

{Anything noticed during verification that was not the primary subject. Performance observations, surprising behaviour, latent issues, opportunities. Tag severity if it matters.}

1. **{finding title}** — {one paragraph}. Severity: low/med/high (optional).
2. **{finding title}** — {one paragraph}.

## Follow-ups

{Items extracted from deviations or findings that need future work.}

| # | Description | Owner | Target | Status |
|---|---|---|---|---|
| 1 | {short description} | {name} | M{N+1} or P-{NNN} | open |
| 2 | {...} | {...} | {...} | open |

## References

- Related V-records: {V-NNN, ...}
- Related D-records: {D-NNN, ...}
- Commits: {hash, hash, hash}
