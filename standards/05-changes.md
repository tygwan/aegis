# 05 — Changes

How changes get committed, branched, and migrated. The discipline is small and consistent.

## Trigger

Continuously. Every commit, every branch, every migration touches this rule.

## Rationale

Inconsistent commit history is expensive in two ways: it makes `git log` and `git blame` weak as forensic tools, and it makes AI agents that read history less able to reason about precedent. A small, predictable convention pays back daily.

## Commit messages

Format:

```
type(scope): summary

Body explaining WHY, not WHAT (the diff already shows what).
References to related records: D-{NNN}, P-{NNN}, V-{NNN}.

Co-Authored-By: ...   (when applicable)
```

**Types** (canonical set; do not invent new ones casually):

- `feat` — new user-visible capability
- `fix` — corrects incorrect behaviour
- `refactor` — internal change, no behaviour change
- `docs` — documentation only
- `test` — tests only
- `chore` — build, deps, tooling
- `perf` — performance improvement (when explicitly measured; otherwise it's `refactor`)

**Scope** is the area touched: package name, top-level directory, or feature name. One word. Optional but encouraged when the project has multiple subsystems.

**Summary** is imperative ("add X", not "added X" or "adds X"), no trailing period, < 70 characters.

**Body** is optional but used for any commit beyond a one-line change. It explains the decision context: why this change, what alternative was rejected, what record (D/P/V) it references. Diffs explain *what*; bodies explain *why*.

## Branches

- Long-lived: `main` (always shippable; protected if the project has CI).
- Working: `feat/{slug}`, `fix/{slug}`, `refactor/{slug}`. Match the commit type.
- Sensitive: `legacy/{slug}` for code preserved for archeology — never deleted, never auto-merged.

Working branches are short-lived. If a branch lives more than two weeks, it is probably hiding a scope problem; either land it incrementally or close it and rewrite.

## Pull requests

- One logical change per PR. A feature with its tests is one change. A feature plus an unrelated refactor is two PRs.
- PR description explains *what* and *why*; commit messages cover *how*. PR description is the human-readable narrative; the commit history is the durable forensic trail.
- Link related D-records, P-records, V-records explicitly.
- Do not squash-merge unless the project's standard requires it; preserving the commit boundary preserves the reasoning.

## Migrations and breaking changes

When a change is not a pure addition:

1. Write a D-record proposing the migration if it is structural.
2. Implement the change with a backwards-compatible path first if possible (deprecation period). If a clean break is required (small projects often have this licence), state that in the D-record.
3. Update `AGENTS.md` and any affected `standards/` references in the same commit as the breaking change.
4. Reference the D-record in the commit body of the breaking change.

For a project pre-v1, breaking changes are routine and lightweight — a D-record plus a commit body is enough. For published artifacts (libraries, plugins), follow the migration discipline of that ecosystem.

## Hooks and gates

aegis M1 does not enforce any of this with hooks. The discipline is manual. Hooks (commit-msg, pre-push) may be added in M5 once the conventions are well-internalised; introducing them too early generates more friction than they save.

## Anti-patterns

- **Sweeping commits.** "WIP" or "stuff" or "fix many things" — these are votes against future-self. If you cannot describe the commit in one imperative line, the commit is too big.
- **Refactor-plus-feature commits.** They make code review and bisecting both harder. Always split.
- **Force-push to shared branches.** History on shared branches is a public good; do not rewrite it without coordination, and never on `main`.
- **Skipping `--no-verify` to bypass failed pre-commit hooks.** Either fix the issue, disable the hook locally with reasoning recorded, or remove the hook. Bypassing it silently ruins the hook's value.

## Cadence

Most projects produce 1-10 commits per active day. This discipline costs perhaps thirty seconds per commit and saves hours of forensic work later.
