# Living Decision Log Specification

**Issue:** #32 — Build living decision log capturing approval rationale
**Initiative:** 2 — Glass Floor
**Epic:** 11 — Verification Chain
**Date:** 2026-03-02
**Author:** Parker (Stakeholder Analyst)
**Depends on:** #31 (Verification Receipt Fields)

---

## Problem Statement

The Bobs flagged that 60% of approved requests have incomplete documentation, and the current system records only the final decision ("approved" or "rejected") without capturing what was actually verified or why. Peter's shadow workflow has no audit trail at all. When The Bobs audit a request, they must reconstruct rationale from emails, hallway conversations, and memory.

> "We need to see not just that it was approved, but what was checked, what was considered, and why the approver decided the way they did." — The Bobs

This spec defines a structured, immutable decision log that records approval rationale at every step across the 3 mandatory tiers. Every approve, reject, flag, escalate, or delegate action generates a log entry that cannot be modified after the fact. The log feeds into Process Analytics (Epic #14) for data-driven improvement.

---

## Scope

The decision log captures events from the 3 mandatory tiers surviving Initiative 1.

| Tier | Name | Primary Approver |
|------|------|-----------------|
| 1 | Requestor Verification | Department Head |
| 2 | Budget Authority Confirmation | Samir (Budget Authority) |
| 3 | Compliance / COI Check | Compliance + Milton (vendor terms) |

Each tier generates one or more decision log entries per request. The log is append-only and immutable once written.

---

## 1. Decision Log Data Model

Each decision event produces a single log record with the following fields.

### 1.1 Core Identity Fields

| Field | Type | Source | Description |
|-------|------|--------|-------------|
| `log_entry_id` | UUID | Auto | Globally unique identifier for this log entry |
| `request_id` | String | Auto | The request this decision applies to |
| `receipt_id` | UUID | Auto | Links to the verification receipt from Story #31 |
| `sequence_number` | Integer | Auto | Monotonically increasing per request, establishes decision order |
| `tier_number` | Integer (1-3) | Auto | Which mandatory tier produced this decision |
| `tier_name` | String | Auto | Human-readable tier name |

### 1.2 Actor Fields

| Field | Type | Source | Description |
|-------|------|--------|-------------|
| `actor_id` | String | Auto | System-captured identity of the person taking action |
| `actor_name` | String | Auto | Display name of the actor |
| `actor_role` | String | Auto | Role at time of action (e.g., "Budget Authority", "Compliance Officer") |
| `delegate_of` | String | Auto | If acting as delegate, the original assignee's ID. Null otherwise |
| `delegation_reason` | String (250 char) | Manual | Required when `delegate_of` is populated. Why the original assignee delegated |

### 1.3 Decision Fields

| Field | Type | Source | Description |
|-------|------|--------|-------------|
| `decision_type` | Enum | Manual | One of: `approve`, `reject`, `flag_hold`, `escalate`, `delegate` |
| `decision_timestamp` | DateTime (UTC) | Auto | System-captured time the decision was recorded |
| `rationale_category` | Enum | Manual | Primary reason category (see Section 3.2) |
| `rationale_text` | String (2000 char) | Manual | Structured free-text explanation of reasoning. Minimum 50 characters |
| `conditions_attached` | Boolean | Manual | Whether the decision carries conditions |
| `condition_text` | String (1000 char) | Manual | Required when `conditions_attached` is true. Describes the conditions |
| `risk_flags` | Array of Enum | Manual | Zero or more risk flags applied (see Section 3.3) |

### 1.4 Evidence and Context Fields

| Field | Type | Source | Description |
|-------|------|--------|-------------|
| `evidence_references` | Array of String | Manual | List of evidence cited (e.g., "PO-2026-0412", "Budget report Q1-2026") |
| `upstream_decisions_reviewed` | Array of UUID | Auto | Log entry IDs of prior-tier decisions displayed to this actor before action |
| `verification_fields_snapshot` | JSON | Auto | Frozen copy of verification receipt field values at time of decision |
| `request_amount` | Currency | Auto | Dollar amount of the request at time of decision |
| `request_type` | String | Auto | Type of request (procurement, travel, etc.) |

### 1.5 Integrity Fields

| Field | Type | Source | Description |
|-------|------|--------|-------------|
| `content_hash` | String (SHA-256) | Auto | Hash of all non-integrity fields in this record |
| `previous_entry_hash` | String (SHA-256) | Auto | Hash of the previous log entry for this request. Null for first entry |
| `chain_valid` | Boolean | Auto | System-verified flag that the hash chain is intact |
| `created_at` | DateTime (UTC) | Auto | Timestamp of record creation (distinct from decision timestamp for queue lag) |

---

## 2. Decision Types

### 2.1 Type Definitions

| Decision Type | Code | Description | When Used |
|---------------|------|-------------|-----------|
| Approve | `approve` | Approves the request to proceed to the next tier or to completion | Approver has verified all required fields and is satisfied |
| Reject | `reject` | Denies the request, returns it to requestor or prior tier | Verification reveals a blocking problem |
| Flag / Hold | `flag_hold` | Pauses the request for further investigation without rejecting | Something needs clarification but may not be a rejection |
| Escalate | `escalate` | Moves the decision to a higher authority | Decision exceeds the approver's authority or comfort level |
| Delegate | `delegate` | Transfers decision responsibility to another qualified actor | Original approver is unavailable or another person is better positioned |

### 2.2 Decision Type by Tier

| Tier | Approve | Reject | Flag/Hold | Escalate | Delegate |
|------|---------|--------|-----------|----------|----------|
| 1 — Requestor Verification | Yes | Yes | Yes | Yes | Yes |
| 2 — Budget Authority | Yes | Yes | Yes | Yes | Yes |
| 3 — Compliance / COI | Yes | Yes | Yes | Yes | Yes |

All decision types are available at all tiers. Escalation and delegation produce additional log entries when the receiving actor takes action.

---

## 3. Rationale Capture Requirements

### 3.1 Minimum Content Rules

| Rule | Requirement |
|------|-------------|
| Minimum length | 50 characters for `rationale_text` |
| Maximum length | 2,000 characters for `rationale_text` |
| Category required | Every decision must select exactly one `rationale_category` |
| Evidence required for rejections | At least one `evidence_reference` is required when `decision_type` is `reject` |
| Conditions required for conditional approvals | `condition_text` must be populated when `conditions_attached` is true |

The system prevents submission until minimum requirements are met. The UI shows a live character count and highlights when the minimum has not been reached.

### 3.2 Rationale Categories

Categories are scoped per tier to keep selections relevant.

**Tier 1 — Requestor Verification**
| Category Code | Label |
|---------------|-------|
| `docs_complete` | Documentation verified complete |
| `docs_incomplete_waived` | Documentation incomplete but waived with justification |
| `request_inconsistent` | Request internally inconsistent |
| `request_duplicate` | Duplicate of existing request |
| `insufficient_detail` | Insufficient detail to evaluate |

**Tier 2 — Budget Authority**
| Category Code | Label |
|---------------|-------|
| `funds_confirmed` | Funds available and aligned to budget line |
| `funds_partial` | Partial funds available, remainder identified |
| `funds_unavailable` | No funds available in identified budget line |
| `authority_exceeded` | Request exceeds spend authority |
| `budget_misaligned` | Request does not align to any budget line |
| `prior_commitment` | Budget line already committed to another request |

**Tier 3 — Compliance / COI**
| Category Code | Label |
|---------------|-------|
| `no_conflict` | No conflict of interest identified |
| `conflict_mitigated` | Conflict identified and mitigated |
| `conflict_blocked` | Conflict identified and cannot be mitigated |
| `vendor_standard` | Vendor terms are standard |
| `vendor_exception` | Non-standard vendor terms accepted with justification |
| `vendor_rejected` | Non-standard vendor terms rejected |
| `regulatory_compliant` | Passes all regulatory checks |
| `regulatory_conditional` | Compliant with conditions |
| `regulatory_blocked` | Fails regulatory compliance |

### 3.3 Risk Flags

Risk flags are optional tags an approver can attach to any decision.

| Flag Code | Label | Description |
|-----------|-------|-------------|
| `high_value` | High-value request | Request amount exceeds $50K threshold |
| `new_vendor` | New vendor | Vendor has no prior history in the system |
| `expedited` | Expedited timeline | Request has compressed timeline |
| `split_suspicion` | Possible split request | Request may be part of a split to avoid thresholds |
| `authority_boundary` | Near authority boundary | Amount is within 10% of the approver's authority limit |
| `repeat_rejection` | Previously rejected | This request or a similar one was previously rejected |
| `coi_adjacent` | COI-adjacent | Not a direct conflict but worth noting for the record |

---

## 4. Immutability Design

### 4.1 Principles

1. **Append-only storage.** Decision log entries are inserted, never updated or deleted.
2. **Hash chain.** Each entry contains a SHA-256 hash of its content and the hash of the previous entry for the same request, forming a tamper-evident chain.
3. **No edit, no delete.** The application layer has no update or delete operations for decision log records. The database user has INSERT and SELECT permissions only.
4. **Correction by amendment.** If an approver needs to correct or clarify a prior rationale, they create a new log entry of type `amendment` that references the original `log_entry_id`. The original record remains unchanged.

### 4.2 Hash Chain Structure

```
Entry 1 (Tier 1 decision):
  content_hash = SHA-256(all non-integrity fields)
  previous_entry_hash = null

Entry 2 (Tier 2 decision):
  content_hash = SHA-256(all non-integrity fields)
  previous_entry_hash = Entry 1.content_hash

Entry 3 (Tier 3 decision):
  content_hash = SHA-256(all non-integrity fields)
  previous_entry_hash = Entry 2.content_hash
```

### 4.3 Integrity Verification

| Check | Frequency | Method |
|-------|-----------|--------|
| Chain validation | On every read | System recalculates hash chain and compares to stored hashes |
| Nightly audit | Daily at 02:00 UTC | Batch job scans all entries created in the last 24 hours, validates chains, reports anomalies |
| Full chain audit | Weekly (Sunday 04:00 UTC) | Validates the entire hash chain for all requests with activity in the last 90 days |
| Tamper alert | Real-time | If any chain validation fails, an alert is sent to The Bobs and system admin immediately |

### 4.4 Amendment Records

When a correction is needed, the system creates an amendment entry.

| Field | Value |
|-------|-------|
| `decision_type` | `amendment` |
| `references_entry_id` | The `log_entry_id` of the original entry being clarified |
| `amendment_reason` | Required text (50-1000 chars) explaining what is being corrected and why |

The original entry is never modified. Audit views display amendments inline below the original entry with clear visual distinction.

---

## 5. Searchability and Reporting

### 5.1 Search Capabilities

The decision log supports the following search dimensions.

| Dimension | Field(s) | Operators |
|-----------|----------|-----------|
| Request | `request_id` | Exact match |
| Actor | `actor_id`, `actor_name` | Exact, contains |
| Decision type | `decision_type` | Exact, multi-select |
| Tier | `tier_number` | Exact, range |
| Date range | `decision_timestamp` | Between, before, after |
| Rationale category | `rationale_category` | Exact, multi-select |
| Risk flags | `risk_flags` | Contains any, contains all |
| Amount range | `request_amount` | Between, greater than, less than |
| Request type | `request_type` | Exact, multi-select |
| Conditions | `conditions_attached` | True, false |
| Full text | `rationale_text`, `condition_text` | Contains, keyword search |

### 5.2 Indexing Strategy

| Index | Fields | Purpose |
|-------|--------|---------|
| Primary | `log_entry_id` | Unique lookup |
| Request chain | `request_id`, `sequence_number` | Reconstruct full decision chain for a request |
| Actor history | `actor_id`, `decision_timestamp` | All decisions by a specific approver |
| Decision type | `decision_type`, `decision_timestamp` | Filter by outcome over time |
| Audit date range | `decision_timestamp`, `tier_number` | The Bobs' primary audit query pattern |
| Risk flag | `risk_flags` | Find flagged decisions quickly |

---

## 6. Integration with Verification Receipts (Story #31)

### 6.1 Relationship

Each decision log entry links to exactly one verification receipt via `receipt_id`. The receipt captures *what was verified* (the structured fields). The decision log captures *what was decided and why*.

```
Verification Receipt (#31)          Decision Log (#32)
┌──────────────────────────┐       ┌──────────────────────────┐
│ receipt_id               │◄──────│ receipt_id               │
│ tier fields (checklist,  │       │ decision_type            │
│   dropdowns, evidence)   │       │ rationale_category       │
│ approver_id              │       │ rationale_text           │
│ approval_timestamp       │       │ risk_flags               │
│ approval_decision        │       │ content_hash             │
└──────────────────────────┘       └──────────────────────────┘
```

### 6.2 Data Flow

1. Approver completes verification receipt fields (Story #31)
2. Approver selects decision type and writes rationale (this spec)
3. System captures a frozen snapshot of receipt fields into `verification_fields_snapshot`
4. System generates `content_hash` and links to `previous_entry_hash`
5. Both records are committed in a single transaction
6. Neither record can be modified after commit

### 6.3 Cross-Reference Rules

| Rule | Description |
|------|-------------|
| Receipt required | A decision log entry cannot be created without a completed verification receipt |
| Decision required | A verification receipt cannot be submitted without a corresponding decision log entry |
| Atomic commit | Receipt and log entry are committed together; if either fails, both roll back |
| Snapshot fidelity | The `verification_fields_snapshot` must exactly match the receipt fields at commit time |

---

## 7. Audit Reporting Views for The Bobs

### 7.1 Standard Audit Views

**View 1: Request Decision Trail**
Shows the complete decision chain for a single request, from Tier 1 through Tier 3, with all rationale text, evidence references, and risk flags. Includes amendment entries inline.

- Default sort: `sequence_number` ascending
- Includes: verification receipt field values at each step
- Export: PDF with digital signature, CSV

**View 2: Approver Decision History**
Shows all decisions made by a specific approver over a given date range.

- Filters: date range, decision type, tier, rationale category
- Summary statistics: approval rate, average rationale length, flag frequency
- Export: CSV, PDF

**View 3: Rejection and Flag Analysis**
Shows all reject and flag_hold decisions across the system for a date range.

- Filters: date range, tier, rationale category, request type
- Grouping: by rationale category, by approver, by tier
- Export: CSV, PDF

**View 4: Risk Flag Dashboard**
Shows requests with risk flags, grouped by flag type.

- Filters: flag type, date range, decision type
- Highlights: requests with multiple flags, requests flagged at multiple tiers
- Export: CSV, PDF

**View 5: Completeness Audit**
Identifies requests where rationale text is at minimum length (exactly 50 characters) or where evidence references are sparse.

- Purpose: detect "checkbox compliance" where approvers write the minimum to pass validation
- Flags: rationale under 100 characters, no evidence references on approvals, identical rationale text across multiple requests
- Export: CSV

### 7.2 Scheduled Reports

| Report | Frequency | Recipients | Content |
|--------|-----------|------------|---------|
| Weekly Decision Summary | Monday 08:00 | The Bobs | Counts by decision type, tier, category. Top risk flags. |
| Monthly Rationale Quality | 1st of month | The Bobs | Average rationale length, minimum-length percentage, repeat-text detection |
| Quarterly Chain Integrity | End of quarter | The Bobs, System Admin | Full hash chain validation results, any anomalies |
| Exception Report | Daily 07:00 | The Bobs | Any amendments, delegations, or escalations from the previous day |

---

## 8. Data Retention and Compliance

### 8.1 Retention Schedule

| Data Category | Retention Period | Justification |
|---------------|-----------------|---------------|
| Decision log entries | 7 years from request closure | Matches financial record retention requirements |
| Hash chain data | Same as decision log entries | Integrity verification requires the full chain |
| Amendment records | Same as the entry they reference | Cannot orphan amendments from originals |
| Archived entries (beyond 7 years) | Permanent cold storage | Moved to read-only archive, still queryable on request |

### 8.2 Compliance Controls

| Control | Description |
|---------|-------------|
| Access logging | Every read of a decision log entry is logged with actor, timestamp, and purpose |
| Export watermarking | Exported PDFs include a watermark with export date, requesting user, and record count |
| Role-based access | Only designated auditors (The Bobs) and system admin can access the full log. Approvers see only their own entries and the chain for requests they are assigned to |
| Segregation of duties | The system admin who manages the database cannot create decision log entries. Approvers who create entries cannot modify database permissions |

---

## 9. Historical Data Backfill Strategy

### 9.1 Approach

Existing approved requests have no rationale data. The backfill creates skeleton decision log entries for historical requests to establish baseline data and enable the hash chain going forward.

### 9.2 Backfill Phases

| Phase | Scope | Timeline | Method |
|-------|-------|----------|--------|
| Phase 1 — Active requests | All requests currently in-progress (any tier) | Week 1 post-deploy | System generates entries from existing approval records with `rationale_category` = `historical_backfill` and `rationale_text` = "Pre-decision-log approval. No rationale captured." |
| Phase 2 — Recent closed | Requests closed in the last 90 days | Weeks 2-3 | Same automated method. The Bobs can optionally annotate entries with reconstructed rationale |
| Phase 3 — Older closed | Requests closed 91-365 days ago | Weeks 4-6 | Automated skeleton entries only. No manual annotation |
| Phase 4 — Archive | Requests older than 365 days | Month 2 | Minimal skeleton entries for chain continuity. Stored in archive tier |

### 9.3 Backfill Record Format

Backfilled entries use the standard data model with these specific values:

| Field | Value |
|-------|-------|
| `decision_type` | `historical_backfill` |
| `rationale_category` | `historical_backfill` |
| `rationale_text` | "Pre-decision-log approval. No structured rationale was captured at time of decision." |
| `evidence_references` | Empty array |
| `risk_flags` | Empty array |
| `content_hash` | Calculated normally |
| `previous_entry_hash` | Calculated normally (chains still apply) |
| `actor_id` | Original approver if known, otherwise `system_backfill` |
| `decision_timestamp` | Original approval timestamp from legacy data |
| `created_at` | Timestamp of backfill operation |

### 9.4 Backfill Validation

- The Bobs review a random sample of 50 backfilled entries per phase before phase sign-off
- Hash chains are validated end-to-end after each phase
- Any discrepancies between legacy approval records and backfill entries are flagged for manual review

---

## 10. Testing and Validation Plan

### 10.1 Unit Tests

| Test | Description | Expected Result |
|------|-------------|-----------------|
| Minimum rationale length | Submit with 49-character rationale | Rejected with validation error |
| Minimum rationale length boundary | Submit with exactly 50 characters | Accepted |
| Required evidence on rejection | Submit a `reject` decision with empty `evidence_references` | Rejected with validation error |
| Required evidence on rejection — valid | Submit a `reject` with at least one evidence reference | Accepted |
| Condition text required | Set `conditions_attached` = true with empty `condition_text` | Rejected with validation error |
| Category required | Submit without `rationale_category` | Rejected with validation error |
| Hash generation | Create an entry and verify `content_hash` matches manual SHA-256 calculation | Hashes match |
| Hash chain linking | Create two entries for the same request, verify second entry's `previous_entry_hash` equals first entry's `content_hash` | Chain links correctly |
| Delegation fields | Submit `delegate` decision without `delegation_reason` | Rejected with validation error |

### 10.2 Immutability Tests

| Test | Description | Expected Result |
|------|-------------|-----------------|
| No update | Attempt SQL UPDATE on a decision log record | Operation denied (permission error) |
| No delete | Attempt SQL DELETE on a decision log record | Operation denied (permission error) |
| Amendment creates new record | Submit an amendment and verify original is unchanged | Original record intact, new amendment record created |
| Hash chain tamper detection | Manually alter a record's content and run chain validation | Validation fails, tamper alert triggered |

### 10.3 Integration Tests

| Test | Description | Expected Result |
|------|-------------|-----------------|
| Receipt linkage | Create a decision log entry and verify `receipt_id` links to a valid verification receipt | Link is valid, receipt exists |
| Atomic commit | Force a failure on the receipt write after the log entry write | Both operations rolled back |
| Upstream visibility | At Tier 2, verify `upstream_decisions_reviewed` contains Tier 1 log entry IDs | IDs match |
| Snapshot accuracy | Compare `verification_fields_snapshot` to the actual receipt fields | Exact match |
| Search results | Search by each dimension listed in Section 5.1 | Correct results returned |
| Export generation | Export View 1 as PDF | PDF generated with correct content and watermark |

### 10.4 User Acceptance Tests

| Test | Stakeholder | Scenario |
|------|-------------|----------|
| Audit trail walkthrough | The Bobs | Walk through a complete request lifecycle using View 1 and confirm all rationale is visible and the chain is intact |
| Approver experience | Samir | Complete a Tier 2 approval with rationale entry and confirm the experience is not significantly slower than today |
| Backfill review | The Bobs | Review the Phase 1 backfill sample and confirm entries are accurately reconstructed from legacy data |
| Search and report | The Bobs | Run each of the 5 standard views and confirm results match expectations |
| Tamper detection | System Admin | Demonstrate that a deliberately tampered record triggers an alert |

---

## Implementation Notes

- Coordinate with Dallas (Process Designer) on verification receipt field integration (#31)
- Rationale categories should be configurable by admin without code changes (stored in reference data table)
- Risk flags are extensible; new flags can be added without schema changes
- The Bobs should have read-only access to all views from day one
- Samir's concern about re-verification is addressed by `upstream_decisions_reviewed` showing what prior approvers already verified
- Michael Bolton to implement hash chain logic and integrity checks
- Process Analytics (Epic #14) will consume decision log data for cycle time, bottleneck, and quality metrics
