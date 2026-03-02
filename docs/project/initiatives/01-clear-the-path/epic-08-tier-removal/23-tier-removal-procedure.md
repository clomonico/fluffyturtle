# Tier Removal Procedure — Elective Tier Deactivation

**Date:** 2026-03-02
**Author:** Lambert (Implementation Planner)
**Issue:** #23
**Initiative:** 1 — Clear the Path
**Epic:** 08 — Tier Removal
**Prerequisite:** Tier classification signed off by the Bobs (Issue #20), stakeholder sign-off package approved (Issue #21)

---

## Executive Summary

This document defines the step-by-step procedure for removing 4 elective tiers from the 7-tier approval workflow, reducing it to the 3 mandatory regulatory control points. Each elective tier gets a dedicated deactivation procedure, pre-removal checklist, in-flight request handling plan, rollback procedure, and post-removal validation steps.

The removal sequence is ordered from lowest risk to highest risk. Removals should be performed during off-hours windows to minimize disruption. Each removal is a discrete operation that can be paused, validated, and rolled back independently.

---

## 1. Removal Sequence and Order Justification

The 4 elective tiers are removed in the following order, progressing from lowest organizational and technical risk to highest.

| Order | Current Tier # | Tier Name | Owner | Risk Level | Rationale for Position |
|-------|----------------|-----------|-------|------------|----------------------|
| 1st | Tier 4 | Tom's Routing Accuracy Review | Tom Smykowski | Lowest | Peter already skips this on 70% of requests with no consequences. No regulatory basis. No downstream dependencies. Removing it changes almost nothing operationally because it's already being bypassed. |
| 2nd | Tier 5 | Tom's Exception Routing Path | Tom Smykowski | Low | Over-scoped temporary patch from 2020. Even Tom acknowledged it could be narrower. The real vendor verification is done by Milton informally at Tier 3. Milton's function must be formalized into Tier 3 before this removal. |
| 3rd | Tier 6 | Samir's Secondary Review | Samir Nagheenanajar | Medium | Samir acknowledged it's not policy-mandated. He duplicates work from Tier 2 (Budget Authority Confirmation) because of a visibility gap, not a regulatory requirement. Once removed, Tier 2 covers the regulatory check. Spacing from Tom's tiers gives him time to see successful removals. |
| 4th | Tier 7 | Lumbergh's Informal Visibility Gate | Bill Lumbergh | Highest | Lumbergh has executive authority and may resist even with a dashboard replacement. His gate is the most organizationally sensitive removal. The dashboard must be live and validated before this tier is deactivated. |

**Why this order works:**
- Tier 4 first because it's already being bypassed, making it functionally removed. Formalizing its removal carries near-zero risk and builds evidence that the process works without it.
- Tier 5 second because it's Tom's other tier and removing both of Tom's tiers together (before the more sensitive Samir and Lumbergh removals) reduces the total number of "impact events" for stakeholders. Milton's vendor terms function must be formalized into Tier 3 before this removal.
- Tier 6 third because Samir himself said it's redundant. His buy-in is there, and by this point two tiers are successfully removed.
- Tier 7 last because Lumbergh needs the most evidence and the most runway. By the time we reach his gate, three tiers are successfully deactivated and the dashboard has been live long enough to prove its value.

---

## 2. Tier Removal Procedures

### 2.1 Tier 4 — Tom's Routing Accuracy Review (Tom Smykowski)

**Removal Order:** 1st
**Scheduled Window:** Weekend Day 1, Saturday 6:00 AM – 10:00 AM

#### Current Configuration

| Field | Detail |
|-------|--------|
| **System name** | Tom's Routing Accuracy Review (confirm exact name/ID in Phase 1 inventory) |
| **Position in chain** | 4th of 7 sequential approval steps |
| **Owner/Reviewer** | Tom Smykowski |
| **What it checks** | Described as a "routing accuracy check." Tom cannot articulate the full scope without consulting his files. Originally a temporary duty that became permanent. |
| **Routing behavior** | Sequential — request waits at this tier until Tom acts or Peter bypasses it via shadow workflow |
| **Current bypass rate** | ~70% (Peter routes around it on most requests) |

#### Pre-Removal Verification Checklist

- [ ] Tier classification document confirms this tier is classified as elective
- [ ] The Bobs have signed off that no regulatory control depends on this tier
- [ ] Tom Smykowski has been briefed and his risk concerns documented (Phase 1, Activity 1.7)
- [ ] All active requests currently sitting at this tier have been inventoried (count and request IDs)
- [ ] Sandbox testing of this tier's deactivation completed successfully (Phase 2, Activity 2.1)
- [ ] No routing rules reference this tier as a prerequisite for any mandatory tier
- [ ] Configuration backup of production routing taken and verified restorable
- [ ] Michael Bolton confirms technical readiness and is available for the deactivation window

#### Deactivation Steps

1. **Michael Bolton** takes a full configuration snapshot of production routing (timestamp and label the backup)
2. Navigate to the routing configuration module in the production environment
3. Locate Tier 4 (Tom's Routing Accuracy Review) by system name/ID
4. Set the tier status to **"Inactive"** (do NOT delete the tier record)
5. Update routing rules to skip Tier 4: requests that previously routed from Tier 3 → Tier 4 → Tier 5 should now route from Tier 3 → Tier 5 directly
6. Verify that the routing rule update is syntactically valid and does not create orphaned paths
7. Run a test request through the updated routing path in production to confirm it bypasses the inactive tier and reaches Tier 5 correctly
8. Confirm that historical request data and audit logs for previously completed requests at Tier 4 remain intact and accessible
9. Document the exact changes made, including timestamps and the configuration values before and after

#### In-Flight Request Handling

**Strategy: Drain then deactivate**

- Before the deactivation window, identify all requests currently sitting at Tier 4
- For requests waiting less than 24 hours at Tier 4: fast-track Tom's review (or have Peter confirm they're safe to advance, since he bypasses this tier 70% of the time)
- For requests waiting more than 24 hours: manually route to the next active tier (Tier 5) with a note that Tier 4 was deactivated per the approved plan
- **Target: zero requests at Tier 4 before deactivation begins.** If any remain, the deactivation proceeds anyway — the routing update will push them to the next active tier automatically (verify this behavior in sandbox first)

#### Rollback Procedure

1. Navigate to routing configuration module
2. Set Tier 4 status back to **"Active"**
3. Restore the original routing rules from the pre-change configuration backup
4. Run a test request to confirm the tier is functioning and receiving requests again
5. Verify that any requests that passed through during the deactivation period have correct audit trails
6. Notify stakeholders that the tier has been re-enabled and provide a reason

**Estimated rollback time:** Under 2 hours (configuration change + test request + verification)

#### Post-Removal Validation

- [ ] Route 3 test requests of different types through the approval chain; confirm none attempt to route to Tier 4
- [ ] Verify audit trail for test requests shows clean path through remaining active tiers
- [ ] Check system logs for any error messages referencing the deactivated tier
- [ ] Confirm historical reports and dashboards still display data from requests previously processed at Tier 4
- [ ] Monitor for 48 hours: no requests stalling at unexpected points in the chain
- [ ] Collect a quick confirmation from Peter that requests are flowing as expected

---

### 2.2 Tier 5 — Tom's Exception Routing Path (Tom Smykowski)

**Removal Order:** 2nd
**Scheduled Window:** Weekend Day 1 (following weekend), Saturday 6:00 AM – 10:00 AM

#### Current Configuration

| Field | Detail |
|-------|--------|
| **System name** | Tom's Exception Routing Path (confirm exact name/ID in Phase 1 inventory) |
| **Position in chain** | 5th of 7 sequential approval steps |
| **Owner/Reviewer** | Tom Smykowski |
| **What it checks** | Routes exception cases (vendor payment discrepancies, duplicate invoices) through an alternate path Tom created in 2020 as a temporary patch for vendor-related issues. The patch hardened into a permanent routing step for all requests containing vendor references. |
| **Routing behavior** | Sequential — request waits here until Tom or his exception routing logic processes it. Originally scoped to intercept only vendor duplicates; it now triggers on any request with a vendor line item. |
| **Scope creep** | Even Tom acknowledged this step could be narrower. Over-triggers on requests that don't actually involve vendor exceptions. |
| **Milton dependency** | Milton Waddams informally performs vendor terms verification (47 tracked vendors, $14K late-fee history) through this routing path. Milton's function is real and must be preserved. Before this tier is deactivated, Milton's vendor terms check must be formalized as a sub-step within Tier 3 (Compliance / COI Check). |

#### Pre-Removal Verification Checklist

- [ ] Tier classification document confirms this tier is classified as elective
- [ ] The Bobs have signed off that no regulatory control depends on this tier
- [ ] Tom Smykowski has been briefed (this is his second tier being removed; manage this communication carefully — present both removals as recognition that his institutional knowledge is being captured, not discarded)
- [ ] **Milton's vendor terms verification has been formalized into Tier 3 (Compliance / COI Check)** — this is a hard prerequisite. Milton's function must be documented, assigned, and operational within Tier 3 before Tier 5 is deactivated. Failure to preserve Milton's check creates real vendor compliance risk.
- [ ] Verified that Milton's 47-vendor tracking list and $14K late-fee history data have been migrated to the Tier 3 compliance data store
- [ ] All active requests currently sitting at this tier have been inventoried
- [ ] Tier 4 deactivation has been stable for at least 5 business days with no issues
- [ ] Sandbox testing of this tier's deactivation completed successfully
- [ ] No routing rules reference this tier as a prerequisite for any mandatory tier
- [ ] Configuration backup of production routing taken and verified restorable
- [ ] Michael Bolton confirms technical readiness and is available for the deactivation window

#### Deactivation Steps

1. **Michael Bolton** takes a full configuration snapshot of production routing
2. **Pre-flight check:** Confirm Milton's vendor terms verification is operational within Tier 3. Run a test vendor-related request through Tier 3 and verify Milton's check fires correctly before proceeding.
3. Navigate to the routing configuration module in the production environment
4. Locate Tier 5 (Tom's Exception Routing Path) by system name/ID
5. Set the tier status to **"Inactive"** (do NOT delete)
6. Update routing rules: requests that previously routed from Tier 3 → Tier 4 (now inactive) → Tier 5 → Tier 6 should now route from Tier 3 → Tier 6 directly (both Tier 4 and Tier 5 are now inactive)
7. Verify no orphaned routing paths exist, particularly for the exception routing logic that Tier 5 used for vendor-flagged requests
8. Run a test request through the updated path to confirm it bypasses both Tier 4 and Tier 5 (both now inactive) and reaches Tier 6
9. **Run a vendor-related test request** specifically to verify that Milton's vendor terms check executes within Tier 3 and the request does not stall looking for the old Tier 5 exception path
10. Confirm historical request data and audit logs at Tier 5 remain intact
11. Document all changes with timestamps and before/after values

#### In-Flight Request Handling

**Strategy: Drain then deactivate**

- Before the deactivation window, identify all requests currently sitting at Tier 5
- For requests in Tom's exception queue: Tom processes them through to the next tier by end of business Friday. For requests he hasn't touched, route them directly to the next active tier (Tier 6) with a note
- **Pay special attention to vendor-flagged requests.** Any request at Tier 5 that involves vendor terms should be flagged for Milton to verify within Tier 3 after it advances. Do not let vendor requests pass through without Milton's check.
- **Target: zero requests at Tier 5 before deactivation.** Tom has two tiers being removed; ensure communication frames this constructively.

#### Rollback Procedure

1. Navigate to routing configuration module
2. Set Tier 5 status back to **"Active"**
3. Restore routing rules from the pre-change configuration backup
4. Run a test request (including a vendor-flagged request) to confirm the exception routing path is receiving requests again
5. Verify audit trail integrity for requests processed during the deactivation period
6. If Milton's Tier 3 integration was the failure point, keep Tier 5 active while the Tier 3 integration is repaired. Tier 5 serves as the fallback for Milton's function until Tier 3 can handle it natively.
7. Notify Tom, Milton, and stakeholders that the tier has been re-enabled

**Estimated rollback time:** Under 2 hours

#### Post-Removal Validation

- [ ] Route 3 test requests through the chain; confirm none attempt to route to Tier 4 or Tier 5
- [ ] **Route 2 vendor-related test requests** through the chain; confirm Milton's vendor terms check fires at Tier 3 (Compliance / COI Check) and does not look for the old Tier 5 exception path
- [ ] Verify Milton can access his vendor tracking data within the Tier 3 tooling
- [ ] Check system logs for errors referencing the deactivated tier or the exception routing logic
- [ ] Historical reports still display Tier 5 data for previously processed requests
- [ ] Monitor for 48 hours: no unexpected stalls or routing failures, especially on vendor-related requests
- [ ] Confirm with Tom that he is no longer receiving exception routing requests at this tier
- [ ] Confirm with Milton that his vendor verification function is working within Tier 3

---

### 2.3 Tier 6 — Samir's Secondary Review (Samir Nagheenanajar)

**Removal Order:** 3rd
**Scheduled Window:** Weekend Day 1 (third weekend), Saturday 6:00 AM – 10:00 AM

#### Current Configuration

| Field | Detail |
|-------|--------|
| **System name** | Samir's Secondary Review (confirm exact name/ID in Phase 1 inventory) |
| **Position in chain** | 6th of 7 sequential approval steps |
| **Owner/Reviewer** | Samir Nagheenanajar |
| **What it checks** | Re-verifies everything the budget holder already checked at Tier 2 (Budget Authority Confirmation), including funds availability, budget line alignment, and spend authority |
| **Routing behavior** | Sequential — request waits here until Samir completes his review |
| **Duplication** | 100% overlap with Tier 2 (Budget Authority Confirmation). Samir described this as a self-imposed safety net due to no visibility into upstream verifications. |

#### Pre-Removal Verification Checklist

- [ ] Tier classification document confirms this tier is classified as elective
- [ ] The Bobs have signed off that no regulatory control depends on this tier
- [ ] Samir has been briefed and acknowledges this tier is not policy-mandated (per his own interview statements)
- [ ] Confirmed that Samir's Tier 2 (Budget Authority Confirmation) role is unaffected — he retains his mandatory function
- [ ] All active requests currently sitting at this tier have been inventoried
- [ ] Tiers 4 and 5 deactivation have been stable for at least 5 business days with no issues
- [ ] Sandbox testing of this tier's deactivation completed successfully
- [ ] No routing rules reference this tier as a prerequisite for any mandatory tier
- [ ] Configuration backup of production routing taken and verified restorable
- [ ] Michael Bolton confirms technical readiness and is available for the deactivation window

#### Deactivation Steps

1. **Michael Bolton** takes a full configuration snapshot of production routing
2. Navigate to the routing configuration module in the production environment
3. Locate Tier 6 (Samir's Secondary Review) by system name/ID
4. Set the tier status to **"Inactive"** (do NOT delete)
5. Update routing rules: requests that previously routed from Tier 3 → Tier 6 (with Tiers 4 and 5 already inactive) → Tier 7 should now route from Tier 3 → Tier 7 directly
6. Verify no orphaned routing paths exist
7. Run a test request through the updated path to confirm it bypasses Tier 6 and reaches Tier 7
8. Confirm historical request data and audit logs at Tier 6 remain intact
9. Document all changes with timestamps and before/after values

#### In-Flight Request Handling

**Strategy: Drain then deactivate**

- Before the deactivation window, identify all requests currently sitting at Tier 6
- For requests in Samir's queue: Samir completes his review of any request he's already started. For requests he hasn't touched, route them directly to the next active tier (Tier 7) with a note
- Samir's review queue should be cleared by end of business Friday before the Saturday deactivation window
- **Target: zero requests at Tier 6 before deactivation.** Samir is likely to cooperate given his own statements about the tier being redundant

#### Rollback Procedure

1. Navigate to routing configuration module
2. Set Tier 6 status back to **"Active"**
3. Restore routing rules from the pre-change configuration backup
4. Run a test request to confirm Samir's review tier is receiving requests again
5. Verify audit trail integrity for requests processed during the deactivation period
6. Notify Samir and stakeholders that the tier has been re-enabled

**Estimated rollback time:** Under 2 hours

#### Post-Removal Validation

- [ ] Route 3 test requests through the chain; confirm none attempt to route to Tier 4, Tier 5, or Tier 6
- [ ] Verify that Tier 2 (Budget Authority Confirmation) continues to function correctly — Samir's mandatory role is unaffected
- [ ] Check system logs for errors referencing the deactivated tier
- [ ] Historical reports still display Tier 6 data for previously processed requests
- [ ] Monitor for 48 hours: no unexpected stalls or routing failures
- [ ] Confirm with Samir that he is no longer receiving review requests at Tier 6 but continues to receive them at Tier 2

---

### 2.4 Tier 7 — Lumbergh's Informal Visibility Gate (Bill Lumbergh)

**Removal Order:** 4th (final)
**Scheduled Window:** Weekend Day 1 (fourth weekend), Saturday 6:00 AM – 10:00 AM

#### Current Configuration

| Field | Detail |
|-------|--------|
| **System name** | Lumbergh's Informal Visibility Gate (confirm exact name/ID in Phase 1 inventory) |
| **Position in chain** | 7th of 7 sequential approval steps (final step) |
| **Owner/Reviewer** | Bill Lumbergh |
| **What it checks** | Nothing, functionally. Lumbergh doesn't reject requests. He reviews for awareness. Described as a "courtesy copy" by Lumbergh, a "1-2 day delay" by Peter. |
| **Routing behavior** | Sequential — request waits here until Lumbergh acknowledges. Adds 1-2 business days to every request over $5K. |
| **Replacement** | Lumbergh Dashboard provides real-time visibility with alerts, metrics, and drill-down without blocking the approval path. |

#### Pre-Removal Verification Checklist

- [ ] Tier classification document confirms this tier is classified as elective
- [ ] The Bobs have signed off that no regulatory control depends on this tier
- [ ] Lumbergh Dashboard is live in production, has been operational for at least 2 weeks, and Lumbergh has confirmed it meets his visibility needs (Phase 2, Activity 2.9)
- [ ] Lumbergh has been briefed on the specific date and time of deactivation
- [ ] Dashboard alerts are functioning: SLA breach warnings, high-value request notifications, rejections, volume spikes
- [ ] Lumbergh is NOT a designated approver at any of the 3 mandatory regulatory tiers (confirm during Phase 1)
- [ ] All active requests currently sitting at this tier have been inventoried
- [ ] Tiers 4, 5, and 6 deactivation have been stable with no issues
- [ ] Sandbox testing of this tier's deactivation completed successfully
- [ ] Configuration backup taken and verified
- [ ] Michael Bolton confirms technical readiness

#### Deactivation Steps

1. **Michael Bolton** takes a full configuration snapshot of production routing
2. Navigate to the routing configuration module
3. Locate Tier 7 (Lumbergh's Informal Visibility Gate) by system name/ID
4. Set the tier status to **"Inactive"** (do NOT delete)
5. Update routing rules: requests that previously routed from Tier 3 → Tier 7 (with Tiers 4, 5, and 6 already inactive) should now complete at Tier 3. Tier 3 (Compliance / COI Check) becomes the final approval step.
6. Verify the dashboard continues to receive data even after the tier is deactivated (the dashboard reads from the approval system's data, not from the tier itself — confirm this architecture with Michael)
7. Run a test request through the full chain: Tier 1 (Requestor Verification) → Tier 2 (Budget Authority Confirmation) → Tier 3 (Compliance / COI Check) and confirm the request completes at Tier 3 without attempting to route further
8. Verify Lumbergh receives a dashboard notification for the test request (proving the dashboard works independently of the approval gate)
9. Confirm historical data and audit logs at Tier 7 remain intact
10. Document all changes with timestamps

#### In-Flight Request Handling

**Strategy: Fast-track then deactivate**

- Identify all requests currently sitting at Tier 7 awaiting Lumbergh's acknowledgment
- Lumbergh acknowledges all pending requests by end of business Friday. Since he never rejects anything, this is a formality of clicking through his queue
- If Lumbergh is unavailable Friday, Peter or Michael manually advances the requests to completion (Peter has been doing this informally already)
- **Target: zero requests at Tier 7 before deactivation**

#### Rollback Procedure

1. Navigate to routing configuration module
2. Set Tier 7 status back to **"Active"**
3. Restore routing rules from pre-change backup
4. Run a test request to confirm Lumbergh's gate is receiving requests again
5. **Keep the dashboard live regardless** — it provides value whether or not the gate exists
6. Notify Lumbergh and stakeholders

**Estimated rollback time:** Under 2 hours

#### Post-Removal Validation

- [ ] Route 3 test requests through the chain; confirm they complete at Tier 3 (Compliance / COI Check) without attempting to route to any deactivated tier
- [ ] Verify the Lumbergh Dashboard shows the test requests with correct status
- [ ] Verify dashboard alerts fire correctly (e.g., high-value request notification)
- [ ] Check system logs for errors referencing any deactivated tier
- [ ] Historical reports still display Tier 7 data for old requests
- [ ] Monitor for 48 hours: no routing failures, no requests stalling after Tier 3
- [ ] Check that Lumbergh is not re-inserting himself informally (asking reviewers to "hold requests until I see them")
- [ ] Confirm with Peter that the 1-2 day delay per request is eliminated

---

## 3. Recommended Timing and Schedule

### Window Selection

All tier removals should occur during **off-hours weekend windows** to minimize impact on active users and give time for validation before the Monday work week begins.

| Removal | Target Window | Duration | Day |
|---------|---------------|----------|-----|
| Tier 4 (Tom's Routing Accuracy Review) | Saturday 6:00 AM – 10:00 AM | 4 hours | Week 5, Day 1 |
| Tier 5 (Tom's Exception Routing Path) | Saturday 6:00 AM – 10:00 AM | 4 hours | Week 5, Day 1 (or Week 6 if spacing preferred) |
| Tier 6 (Samir's Secondary Review) | Saturday 6:00 AM – 10:00 AM | 4 hours | Week 6, Day 1 |
| Tier 7 (Lumbergh's Visibility Gate) | Saturday 6:00 AM – 10:00 AM | 4 hours | Week 7, Day 1 |

**Option A (compressed):** Remove Tier 4 and Tier 5 on the same weekend (morning: Tier 4, afternoon: Tier 5 after Tier 4 validation passes). Both are Tom's tiers and removing them together reduces stakeholder disruption. However, Milton's vendor terms integration into Tier 3 must be verified between the two removals, which may make same-day execution risky.

**Option B (spaced, recommended):** One removal per weekend. Each tier gets a full week of monitoring before the next removal begins. This is slower but gives maximum evidence accumulation and stakeholder comfort. Particularly important for Tier 5 because Milton's vendor terms function needs real-world validation within Tier 3 before the next removal begins.

### Why Weekends
- Lowest request submission volume
- Michael Bolton is available without competing priorities
- Full day available for validation before Monday's request volume
- If rollback is needed, it happens before business users are affected

### Why Not All at Once
- Michael flagged the platform as fragile. Sequential removal isolates failure to a single change
- If multiple tiers are removed simultaneously and something breaks, root cause analysis becomes ambiguous
- Stakeholder trust builds incrementally. Removing everything at once looks like a power move. Removing one at a time looks like careful engineering.

---

## 4. Communication Touchpoints

Each tier removal requires communication before, during, and after.

### Before Each Removal (Thursday before the weekend window)

| Audience | Channel | Message |
|----------|---------|---------|
| Tier owner (Tom, Samir, or Lumbergh) | 1:1 meeting or call | Confirm the removal is happening this weekend. Review the plan. Ask if they have any last concerns. |
| All approvers in the chain | Email | "This weekend, [Tier Name] will be deactivated as part of the approved process simplification. Requests will route around this step starting Monday. No action needed from you. Contact [Lambert/Michael] with questions." |
| Requestors (via department managers) | Email from department managers | "Starting Monday, the approval process will have one fewer step. Your submission experience does not change. Approvals may be faster." |
| Michael Bolton | Standup or sync | Confirm he has the configuration backup, the deactivation steps reviewed, and his availability locked for Saturday morning. |
| Milton Waddams (Tier 5 removal only) | 1:1 meeting | Confirm his vendor terms verification is operational within Tier 3. Walk through 2-3 vendor requests together to verify. |

### During Each Removal (Saturday morning)

| Checkpoint | When | Who |
|------------|------|-----|
| Deactivation started | At execution start | Michael → Lambert (Slack/Teams message) |
| Deactivation complete | After status set to Inactive | Michael → Lambert |
| Routing rules updated | After routing rule change | Michael → Lambert |
| Test request passed | After test request completes | Michael → Lambert |
| Vendor test passed (Tier 5 only) | After vendor-flagged test completes at Tier 3 | Michael → Lambert + Milton |
| Post-removal validation started | After test | Michael → Lambert |
| Validation complete, all clear | After validation checklist passes | Lambert → stakeholders (email) |

### After Each Removal (Monday morning)

| Audience | Channel | Message |
|----------|---------|---------|
| All stakeholders | Email | "[Tier Name] has been successfully deactivated. Monitoring is active. The approval process now has [N] steps. No issues observed over the weekend. Reach out to [Lambert] with any questions." |
| The Bobs | Brief email or Slack | "Deactivation complete. Audit trail intact. Historical data preserved. Will send 48-hour monitoring summary on Wednesday." |
| Lumbergh | Dashboard walkthrough (for Tier 7 only) | Confirm dashboard is capturing all data that the gate previously surfaced. |

---

## 5. Testing Plan Post-Removal

### After Each Individual Tier Removal

Run the following tests within 4 hours of deactivation:

1. **Happy path test:** Submit 3 requests of different types (standard purchase, renewal, high-value). Verify each routes correctly through remaining active tiers without hitting the deactivated tier.
2. **Audit trail test:** For each test request, verify the audit log captures reviewer name, timestamp, and decision at every active tier.
3. **Edge case test:** Submit a request that historically required action at the deactivated tier (if identifiable). Verify it routes cleanly.
4. **Vendor terms test (after Tier 5 removal):** Submit a request involving a vendor from Milton's tracking list. Verify Milton's vendor terms check fires within Tier 3 and the request does not stall looking for the old exception path.
5. **Dashboard test (after Tier 7 removal):** Verify Lumbergh's dashboard shows test requests in real time.
6. **Historical data test:** Pull a report on requests that previously passed through the deactivated tier. Verify all data is accessible and unchanged.

### After All 4 Tiers Are Removed (Full Regression)

Once all elective tiers are deactivated, run a comprehensive validation:

1. **End-to-end flow test:** Submit 10 requests spanning all request types (standard purchase, renewal, high-value, new vendor, vendor exception). Verify each routes through exactly the 3 mandatory tiers: Tier 1 (Requestor Verification) → Tier 2 (Budget Authority Confirmation) → Tier 3 (Compliance / COI Check).
2. **Volume test:** Submit 20 requests in rapid succession. Verify no routing bottleneck or system instability.
3. **Compliance verification:** The Bobs review 5 completed requests and confirm the audit trail at each mandatory tier captures all required data points per the control matrix.
4. **Cycle time measurement:** Compare average cycle time for the 10 test requests against the baseline from the parallel run. Expect 30%+ improvement.
5. **Vendor terms verification:** Submit 5 vendor-related requests and verify Milton's vendor terms check fires at Tier 3 for each. Cross-reference against Milton's 47-vendor tracking list to confirm coverage.
6. **Dashboard validation:** Lumbergh reviews the dashboard with all test data and confirms visibility is maintained.
7. **Rollback drill:** Temporarily re-enable one deactivated tier, route a test request through it, then deactivate it again. Confirm the rollback and re-deactivation both work cleanly. This validates that rollback remains viable even after extended deactivation.
8. **Intake gate under load:** Submit 5 intentionally incomplete requests and 5 complete requests simultaneously. Verify the gate correctly blocks the incomplete ones and queues the complete ones.

### Monitoring Period

- **Days 1-5 after each removal:** Daily check of system logs, routing paths, request volumes, and error rates
- **Days 1-10 after final removal:** Extended monitoring of the full 3-tier flow with daily reports to Lambert and the Bobs
- **Day 15:** Final sign-off report summarizing all removals, test results, monitoring data, and any adjustments made

---

## 6. System Configuration Changes Summary

| Change | Where | What | Reversible |
|--------|-------|------|------------|
| Tier 4 status → Inactive | Routing configuration module | Status field change | Yes — set back to Active |
| Tier 5 status → Inactive | Routing configuration module | Status field change | Yes — set back to Active |
| Tier 6 status → Inactive | Routing configuration module | Status field change | Yes — set back to Active |
| Tier 7 status → Inactive | Routing configuration module | Status field change | Yes — set back to Active |
| Routing rule: skip Tier 4 | Routing rules config | Path update: Tier 3 → Tier 5 (skip Tier 4) | Yes — restore from backup |
| Routing rule: skip Tier 5 | Routing rules config | Path update: Tier 3 → Tier 6 (skip Tier 4 and 5) | Yes — restore from backup |
| Routing rule: skip Tier 6 | Routing rules config | Path update: Tier 3 → Tier 7 (skip Tiers 4, 5, 6) | Yes — restore from backup |
| Routing rule: skip Tier 7 | Routing rules config | Path update: Tier 3 is final step (skip Tiers 4, 5, 6, 7) | Yes — restore from backup |
| Milton's vendor terms → Tier 3 | Tier 3 compliance config | Add vendor terms sub-step within Compliance / COI Check | No — new permanent capability. Should remain even if Tier 5 is rolled back. |

**Critical pre-condition for every change:** Michael Bolton takes a labeled, timestamped configuration backup before each modification. This is the rollback safety net. No backup, no change.

**Critical pre-condition for Tier 5 removal:** Milton's vendor terms verification must be operational within Tier 3 before Tier 5 is deactivated. This is not just a config change — it's a capability migration.

---

## 7. Risk Considerations for Removal

| Risk | Mitigation |
|------|------------|
| Platform instability from routing changes | One tier at a time. Sandbox-validated. Configuration backups before each change. Michael on standby. |
| Tom escalates resistance after second tier removal | Both of Tom's tiers (4 and 5) are removed in sequence. Brief Tom before each. Frame the removals as capturing his institutional knowledge into the system rather than eliminating his role. Give him a monitoring role during validation periods. |
| Milton's vendor terms function lost during Tier 5 removal | Hard prerequisite: Milton's vendor terms check must be formalized into Tier 3 before Tier 5 is deactivated. Pre-flight test with vendor requests verifies the integration. If vendor check fails at Tier 3, Tier 5 stays active as the fallback. |
| Samir loses confidence in the process | Confirm Samir's Tier 2 (mandatory) role is unaffected. He retains his regulatory function. The redundant Tier 6 removal frees his time without reducing his authority. |
| Lumbergh re-inserts himself after gate removal | Dashboard must be genuinely good. Monitor for informal blocking behavior post-removal. |
| In-flight requests get stuck at deactivated tier | Drain strategy for each tier. Verify platform behavior when a tier is deactivated with requests in queue (sandbox test). |
| Dual-role approver loses mandatory access | Phase 1 must confirm no elective-tier owner also holds a mandatory role. Samir is the most likely overlap — his Tier 2 role must be protected when Tier 6 is removed. |
| Cumulative routing changes create unexpected interactions | Full regression test after all 4 removals. Rollback drill validates recovery. |

---

## Appendix: Quick Reference — Removal Checklist Per Tier

Use this as a gate check before each Saturday morning deactivation.

### Go/No-Go Gate (all must be YES)

- [ ] Configuration backup taken and verified?
- [ ] Sandbox test of this specific deactivation passed?
- [ ] Zero in-flight requests at this tier (or handling plan confirmed)?
- [ ] Michael Bolton available and ready?
- [ ] Stakeholders notified (Thursday communication sent)?
- [ ] Previous tier removal (if applicable) stable for 5+ business days?
- [ ] Rollback procedure reviewed by Michael?
- [ ] Milton's vendor terms operational in Tier 3? (Tier 5 removal only)

If any item is NO, postpone to the following weekend. Do not proceed with an incomplete checklist.
