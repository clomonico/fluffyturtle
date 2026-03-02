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
| 1st | Tier 4 | Operational Process Check | Tom Smykowski | Lowest | Peter already skips this on 70% of requests with no consequences. No regulatory basis. No downstream dependencies. Removing it changes almost nothing operationally because it's already being bypassed. |
| 2nd | Tier 3 | Secondary Budget Review | Samir Nagheenanajar | Low | Samir acknowledged it's not policy-mandated. He duplicates work from Tier 2 (Budget Holder Verification) because of a visibility gap, not a regulatory requirement. Once removed, Tier 2 covers the regulatory check. |
| 3rd | Tier 1 | Departmental Review | Tom Smykowski | Medium | This is Tom's other legacy step. Unlike Tier 4, some department managers have built habits around it. Removal disrupts a visible (if unnecessary) touchpoint. The intake gate replaces its documentation-check function. |
| 4th | Tier 6 | Executive Visibility Gate | Bill Lumbergh | Highest | Lumbergh has executive authority and may resist even with a dashboard replacement. His gate is the most organizationally sensitive removal. The dashboard must be live and validated before this tier is deactivated. |

**Why this order works:**
- Tier 4 first because it's already being bypassed, making it functionally removed. Formalizing its removal carries near-zero risk and builds evidence that the process works without it.
- Tier 3 second because Samir himself said it's redundant. His buy-in is already there.
- Tier 1 third because it affects Tom again, and doing both of his tiers back-to-back would amplify his resistance. Spacing them with Samir's tier in between gives Tom time to see that Tier 4's removal caused no problems.
- Tier 6 last because Lumbergh needs the most evidence and the most runway. By the time we reach his gate, three tiers are successfully deactivated and the dashboard has been live long enough to prove its value.

---

## 2. Tier Removal Procedures

### 2.1 Tier 4 — Operational Process Check (Tom Smykowski)

**Removal Order:** 1st
**Scheduled Window:** Weekend Day 1, Saturday 6:00 AM – 10:00 AM

#### Current Configuration

| Field | Detail |
|-------|--------|
| **System name** | Operational Process Check (confirm exact name/ID in Phase 1 inventory) |
| **Position in chain** | 4th of 7 sequential approval steps |
| **Owner/Reviewer** | Tom Smykowski |
| **What it checks** | Described as an "operational process check." Tom cannot articulate the full scope without consulting his files. Originally a temporary patch that became permanent. |
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
3. Locate Tier 4 (Operational Process Check) by system name/ID
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

### 2.2 Tier 3 — Secondary Budget Review (Samir Nagheenanajar)

**Removal Order:** 2nd
**Scheduled Window:** Weekend Day 1 (following weekend), Saturday 6:00 AM – 10:00 AM

#### Current Configuration

| Field | Detail |
|-------|--------|
| **System name** | Secondary Budget Review (confirm exact name/ID in Phase 1 inventory) |
| **Position in chain** | 3rd of 7 sequential approval steps |
| **Owner/Reviewer** | Samir Nagheenanajar |
| **What it checks** | Re-verifies everything the budget holder already checked at Tier 2, including funds availability, budget line alignment, and spend authority |
| **Routing behavior** | Sequential — request waits here until Samir completes his review |
| **Duplication** | 100% overlap with Tier 2 (Budget Holder Verification). Samir described this as a self-imposed safety net due to no visibility into upstream verifications. |

#### Pre-Removal Verification Checklist

- [ ] Tier classification document confirms this tier is classified as elective
- [ ] The Bobs have signed off that no regulatory control depends on this tier
- [ ] Samir has been briefed and acknowledges this tier is not policy-mandated (per his own interview statements)
- [ ] Confirmed that Samir does NOT hold a mandatory role at any of the 3 remaining regulatory tiers (Open Question #2 from implementation plan resolved)
- [ ] All active requests currently sitting at this tier have been inventoried
- [ ] Tier 4 deactivation has been stable for at least 5 business days with no issues
- [ ] Sandbox testing of this tier's deactivation completed successfully
- [ ] No routing rules reference this tier as a prerequisite for any mandatory tier
- [ ] Configuration backup of production routing taken and verified restorable
- [ ] Michael Bolton confirms technical readiness and is available for the deactivation window

#### Deactivation Steps

1. **Michael Bolton** takes a full configuration snapshot of production routing
2. Navigate to the routing configuration module in the production environment
3. Locate Tier 3 (Secondary Budget Review) by system name/ID
4. Set the tier status to **"Inactive"** (do NOT delete)
5. Update routing rules: requests that previously routed from Tier 2 → Tier 3 → Tier 4 (now inactive) → Tier 5 should now route from Tier 2 → Tier 5 directly
6. Verify no orphaned routing paths exist
7. Run a test request through the updated path to confirm it bypasses both Tier 3 and Tier 4 (both now inactive) and reaches Tier 5
8. Confirm historical request data and audit logs at Tier 3 remain intact
9. Document all changes with timestamps and before/after values

#### In-Flight Request Handling

**Strategy: Drain then deactivate**

- Before the deactivation window, identify all requests currently sitting at Tier 3
- For requests in Samir's queue: Samir completes his review of any request he's already started. For requests he hasn't touched, route them directly to the next active tier with a note
- Samir's review queue should be cleared by end of business Friday before the Saturday deactivation window
- **Target: zero requests at Tier 3 before deactivation.** Samir is likely to cooperate given his own statements about the tier being redundant

#### Rollback Procedure

1. Navigate to routing configuration module
2. Set Tier 3 status back to **"Active"**
3. Restore routing rules from the pre-change configuration backup
4. Run a test request to confirm Samir's review tier is receiving requests again
5. Verify audit trail integrity for requests processed during the deactivation period
6. Notify Samir and stakeholders that the tier has been re-enabled

**Estimated rollback time:** Under 2 hours

#### Post-Removal Validation

- [ ] Route 3 test requests through the chain; confirm none attempt to route to Tier 3 or Tier 4
- [ ] Verify that Tier 2 (Budget Holder Verification) continues to function correctly and is now followed by Tier 5 (Compliance) directly
- [ ] Check system logs for errors referencing the deactivated tier
- [ ] Historical reports still display Tier 3 data for previously processed requests
- [ ] Monitor for 48 hours: no unexpected stalls or routing failures
- [ ] Confirm with Samir that he is no longer receiving review requests at this tier

---

### 2.3 Tier 1 — Departmental Review (Tom Smykowski)

**Removal Order:** 3rd
**Scheduled Window:** Weekend Day 1 (third weekend), Saturday 6:00 AM – 10:00 AM

#### Current Configuration

| Field | Detail |
|-------|--------|
| **System name** | Departmental Review (confirm exact name/ID in Phase 1 inventory) |
| **Position in chain** | 1st of 7 sequential approval steps (entry point after submission) |
| **Owner/Reviewer** | Requesting department manager (varies) |
| **What it checks** | Manager confirms the request "makes sense." No specific regulatory criteria. Tom added it as a quality gate. |
| **Routing behavior** | Sequential — first stop after submission. Request waits here until department manager acts. |
| **Replacement** | Intake gate (documentation completeness enforcement) replaces the quality-check function. Budget holder at Tier 2 evaluates business justification. |

#### Pre-Removal Verification Checklist

- [ ] Tier classification document confirms this tier is classified as elective
- [ ] The Bobs have signed off that no regulatory control depends on this tier
- [ ] Tom Smykowski has been briefed (this is his second tier being removed; manage this communication carefully)
- [ ] Department managers who previously reviewed at this tier have been notified that the intake gate now handles documentation validation
- [ ] Intake gate validation rules are live and functioning in production (requests cannot enter the queue incomplete)
- [ ] All active requests currently sitting at this tier have been inventoried
- [ ] Tiers 3 and 4 deactivation have been stable with no issues
- [ ] Sandbox testing of this tier's deactivation completed successfully
- [ ] No routing rules reference this tier as a prerequisite for any mandatory tier
- [ ] Configuration backup taken and verified
- [ ] Michael Bolton confirms technical readiness

#### Deactivation Steps

1. **Michael Bolton** takes a full configuration snapshot of production routing
2. Navigate to the routing configuration module
3. Locate Tier 1 (Departmental Review) by system name/ID
4. Set the tier status to **"Inactive"** (do NOT delete)
5. Update routing rules: requests that previously routed from submission → Tier 1 → Tier 2 should now route from submission (after passing the intake gate) → Tier 2 directly
6. Verify the intake gate still functions correctly as the new first touchpoint — submissions must still be validated for completeness before entering the queue
7. Run a test request from submission through intake gate → Tier 2 to confirm the path works end-to-end
8. Confirm historical data and audit logs at Tier 1 remain intact
9. Document all changes with timestamps

#### In-Flight Request Handling

**Strategy: Drain then deactivate**

- Identify all requests currently sitting at Tier 1 awaiting department manager review
- Since this is the entry tier, there may be a higher volume of in-flight requests than other tiers
- For requests already at Tier 1: department managers complete their review by end of business Friday. Any remaining requests are manually routed to Tier 2 with a note
- Requests submitted after the deactivation will flow directly to Tier 2 through the intake gate — no action needed for new submissions
- **Target: zero requests at Tier 1 before deactivation.** If volume is high, extend the drain period to the previous Thursday and notify department managers of the Friday deadline

#### Rollback Procedure

1. Navigate to routing configuration module
2. Set Tier 1 status back to **"Active"**
3. Restore routing rules from pre-change backup
4. Run a test request to confirm departmental review is receiving requests again
5. Verify the intake gate continues to function even with Tier 1 re-enabled (they're independent)
6. Notify department managers and stakeholders

**Estimated rollback time:** Under 2 hours

#### Post-Removal Validation

- [ ] Route 3 test requests from submission through the full chain; confirm they land at Tier 2 (Budget Holder Verification) as the first approval step after the intake gate
- [ ] Verify the intake gate is correctly blocking incomplete submissions (it must not break when Tier 1 is removed)
- [ ] Check system logs for errors referencing Tier 1
- [ ] Historical reports still display Tier 1 data for old requests
- [ ] Monitor for 48 hours: no routing failures, no requests landing in unexpected queues
- [ ] Collect feedback from 2-3 department managers on whether the transition is smooth

---

### 2.4 Tier 6 — Executive Visibility Gate (Bill Lumbergh)

**Removal Order:** 4th (final)
**Scheduled Window:** Weekend Day 1 (fourth weekend), Saturday 6:00 AM – 10:00 AM

#### Current Configuration

| Field | Detail |
|-------|--------|
| **System name** | Executive Visibility Gate (confirm exact name/ID in Phase 1 inventory) |
| **Position in chain** | 6th of 7 sequential approval steps |
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
- [ ] Tiers 1, 3, and 4 deactivation have been stable with no issues
- [ ] Sandbox testing of this tier's deactivation completed successfully
- [ ] Configuration backup taken and verified
- [ ] Michael Bolton confirms technical readiness

#### Deactivation Steps

1. **Michael Bolton** takes a full configuration snapshot of production routing
2. Navigate to the routing configuration module
3. Locate Tier 6 (Executive Visibility Gate) by system name/ID
4. Set the tier status to **"Inactive"** (do NOT delete)
5. Update routing rules: requests that previously routed from Tier 5 → Tier 6 → Tier 7 should now route from Tier 5 → Tier 7 directly
6. Verify the dashboard continues to receive data even after the tier is deactivated (the dashboard reads from the approval system's data, not from the tier itself — confirm this architecture with Michael)
7. Run a test request through Tier 5 → Tier 7 to confirm the path works
8. Verify Lumbergh receives a dashboard notification for the test request (proving the dashboard works independently of the approval gate)
9. Confirm historical data and audit logs at Tier 6 remain intact
10. Document all changes with timestamps

#### In-Flight Request Handling

**Strategy: Fast-track then deactivate**

- Identify all requests currently sitting at Tier 6 awaiting Lumbergh's acknowledgment
- Lumbergh acknowledges all pending requests by end of business Friday. Since he never rejects anything, this is a formality of clicking through his queue
- If Lumbergh is unavailable Friday, Peter or Michael manually advances the requests to Tier 7 (Peter has been doing this informally already)
- **Target: zero requests at Tier 6 before deactivation**

#### Rollback Procedure

1. Navigate to routing configuration module
2. Set Tier 6 status back to **"Active"**
3. Restore routing rules from pre-change backup
4. Run a test request to confirm Lumbergh's gate is receiving requests again
5. **Keep the dashboard live regardless** — it provides value whether or not the gate exists
6. Notify Lumbergh and stakeholders

**Estimated rollback time:** Under 2 hours

#### Post-Removal Validation

- [ ] Route 3 test requests through the chain; confirm they flow from Tier 5 (Compliance) directly to Tier 7 (Financial Controls Sign-off)
- [ ] Verify the Lumbergh Dashboard shows the test requests with correct status
- [ ] Verify dashboard alerts fire correctly (e.g., high-value request notification)
- [ ] Check system logs for errors referencing the deactivated tier
- [ ] Historical reports still display Tier 6 data for old requests
- [ ] Monitor for 48 hours: no routing failures, no requests stalling between Tier 5 and Tier 7
- [ ] Check that Lumbergh is not re-inserting himself informally (asking reviewers to "hold requests until I see them")
- [ ] Confirm with Peter that the 1-2 day delay per request is eliminated

---

## 3. Recommended Timing and Schedule

### Window Selection

All tier removals should occur during **off-hours weekend windows** to minimize impact on active users and give time for validation before the Monday work week begins.

| Removal | Target Window | Duration | Day |
|---------|---------------|----------|-----|
| Tier 4 (Tom's process check) | Saturday 6:00 AM – 10:00 AM | 4 hours | Week 5, Day 1 |
| Tier 3 (Samir's review) | Saturday 6:00 AM – 10:00 AM | 4 hours | Week 5, Day 1 (or Week 6 if spacing preferred) |
| Tier 1 (Departmental review) | Saturday 6:00 AM – 10:00 AM | 4 hours | Week 6, Day 1 |
| Tier 6 (Lumbergh's gate) | Saturday 6:00 AM – 10:00 AM | 4 hours | Week 7, Day 1 |

**Option A (compressed):** Remove Tier 4 and Tier 3 on the same weekend (morning: Tier 4, afternoon: Tier 3 after Tier 4 validation passes). This compresses the schedule but increases the blast radius if something goes wrong.

**Option B (spaced, recommended):** One removal per weekend. Each tier gets a full week of monitoring before the next removal begins. This is slower but gives maximum evidence accumulation and stakeholder comfort.

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

### During Each Removal (Saturday morning)

| Checkpoint | When | Who |
|------------|------|-----|
| Deactivation started | At execution start | Michael → Lambert (Slack/Teams message) |
| Deactivation complete | After step 4 (status set to Inactive) | Michael → Lambert |
| Routing rules updated | After step 5 | Michael → Lambert |
| Test request passed | After test request completes | Michael → Lambert |
| Post-removal validation started | After test | Michael → Lambert |
| Validation complete, all clear | After validation checklist passes | Lambert → stakeholders (email) |

### After Each Removal (Monday morning)

| Audience | Channel | Message |
|----------|---------|---------|
| All stakeholders | Email | "[Tier Name] has been successfully deactivated. Monitoring is active. The approval process now has [N] steps. No issues observed over the weekend. Reach out to [Lambert] with any questions." |
| The Bobs | Brief email or Slack | "Deactivation complete. Audit trail intact. Historical data preserved. Will send 48-hour monitoring summary on Wednesday." |
| Lumbergh | Dashboard walkthrough (for Tier 6 only) | Confirm dashboard is capturing all data that the gate previously surfaced. |

---

## 5. Testing Plan Post-Removal

### After Each Individual Tier Removal

Run the following tests within 4 hours of deactivation:

1. **Happy path test:** Submit 3 requests of different types (standard purchase, renewal, high-value). Verify each routes correctly through remaining active tiers without hitting the deactivated tier.
2. **Audit trail test:** For each test request, verify the audit log captures reviewer name, timestamp, and decision at every active tier.
3. **Edge case test:** Submit a request that historically required action at the deactivated tier (if identifiable). Verify it routes cleanly.
4. **Dashboard test (after Tier 6 removal):** Verify Lumbergh's dashboard shows test requests in real time.
5. **Intake gate test (after Tier 1 removal):** Submit an intentionally incomplete request. Verify it's blocked before entering the queue.
6. **Historical data test:** Pull a report on requests that previously passed through the deactivated tier. Verify all data is accessible and unchanged.

### After All 4 Tiers Are Removed (Full Regression)

Once all elective tiers are deactivated, run a comprehensive validation:

1. **End-to-end flow test:** Submit 10 requests spanning all request types (standard purchase, renewal, high-value, new vendor, vendor exception). Verify each routes through exactly the 3 mandatory tiers: Budget Verification → Compliance Review → Financial Controls Sign-off.
2. **Volume test:** Submit 20 requests in rapid succession. Verify no routing bottleneck or system instability.
3. **Compliance verification:** The Bobs review 5 completed requests and confirm the audit trail at each mandatory tier captures all required data points per the control matrix.
4. **Cycle time measurement:** Compare average cycle time for the 10 test requests against the baseline from the parallel run. Expect 30%+ improvement.
5. **Dashboard validation:** Lumbergh reviews the dashboard with all test data and confirms visibility is maintained.
6. **Rollback drill:** Temporarily re-enable one deactivated tier, route a test request through it, then deactivate it again. Confirm the rollback and re-deactivation both work cleanly. This validates that rollback remains viable even after extended deactivation.
7. **Intake gate under load:** Submit 5 intentionally incomplete requests and 5 complete requests simultaneously. Verify the gate correctly blocks the incomplete ones and queues the complete ones.

### Monitoring Period

- **Days 1-5 after each removal:** Daily check of system logs, routing paths, request volumes, and error rates
- **Days 1-10 after final removal:** Extended monitoring of the full 3-tier flow with daily reports to Lambert and the Bobs
- **Day 15:** Final sign-off report summarizing all removals, test results, monitoring data, and any adjustments made

---

## 6. System Configuration Changes Summary

| Change | Where | What | Reversible |
|--------|-------|------|------------|
| Tier 4 status → Inactive | Routing configuration module | Status field change | Yes — set back to Active |
| Tier 3 status → Inactive | Routing configuration module | Status field change | Yes — set back to Active |
| Tier 1 status → Inactive | Routing configuration module | Status field change | Yes — set back to Active |
| Tier 6 status → Inactive | Routing configuration module | Status field change | Yes — set back to Active |
| Routing rule: skip Tier 4 | Routing rules config | Path update: Tier 3 → Tier 5 | Yes — restore from backup |
| Routing rule: skip Tier 3 | Routing rules config | Path update: Tier 2 → Tier 5 | Yes — restore from backup |
| Routing rule: skip Tier 1 | Routing rules config | Path update: Intake → Tier 2 | Yes — restore from backup |
| Routing rule: skip Tier 6 | Routing rules config | Path update: Tier 5 → Tier 7 | Yes — restore from backup |

**Critical pre-condition for every change:** Michael Bolton takes a labeled, timestamped configuration backup before each modification. This is the rollback safety net. No backup, no change.

---

## 7. Risk Considerations for Removal

| Risk | Mitigation |
|------|------------|
| Platform instability from routing changes | One tier at a time. Sandbox-validated. Configuration backups before each change. Michael on standby. |
| Tom escalates resistance after second tier removal | Space removals apart. Brief Tom before each. Give him monitoring role during validation periods. |
| Lumbergh re-inserts himself after gate removal | Dashboard must be genuinely good. Monitor for informal blocking behavior post-removal. |
| In-flight requests get stuck at deactivated tier | Drain strategy for each tier. Verify platform behavior when a tier is deactivated with requests in queue (sandbox test). |
| Dual-role approver loses mandatory access | Phase 1 must confirm no elective-tier owner also holds a mandatory role. Samir is the most likely overlap; resolve before any removal. |
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

If any item is NO, postpone to the following weekend. Do not proceed with an incomplete checklist.
