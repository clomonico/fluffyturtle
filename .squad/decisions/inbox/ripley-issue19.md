# Decision: Control Matrix Classification Validated

**Date:** 2026-03-02
**By:** Ripley (Lead)
**Issue:** #19
**PR:** #57

## What

Audited the Bobs' control matrix against all 8 stakeholder interviews. Confirmed the 3 mandatory / 4 elective tier classification is accurate and supported by operational evidence. Documented that all 4 elective tiers are removable, but Tier 3 (compliance/COI check) has active compliance gaps that must be closed as part of any simplification.

## Why

The control matrix existed for 18 months but nobody read it. The team needed an evidence-backed validation, not just a matrix reference, to proceed with confidence on tier removal. The audit also surfaced gaps the matrix doesn't cover (PO-splitting bypass, Milton's off-system role) that affect the risk profile of simplification.

## Impact on Squad

- Dallas (process design) can use this as the structural foundation for the 3-tier redesign
- Lambert (stakeholder analysis) should note that Tom's resistance will likely center on Tiers 4 and 5, and Samir's removal depends on Initiative 3 delivery
- Parker (technical planning) should account for Milton's vendor check needing system formalization
- Kane (validation) should use the discrepancies section to design parallel-run test scenarios, especially the PO-splitting compliance gap

## Open Question

The Bobs confirmed the classification but won't recommend the removal themselves ("who approved weakening the controls?"). The squad needs to decide who owns the recommendation to remove elective tiers. Options: frame it as the Bobs' matrix being adopted (they wrote it), or have the VP of Operations own the decision with Bobs' sign-off on the parallel-run results.
