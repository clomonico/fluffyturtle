# Lambert — History

## Project Context
**Project:** Approval Process Redesign
**User:** CB Lomonico
**Stack:** Design thinking, process architecture, documentation
**Description:** Redesigning a 7-tier approval process (only 3 are regulatory). 5 initiatives with varying timelines: Clear the Path (Weeks), Smart Routing (Months), Glass Floor (Weeks), One Front Door (Months), Flip the Default (Quarters).

## Learnings

### 2026-03-02: Initiative 1 — Clear the Path Implementation Plan

**Key file:** `docs/project/initiatives/01-clear-the-path/02-implementation-plan.md`

**Architecture decisions:**
- 5-week timeline (4 phases: Preparation, Configuration, Parallel Run, Cutover) with 1-week contingency buffer
- Manual dual-tracking for parallel run instead of technical dual-routing, because Michael Bolton flagged the platform as fragile. Running two routing configs simultaneously is riskier than having Peter compare outcomes manually.
- Tiers set to "Inactive" rather than deleted, preserving rollback capability
- Dashboard replaces Lumbergh's informal gate. Read-only access, no approval action from the dashboard.
- Intake form enforces documentation completeness to address the Bobs' 60% incomplete documentation finding

**Patterns observed:**
- Michael Bolton is the single point of failure for all technical changes. No backup for his platform knowledge. This is a schedule risk for every initiative.
- Tom Smykowski's resistance is predictable but manageable if he's included visibly in Phase 1. Exclusion makes it worse.
- Peter's shadow workflow documentation (Phase 1) becomes a direct input to Initiative 4 (One Front Door). Cross-initiative dependency to track.
- Initiative 3 (Glass Floor) will configure parallel routing and auto-escalation on the same platform. Must coordinate with Init 1's configuration changes to avoid conflicts.

**Key context files used:**
- `docs/design-thinking/context/2026-03-02-04-implementation-options.md`
- `docs/design-thinking/context/2026-03-02-02-input-synthesis.md`
- `docs/design-thinking/context/2026-03-02-01-stakeholder-map.md`
- `docs/design-thinking/context/2026-03-02-03-hmw-and-brainstorm.md`

### 2026-03-02: Issue #22 — Parallel-Run Plan for Tier Removal

**Key file:** `docs/project/initiatives/01-clear-the-path/epic-08-tier-removal/22-parallel-run-plan.md`

**Architecture decisions:**
- Extended parallel run from 1 week (original impl plan Phase 3) to 2 weeks. Single week risks insufficient sample size at 15-25 requests/week volume. Two weeks captures more request types and edge cases including Milton's 47 vendor exceptions.
- Maximum extension: 4 weeks total. Beyond that, the initiative pauses for root cause analysis rather than continuing to burn dual-tracking resources.
- Bias mitigation for Peter's dual-tracking: the Bobs spot-check 3 randomly selected "no variance" cases per day, not just the flagged ones. Prevents confirmation bias in manual comparison.
- Michael Bolton backup operator: identified as a must-do before parallel run starts. He is still the single point of failure; this plan addresses it by requiring him to document monitoring and rollback procedures for a backup IT person.
- Four decision outcomes at Go/No-Go instead of binary: Go, Conditional Go, No-Go Extend, No-Go Pause. Gives Lumbergh graduated options instead of forcing an all-or-nothing call.

**Patterns observed:**
- The original implementation plan's Phase 3 (1-week parallel run) was underspecified. This plan fills the gap with structured data collection, comms cadence, and decision criteria.
- Tom Smykowski risk remains high during parallel run. Giving him a RACI role (elective tier risk documentation) keeps him engaged and makes his concerns visible rather than disruptive.
- Vendor exception scenarios (Milton's list) are a sleeper risk. Requests touching those vendors may have relied on judgment from elective tiers. Milton's active review during the parallel run is the mitigation.

### 2026-03-02: Issue #26 — Intake Validation Spec

**Key file:** `docs/project/initiatives/01-clear-the-path/epic-09-intake/26-intake-validation-spec.md`

**Architecture decisions:**
- Validation is a hard block (not a soft warning) with a 30-day grace period offering a 48-hour correction window. After grace period, immediate rejection. This matches Dallas's process design decision from the intake gate.
- Phased rollout across 6 request types: Procurement first (highest volume, most of the Bobs' 60% finding), then Software License/Renewal, then Budget Reallocation + Vendor Onboarding, then Facilities + Credit Card. Each phase has Go/No-Go criteria before expanding.
- Integration endpoint availability for budget code and vendor master lookups is the biggest scope expansion risk. Estimated 12+ hours additional effort if endpoints don't exist and must be built. Interim mitigation: nightly CSV sync to local lookup table.
- Per-field rollback capability: each field's required flag is an independent toggle. No all-or-nothing switch. If one validation rule causes excessive friction, it can be relaxed without disabling the entire gate.
- Error messages designed to tell requestors what's missing AND where to find it (linking to Finance Portal, IT Help Desk, etc.). Consolidated error display — all errors shown at once, not one-at-a-time.

**Patterns observed:**
- Michael Bolton single-point-of-failure risk is amplified here. The estimated 33-57 hours of configuration work all flows through him. Mitigation: document all config steps in Section 8 so a backup admin can execute.
- Kane's earlier warning about intake gates becoming bypass magnets is directly relevant. If validation is too strict, requestors route spend through credit cards and informal channels — worsening the compliance gap. Phased rollout and feedback loops are the countermeasure.
- Issue #25 (Dallas's documentation checklist) is a hard dependency. If #25 changes field requirements, this spec's validation rules must update. The two issues should stay synchronized.
- Credit card reconciliation and Facilities request types are forward-looking scope from Initiative 4 (One Front Door). Including them in the validation spec now avoids rework later but creates a coordination dependency.
