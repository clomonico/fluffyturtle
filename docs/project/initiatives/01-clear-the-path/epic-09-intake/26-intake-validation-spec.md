# Intake Validation Spec — Blocking Incomplete Submissions

**Date:** 2026-03-02
**Author:** Lambert (Implementation Planner)
**Issue:** #26
**Epic:** #9 (Intake Completeness Enforcement)
**Dependency:** #25 (Documentation checklist by request type, owned by Dallas)

---

## 1. Summary

This spec defines the validation rules, technical implementation approach, error message design, testing plan, metrics strategy, and phased rollout for blocking incomplete approval request submissions at intake. The goal is to drop the current 60% incomplete documentation rate below 5% within 60 days of launch by preventing incomplete packages from entering the approval queue.

All validation rules reference Dallas's documentation checklist from Issue #25. This spec does not define *what* documentation is required (that's #25). It defines *how the system enforces it*.

---

## 2. Validation Rules by Request Type

Each request type requires a base set of fields plus type-specific documentation. Validation is a hard block: the system rejects submission and returns the request to the requestor with a list of what's missing.

### 2.1 Base Validation (All Request Types)

These fields are required regardless of request type:

| Field | Validation Rule | Format Check |
|-------|----------------|--------------|
| Request type | Must select from dropdown | Dropdown, no blank |
| Business justification | Free text, minimum 50 characters | Character count enforced |
| Requestor name | Auto-populated from employee record | Read-only |
| Requestor department | Auto-populated from employee record | Read-only |
| Requestor cost center | Auto-populated from employee record | Read-only |
| Estimated total cost | Numeric, greater than $0 | Currency format, positive value |
| Budget code | Must match active budget code in financial system | Lookup validation against budget master |
| Compliance category | Must select from dropdown (maps to Bobs' control matrix) | Dropdown, no blank |

### 2.2 Procurement Requests

| Field | Validation Rule | Format Check |
|-------|----------------|--------------|
| Vendor name | Required, validated against vendor master | Lookup; new vendor triggers flag (warning, not block) |
| Vendor quote or cost estimate | At least one file attachment | PDF, XLSX, PNG, JPG accepted |
| Contract reference | Required if existing vendor | Free text |
| Conflict-of-interest declaration | Checkbox must be checked | Boolean |
| Consolidated spend summary | Total spend with this vendor in current fiscal period | Numeric, auto-populated if available, manual entry otherwise |

### 2.3 Vendor Onboarding

| Field | Validation Rule | Format Check |
|-------|----------------|--------------|
| Vendor name | Required, must NOT exist in vendor master | Lookup; existing vendor redirects to procurement |
| W-9 or equivalent tax form | File attachment required | PDF only |
| Insurance certificate | File attachment required | PDF only |
| Vendor qualification questionnaire | File attachment required | PDF, XLSX accepted |
| Conflict-of-interest declaration | Checkbox must be checked | Boolean |
| Justification for new vendor | Free text, minimum 100 characters | Character count enforced |

### 2.4 Budget Reallocation

| Field | Validation Rule | Format Check |
|-------|----------------|--------------|
| Source budget code | Must match active budget code | Lookup validation |
| Destination budget code | Must match active budget code, must differ from source | Lookup + comparison |
| Reallocation amount | Numeric, positive, cannot exceed source budget remaining balance | Currency format, balance check |
| Approval from source budget holder | File attachment (signed memo or email) | PDF, EML, MSG accepted |
| Impact statement | Free text, minimum 50 characters | Character count enforced |

### 2.5 Software License / Renewal

| Field | Validation Rule | Format Check |
|-------|----------------|--------------|
| Software name | Required | Free text |
| License type | Dropdown (new, renewal, upgrade) | Dropdown, no blank |
| Number of seats / units | Numeric, positive integer | Integer format |
| Vendor quote | File attachment required | PDF, XLSX accepted |
| IT review confirmation | Checkbox: "IT has reviewed compatibility" | Boolean |
| Existing contract reference | Required for renewals and upgrades | Free text |

### 2.6 Facilities / Maintenance

| Field | Validation Rule | Format Check |
|-------|----------------|--------------|
| Location / building | Dropdown from facilities list | Dropdown, no blank |
| Service description | Free text, minimum 50 characters | Character count enforced |
| Vendor quote | File attachment required for amounts > $1,000 | Conditional: only enforced above threshold |
| Safety or compliance flag | Checkbox if work involves regulated systems | Boolean |

### 2.7 Credit Card Reconciliation

| Field | Validation Rule | Format Check |
|-------|----------------|--------------|
| Transaction date | Date field, cannot be future | Date format |
| Merchant name | Required | Free text |
| Receipt | File attachment required | PDF, PNG, JPG accepted |
| Business purpose | Free text, minimum 50 characters | Character count enforced |
| Pre-approval reference | Optional but flagged if empty for amounts > $500 | Conditional warning |

---

## 3. Technical Implementation Approach

### 3.1 Platform Capabilities Assessment

Michael Bolton confirmed the current platform supports conditional branching, delegation rules, and form-level validation, but none of it is configured. The intake validation implementation uses the platform's existing form builder and workflow engine features. No new platform or tooling is required.

Specifically, the platform supports:
- **Required field enforcement** — native form configuration, field-level "required" toggle
- **Format validation** — character minimums, numeric format, date format via built-in rules
- **Lookup validation** — the platform can query external data sources (budget code master, vendor master) if an integration endpoint exists. Michael will need to confirm or build the integration connector.
- **File attachment requirements** — the form builder supports "at least one attachment" rules and file type restrictions
- **Conditional field visibility** — fields can appear/disappear based on request type selection, so requestors only see fields relevant to their request type
- **Pre-submission validation** — the platform evaluates all rules before allowing submission and can display multiple errors simultaneously

### 3.2 Platform Configuration Steps (Michael Bolton's Work)

| Step | Description | Estimated Effort |
|------|-------------|-----------------|
| 1 | Enable request type dropdown as the first field on the intake form, making all subsequent fields conditional on this selection | 2 hours |
| 2 | Configure base validation fields (Section 2.1) with required flag and format checks | 4 hours |
| 3 | Build conditional field sets per request type (Sections 2.2–2.7), toggling visibility based on request type selection | 8 hours |
| 4 | Set up budget code lookup integration: API call to financial system to validate budget code is active | 4 hours (if endpoint exists) / 16 hours (if endpoint must be built) |
| 5 | Set up vendor master lookup integration: API call to vendor database | 4 hours (if endpoint exists) / 16 hours (if endpoint must be built) |
| 6 | Configure file attachment requirements per request type, including file type filters | 3 hours |
| 7 | Configure pre-submission validation to run all rules and display consolidated error list | 4 hours |
| 8 | Build the "new vendor" warning flag logic (vendor not found in master — warn but don't block) | 2 hours |
| 9 | Deploy to sandbox for testing | 2 hours |

**Total estimated effort:** 33–57 hours depending on integration endpoint availability.

**Critical dependency:** Steps 4 and 5 depend on whether the financial system and vendor master have accessible API endpoints. If not, Michael will need to build lightweight integration connectors. This is the most likely scope expansion risk and should be scouted in Week 1 of rollout.

### 3.3 Architecture Notes

- **No new platform.** Michael wants a cloud-native rebuild. That's Initiative 5 territory. For now, we work within the existing workflow engine.
- **Sandbox first.** All configuration happens in the sandbox environment (per Implementation Plan Phase 2). Nothing touches production until parallel run validation passes.
- **Audit trail.** Every validation rejection must be logged: timestamp, requestor, request type, fields that failed, error messages shown. This feeds the metrics in Section 6.
- **Rollback.** If validation causes unacceptable friction, field requirements can be toggled back to optional per-field. Each field's required flag is an independent configuration toggle — there is no all-or-nothing switch.

---

## 4. Error Message Design

### 4.1 Design Principles

1. **Tell them what's missing, not just that something's wrong.** "Budget code is required" is better than "Form validation failed."
2. **Tell them where to find it.** Requestors shouldn't have to guess where to get a budget code. The error message links to the resource.
3. **Show all errors at once.** Don't make requestors fix one error, resubmit, hit another error, repeat. Validate the full form and show everything on one screen.
4. **No jargon.** Use language a department manager (Joanna) would understand, not system terms.

### 4.2 Error Message Templates

| Validation Failure | Error Message |
|-------------------|---------------|
| Business justification too short | "Business justification must be at least 50 characters. Describe why this purchase or request is needed and what it supports." |
| Budget code missing | "Budget code is required. You can find your department budget codes in the Finance Portal under Budget > My Budget Lines." |
| Budget code invalid | "The budget code you entered doesn't match an active budget. Double-check the code in the Finance Portal, or contact your budget holder for the correct code." |
| Vendor name missing | "Vendor name is required. Enter the vendor or supplier you're requesting this purchase from." |
| New vendor detected | ⚠️ "This vendor is not in our vendor master. Your request will be flagged for additional compliance review. If this is an existing vendor under a different name, update the vendor name field." (Warning, not block) |
| Vendor quote missing | "A vendor quote or cost estimate is required. Attach a PDF or spreadsheet with the pricing details." |
| Conflict of interest not declared | "You must complete the conflict-of-interest declaration before submitting. Check the box to confirm no conflicts exist, or contact Compliance if you need guidance." |
| Attachment missing | "At least one supporting document is required. Attach the relevant quote, contract, or authorization memo." |
| File type not accepted | "The file type you attached isn't accepted for this field. Accepted types: PDF, XLSX, PNG, JPG. Convert your document and try again." |
| Estimated cost missing or zero | "Estimated total cost is required and must be greater than $0. Enter the total expected cost of this request." |
| Compliance category not selected | "Select a compliance category. This determines which review steps apply to your request. If you're unsure, select 'General' and the compliance team will route it." |
| Source and destination budget codes match | "Source and destination budget codes can't be the same. Enter a different destination budget code for the reallocation." |
| Reallocation exceeds balance | "The reallocation amount exceeds the remaining balance in the source budget. Contact your budget holder to confirm available funds." |
| IT review not confirmed | "IT review confirmation is required for software requests. Check the box to confirm IT has reviewed compatibility, or contact IT Help Desk to request a review." |

### 4.3 Error Display Behavior

- All errors appear in a consolidated panel at the top of the form after the requestor clicks "Submit."
- Each error message is anchored to its field — clicking the error scrolls to the relevant field.
- Fields with errors are highlighted with a red border and the error text appears below the field.
- The submit button remains visible but disabled until all required fields pass validation.
- A progress indicator shows "X of Y required items complete" as the requestor fills in the form.

---

## 5. Testing Plan

### 5.1 Test Strategy

Testing happens in the sandbox environment during Implementation Plan Phase 2 (Weeks 2–3). Joanna's team provides real-world test data. The Bobs validate that the audit trail captures rejection events. Michael Bolton validates the technical configuration.

### 5.2 Test Cases by Request Type

#### Procurement

| Test ID | Scenario | Expected Result |
|---------|----------|-----------------|
| P-01 | Submit with all fields complete and valid | Submission accepted |
| P-02 | Submit with business justification under 50 characters | Blocked, error message displayed |
| P-03 | Submit without vendor quote attachment | Blocked, error message displayed |
| P-04 | Submit with invalid budget code | Blocked, error message displayed |
| P-05 | Submit with new vendor (not in master) | Warning displayed, submission allowed |
| P-06 | Submit without conflict-of-interest declaration | Blocked, error message displayed |
| P-07 | Submit with $0 estimated cost | Blocked, error message displayed |
| P-08 | Submit with unsupported file type (.doc instead of .docx) | Blocked, error message displayed |

#### Vendor Onboarding

| Test ID | Scenario | Expected Result |
|---------|----------|-----------------|
| V-01 | Submit with all required vendor docs | Submission accepted |
| V-02 | Submit without W-9 attachment | Blocked |
| V-03 | Submit without insurance certificate | Blocked |
| V-04 | Submit with existing vendor name | Redirected to procurement request type |
| V-05 | Submit with new vendor justification under 100 chars | Blocked |

#### Budget Reallocation

| Test ID | Scenario | Expected Result |
|---------|----------|-----------------|
| B-01 | Submit with valid source, destination, and amount | Submission accepted |
| B-02 | Submit with same source and destination codes | Blocked |
| B-03 | Submit with amount exceeding source balance | Blocked |
| B-04 | Submit without signed authorization memo | Blocked |

#### Software License / Renewal

| Test ID | Scenario | Expected Result |
|---------|----------|-----------------|
| S-01 | Submit new license request with all fields | Submission accepted |
| S-02 | Submit renewal without existing contract reference | Blocked |
| S-03 | Submit without IT review confirmation | Blocked |
| S-04 | Submit with zero seats | Blocked |

#### Facilities / Maintenance

| Test ID | Scenario | Expected Result |
|---------|----------|-----------------|
| F-01 | Submit request under $1,000 without vendor quote | Submission accepted |
| F-02 | Submit request over $1,000 without vendor quote | Blocked |
| F-03 | Submit without location selection | Blocked |

#### Credit Card Reconciliation

| Test ID | Scenario | Expected Result |
|---------|----------|-----------------|
| C-01 | Submit with receipt and all fields | Submission accepted |
| C-02 | Submit without receipt attachment | Blocked |
| C-03 | Submit over $500 without pre-approval reference | Warning displayed, submission allowed |
| C-04 | Submit with future transaction date | Blocked |

### 5.3 Cross-Cutting Tests

| Test ID | Scenario | Expected Result |
|---------|----------|-----------------|
| X-01 | Submit with 3+ validation errors simultaneously | All errors displayed in consolidated panel |
| X-02 | Fix one error and resubmit with remaining errors | Fixed field clears, remaining errors persist |
| X-03 | Switch request type mid-form | Conditional fields update, completed fields for previous type are cleared |
| X-04 | Verify audit log captures rejection event | Timestamp, requestor, request type, failed fields logged |
| X-05 | Verify progress indicator updates as fields are completed | Indicator reflects completion state accurately |

### 5.4 Test Execution

| Phase | Tester | Focus |
|-------|--------|-------|
| Config validation | Michael Bolton | Technical: rules fire correctly, integrations work |
| Functional testing | Joanna's team (2 requestors) | Usability: messages are clear, form behaves as expected |
| Compliance validation | The Bobs | Audit trail: rejection events logged correctly for audit |
| Edge case testing | Peter Gibbons | Boundary: unusual requests, Milton's vendor exceptions |

---

## 6. Metrics: Incomplete Submission Rate Tracking

### 6.1 Primary Metric

**Incomplete submission rate** = (Number of submissions blocked at intake) / (Total submission attempts) x 100

**Baseline (current state):** 60% of approved requests have incomplete documentation (per the Bobs' audit finding). This is a proxy — the current system doesn't block anything, so the true attempt rate is higher.

**Target:** Below 5% incomplete submission rate within 60 days of launch.

### 6.2 Supporting Metrics

| Metric | What It Measures | Data Source |
|--------|-----------------|-------------|
| Rejection rate by field | Which fields cause the most rejections | Validation rejection log |
| Rejection rate by request type | Which request types have the highest rejection rates | Validation rejection log |
| Time to resubmission | How long requestors take to fix and resubmit after rejection | Timestamps in request log |
| First-attempt success rate | Percentage of submissions that pass validation on the first try | Submission log |
| Requestor feedback score | Perceived helpfulness of error messages | Post-submission survey (optional, quarterly) |
| Approval cycle time (before vs. after) | Whether eliminating incomplete submissions speeds up approvals | Approval workflow timestamps |

### 6.3 Dashboard Integration

These metrics feed into Lumbergh's visibility dashboard (Implementation Plan Section 5.3):
- A new "Intake Health" widget showing daily/weekly incomplete submission rate
- A trend line comparing baseline period to post-launch
- Drill-down by request type and department

### 6.4 Before/After Measurement Plan

| Period | Duration | Purpose |
|--------|----------|---------|
| Baseline | 4 weeks before launch | Establish current incomplete rate using manual audit of recent submissions |
| Grace period | First 30 days post-launch | 48-hour correction window active, measure with and without grace |
| Steady state | Days 31–90 post-launch | Full enforcement, measure against 5% target |
| Ongoing | Monthly | Continued tracking as part of operational reporting |

---

## 7. Phased Rollout Plan

### 7.1 Rationale

Rolling out validation to all request types simultaneously risks overwhelming requestors and support staff. A phased approach lets us validate the design with the highest-volume request type first, learn from the initial rollout, and adjust before expanding.

### 7.2 Phase Schedule

| Phase | Request Type | Duration | Why This Order |
|-------|-------------|----------|----------------|
| Phase 1 | Procurement | 2 weeks | Highest volume. Addresses the most common incomplete documentation scenarios. Most of the Bobs' 60% finding comes from procurement. |
| Phase 2 | Software License / Renewal | 1 week | Second-highest volume. Straightforward validation rules. |
| Phase 3 | Budget Reallocation + Vendor Onboarding | 1 week | Medium volume. Budget reallocation has the balance-check integration that needs real-world validation. Vendor onboarding has the most attachments. |
| Phase 4 | Facilities + Credit Card Reconciliation | 1 week | Lower volume. Credit card reconciliation is a new pathway coordinated with Initiative 4 (One Front Door). |

### 7.3 Phase 1 Detailed Plan

| Week | Activity | Owner |
|------|----------|-------|
| Week 1 | Deploy procurement validation to sandbox, run test cases P-01 through P-08 and cross-cutting X-01 through X-05 | Michael Bolton, Joanna |
| Week 1 | Brief Joanna's team on new intake requirements, distribute reference guide | Peter Gibbons |
| Week 2 | Deploy procurement validation to production with 48-hour grace period | Michael Bolton |
| Week 2 | Monitor rejection rate, review error messages with 3 requestors who hit validation errors | Lambert, Joanna |
| Week 2 | Adjust any error messages or field rules based on feedback | Michael Bolton |
| End of Phase 1 | Go/No-Go decision for Phase 2 based on first-attempt success rate and requestor feedback | The Bobs, Lumbergh |

### 7.4 Go/No-Go Criteria Between Phases

| Criterion | Threshold |
|-----------|-----------|
| First-attempt success rate for current phase | At least 60% (requestors are learning) |
| Error message clarity feedback | No more than 2 "unclear" reports per week |
| System stability | Zero platform errors caused by validation rules |
| Support ticket volume from validation | Manageable by existing support staff without backlog |
| Audit trail capture | 100% of rejection events logged correctly |

---

## 8. Platform Configuration Reference

This section gives Michael Bolton's team the steps to configure each capability area. All work happens in the sandbox first.

### 8.1 Form Builder Configuration

1. Open the intake form in the platform's form builder (admin mode)
2. Add "Request Type" as the first field, type: dropdown, values: Procurement, Vendor Onboarding, Budget Reallocation, Software License/Renewal, Facilities/Maintenance, Credit Card Reconciliation
3. Set "Request Type" as required
4. For each request type, create a conditional field group that shows/hides based on the selected type
5. Within each group, set field-level properties: required (yes/no), format (text/numeric/date/file), minimum character count where applicable
6. Configure file upload fields with accepted type filters (PDF, XLSX, PNG, JPG per field)
7. Set auto-populate rules for requestor name, department, and cost center from the employee directory integration

### 8.2 Lookup Integrations

**Budget code validation:**
1. Create an integration endpoint that queries the financial system's budget code table
2. Input: budget code string. Output: active (yes/no), budget holder name, remaining balance
3. Wire to the budget code field's on-blur validation event
4. If endpoint doesn't exist: build a lightweight REST adapter or a nightly CSV sync to a local lookup table

**Vendor master validation:**
1. Create an integration endpoint that queries the vendor master database
2. Input: vendor name string (fuzzy match). Output: match found (yes/no), vendor ID, contract status
3. Wire to the vendor name field's on-blur validation event
4. New vendor detection: if no match, display warning banner but allow submission

### 8.3 Pre-Submission Validation Engine

1. Enable the platform's pre-submission validation hook (this runs all field rules before the submission event fires)
2. Configure the validation to return a consolidated error list, not fail on the first error
3. Map each error to its field anchor for scroll-to-field behavior
4. Configure the submit button state: disabled until error count = 0, with live progress indicator

### 8.4 Audit Logging

1. Enable event logging for the "submission rejected" event type
2. Log fields: timestamp, requestor ID, request type, list of failed validations, error messages displayed
3. Confirm logs write to the platform's audit table (not flat files, per Michael's concern about flat file audit storage)
4. If the platform only supports flat file logging for this event type, build a nightly sync to push rejection logs into the structured audit database

---

## 9. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Budget code or vendor master integration endpoints don't exist | Medium | High — blocks Steps 4–5, adds 12+ hours | Scout in Week 1; if endpoints unavailable, use nightly CSV sync as interim |
| Requestors bypass the form by emailing approvers directly | Medium | Medium — undermines the gate | Approvers trained to redirect. Lumbergh's dashboard shows only form-submitted requests; anything off-system is invisible to him. |
| Error messages confuse requestors, driving support ticket volume | Medium | Medium | Grace period + Phase 1 limited rollout. Iterate messages based on real feedback before expanding. |
| Michael Bolton availability (single point of failure for all config) | High | High — timeline slips | Document all configuration steps (Section 8) so a backup IT person can execute if needed. Pair Michael with a junior admin during setup. |
| Validation too strict, pushes spend to credit cards and informal channels | Medium | High — worsens compliance gap | Kane flagged this in validation report. Grace period, phased rollout, and requestor feedback loop are the mitigations. Monitor credit card volume as a leading indicator. |
| Tom Smykowski lobbies against validation as "bureaucratic overreach" | Medium | Low | Tom's concern is process relevance, not documentation. This doesn't touch his tiers. Frame as "the system does the paperwork check so people don't have to." |

---

## 10. Cross-Initiative Dependencies

| Initiative | Dependency | Impact |
|-----------|-----------|--------|
| #25 (Documentation Checklist) | This spec implements the checklist Dallas defines. If #25 changes, validation rules here update. | Direct input |
| Epic 8 (Tier Removal) | Tier removal reduces the approval chain. Validation at intake ensures the shortened chain gets complete packages. | Complementary |
| Initiative 3 (Glass Floor) | Glass Floor configures parallel routing and auto-escalation on the same platform. Coordinate with Michael to avoid configuration conflicts. | Shared platform risk |
| Initiative 4 (One Front Door) | Credit card reconciliation and Facilities request types are scoped for One Front Door but included here for intake validation coverage. | Forward-looking |
| #27 (Self-Service Guidance) | Error messages reference help resources. #27 creates those resources. Ideally #27 ships before or alongside Phase 2. | Sequencing |
