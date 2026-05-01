# D-003: Standards scope — refine dev-standards 14 → 7

| | |
|---|---|
| **Status** | Accepted |
| **Date** | 2026-05-02 |
| **Deciders** | coffin |
| **Supersedes** | — |
| **Superseded by** | — |

## Context

`dev-standards` (the predecessor; see [D-001](D-001-project-charter.md)) accumulated 14 core rules (C1-C14), 4 profiles (P1-P4), and 7 adapters (A0-A6). Some are universally applicable; others assume a specific stack (web/SaaS), a specific maturity (production with users), or a specific domain (research, incident operations).

aegis's universal-applicability constraint (per D-001) requires every standard to make sense for any project type. Carrying forward all 14 rules verbatim would violate that constraint.

Additionally, the user explicitly requested two capabilities that dev-standards underweighted:

- **Plan records** before non-trivial work (dev-standards has implicit planning under several rules but no first-class P-record).
- **Milestone framework** (dev-standards has change management and verification but no opinion on grouping work into milestones).

This decision sets the M1 standards scope.

## Options considered

### Option A — Carry forward all 14 dev-standards rules

Keep C1-C14 as-is, port to aegis with light editing.

**Pros**
- Preserves familiarity for existing dev-standards users.
- No work to decide what to drop.

**Cons**
- Several rules assume stack or maturity that violates universal applicability.
- 14 rules is too many for a "memorable, applies-everywhere" standards system.
- New project owner faces a 14-rule learning curve before understanding what is core.

### Option B — Minimum 5 (universal core only)

Keep only C5 (decisions), C8 (verification), C13 (learn-from-friction), C4 (changes), C1 (scope/conventions) — refined into 5 universal standards.

**Pros**
- Maximum minimalism. Easy to learn.
- All 5 are unambiguously universal.

**Cons**
- Misses plan-record discipline (user explicitly requested).
- Misses milestone framework (user explicitly requested).
- Five rules in a single dogfood scenario is too thin to validate the system across project types.

### Option C — Refined 7 (universal core + planning + milestones)

5 universal rules from B, plus 2 additions:

- `03-plans.md` (Plan records — captures user request "고품질 개발 계획 지원")
- `06-milestones.md` (Milestone framework — captures user request "프로젝트 관리" + closed-loop guarantee)

Other dev-standards rules (C9 interfaces, C10 observability, C11 security, C12 documentation, C14 concept-hub, C2 repo-contract, C3 config, C6 problem-tracking, C7 dependency-management) are deferred or dropped:

- **Deferred:** C7 (likely returns as `languages/`-specific guidance), C2 (folded into 01-conventions), C6 (folded into 04-verification's follow-ups).
- **Dropped from core:** C9, C10, C11, C12, C14, C3 — domain or maturity-specific. They may return as profiles in M3+ if demand emerges.

Profiles P1-P4: deferred entirely. None included in M1. They reappear if usage justifies them.

**Pros**
- Covers user-requested capabilities (plans, milestones) explicitly.
- Each retained rule is universal.
- Number is small enough to memorize, large enough to dogfood.

**Cons**
- Dropping C9-C12 means projects with API surfaces, runtime services, security postures lose written guidance. Mitigation: those projects can refine aegis with their own profile or supplementary rules; aegis core does not preclude adding them.

### Option D — Keep 14, mark some optional

Keep all 14 rules; mark some "optional / project-stage-dependent."

**Pros**
- No content lost.

**Cons**
- "Optional rules" is a code smell. Either a rule is the standard, or it is not.
- Doesn't address the universality problem.

## Decision

**Option C.** Seven standards in M1 — five refined from dev-standards core plus two new (plans, milestones). All deferred / dropped rules are documented here and may return as profiles or domain packs if demand emerges.

The seven standards:

1. `01-conventions.md` (refined from C1 + C2)
2. `02-decisions.md` (refined from C5)
3. `03-plans.md` (new)
4. `04-verification.md` (refined from C8)
5. `05-changes.md` (refined from C4)
6. `06-milestones.md` (new)
7. `07-learn-from-friction.md` (refined from C13, with retrospective integrated)

## Consequences

- **Unblocks:**
  - Universal applicability across project types (constraint from D-001 satisfied).
  - User-requested planning and PM capabilities from the start.
  - Memorable standards count.
- **Locks in:**
  - The `01`...`07` numbering. Reordering is expensive (links and references break).
  - "Universal applicability" as a hard constraint. Domain-specific rules go elsewhere.
- **Cost:**
  - Existing dev-standards users need migration: their AGENTS.md and references update.
  - Some loss of guidance for projects that needed C9-C12 — mitigated by the option of profiles in M3+.
- **Open questions:**
  - Whether profiles return in M3 or later, and which ones (likely candidates: web-app profile bundling C9 + C10 + C11; library profile bundling parts of C12).
  - Whether language-specific dependency guidance (the deferred C7) becomes its own `languages/{lang}.md` or stays in `AGENTS.md` per project.

## What was dropped or deferred

| dev-standards rule | Status in aegis M1 | Reason |
|---|---|---|
| C1 scope-and-applicability | Refined into `01-conventions.md` | Conventions discipline reframed for universality |
| C2 repository-contract | Folded into `01-conventions.md` | Overlapped with AGENTS.md convention |
| C3 configuration-and-environment | **Dropped** (may return as adapter or profile) | Domain-specific |
| C4 change-management | Refined into `05-changes.md` | Universal; tightened |
| C5 decision-records | Refined into `02-decisions.md` | Universal; format clarified |
| C6 problem-tracking | Folded into `04-verification.md` § Follow-ups | Overlapped |
| C7 dependency-management | **Deferred to languages/** | Language-ecosystem-specific |
| C8 verification | Refined into `04-verification.md` | Universal |
| C9 interfaces-and-contracts | **Dropped** (may return as profile) | Assumes API surface |
| C10 observability | **Dropped** (may return as profile) | Assumes runtime services |
| C11 security-and-risk | **Dropped** (may return as profile) | Maturity-dependent |
| C12 documentation-and-discoverability | **Dropped** (overlapped with 02-decisions and AGENTS.md) | Redundancy |
| C13 learn-from-friction | Refined into `07-learn-from-friction.md` | Universal; retrospective integrated |
| C14 concept-hub | **Dropped** | Unclear value, academic framing |
| P1-P4 profiles | **Deferred** | No demand demonstrated yet |
| A0-A6 adapters | **Restructured** | aegis adapters are per-agent (claude-code, codex, mcp), not per-tool category |
| (new) Plan records | **Added as 03-plans.md** | User requested high-quality planning |
| (new) Milestones | **Added as 06-milestones.md** | User requested project management |

## References

- Charter: [D-001](D-001-project-charter.md)
- Architecture: [D-002](D-002-architecture-monolith-dual-agent-mcp.md)
- Predecessor rules: dev-standards `core/`, `profiles/`, `adapters/` (to be archived)
