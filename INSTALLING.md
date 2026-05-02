# INSTALLING.md — aegis

> **This file is the agent's script for installing aegis into a target project.** Read it in full, then follow it step by step. Do not improvise — improvisation in install procedures destroys user content.
>
> If you are a human reader, this file is also your reference for the manual installation procedure (see § Manual procedure at the end).

## What this file is

When a user gives you (an AI agent — Claude Code, Codex CLI, or similar) a prompt like:

> "Install aegis from https://github.com/tygwan/aegis into this project."

…you read this file in full and follow the five-phase procedure below. Each phase has explicit actions, explicit safety gates, and an explicit confirmation step before any destructive operation.

The procedure is intentionally cautious. **Default to fail-closed**: when ambiguity exists, stop and ask the user.

## Prerequisites

Before starting, verify:

- The user has indicated which project should receive aegis (the **target project**).
- The target project root is a directory you can read and write to.
- You can read aegis source files — by `git clone https://github.com/tygwan/aegis ~/aegis`, or by fetching specific files via your tools.

If any prerequisite is missing, stop and clarify with the user before proceeding.

## Phase 1 — Detect

Scan the target project's root for aegis-related artifacts. Build a detection report. **Do not modify anything during Phase 1.**

Check each of the following and record the result:

| Check | Record |
|---|---|
| `AGENTS.md` exists at root? | Yes / No, byte size, presence of aegis markers (`<!-- aegis:` HTML comments) |
| `CLAUDE.md` exists at root? | Yes / No, byte size, presence of aegis markers |
| Are `AGENTS.md` and `CLAUDE.md` byte-equal? | Yes / No / Skip (one or both missing) |
| `docs/decisions/` exists? | Yes / No, count of `D-*.md` files |
| `docs/verifications/` exists? | Yes / No, count of `V-*.md` files |
| `docs/plans/` exists? | Yes / No, count of `P-*.md` files |
| `docs/milestones/` exists? | Yes / No |
| Other root-level files of interest (`README.md`, package metadata, etc.) | List |

## Phase 2 — Classify

Map the detection report to one of six install states.

### State A — Greenfield / empty

- No `AGENTS.md` and no `CLAUDE.md`.
- May or may not have a partial `docs/` structure.

→ Safe to copy templates directly. No project content at risk.

### State B — `AGENTS.md` only

- `AGENTS.md` exists; `CLAUDE.md` does not.
- The existing `AGENTS.md` may or may not already contain aegis markers.

→ Preserve `AGENTS.md` content. If aegis markers are absent, prepare to inject them. After injection, mirror to `CLAUDE.md`.

### State C — `CLAUDE.md` only

- `CLAUDE.md` exists; `AGENTS.md` does not.

→ Symmetric to State B: preserve content, inject markers if needed, mirror to `AGENTS.md`.

### State D — Both files, identical content

- Both files exist and are byte-equal.

→ A single logical state. If aegis markers are present, treat as "already installed" (proceed to State F check). If absent, prepare to inject markers and re-mirror.

### State E — Both files, divergent content

- Both files exist but byte-differ.

→ **Ambiguous. Stop. Show the diff to the user.** Ask which file should be canonical, or whether to merge. Do not proceed without explicit direction. This is a fail-closed gate.

### State F — Already aegis-installed (current version)

- Both files exist, byte-equal, contain aegis markers, and the marker contents match the current aegis source.

→ Nothing to do. Report "already installed" and exit cleanly.

If marker contents are out of date relative to current aegis (e.g. the orchestration table has been refined), this is an **update**, handled the same way as State D with markers — replace marker regions, leave outside-marker content untouched.

## Phase 3 — Plan

Compose a written plan tailored to the detected state. Present it to the user as a formatted markdown block. The plan must list:

- **Files that will be created** (new content from templates).
- **Files that will be modified**, and exactly which sections of each (the aegis-marker regions only — never project-specific content outside markers).
- **Files that will be left untouched**.
- **Anything ambiguous**, with a question for the user.

**Do not modify any file during Phase 3.** The plan is for review only.

### Plan templates by state

#### State A (Greenfield)

```
Plan:
- Create AGENTS.md from aegis/templates/AGENTS.template.md
  (you will fill in {placeholders}: project name, stack, reading order, etc.)
- Create CLAUDE.md as byte-identical copy of AGENTS.md
- Create docs/decisions/README.md
- Create docs/verifications/README.md
- Create docs/plans/README.md (optional; create only if user uses plan records)
- Total: 5 new files, 0 modifications
```

#### State B (existing AGENTS.md, no CLAUDE.md)

```
Plan:
- Preserve all existing content in AGENTS.md.
- Inject two aegis-managed sections (with HTML comment markers) into AGENTS.md:
    <!-- aegis:orchestration:begin --> ... <!-- aegis:orchestration:end -->
    <!-- aegis:discipline:begin --> ... <!-- aegis:discipline:end -->
  Injection point: immediately after AGENTS.md's existing § Active conventions
  section, or at end-of-file if no such section exists.
- Mirror AGENTS.md to a new CLAUDE.md (byte-identical copy).
- Create docs/decisions/, docs/verifications/, docs/plans/ with README placeholders
  if missing.
- Total: 1 modification (AGENTS.md), N new files (CLAUDE.md + docs/ READMEs as needed)
```

#### State C (existing CLAUDE.md, no AGENTS.md)

Symmetric to B.

#### State D (both files, identical, no aegis markers)

Same as B but with one extra step: after injecting markers into AGENTS.md, copy the result to CLAUDE.md (overwriting it, since both files were already identical and the user expects them to remain so).

#### State E (both files, divergent)

```
Detected: AGENTS.md and CLAUDE.md exist with different content.
- AGENTS.md: {size}, {one-line summary}
- CLAUDE.md: {size}, {one-line summary}

aegis requires both files to have identical content (per D-004). Before
proceeding, please confirm one of the following:

(a) Use AGENTS.md as canonical (overwrite CLAUDE.md with AGENTS.md content).
(b) Use CLAUDE.md as canonical (overwrite AGENTS.md with CLAUDE.md content).
(c) Show me the diff first so I can decide.
(d) Cancel installation.
```

After user picks, re-run Phase 3 with the chosen direction, then proceed.

#### State F (already installed, current version)

```
aegis is already installed and up to date in this project.
- AGENTS.md and CLAUDE.md identical: yes
- aegis markers present: yes
- marker content matches current aegis source: yes

Nothing to do. Exiting.
```

## Phase 4 — Confirm

**Wait for explicit user confirmation.** Acceptable confirmations include "yes", "go ahead", "OK", "proceed", or equivalent unambiguous affirmation.

If the user requests changes to the plan, revise it and present again. Loop until confirmed.

If the user explicitly cancels, stop and exit cleanly. Make **no** modifications.

If the user is silent or ambiguous, ask again rather than guessing.

## Phase 5 — Execute

Perform the confirmed plan. Follow these patterns for each operation type.

### Creating a new file

Use the appropriate aegis template as the source. Copy verbatim — do not paraphrase, do not add commentary. After writing the file, advise the user to fill in `{placeholders}` and remove instructional blockquotes.

### Injecting an aegis-managed section

Insert the following block at the agreed-upon location:

```markdown
<!-- aegis:orchestration:begin -->

## Orchestration — when to do what

| ... canonical orchestration table from aegis/standards/01-conventions.md ... |

<!-- aegis:orchestration:end -->


<!-- aegis:discipline:begin -->

## Discipline reminders

- ... canonical reminders from aegis/standards/01-conventions.md ...

<!-- aegis:discipline:end -->
```

The exact content between markers comes from the current aegis source. **Do not paraphrase, do not shorten, do not localize.** If the canonical content seems wrong for the user's project, that is feedback for aegis itself (a D-record candidate), not a reason to deviate during install.

### Updating an aegis-managed section (markers already present)

Replace **only the content between matching `:begin` and `:end` markers**. Leave content outside markers untouched. Match by marker name (`orchestration`, `discipline`) — do not assume position.

If a marker pair is malformed (begin without end, or vice versa), stop and report the error to the user. Do not attempt to repair silently.

### Mirroring AGENTS.md to CLAUDE.md

After all `AGENTS.md` modifications complete, perform a byte-exact copy to `CLAUDE.md`. Verify with `diff` (or equivalent) that the two files are identical. Report any discrepancy.

The user's project should now satisfy aegis [D-004](docs/decisions/D-004-agents-claude-md-full-sync.md): both files exist, both contain identical content, both contain aegis markers around managed sections.

### Final verification

After all operations:

- [ ] `AGENTS.md` and `CLAUDE.md` exist and are byte-identical
- [ ] Aegis markers are present and properly closed
- [ ] No file outside the plan was modified
- [ ] `docs/` structure exists as planned

Report a short summary to the user: which files were created, which were modified, what the user should do next (typically: fill in `AGENTS.md` placeholders, then make first commit).

## Marker semantics — reference

Aegis-managed sections in user-project files are bracketed:

```html
<!-- aegis:NAME:begin -->
... content owned by aegis ...
<!-- aegis:NAME:end -->
```

Where `NAME` is one of:

- `orchestration` — the orchestration lookup table
- `discipline` — the discipline reminders block

Future aegis versions may add more marker names. The marker syntax shape is stable; the content within may evolve.

Rules:

- Markers are HTML comments — invisible in rendered markdown but parseable mechanically.
- Each marker pair must be properly closed; mismatched markers are a bug to report, not to repair.
- Content **between** markers is owned by aegis. The user should not edit it manually — edits will be overwritten on the next install/update. To change it, file a refinement upstream.
- Content **outside** markers is owned by the project. Aegis never modifies it.

## Examples

### Example 1 — Greenfield install

User: "Install aegis from https://github.com/tygwan/aegis into this project."

Phase 1 detection: project root contains only `README.md` and `.git/`.
Phase 2 classify: **State A**.
Phase 3 plan:

```
Plan:
- Create AGENTS.md from aegis template (you'll fill in placeholders for project name, stack, reading order)
- Create CLAUDE.md identical to AGENTS.md
- Create docs/decisions/README.md, docs/verifications/README.md, docs/plans/README.md
- Total: 5 new files, 0 modifications
```

Phase 4: user says "yes".
Phase 5: copy templates, create directories, verify identical, report summary.

### Example 2 — Brownfield with existing AGENTS.md

User: "Install aegis into this project."

Phase 1 detection: `AGENTS.md` exists (12 KB), no `CLAUDE.md`, no aegis-shaped `docs/`.
Phase 2 classify: **State B**.
Phase 3 plan:

```
Plan: preserve and augment.
- Keep all existing content in AGENTS.md.
- Inject aegis orchestration table and discipline reminders (with HTML comment
  markers) immediately after AGENTS.md's § Active conventions section.
- Create CLAUDE.md as byte-identical mirror of AGENTS.md.
- Create docs/decisions/, docs/verifications/, docs/plans/ with README placeholders.
- Total: 1 modification (AGENTS.md), 4 new files.
```

Phase 4: user says "yes".
Phase 5: insert markers, copy AGENTS.md → CLAUDE.md, create directories, verify, report.

### Example 3 — Both files exist but differ (ambiguous)

User: "Install aegis into this project."

Phase 1 detection: `AGENTS.md` 12 KB, `CLAUDE.md` 3 KB, byte-different. `CLAUDE.md` looks like a thin pointer (one line referencing `AGENTS.md`).
Phase 2 classify: **State E**.
Phase 3 plan: stop and present the choice (a/b/c/d as in the State E template above).

User picks (a): "Use AGENTS.md as canonical."

Phase 3 re-plan with chosen direction:

```
Plan: AGENTS.md is canonical.
- Keep existing AGENTS.md content; inject aegis markers as in State B.
- Overwrite CLAUDE.md with the resulting AGENTS.md content (byte-identical mirror).
- Create docs/ structure as needed.
- Total: 2 modifications (AGENTS.md, CLAUDE.md), N new files.
```

Phase 4 confirm. Phase 5 execute.

### Example 4 — Update of already-installed aegis

User: "Update aegis in this project to the latest version."

Phase 1: `AGENTS.md` and `CLAUDE.md` exist, byte-equal, contain aegis markers. Marker content (orchestration table) is older than current aegis source.
Phase 2: **State D / F transition** — installed but stale.
Phase 3 plan:

```
Plan: marker-only update.
- AGENTS.md: replace content between aegis:orchestration markers with current
  canonical version. Replace content between aegis:discipline markers similarly.
- All content outside markers is left untouched.
- Mirror updated AGENTS.md to CLAUDE.md.
- Total: 2 modifications, marker regions only.
```

Phase 4 confirm. Phase 5 execute.

## Manual procedure (without an AI agent)

If you prefer to install aegis without an AI agent, do the following manually.

### For an empty project (greenfield)

```bash
git clone https://github.com/tygwan/aegis ~/aegis
cd <your-project>
cp ~/aegis/templates/AGENTS.template.md AGENTS.md
# Open AGENTS.md, fill in {placeholders}, remove the instructional blockquote at top
cp AGENTS.md CLAUDE.md   # byte-identical mirror
mkdir -p docs/decisions docs/verifications docs/plans docs/milestones
```

Verify: `diff AGENTS.md CLAUDE.md` should output nothing (files identical).

### For an existing project (brownfield)

Do **not** overwrite your existing `AGENTS.md` or `CLAUDE.md`. Instead:

1. Preserve your existing `AGENTS.md`. Open it for editing.
2. From `~/aegis/standards/01-conventions.md`, copy the "Orchestration — when to do what" table and the "Discipline reminders" block.
3. Wrap each in markers and insert into your `AGENTS.md` after § Active conventions:

   ```markdown
   <!-- aegis:orchestration:begin -->
   ## Orchestration — when to do what
   | ... pasted table ... |
   <!-- aegis:orchestration:end -->


   <!-- aegis:discipline:begin -->
   ## Discipline reminders
   - ... pasted reminders ...
   <!-- aegis:discipline:end -->
   ```

4. Copy your updated `AGENTS.md` to `CLAUDE.md` (byte-identical).
5. Create `docs/decisions/`, `docs/verifications/`, `docs/plans/` if absent.
6. Verify: `diff AGENTS.md CLAUDE.md` returns nothing.

For future aegis updates, replace only the content between `<!-- aegis:NAME:begin -->` and `<!-- aegis:NAME:end -->` markers; never edit outside markers automatically.

## Rollback

If something goes wrong:

- Most install changes affect `AGENTS.md`, `CLAUDE.md`, and a few `docs/` README files.
- `git status` shows what changed; `git diff` shows the content.
- Single-file revert: `git checkout -- AGENTS.md` (or the offending file).
- Full revert (if not yet committed): `git reset --hard HEAD`.
- Re-run install only after the original cause is resolved.

## References

- Standard: [`standards/01-conventions.md`](standards/01-conventions.md) — canonical orchestration table, marker spec
- Decision: [`docs/decisions/D-005-installation-procedure.md`](docs/decisions/D-005-installation-procedure.md) — why this procedure shape
- Two-file sync: [`docs/decisions/D-004-agents-claude-md-full-sync.md`](docs/decisions/D-004-agents-claude-md-full-sync.md) — why AGENTS.md and CLAUDE.md are byte-identical
- Templates: [`templates/AGENTS.template.md`](templates/AGENTS.template.md) — the source for greenfield install
