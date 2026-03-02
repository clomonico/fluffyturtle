# Verification Receipt Field Specification

**Issue:** #31 — Add verification receipt fields to each approval step
**Initiative:** 2 — Glass Floor
**Epic:** 11 — Verification Chain
**Date:** 2026-03-02
**Author:** Dallas (Process Designer)

---

## Problem Statement

The Bobs flagged that 60% of approved requests have incomplete documentation. Approvers click "approve" without recording what they actually checked. Downstream approvers have no visibility into upstream decisions, which forces duplication. Samir re-verifies everything from scratch because the system shows him nothing about prior steps.

> "If I could see that the budget line was verified at step 2 with a timestamp and a name, I wouldn't need to re-do it at step 5." — Samir

This spec defines the structured verification receipt fields that must be completed at each of the 3 mandatory approval tiers before an approver can submit their decision. The goal is to make "approved" mean something specific and visible.

---

## Scope

Only the 3 mandatory tiers that survive Initiative 1 (Clear the Path) are in scope.

| Tier | Name | Primary Approver | Purpose |
|------|------|-----------------|---------|
| 1 | Requestor Verification | Department Head | Verify the request makes sense and documentation is complete |
| 2 | Budget Authority Confirmation | Samir (Budget Authority) | Verify funds availability, budget line alignment, spend authority |
| 3 | Compliance / COI Check | Compliance + Milton (vendor terms) | Conflict of interest, vendor terms, regulatory compliance |

---

## 1. Field Definitions by Tier

### Tier 1 — Requestor Verification (Department Head)

| Field | Entry Type | Required | Description |
|-------|-----------|----------|-------------|
| `request_coherence_check` | Manual (dropdown) | Yes | Confirms the request is internally consistent. Values: `Verified consistent`, `Verified with corrections noted`, `Returned for revision` |
| `coherence_notes` | Manual (text, 500 char) | Conditional | Required when value is `Verified with corrections noted`. Free-text description of what was corrected. |
| `documentation_completeness` | Manual (checklist) | Yes | Checklist of required attachments per request type. All items must be checked. |
| `missing_docs_action` | Manual (dropdown) | Conditional | Required when any checklist item is unchecked. Values: `Waived with justification`, `Returned to requestor` |
| `waiver_justification` | Manual (text, 500 char) | Conditional | Required when `missing_docs_action` is `Waived with justification` |
| `verification_method_t1` | Manual (dropdown) | Yes | How verification was performed. Values: `Reviewed submitted documents`, `Consulted with requestor`, `Verified against prior request`, `Other` |
| `verification_method_other` | Manual (text, 250 char) | Conditional | Required when `verification_method_t1` is `Other` |
| `evidence_reference_t1` | Manual (text, 250 char) | Yes | Reference to supporting evidence (e.g., "PO-2026-0412", "Email from J. Smith 2026-02-28") |
| `approver_id` | Auto | Yes | System-captured identity of the approver |
| `approval_timestamp` | Auto | Yes | System-captured UTC timestamp of submission |
| `approval_decision_t1` | Manual (dropdown) | Yes | Values: `Approved`, `Returned for revision`, `Escalated` |

### Tier 2 — Budget Authority Confirmation (Samir)

| Field | Entry Type | Required | Description |
|-------|-----------|----------|-------------|
| `funds_availability_check` | Manual (dropdown) | Yes | Confirms funds are available. Values: `Funds confirmed available`, `Partial funds available`, `Funds not available` |
| `budget_line_reference` | Manual (text, 100 char) | Yes | The specific budget line item verified (e.g., "OPEX-2026-IT-0400") |
| `budget_line_verified` | Manual (dropdown) | Yes | Confirms alignment between request and budget line. Values: `Aligned`, `Misaligned — corrected`, `Misaligned — returned` |
| `budget_correction_notes` | Manual (text, 500 char) | Conditional | Required when value is `Misaligned — corrected`. What was corrected. |
| `spend_authority_confirmed` | Manual (dropdown) | Yes | Confirms requestor has spend authority for this amount. Values: `Within authority`, `Exceeds authority — escalated`, `Exceeds authority — waived with justification` |
| `authority_waiver_justification` | Manual (text, 500 char) | Conditional | Required when spend authority is waived |
| `amount_verified` | Manual (currency) | Yes | The dollar amount verified against the budget |
| `verification_method_t2` | Manual (dropdown) | Yes | How verification was performed. Values: `Checked budget system`, `Confirmed with finance`, `Reviewed prior allocation`, `Other` |
| `verification_method_other_t2` | Manual (text, 250 char) | Conditional | Required when `verification_method_t2` is `Other` |
| `evidence_reference_t2` | Manual (text, 250 char) | Yes | Reference to supporting evidence (e.g., "Budget report Q1-2026, line 412") |
| `upstream_review_acknowledged` | Auto (boolean) | Yes | System confirms Tier 1 receipt was displayed to this approver before action |
| `approver_id` | Auto | Yes | System-captured identity of the approver |
| `approval_timestamp` | Auto | Yes | System-captured UTC timestamp of submission |
| `approval_decision_t2` | Manual (dropdown) | Yes | Values: `Approved`, `Returned to prior tier`, `Escalated` |

### Tier 3 — Compliance / COI Check

| Field | Entry Type | Required | Description |
|-------|-----------|----------|-------------|
| `coi_check_result` | Manual (dropdown) | Yes | Conflict of interest determination. Values: `No conflict identified`, `Conflict identified — mitigated`, `Conflict identified — blocked` |
| `coi_mitigation_notes` | Manual (text, 500 char) | Conditional | Required when conflict is mitigated. Describe the mitigation. |
| `vendor_terms_reviewed` | Manual (dropdown) | Yes | Milton's vendor terms check. Values: `Standard terms confirmed`, `Non-standard terms — accepted with justification`, `Non-standard terms — rejected`, `Not applicable (no vendor)` |
| `vendor_terms_justification` | Manual (text, 500 char) | Conditional | Required when non-standard terms are accepted |
| `vendor_exception_reference` | Manual (text, 100 char) | Conditional | Reference to Milton's vendor exception record, if applicable |
| `regulatory_compliance_check` | Manual (dropdown) | Yes | Values: `Compliant`, `Compliant with conditions`, `Non-compliant — blocked` |
| `compliance_conditions` | Manual (text, 500 char) | Conditional | Required when `Compliant with conditions`. State the conditions. |
| `verification_method_t3` | Manual (dropdown) | Yes | Values: `Policy lookup`, `Vendor database check`, `Legal consultation`, `Prior precedent`, `Other` |
| `verification_method_other_t3` | Manual (text, 250 char) | Conditional | Required when `verification_method_t3` is `Other` |
| `evidence_reference_t3` | Manual (text, 250 char) | Yes | Reference to supporting evidence (e.g., "Policy §4.2.1", "Vendor record V-2026-088") |
| `upstream_review_acknowledged` | Auto (boolean) | Yes | System confirms Tier 1 and Tier 2 receipts were displayed before action |
| `approver_id` | Auto | Yes | System-captured identity of the approver |
| `approval_timestamp` | Auto | Yes | System-captured UTC timestamp of submission |
| `approval_decision_t3` | Manual (dropdown) | Yes | Values: `Approved`, `Returned to prior tier`, `Blocked` |

---

## 2. Required vs. Optional Fields Summary

All fields listed above are **required** at their respective tiers. There are no optional fields in this spec.

Conditional fields become required when their trigger condition is met (e.g., selecting "Other" makes the free-text explanation required). This enforces the principle that approvers cannot proceed without recording what they verified.

**Rule:** The "Approve" / "Escalate" / "Return" action button is disabled until all required and conditionally-triggered fields are completed.

---

## 3. Data Model

### Field Types

| Type | Storage | Constraints |
|------|---------|-------------|
| `dropdown` | `VARCHAR(100)` | Enumerated values only; validated server-side |
| `text` | `VARCHAR(n)` where n = max chars | UTF-8, trimmed, min 10 characters for free-text fields |
| `checklist` | `JSON array of booleans` | Must match the request-type-specific checklist template |
| `currency` | `DECIMAL(12,2)` | Positive values only, max 999,999,999.99 |
| `boolean` | `BOOLEAN` | System-set only; not user-editable |
| `identity` | `VARCHAR(100)` | System-resolved from authenticated session (email or employee ID) |
| `timestamp` | `DATETIME` | UTC, ISO 8601 format, system-generated at submission |

### Entity Relationship

```
Request (1) ──── has many ────▶ VerificationReceipt (1 per tier)
VerificationReceipt (1) ──── belongs to ────▶ ApprovalTier (enum: T1, T2, T3)
VerificationReceipt (1) ──── has many ────▶ ReceiptField (field name + value pairs)
```

### Table: `verification_receipt`

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| `receipt_id` | `INT AUTO_INCREMENT` | No | Primary key |
| `request_id` | `INT` | No | FK to the parent request |
| `tier` | `ENUM('T1','T2','T3')` | No | Which approval tier |
| `approver_id` | `VARCHAR(100)` | No | Authenticated user identity |
| `approval_decision` | `VARCHAR(100)` | No | The decision value |
| `submitted_at` | `DATETIME` | No | UTC timestamp |
| `field_data` | `JSON` | No | All manual-entry fields as key-value JSON |
| `upstream_acknowledged` | `BOOLEAN` | No | Whether upstream receipts were displayed (T2, T3 only; always TRUE for T1) |

**Index:** Composite index on `(request_id, tier)` with a unique constraint (one receipt per tier per request).

---

## 4. Auto-Captured vs. Manual Entry Fields

### Auto-Captured (system-populated, not editable by users)

| Field | Source | When Captured |
|-------|--------|---------------|
| `approver_id` | Authenticated session (SSO / directory) | At form submission |
| `approval_timestamp` | Server clock (UTC) | At form submission |
| `upstream_review_acknowledged` | System flag set when upstream receipt panel is rendered and viewed | When the approver opens the approval form (Tier 2 and Tier 3 only) |

### Manual Entry (approver completes before submission)

All other fields listed in Section 1. These represent the "what was checked" and "how it was checked" answers that are currently missing from the approval process.

---

## 5. Validation Rules

### Global Rules (apply to all tiers)

1. **No empty required fields.** The submit action is disabled until all required fields and all conditionally-triggered fields are populated.
2. **Minimum text length.** Free-text fields require at least 10 characters to prevent placeholder entries like "ok" or "done."
3. **Dropdown values are server-validated.** Client-side selection is re-checked server-side to prevent injection of invalid values.
4. **Immutable after submission.** Once a receipt is submitted, fields cannot be edited. Corrections require a new approval cycle (return and re-approve).
5. **Upstream acknowledgment gate.** Tier 2 and Tier 3 forms will not render the decision fields until the system confirms the approver has viewed the upstream receipt panel.

### Tier-Specific Rules

| Tier | Rule |
|------|------|
| T1 | If any documentation checklist item is unchecked, `missing_docs_action` becomes required. If action is "Waived," justification is required. |
| T2 | `amount_verified` must be greater than $0. If `spend_authority_confirmed` is "Exceeds authority — waived," justification is required and the receipt is flagged for audit. |
| T3 | If `vendor_terms_reviewed` is "Non-standard terms — accepted," both justification and `vendor_exception_reference` are required. If `coi_check_result` is "Conflict identified — mitigated," `coi_mitigation_notes` are required and the receipt is flagged for audit. |

### Anti-Gaming Safeguards

- Time-on-form tracking: if form is submitted in under 30 seconds, a warning is logged (not blocked, but flagged for audit review).
- Repeated identical free-text entries across multiple requests by the same approver are flagged in a weekly audit report.

---

## 6. Downstream Display Format

When an approver at Tier 2 or Tier 3 opens a request, the upstream verification receipts are displayed in a read-only panel above the approval form.

### Receipt Card Layout (per tier)

```
┌─────────────────────────────────────────────────────┐
│  TIER [N] — [Tier Name]                             │
│  Approved by: [approver_id]                         │
│  Timestamp: [approval_timestamp, local time]        │
├─────────────────────────────────────────────────────┤
│  Decision: [approval_decision]                      │
│                                                     │
│  What was checked:                                  │
│  • [field_label]: [field_value]                     │
│  • [field_label]: [field_value]                     │
│  ...                                                │
│                                                     │
│  Verification method: [verification_method value]   │
│  Evidence: [evidence_reference value]               │
│                                                     │
│  ⚠ Flags: [any audit flags, if present]            │
└─────────────────────────────────────────────────────┘
```

### Display Rules

- Receipts are ordered by tier (T1 first, then T2).
- Conditional fields that were not triggered are omitted from display (no empty rows).
- Audit-flagged items (spend authority waivers, COI mitigations, fast submissions) show a yellow warning indicator.
- The panel is collapsible but defaults to expanded on first view (required for `upstream_review_acknowledged` to fire).
- Timestamps display in the viewer's local timezone with UTC in a tooltip.

---

## 7. Migration Plan for Existing Data

### Current State

Existing approved requests have no verification receipt data. Historical records contain only approver identity and timestamp (when captured). The Bobs report 60% have incomplete or absent documentation.

### Migration Approach

| Phase | Action | Timeline |
|-------|--------|----------|
| 1 — Schema deployment | Add `verification_receipt` table and related objects to the database. No application changes yet. | Week 1 |
| 2 — Backfill stub records | For all active (in-flight) requests, generate stub receipt records with `field_data` set to `{"migrated": true, "note": "Pre-verification-receipt request"}` and existing approver/timestamp data where available. | Week 1 |
| 3 — Feature flag rollout | Enable verification receipt fields behind a feature flag. Initially on for a pilot group (one department). | Week 2 |
| 4 — Full rollout | Remove feature flag; all new approvals require receipt fields. | Week 4 (after pilot validation) |

### Historical Data

- Historical approved requests will not be backfilled with verification data (it does not exist).
- A `migrated` flag in the receipt JSON distinguishes legacy stubs from real receipts.
- Reporting dashboards will filter on `migrated = false` for verification completeness metrics.

---

## 8. Testing Requirements

Per acceptance criteria, all active approval step types must be tested.

### Test Matrix

| Test Category | Scenario | Expected Result | Tier(s) |
|---------------|----------|-----------------|---------|
| Required fields | Submit with one required field empty | Submit button remains disabled | T1, T2, T3 |
| Conditional fields | Select "Other" for verification method, leave explanation blank | Submit button remains disabled | T1, T2, T3 |
| Conditional fields | Select "Waived with justification" for missing docs, leave justification blank | Submit button remains disabled | T1 |
| Minimum text length | Enter "ok" in a free-text field (2 chars) | Validation error displayed | T1, T2, T3 |
| Auto-capture: identity | Submit a receipt and verify `approver_id` matches authenticated user | Identity matches SSO session | T1, T2, T3 |
| Auto-capture: timestamp | Submit a receipt and verify `approval_timestamp` is within 2 seconds of server time | Timestamp accurate | T1, T2, T3 |
| Upstream acknowledgment | Open T2 form without viewing T1 receipt panel | Decision fields are not rendered | T2 |
| Upstream acknowledgment | Open T3 form, view T1 and T2 receipt panels | Decision fields render; `upstream_review_acknowledged` = true | T3 |
| Downstream display | Complete T1, then open T2 form | T1 receipt card is visible in read-only panel | T2 |
| Downstream display | Complete T1 and T2, then open T3 form | T1 and T2 receipt cards visible | T3 |
| Immutability | Attempt to edit a submitted receipt via API | 403 Forbidden | T1, T2, T3 |
| Anti-gaming | Submit a receipt in under 30 seconds | Submission succeeds but audit flag is logged | T1, T2, T3 |
| Anti-gaming | Submit 5 requests with identical free-text in a field | Weekly audit report includes a flag | T1, T2, T3 |
| Server-side validation | Submit a dropdown field with a value not in the enum list (via API) | 400 Bad Request | T1, T2, T3 |
| Migration stubs | Query a pre-migration request's receipt | Stub record returned with `migrated: true` | T1, T2, T3 |
| Currency field | Enter a negative value in `amount_verified` | Validation error displayed | T2 |
| Decision gating | All fields valid, submit "Approved" | Receipt saved, request advances to next tier | T1, T2, T3 |

### Acceptance Test Run Requirements

- All scenarios must pass on each of the 3 tiers.
- Tests must cover at least 3 request types (e.g., new purchase, renewal, vendor change).
- One full end-to-end run: request submitted, T1 receipt completed, T2 sees T1 receipt and completes T2, T3 sees T1+T2 receipts and completes T3.
- Regression: existing approval flows without verification fields must continue to work for in-flight requests during migration.

---

## 9. Implementation Notes for Michael Bolton (IT Lead)

### Platform Considerations

The current platform is described as "brittle." The following guidance applies.

1. **Use the existing form engine.** The platform already supports custom fields on approval forms. Add receipt fields as a new field group per tier rather than building a separate form. This minimizes integration risk.

2. **Feature flag the rollout.** Wrap verification receipt fields in a feature flag so they can be enabled per department during pilot and disabled immediately if issues emerge. Do not deploy all-at-once.

3. **JSON storage for field data.** Store manual-entry fields as a JSON blob in the `field_data` column rather than adding 30+ individual columns. This avoids schema sprawl and makes it easier to add or modify fields per tier without database migrations.

4. **Upstream receipt panel is read-only.** The panel that displays upstream receipts to downstream approvers must be rendered server-side (not client-side) to prevent tampering. It pulls from `verification_receipt` records in the database.

5. **Immutability enforcement.** Once a receipt row is inserted, the application layer must reject all UPDATE requests on that row. Add a database trigger or application middleware guard. This is a compliance requirement.

6. **Auto-capture fields are never user-supplied.** `approver_id` and `approval_timestamp` must be set server-side from the authenticated session and server clock. Never trust client-submitted values for these fields.

7. **Test in a staging environment first.** Given the platform's fragility, deploy to staging and run the full test matrix (Section 8) before touching production. Allow at least one week of staging validation with real users from the pilot group.

8. **Document the configuration.** Every field, validation rule, and display setting added to the platform must be documented in a configuration log. Tom's experience shows that undocumented changes become invisible technical debt.

### Effort Estimate

| Task | Estimated Effort |
|------|-----------------|
| Schema + table creation | 2 hours |
| Tier 1 field group configuration | 4 hours |
| Tier 2 field group configuration | 4 hours |
| Tier 3 field group configuration | 4 hours |
| Upstream receipt display panel | 8 hours |
| Validation rules (client + server) | 6 hours |
| Feature flag setup | 2 hours |
| Migration script (stubs) | 3 hours |
| Staging test execution | 8 hours |
| Pilot monitoring (1 week) | ~4 hours of spot-checks |
| **Total** | **~45 hours** |

---

## Dependencies

| Dependency | Status | Impact if delayed |
|------------|--------|-------------------|
| Initiative 1 (Clear the Path) tier reduction | In progress | Spec is already scoped to surviving 3 tiers; no impact unless tier count changes |
| Milton's vendor exception records (#25) | In progress | Tier 3 `vendor_exception_reference` field references Milton's data; can proceed with placeholder format |
| Documentation checklist by request type (#25) | In progress | Tier 1 `documentation_completeness` checklist depends on this; can use a draft checklist for pilot |

---

## DT Source

- **Session:** 2026-03-02 HVE Design Thinking
- **Scenario:** Glass Floor (Option 3)
- **Route:** Frozen Request → Scope Conversations → Design Research → SCAMPER Brainstorm → Implementation Options
- **Key inputs:** Samir interview (upstream visibility quote), The Bobs interview (60% incomplete documentation), Michael Bolton interview (platform capabilities and constraints)
