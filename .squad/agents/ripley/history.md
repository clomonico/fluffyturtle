# Ripley — History

## Project Context
**Project:** Approval Process Redesign
**User:** CB Lomonico
**Stack:** Design thinking, process architecture, documentation
**Description:** Redesigning a 7-tier approval process (only 3 are regulatory). 5 initiatives to work through. 8 stakeholders interviewed. Key finding: this is a process trust and visibility problem, not a tooling problem.

## Learnings

### 2026-03-02: Initiative 1 Scoping (Clear the Path)

**Architecture decisions:**
- The 4 elective tiers map to: Tom's 2 legacy patches, Samir's self-imposed secondary review, Lumbergh's informal "loop me in" gate. All four are removable per the Bobs' control matrix.
- The 3 mandatory tiers are: budget holder verification, compliance/conflict-of-interest check, final audit sign-off. These are the load-bearing walls.
- Lumbergh's gate gets replaced with a dashboard (visibility, not approval authority).
- Documentation completeness enforcement at intake addresses the Bobs' 60% incomplete-docs finding.
- Parallel-run validation (4–6 weeks) is non-negotiable for proving the elective tiers are truly elective.

**Patterns observed:**
- Tom will resist through "risk concern" framing. Include him early as edge-case reviewer to channel resistance productively.
- Samir's redundant review removal is reinforced by Initiative 3 (Glass Floor) — upstream visibility receipts make the removal cleaner.
- Initiative 1 and Initiative 3 are complementary but independently executable.
- Initiative 4 (One Front Door) shares the intake-enforcement concern — design coordination needed but no hard blocker.

**Key file paths:**
- Scope document: `docs/project/initiatives/01-clear-the-path/00-initiative-scope.md`
- Research context: `docs/design-thinking/context/2026-03-02-02-input-synthesis.md` (themes), `2026-03-02-04-implementation-options.md` (options), `2026-03-02-01-stakeholder-map.md` (stakeholder dynamics), `2026-03-02-03-hmw-and-brainstorm.md` (brainstorm ideas)

**User preferences:**
- Direct, colleague-to-colleague tone. No buzzwords. End with options, not directives.

### 2026-03-02: Issue #19 — Control Matrix Audit

**What was done:**
- Reconstructed the full 7-tier approval chain from all interview transcripts
- Classified each tier as mandatory (regulatory) or elective (organizational)
- Cross-referenced the Bobs' control matrix against operational reality
- Documented 5 discrepancies between what the Bobs say and what actually happens
- Created deliverable at `docs/project/initiatives/01-clear-the-path/epic-07-control-matrix/19-control-matrix-audit.md`

**Key findings:**
- The Bobs' control matrix is accurate: 3 mandatory, 4 elective. Published 18 months ago, nobody read it.
- Tom's 2 tiers (routing review + exception path) are the weakest. Peter skips both on 70% of requests. Even Tom admits the exception path is over-scoped.
- Samir's secondary review is a symptom of missing upstream visibility, not a real control gap. He described his own fix (verification receipts) in the interview.
- Lumbergh's gate costs 1-2 days per request. A dashboard solves his actual need.
- Tier 3 (compliance/COI check) has active gaps: PO-splitting bypasses the threshold trigger, and Milton operates off-system with no RACI entry. These gaps must be closed before any elective tier removal.
- Milton is invisible but load-bearing. His vendor terms check maps to Tier 3 but exists only in a personal spreadsheet and informal email routing.

**Patterns observed:**
- Evidence-based classification is more persuasive than matrix-based classification. The matrix was right but nobody read it. The interview evidence makes the same case with operational proof.
- "Temporary" patches are the most dangerous tier type. They accumulate without review and their creators eventually can't explain the rationale without consulting their own files.
