# Parallel Routing Configuration Specification

**Issue:** #35
**Initiative:** Glass Floor (Init 2)
**Epic:** 12 — Parallel Routing
**Author:** Dallas (Process Designer)
**Date:** 2026-03-02
**Prerequisite:** Feasibility spike (#34) returned a "go" recommendation
**Configurator:** Michael Bolton (platform lead)

---

## 1. Parallel routing architecture

### Current sequential flow (before)

```
Request submitted
  → Tier 1: Requestor Verification (Dept Head)
    → Tier 2: Budget Authority Confirmation (Samir)
      → Tier 3: Compliance / COI Check (Milton's vendor terms)
        → Complete
```

### Target parallel flow (after)

```
Request submitted
  → Tier 1: Requestor Verification (Dept Head)  [sequential gate]
    → FORK ──┬── Tier 2: Budget Authority Confirmation (Samir)
             │
             ├── Tier 3: Compliance / COI Check (Milton's vendor terms)
             │
    → MERGE ─┘
      → Complete
```

### Architectural decisions

| Decision | Choice | Rationale |
|---|---|---|
| Tier 1 remains sequential | Yes | Requestor verification is a prerequisite for all downstream approvals. Budget and compliance reviewers need a verified request. |
| Tier 2 and Tier 3 run in parallel | Yes | Budget review and compliance check are functionally independent. Neither requires the other's output to begin. |
| Fork type | AND-split (both paths always execute) | Every request requires both budget and compliance approval. No conditional branching. |
| Merge type | AND-join with early termination on rejection | Both must approve for the request to advance. A rejection on either path halts the other. |

### What does NOT change

- Tier 1 approval criteria and approver assignment (Dept Head)
- Tier 2 approval criteria (budget thresholds, Samir's delegation rules)
- Tier 3 approval criteria (compliance checklist, Milton's vendor terms review)
- Request submission form and data capture
- Notification templates (content unchanged; delivery timing changes)

---

## 2. Routing flow diagram

### Primary flow (both approve)

```
 ┌──────────────────────────┐
 │   Request Submitted      │
 └────────────┬─────────────┘
              ▼
 ┌──────────────────────────┐
 │  Tier 1: Dept Head       │
 │  Requestor Verification  │
 └────────────┬─────────────┘
              │ APPROVED
              ▼
        ┌─── FORK ───┐
        │             │
        ▼             ▼
 ┌─────────────┐ ┌─────────────┐
 │  Tier 2:    │ │  Tier 3:    │
 │  Budget     │ │  Compliance │
 │  Authority  │ │  / COI      │
 │  (Samir)    │ │  (Milton's  │
 │             │ │   terms)    │
 └──────┬──────┘ └──────┬──────┘
        │ APPROVED      │ APPROVED
        │               │
        └──── MERGE ────┘
              │
              ▼
 ┌──────────────────────────┐
 │       Complete           │
 └──────────────────────────┘
```

### Rejection flow (either path rejects)

```
        ┌─── FORK ───┐
        │             │
        ▼             ▼
 ┌─────────────┐ ┌─────────────┐
 │  Tier 2:    │ │  Tier 3:    │
 │  Budget     │ │  Compliance │
 │  (Samir)    │ │  (Milton's  │
 └──────┬──────┘ └──────┬──────┘
        │               │ REJECTED
        │ (still open)  │
        │               ▼
        │        ┌─────────────┐
        │        │  Halt signal│
        │        │  sent to    │
        │◄───────│  Tier 2     │
        │        └─────────────┘
        ▼
 ┌──────────────────────────┐
 │  Request REJECTED        │
 │  Reason: Tier 3 finding  │
 │  Tier 2 task cancelled   │
 └──────────────────────────┘
```

### Tier 1 rejection flow

```
 ┌──────────────────────────┐
 │   Request Submitted      │
 └────────────┬─────────────┘
              ▼
 ┌──────────────────────────┐
 │  Tier 1: Dept Head       │
 │  REJECTED                │
 └────────────┬─────────────┘
              ▼
 ┌──────────────────────────┐
 │  Request returned to     │
 │  requestor. No fork.     │
 └──────────────────────────┘
```

---

## 3. Merge logic specification

### 3.1 Primary rule: all-must-approve

Both Tier 2 and Tier 3 must return an "Approved" decision for the request to advance to the Complete state.

| Tier 2 result | Tier 3 result | Request outcome |
|---|---|---|
| Approved | Approved | **Approved** — advances to Complete |
| Approved | Rejected | **Rejected** — compliance finding |
| Rejected | Approved | **Rejected** — budget authority finding |
| Rejected | Rejected | **Rejected** — both findings recorded |

### 3.2 First-rejection-halts-all

When one parallel path returns a rejection:

1. The merge node records the rejection reason and timestamp
2. The platform sends a cancellation signal to the still-open parallel path
3. The pending approver's task is marked "Cancelled — parallel path rejected" (not "Rejected")
4. The requestor receives a single rejection notification citing the rejecting tier

**Why halt early?** Continuing the second review wastes approver time. If compliance has already flagged a policy violation, there is no value in completing the budget review for a request that cannot proceed.

### 3.3 Timeout handling

| Scenario | Timeout period | Action |
|---|---|---|
| One path completes, other path pending | 3 business days from fork | Escalation notification sent to the pending approver's manager |
| Both paths pending, no action | 3 business days from fork | Escalation notifications sent to both approvers' managers |
| One path pending after escalation | 5 business days from fork | Request auto-escalated to Samir (for Tier 2) or designated compliance backup (for Tier 3) |
| Both paths still pending after escalation | 5 business days from fork | Request flagged for manual intervention; admin notification sent to Michael Bolton |

Timeout clock starts when the fork dispatches both tasks. Weekends and company holidays are excluded from business day counts.

### 3.4 Return-for-rework

If an approver returns a request for rework (not a rejection, but "needs more information"):

1. Both parallel paths are paused
2. The request returns to the requestor with the rework comments
3. When the requestor resubmits, the request re-enters the fork and both Tier 2 and Tier 3 review again from scratch
4. Previous partial approvals are not carried forward (clean slate on resubmission)

---

## 4. Segregation of duties enforcement

### 4.1 Core rule

No individual may hold approval authority on both Tier 2 and Tier 3 for the same request. This prevents a single person from controlling both budget and compliance decisions.

### 4.2 Enforcement mechanisms

| Mechanism | Implementation |
|---|---|
| Role-based assignment | Tier 2 approvers are assigned from the "Budget Authority" role. Tier 3 approvers are assigned from the "Compliance Reviewer" role. These roles are mutually exclusive in the platform. |
| Same-person block | If a delegate or backup assignment would result in the same person on both parallel paths, the platform rejects the assignment and alerts the admin. |
| Audit check | Every completed parallel request is flagged if the same user ID appears on both Tier 2 and Tier 3 (should never happen, but provides a safety net). |

### 4.3 Delegation and backup scenarios

| Scenario | Allowed? | Rule |
|---|---|---|
| Samir delegates Tier 2 to a budget team member | Yes | Delegate must not hold a Compliance Reviewer role |
| Milton is unavailable; compliance backup is assigned | Yes | Backup must not hold a Budget Authority role |
| Samir is temporarily assigned compliance backup duty | No | Blocked by mutual exclusion rule |
| A new employee is added to both role groups by mistake | Blocked | Platform prevents dual-role assignment for parallel workflow participants |

### 4.4 Requestor exclusion

The requestor who submitted the request cannot serve as an approver at any tier. This is enforced at Tier 1 (Dept Head cannot be the requestor's self) and carries through to Tier 2 and Tier 3.

---

## 5. Configuration steps for Michael Bolton

### Pre-configuration checklist

- [ ] Feasibility spike (#34) returned "go" recommendation
- [ ] Sandbox testing of parallel routing completed successfully
- [ ] Rollback procedure (Section 9) reviewed and ready
- [ ] Change window scheduled (recommend off-peak: Friday after 4 PM or weekend)
- [ ] Samir and Milton's team notified of upcoming change

### Step-by-step configuration

**Phase 1: Workflow structure**

1. Open the workflow editor for the production approval workflow
2. Select Tier 1 (Requestor Verification) as the entry node — no changes needed
3. After Tier 1's "Approved" transition, insert a **parallel split node** (AND-split)
4. Create two parallel branches from the split node:
   - Branch A: Tier 2 — Budget Authority Confirmation
   - Branch B: Tier 3 — Compliance / COI Check
5. After both branches, insert a **parallel merge node** (AND-join)
6. Configure the merge node completion rule: "All branches must complete"
7. Connect the merge node's "All Approved" transition to the Complete state
8. Connect the merge node's "Any Rejected" transition to the Rejected state

**Phase 2: Merge logic**

9. On the merge node, enable "Early termination on rejection"
10. Configure the cancellation message for the halted path: "This review has been cancelled. The parallel review path returned a rejection."
11. Set the merge node timeout to 3 business days (escalation) / 5 business days (auto-escalation)
12. Configure timeout notifications to route to the pending approver's manager

**Phase 3: Segregation of duties**

13. Open the role management console
14. Create (or verify) two roles: "Budget Authority" and "Compliance Reviewer"
15. Enable mutual exclusion constraint between these two roles for the parallel workflow
16. Add Samir (and any delegates) to "Budget Authority"
17. Add compliance team members to "Compliance Reviewer"
18. Enable the "same-person block" rule on the parallel split node
19. Test: attempt to assign the same user to both roles — confirm the platform rejects it

**Phase 4: Notifications**

20. Update fork notification: when the split node fires, send simultaneous task notifications to Tier 2 and Tier 3 approvers
21. Update rejection notification: include the rejecting tier name and reason in the requestor notification
22. Update cancellation notification: the halted-path approver receives a clear explanation that their review is no longer needed
23. Update timeout escalation notification: include request ID, pending approver name, and elapsed time

**Phase 5: Validation**

24. Run one end-to-end test request through the parallel flow (both approve)
25. Run one test with a Tier 3 rejection to verify early termination
26. Verify audit trail captures both parallel path events with correct timestamps
27. Confirm segregation of duties block triggers when tested
28. Review notification delivery for all scenarios

### Post-configuration

- [ ] Screenshot final workflow configuration for documentation
- [ ] Export workflow definition as backup
- [ ] Update the platform configuration changelog
- [ ] Notify Samir and Milton's team that parallel routing is live

---

## 6. Fallback: degradation and failure handling

### 6.1 Automatic fallback to sequential

If the platform's parallel routing engine encounters an error during the fork operation, the system should degrade gracefully:

| Failure mode | Detection | Fallback behavior |
|---|---|---|
| Fork node fails to dispatch both tasks | Only one task created within 60 seconds of fork | System completes the dispatched task sequentially, then dispatches the second. Admin alert sent. |
| Merge node fails to detect completion | Both tasks marked complete but merge node doesn't fire within 5 minutes | Admin alert. Manual merge by Michael (advance request to Complete). |
| Platform outage during parallel review | Platform unavailable for more than 30 minutes while parallel tasks are active | Tasks resume when platform returns. No data loss (tasks persist in database). Approvers re-notified. |
| Notification failure | Task created but approver not notified | Retry notification after 15 minutes. If retry fails, admin alert for manual notification. |

### 6.2 Manual override

Michael Bolton (or designated admin backup) can:

- Force-advance a request past the merge node if both approvals are recorded but the merge didn't trigger
- Reassign a parallel task if an approver is unexpectedly unavailable
- Convert a specific request from parallel back to sequential routing without changing the workflow for all requests

### 6.3 Circuit breaker

If more than 3 requests fail at the fork or merge node within a single business day:

1. Platform automatically reverts all new requests to sequential routing
2. Admin alert sent to Michael Bolton
3. Requests already in parallel continue in parallel (no mid-flight changes)
4. Michael investigates and manually re-enables parallel routing after root cause is resolved

---

## 7. Audit trail requirements

### 7.1 Events to capture

Every parallel routing event must be logged with the following data:

| Event | Logged data |
|---|---|
| Fork dispatched | Request ID, fork timestamp, Branch A task ID, Branch B task ID, assigned approvers |
| Tier 2 task received | Task ID, approver user ID, received timestamp |
| Tier 3 task received | Task ID, approver user ID, received timestamp |
| Tier 2 decision | Task ID, approver user ID, decision (Approved/Rejected), decision timestamp, comments |
| Tier 3 decision | Task ID, approver user ID, decision (Approved/Rejected), decision timestamp, comments |
| Early termination | Halted task ID, reason ("parallel path rejected"), cancellation timestamp |
| Merge completed | Request ID, merge timestamp, final outcome, elapsed time from fork to merge |
| Timeout escalation | Task ID, escalation level, escalation timestamp, escalated-to user ID |
| Fallback triggered | Request ID, failure mode, fallback action, timestamp |

### 7.2 Timestamp precision

All timestamps recorded in UTC with millisecond precision. This is critical for:

- Determining which path completed first
- Proving that fork dispatch was simultaneous (within 60-second window)
- Regulatory audit evidence that both reviews occurred

### 7.3 Sequence tracking

Because parallel events don't have a natural sequence, the audit trail must support:

- **Branch identification:** Every event tagged with its branch (A or B) so auditors can reconstruct each path independently
- **Causal ordering:** Fork timestamp < branch events < merge timestamp
- **Elapsed time per branch:** Time from task receipt to decision, per approver

### 7.4 Audit report

A new report (or a filter on the existing approval history report) showing:

- All requests that used parallel routing
- Per-request: fork time, both branch decisions with timestamps, merge time, total elapsed time
- Comparison: parallel elapsed time vs. estimated sequential elapsed time (sum of both branch durations)
- Anomalies: early terminations, timeouts, fallback events

---

## 8. Edge cases

### 8.1 Simultaneous completion

**Scenario:** Tier 2 and Tier 3 approvers submit their decisions at the exact same moment (within the same database transaction window).

**Handling:**
- The merge node processes whichever decision the database commits first
- If both are "Approved," the merge fires immediately — no issue
- If both are "Rejected," both rejection reasons are captured
- If one approves and one rejects at the same instant, the rejection takes precedence and early termination fires (but the other task is already complete, so cancellation is a no-op)
- The audit trail records both decisions with their respective timestamps; the merge event follows the later timestamp

### 8.2 Approver unavailability

**Scenario:** An assigned approver is out of office, on leave, or otherwise unable to act.

**Handling:**
- Timeout escalation kicks in at 3 business days (see Section 3.3)
- If the approver has a designated delegate configured in the platform, the task routes to the delegate automatically
- If no delegate is configured, the timeout escalation sends to the approver's manager
- The completed parallel path's decision is held at the merge node until the pending path resolves

**Important:** The completed path's approver is NOT asked to re-review just because the other path is slow.

### 8.3 Approver reassignment mid-review

**Scenario:** A Tier 2 or Tier 3 task is reassigned to a different person while the parallel flow is active.

**Handling:**
- Reassignment is allowed as long as the new assignee passes the segregation of duties check
- The original assignee's task is marked "Reassigned" (not "Cancelled")
- Audit trail captures: original assignee, reassignment timestamp, new assignee, reason for reassignment

### 8.4 Request recall during parallel review

**Scenario:** The requestor cancels/recalls their request while both Tier 2 and Tier 3 tasks are open.

**Handling:**
- Both parallel tasks are cancelled immediately
- Both approvers receive a "Request recalled by requestor" notification
- Audit trail captures the recall event and both cancellations
- The request status changes to "Recalled" (not "Rejected")

### 8.5 Approver changes decision after submission

**Scenario:** An approver submits "Approved" and then wants to change to "Rejected" (or vice versa).

**Handling:**
- If the merge node has NOT yet fired: the approver can amend their decision. The amended decision replaces the original in the merge logic.
- If the merge node HAS fired (both decisions are in and the request advanced): no amendment possible through the parallel workflow. A separate correction process must be used.
- Audit trail captures both the original and amended decisions.

### 8.6 System sends fork but only one task is created

**Scenario:** Platform glitch causes only one of the two parallel tasks to be created.

**Handling:**
- The fork node checks that both tasks exist within 60 seconds of dispatch
- If only one task exists, the fork node retries creation of the missing task (up to 3 retries)
- If retries fail, the system falls back to sequential routing for that request and sends an admin alert
- See Section 6.1 for full fallback behavior

---

## 9. Rollback procedure to sequential routing

### 9.1 When to rollback

- Parallel routing causes repeated failures (circuit breaker threshold: 3 failures/day)
- Platform upgrade breaks parallel routing functionality
- Business decision to revert (e.g., regulatory requirement for sequential processing)

### 9.2 Rollback steps

1. **Announce:** Notify Samir, Milton's team, and Dept Heads that routing is reverting to sequential
2. **Drain:** Allow all in-flight parallel requests to complete (do not interrupt mid-flow)
3. **Disable fork node:** In the workflow editor, disable the parallel split node
4. **Reconnect:** Route Tier 1 "Approved" transition directly to Tier 2, and Tier 2 "Approved" transition to Tier 3 (restore original sequential chain)
5. **Test:** Submit one test request to verify sequential flow works end to end
6. **Confirm:** Verify audit trail captures the test request sequentially
7. **Document:** Record rollback date, reason, and any data from the parallel period in the configuration changelog

### 9.3 In-flight request handling during rollback

| Request state at rollback time | Action |
|---|---|
| At Tier 1 (not yet forked) | Routes sequentially — no issue |
| At fork (both tasks dispatched) | Both tasks complete normally. Merge fires as configured. |
| One path complete, one pending | Pending path completes. Merge fires. Only NEW requests go sequential. |
| At merge (both complete, merge pending) | Merge fires. Request completes as parallel. |

**Rule:** Never interrupt or restructure a request that is already inside the parallel fork. Let it finish. Sequential routing applies only to requests that have not yet reached the fork node.

### 9.4 Re-enabling parallel routing

After the root cause is resolved:

1. Re-enable the fork node in the workflow editor
2. Run validation tests (Section 5, Phase 5)
3. Monitor the first 10 parallel requests closely
4. If all 10 succeed, resume normal operations

---

## 10. Documentation requirements for ongoing maintenance

### 10.1 Configuration documentation

| Document | Location | Owner | Update trigger |
|---|---|---|---|
| Workflow configuration screenshot | Platform admin wiki | Michael Bolton | Any workflow change |
| Parallel routing config spec (this document) | Project repository | Dallas | Architecture or logic changes |
| Role assignment matrix | Platform admin wiki | Michael Bolton | Any role membership change |
| Timeout and escalation settings | Platform admin wiki | Michael Bolton | Any threshold change |

### 10.2 Runbook entries

Michael Bolton maintains a runbook covering:

- How to diagnose a stuck merge node
- How to manually advance a request past a failed merge
- How to reassign a parallel task
- How to trigger a rollback to sequential routing
- How to re-enable parallel routing after a rollback
- How to add or remove approvers from Budget Authority / Compliance Reviewer roles

### 10.3 Knowledge transfer

Because Michael Bolton is the single point of failure for platform knowledge:

- All configuration steps (Section 5) are written for reproducibility by a second person
- At least one additional team member (designated backup) must walk through the configuration steps in sandbox before go-live
- The backup person must be able to execute the rollback procedure (Section 9) independently
- Quarterly review: Michael and the backup walk through the runbook together to keep knowledge current

### 10.4 Change management

Any change to the parallel routing configuration requires:

1. Description of the change and business justification
2. Impact assessment (which requests are affected)
3. Testing in sandbox before production
4. Approval from process owner (Dallas) and platform owner (Michael)
5. Update to this spec and the platform admin wiki
6. Notification to affected approvers (Samir, compliance team)

---

## Appendix A: Glossary

| Term | Definition |
|---|---|
| AND-split (fork) | A workflow node that dispatches two or more tasks simultaneously. All paths execute. |
| AND-join (merge) | A workflow node that waits for all incoming paths to complete before advancing. |
| Early termination | When a rejection on one parallel path cancels the remaining open paths. |
| Segregation of duties | A control requiring that different individuals perform different approval functions for the same request. |
| Circuit breaker | An automatic safety mechanism that reverts to sequential routing after repeated parallel routing failures. |

## Appendix B: Related issues

| Issue | Title | Relationship |
|---|---|---|
| #34 | Feasibility spike for parallel routing | Prerequisite — must return "go" before this configuration is applied |
| #20 | Tier classification audit | Defines the 3 mandatory tiers referenced in this spec |
| #21 | Stakeholder sign-off package | Samir and Milton sign off on parallel routing changes to their approval steps |
