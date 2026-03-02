# Documentation Checklist by Request Type

**Date:** 2026-03-02
**Author:** Dallas (Process Designer)
**Issue:** #25
**Initiative:** 01 — Clear the Path
**Epic:** 09 — Intake Gate Design

---

## Purpose

60% of approved requests have incomplete documentation (The Bobs, both interviews). Approvers either chase missing paperwork themselves or approve without it. Both outcomes create audit risk and add days of rework cycles.

This checklist defines exactly what documentation is required for each request type before a submission can enter the approval queue. The intake gate enforces these checklists as a hard gate. If the package is incomplete, the system returns it to the requestor with a clear list of what's missing. No human reviewer touches it until the package is complete.

**Design source:** Process design Section 3 (Intake Gate), The Bobs' audit requirements, Samir's financial verification needs.

---

## Request Type Inventory

Seven request types identified from stakeholder interviews, shadow process analysis, and operational data.

| # | Request Type | Frequency | Typical Range | Key Stakeholder Concerns |
|---|---|---|---|---|
| 1 | Procurement (goods and services) | High | $500–$100K+ | The Bobs: three-bid summary required before approval, not after. Samir: budget codes frequently wrong. |
| 2 | Vendor Engagement (new or renewed) | Medium | $1K–$500K+ | Milton: 47 vendor exceptions tracked in a personal spreadsheet. The Bobs: vendor compliance status must be current. |
| 3 | Budget Reallocation | Medium | $5K–$250K | Samir: cost center mismatches are the #1 rework trigger. The Bobs: segregation of duties between requestor and authorizer. |
| 4 | Software License (new or renewal) | High | $200–$50K | Joanna: same $1,200 license renewed 4 years running still requires full review. Samir: GL account errors common. |
| 5 | Capital Expenditure | Low | $25K–$1M+ | The Bobs: three-bid summary plus capital justification. Regulatory threshold triggers at $25K. |
| 6 | Facilities and Infrastructure | Low–Medium | $500–$100K | Milton: Facilities SharePoint list bypasses the main system. The Bobs: no audit trail on shadow system. |
| 7 | Small Purchase / P-Card Reconciliation | High | $0–$2,500 | Joanna: team uses credit cards to bypass procurement. The Bobs: P-card transactions have zero approval trail. |

---

## Documentation Checklists by Request Type

### 1. Procurement (Goods and Services)

**Threshold note:** Requests over $25K require the three-bid summary. Under $25K, a single vendor quote is sufficient.

| Document | Required | Format | Why It Exists |
|---|---|---|---|
| Business justification | Yes | Free text, min 50 characters | Tier 1 (Budget Verification) needs to assess whether the spend aligns with budget intent. Addresses Bobs' finding that approvers click "approve" without documented rationale. |
| Budget line reference | Yes | Must match valid budget line in financial system | Samir: "Half the time the budget codes are wrong." System validates at intake instead of Samir catching it manually. |
| Cost estimate or vendor quote | Yes | PDF, XLSX, or image attachment | The Bobs: supporting artifact must be present before approval, not uploaded after. |
| Three-bid summary | Yes (>$25K) / Optional (<$25K) | PDF or structured form | The Bobs: "Purchase over $25K? We need the three-bid summary attached before the approval." |
| Vendor name and contract reference | Yes | Text fields; contract ref if existing vendor | Tier 2 (Compliance Review) checks vendor compliance status. |
| Conflict-of-interest declaration | Yes | Checkbox declaration | Verified at Tier 2. Prevents the PO-splitting compliance gap Milton identified. |
| Consolidated spend summary | Yes | Auto-populated or manual entry | Prevents circumvention of consolidated spend thresholds the Bobs flag as a compliance gap. |
| Delivery timeline | Optional | Date field | Helps prioritization; not an audit requirement. |

**How this addresses the 60% stat:** Budget line validation and vendor quote attachment are enforced at intake. These two items account for the majority of Samir's rework cycles ("wrong budget codes" and "no backup documentation"). System-level validation eliminates the need for human reviewers to chase paperwork.

---

### 2. Vendor Engagement (New or Renewed)

**Threshold note:** New vendors require compliance pre-check. Renewals with unchanged terms require prior contract reference only.

| Document | Required | Format | Why It Exists |
|---|---|---|---|
| Business justification | Yes | Free text, min 50 characters | Establishes why this vendor, not just any vendor. |
| Vendor details (name, address, tax ID) | Yes | Structured form fields | Tier 2 (Compliance Review) needs this for vendor compliance verification. |
| Vendor compliance status | Yes (new) / Auto-populated (existing) | System lookup or manual attestation | The Bobs: "vendor is in good standing" is a pass criterion at Tier 2. |
| Contract or SOW | Yes | PDF attachment | Audit trail: what was agreed, by whom, under what terms. |
| Budget line reference | Yes | Must match valid budget line | Same intake validation as procurement. |
| Cost estimate or rate schedule | Yes | PDF, XLSX, or image | Financial verification at Tier 1. |
| Conflict-of-interest declaration | Yes | Checkbox declaration | Required for all vendor-facing requests. |
| Consolidated spend summary | Yes | Auto-populated or manual entry | Tracks total vendor exposure across fiscal period. |
| Prior contract reference | Yes (renewal) / N/A (new) | Text field linking to prior agreement | Enables comparison for renewals. Addresses Joanna's point about repeat approvals. |
| Milton's vendor exception check | Yes (if vendor is on exception list) | Auto-flagged from vendor master | Absorbs Milton's 47-vendor spreadsheet into the intake process. No more personal spreadsheet as the only record. |

**How this addresses the 60% stat:** Vendor compliance status and contract/SOW are the most frequently missing items on vendor requests. Requiring attachment before submission eliminates downstream chasing and The Bobs' finding of approvals without supporting artifacts.

---

### 3. Budget Reallocation

| Document | Required | Format | Why It Exists |
|---|---|---|---|
| Business justification | Yes | Free text, min 50 characters | Why funds need to move between lines. |
| Source budget line | Yes | Must match valid budget line | Where the money is coming from. System validates funds exist. |
| Destination budget line | Yes | Must match valid budget line | Where the money is going. |
| Reallocation amount | Yes | Currency field | Samir: amount verification is step one of his review. |
| Impact statement | Yes | Free text | What the source budget line loses by reallocating. Prevents "robbing Peter to pay Paul" without visibility. |
| Department head authorization | Yes | Digital signature or system-recorded approval | The Bobs: segregation of duties — requestor cannot be the same person who authorizes the transfer. |
| Remaining balance confirmation | Yes | Auto-populated from financial system | Samir's 2022 incident: $40K overrun because nobody checked remaining budget. System enforces this at intake. |
| Conflict-of-interest declaration | Yes | Checkbox declaration | Required when reallocation benefits a specific vendor or external party. |

**How this addresses the 60% stat:** Cost center mismatches are the #1 rework trigger for Samir. System-level validation of both source and destination budget lines catches the error before any human reviews it.

---

### 4. Software License (New or Renewal)

**Threshold note:** Renewals with unchanged terms, same vendor, and same amount qualify for streamlined intake (fewer required fields). This addresses Joanna's complaint about reviewing the same $1,200 license for the fourth time.

| Document | Required | Format | Why It Exists |
|---|---|---|---|
| Business justification | Yes (new) / Streamlined (renewal) | Free text; renewal can reference prior approval | New licenses need full justification. Renewals reference prior approved request. |
| Software name and version | Yes | Text field | Identifies what is being licensed. |
| License type and seat count | Yes | Structured fields | Financial verification: per-seat vs. enterprise vs. subscription. |
| Vendor quote or renewal notice | Yes | PDF, XLSX, or image | The Bobs: artifact before approval. |
| Budget line reference | Yes | Must match valid budget line | Standard intake validation. |
| GL account code | Yes | Must match valid GL account | Samir: "GL account errors common" on license requests. System validates. |
| Prior approval reference | Yes (renewal) / N/A (new) | System link to prior request | Enables comparison. If terms unchanged, Tier 1 review is accelerated. |
| Security/IT review confirmation | Optional | Checkbox or attachment | Not audit-required but prevents downstream security blocks. |
| Conflict-of-interest declaration | Yes | Checkbox declaration | Standard for all vendor-facing requests. |

**How this addresses the 60% stat:** GL account validation at intake and vendor quote enforcement eliminate the two most common deficiencies on license requests. Streamlined renewal path reduces requestor burden for repeat purchases.

---

### 5. Capital Expenditure

**Threshold note:** All capital expenditure requests require the full documentation package. No streamlined path. These are the highest-risk, lowest-frequency requests.

| Document | Required | Format | Why It Exists |
|---|---|---|---|
| Business justification with ROI analysis | Yes | Structured template (narrative + financial model) | Capital spend requires demonstrated return. The Bobs expect this for any audit of capital commitments. |
| Three-bid summary | Yes | PDF or structured form | The Bobs: mandatory for all capital expenditure regardless of amount. |
| Budget line reference | Yes | Must match valid capital budget line | System validates against capital budget, not operating budget. |
| Capital project code | Yes | Must match valid project code | Links expenditure to approved capital project. |
| Vendor details and contract | Yes | Structured fields + PDF attachment | Full vendor documentation for capital-grade spend. |
| Asset classification | Yes | Drop-down selection | Determines depreciation schedule and accounting treatment. |
| Delivery and installation timeline | Yes | Date fields | Capital projects have milestone dependencies. |
| Conflict-of-interest declaration | Yes | Checkbox declaration | Standard. |
| Consolidated spend summary | Yes | Auto-populated or manual entry | Capital spend thresholds are regulatory triggers. |
| Department head and division head authorization | Yes | Dual digital signature | Higher authorization threshold for capital spend. The Bobs: segregation of duties at elevated level. |

**How this addresses the 60% stat:** Capital requests have the highest documentation requirements and the highest deficiency rate when submitted. Requiring the full package at intake means Tier 1 reviewers receive complete submissions, not partial ones they have to send back.

---

### 6. Facilities and Infrastructure

**Threshold note:** This request type absorbs the Facilities SharePoint shadow system into the main approval workflow. All facilities requests now go through the same intake gate.

| Document | Required | Format | Why It Exists |
|---|---|---|---|
| Business justification | Yes | Free text, min 50 characters | Why the facilities work is needed. |
| Scope of work description | Yes | Free text or attachment | What physical changes, installations, or infrastructure work is requested. |
| Budget line reference | Yes | Must match valid budget line | Standard intake validation. |
| Cost estimate or contractor quote | Yes | PDF, XLSX, or image | Artifact before approval. |
| Vendor/contractor details | Yes (if external) | Structured fields | Compliance verification for external contractors. |
| Facilities impact assessment | Yes | Structured form | Safety, access, and operational impact of the work. |
| Building/location reference | Yes | Text field or drop-down | Which facility is affected. |
| Conflict-of-interest declaration | Yes | Checkbox declaration | Standard for all vendor-facing requests. |
| Timeline and access requirements | Optional | Date fields + text | Scheduling coordination. Not audit-required. |

**How this addresses the 60% stat:** Facilities requests currently bypass the main system entirely (Milton's SharePoint list). Bringing them into the intake gate means they get the same documentation enforcement as every other request type. The Bobs flagged zero audit trail on shadow system as a control gap.

---

### 7. Small Purchase / P-Card Reconciliation

**Threshold note:** Purchases under $2,500 made via P-card. This is not a pre-approval path. It's a post-purchase reconciliation that creates the audit trail Joanna's team currently skips entirely.

| Document | Required | Format | Why It Exists |
|---|---|---|---|
| Purchase description | Yes | Free text | What was bought and why. |
| Receipt or invoice | Yes | PDF or image attachment | The Bobs: "P-card transactions bypass the approval workflow entirely." This creates the missing artifact. |
| Budget line reference | Yes | Must match valid budget line | Even small purchases need budget line tracking. |
| GL account code | Yes | Must match valid GL account | Proper accounting classification. |
| Cardholder confirmation | Yes | Digital signature or system-recorded | Who made the purchase. Segregation of duties. |
| Manager attestation | Yes | Digital signature or system-recorded | Confirms the purchase was appropriate. Lightweight approval after the fact. |
| Business purpose statement | Yes | Free text, min 25 characters | Audit-grade justification. "Operational need" is not sufficient (Samir's complaint about vague justifications). |
| Vendor name | Yes | Text field | Enables consolidated spend tracking even for small purchases. |

**How this addresses the 60% stat:** P-card purchases currently have 0% documentation. This reconciliation path creates 100% documentation for spend that was previously invisible to audit. The Bobs identified P-card bypass as something that "keeps us up at night."

---

## Cross-Reference: Audit and Financial Verification Requirements

### The Bobs' Audit Trail Requirements (Both Interviews)

The Bobs specified three non-negotiable audit requirements. Every checklist above is designed to satisfy all three.

| Requirement | How the Checklists Address It |
|---|---|
| **Immutable audit log** — no record alteration after the fact | All documents attached at intake are timestamped and locked. Verification records at each tier are append-only. |
| **Identity verification at every approval node** — no shared accounts, no "approved on behalf of" | Digital signatures and system-recorded approvals required. No proxy approvals. Department head authorization is a personal signature, not a delegation. |
| **Real-time documentation attachment** — artifact present before approval proceeds | Every checklist enforces attachment before the request enters the queue. The system blocks submission if required documents are missing. This is the Bobs' "single most impactful improvement" — enforced, not requested. |

### Samir's Financial Verification Needs (Both Interviews)

Samir's rework cycles stem from three categories of input quality problems. The checklists address each one at intake.

| Problem | Frequency (Samir's Estimate) | How the Checklists Address It |
|---|---|---|
| **Wrong budget codes** | ~50% of submissions | Budget line reference validated against the financial system at intake. Invalid codes rejected before submission completes. |
| **Missing vendor quotes or backup documentation** | ~40% of submissions | Vendor quote or cost estimate is a required attachment. System blocks submission without it. |
| **Vague justification ("operational need")** | Common on small and repeat requests | Minimum character count on business justification. P-card reconciliation requires a specific business purpose statement. "Operational need" is flagged as insufficient. |

---

## Complete vs. Incomplete Submission: Side-by-Side Example

**Scenario:** Procurement request for 50 ergonomic keyboards at $85 each ($4,250 total) from an existing vendor.

### Incomplete Submission (What Happens Today — 60% of Requests)

| Field | Submitted Value | Problem |
|---|---|---|
| Business justification | "Operational need" | Too vague. Samir sends it back. Adds 2–3 days. |
| Budget line reference | "IT budget" | Not a valid budget line code. Samir sends it back again. Another 2 days. |
| Vendor quote | Not attached | "Will upload later." The Bobs: this is exactly how 60% of approvals end up with missing documentation. |
| Conflict-of-interest declaration | Not completed | Compliance review stalls until this is done. |
| Consolidated spend summary | Not provided | The Bobs can't verify whether this pushes the vendor past a threshold. |

**Result:** Request enters the queue, sits with Samir, gets returned twice, resubmitted twice, finally approved after 8–12 days. Samir re-reviews from scratch each time. The Bobs find incomplete documentation in the next quarterly audit. Everyone is frustrated.

### Complete Submission (What the Intake Gate Enforces)

| Field | Submitted Value | Validation |
|---|---|---|
| Business justification | "Replace aging keyboards in Building C, floors 2–3. Current units are 5+ years old with multiple ergonomic complaints filed through HR (ref: HR-2026-0142). Ergonomic assessment recommends replacement." | Passes minimum character count. Provides specific, auditable rationale. |
| Budget line reference | IT-OPS-2026-4420 | System validates: valid budget line, sufficient funds ($4,250 against $12,800 remaining). |
| Vendor quote | Attached: ErgoTech_Quote_2026-0302.pdf | PDF present. Matches stated amount. Artifact exists before any approver sees the request. |
| Conflict-of-interest declaration | "No conflict" (checkbox) | Completed. Tier 2 will verify. |
| Consolidated spend summary | ErgoTech YTD: $8,400 (auto-populated). This purchase brings total to $12,650. | Below $25K threshold. No three-bid summary needed. |
| Vendor details | ErgoTech Solutions, Contract #ET-2024-0088 | Existing vendor, compliance status current (auto-verified). |

**Result:** Request passes the intake gate on first submission. Enters the approval queue with a complete documentation package. Tier 1 reviewer sees validated budget data and a clear justification. Tier 2 sees vendor compliance is current and no conflict declared. Tier 3 confirms the full chain is documented. Approved in 3–5 business days with zero rework cycles. Audit-ready from day one.

---

## Format Requirements Summary

| Format Constraint | Applies To | Rationale |
|---|---|---|
| PDF, XLSX, or image only for financial documents | Quotes, contracts, bids, receipts | Prevents editable formats that could be altered post-approval. Supports immutable audit log. |
| System-validated budget line codes | All request types | Catches the #1 rework trigger (wrong budget codes) before any human reviews it. |
| Minimum character count on justifications | Business justification (50 chars), P-card business purpose (25 chars) | Prevents "operational need" as a complete justification. Forces requestors to state a specific reason. |
| Digital signatures (not email or verbal confirmation) | Department head authorization, manager attestation, dual sign-off | The Bobs: "no shared accounts, no approved on behalf of." Identity verification at every node. |
| Auto-population where possible | Consolidated spend, vendor compliance status, remaining budget balance | Reduces requestor burden and eliminates manual data entry errors. |

---

## Stakeholder Review Status

| Reviewer | Scope | Status |
|---|---|---|
| The Bobs (Internal Audit) | Audit trail requirements, documentation completeness thresholds, P-card controls | Pending review |
| Samir Nagheenanajar (Finance) | Financial documentation, budget validation rules, GL account requirements | Pending review |
| Process Council / Stakeholders | Full checklist approval | Pending review |

---

## Traceability

| Source | Reference |
|---|---|
| The Bobs, Interview 1 | "60% documentation deficiency rate on first submission." "Enforcing attachment-before-approval would be the single most impactful improvement." |
| The Bobs, Interview 2 | "Enforce completeness at intake. If a request can't be submitted without the required documentation attached, the downstream review steps get faster." |
| Samir, Interview 1 | "Most of what lands on my desk is not clean. Half the time the budget codes are wrong." |
| Samir, Interview 2 | "Maybe 40% of what lands on my desk is missing something. Wrong cost center. No vendor quote. Vague justification." |
| Joanna, Interview 1 | Credit card bypass, "same $1,200 license renewed four years running," "shouting into a void." |
| Milton, Interview 1 | 47 vendor exceptions, Facilities SharePoint shadow system. |
| Process Design, Section 3 | Intake gate as hard gate, required attachments, 30-day grace period. |
| Input Synthesis | Theme 3 (duplication from no visibility), Theme 4 (workaround compliance gaps). |
