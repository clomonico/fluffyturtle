# Pilot Validation Plan — Parallel Routing

**Issue:** #36
**Initiative:** Glass Floor (Init 2)
**Epic:** 12 — Parallel Routing
**Depends on:** #35 (Parallel Routing Configuration)
**Author:** Kane (Validator)
**Date:** 2026-03-02

---

## 1. Pilot group selection

### Selection criteria

| Criterion | Rationale |
|---|---|
| Minimum 2 distinct approval chains | Ensures parallel behavior is validated across different approver pairings, not just Samir + Milton |
| Covers at least 3 of the 5 request types | Confirms routing logic works for varied dollar thresholds and compliance requirements |
| Includes both high-volume and low-volume requestors | Tests queue pressure and timeout scenarios |
| Approvers willing to provide structured feedback | Pilot only works if approvers flag issues in real time |
| Department heads familiar with the current sequential flow | Tier 1 approvers need to recognize any behavioral changes at fork point |

### Pilot composition

| Role | Person | Tier | Responsibility during pilot |
|---|---|---|---|
| Tier 2 approver (primary) | Samir Nagheenanajar | Budget Authority | Approve/reject budget line for pilot requests |
| Tier 3 approver (primary) | Milton Waddams (vendor terms) | Compliance / COI | Approve/reject compliance check for pilot requests |
| Tier 2 backup | Samir's designated delegate | Budget Authority | Covers Samir's absence; validates delegation in parallel mode |
| Tier 3 backup | Designated compliance backup | Compliance / COI | Covers Milton's absence; validates backup routing in parallel mode |
| Tier 1 approvers | 2 department heads (from pilot departments) | Requestor Verification | Verify requests before fork; confirm fork behavior after their approval |
| Requestors | 8-12 staff from 2 pilot departments | Submission | Submit real requests through the parallel path |
| Platform monitor | Michael Bolton | Platform admin | Monitor system logs, routing events, and error conditions |
| Audit observers | The Bobs | Audit trail | Validate audit completeness and integrity throughout pilot |

### Pilot departments

Select 2 departments based on:

- Request volume of at least 3-5 requests per week (enough data in 2 weeks)
- Mix of request types (at least one department with new vendor requests)
- Department heads who participated in the design thinking sessions
- Low risk tolerance, meaning if the pilot surfaces problems the blast radius is contained

---

## 2. Request type coverage

All pilot requests flow through the parallel path (Tier 1 sequential, then Tier 2 + Tier 3 in parallel). The pilot must process requests across these categories:

| Request type | Target count during pilot | Why included |
|---|---|---|
| Standard purchase | 6-10 | Highest volume; validates the happy path |
| Renewal | 3-5 | Tests re-approval flow where prior history exists |
| High-value (>$50K) | 2-3 | Validates timeout escalation and executive alert integration (#29/#30) |
| New vendor | 2-3 | Heaviest compliance review; tests whether Milton's Tier 3 runs correctly in parallel |
| Vendor exception | 1-2 | Edge case; confirms merge logic when compliance returns "needs rework" |

**Minimum coverage threshold:** Pilot cannot enter evaluation phase until at least 15 requests have completed the full parallel flow and at least 3 of the 5 request types are represented.

---

## 3. Pilot duration and phases

Total duration: 3 weeks (1 week ramp-up + 2 weeks full pilot + evaluation overlaps final days)

### Phase 1 — Ramp-up (Days 1-5)

| Activity | Owner | Detail |
|---|---|---|
| Enable parallel routing for pilot departments only | Michael Bolton | Feature flag scoped to pilot department codes |
| Brief pilot approvers | Kane | 30-minute walkthrough of what changes (simultaneous notifications, merge behavior, cancellation on rejection) |
| Brief pilot requestors | Kane + dept heads | 15-minute notice: "Your requests now go to budget and compliance at the same time. You do nothing different." |
| Confirm monitoring dashboards are live | Michael Bolton | Routing event log, fork/merge timestamps, error queue |
| Shadow run: first 3 requests monitored in real time | Kane + Michael Bolton | Watch first requests fork and merge; intervene manually if routing fails |
| Bobs confirm audit trail capture is active | The Bobs | Verify parallel events appear in the audit log with correct timestamps and sequencing |

**Ramp-up exit criteria:** 3 requests successfully processed through parallel flow with no routing errors. Bobs confirm audit entries are recording. Approvers confirm they received simultaneous notifications.

### Phase 2 — Full pilot (Days 6-15)

| Activity | Owner | Frequency |
|---|---|---|
| All pilot department requests route through parallel flow | Automatic | Continuous |
| Daily monitoring check | Kane + Michael Bolton | Every morning, 15-minute standup |
| Cycle time tracking | Kane | Logged per request at completion |
| Approver pulse check | Kane | Brief async check-in on Day 8 and Day 12 |
| Audit spot checks | The Bobs | 2 spot checks: Day 8 and Day 13 |
| Issue log review | Kane | Daily; any P1 triggers immediate escalation |

### Phase 3 — Evaluation (Days 14-17, overlaps end of Phase 2)

| Activity | Owner | Detail |
|---|---|---|
| Compile cycle time data | Kane | Compare pilot parallel times vs. sequential baseline |
| Collect approver feedback (structured) | Kane | Survey + 15-minute 1:1 with Samir and Milton |
| Final audit trail review | The Bobs | Full review of all pilot request audit records |
| Draft go/no-go recommendation | Kane | Present findings and recommendation to stakeholders |
| Go/no-go decision meeting | Kane + The Bobs + Michael Bolton + dept heads | 30-minute meeting; decision recorded |

---

## 4. Baseline measurement methodology

### What we measure

Sequential cycle time is the current baseline. All measurements use business hours only (exclude weekends and company holidays, consistent with the timeout logic in #35).

| Metric | Definition | Data source |
|---|---|---|
| Total cycle time | Time from Tier 1 approval to final merge (Complete status) | Platform timestamps |
| Tier 2 cycle time | Time from Tier 2 task assignment to Tier 2 decision | Platform timestamps |
| Tier 3 cycle time | Time from Tier 3 task assignment to Tier 3 decision | Platform timestamps |
| Wait time (sequential) | Time Tier 3 waited for Tier 2 to finish before starting (sequential mode only) | Platform timestamps |
| Fork-to-merge time | Time from parallel fork to merge decision (parallel mode only) | Platform timestamps |

### Baseline data collection

Pull sequential cycle time data for the 30 calendar days preceding the pilot start date:

- Filter to the same 2 pilot departments (apples-to-apples comparison)
- Filter to the same 5 request types
- Exclude outliers caused by known system outages or approver PTO longer than 5 days
- Record median, mean, P75, and P90 cycle times for each metric

### Pilot measurement

Same metrics, captured in real time for each pilot request. Comparison will use:

- **Primary metric:** Median total cycle time (pilot parallel vs. baseline sequential)
- **Secondary metric:** P90 total cycle time (catches worst-case scenarios)
- **Directional metric:** Fork-to-merge time vs. sum of sequential Tier 2 + Tier 3 times

### Expected improvement

Parallel routing should reduce the wait time between Tier 2 and Tier 3 to near zero. If Tier 2 takes 4 hours and Tier 3 takes 6 hours sequentially (10 hours total), parallel should complete in approximately 6 hours (the longer of the two). The pilot validates whether this theoretical improvement holds with real approver behavior.

---

## 5. Success criteria and tracked metrics

### Primary success criteria

| Criterion | Threshold | Measured by |
|---|---|---|
| Cycle time reduction | Median parallel total cycle time is at least 20% faster than median sequential baseline | Kane, from platform timestamps |
| Zero routing errors | No requests routed incorrectly (wrong approver, skipped tier, lost in merge) | Michael Bolton, from error logs |
| Audit trail integrity | 100% of pilot requests have complete, correctly sequenced audit records | The Bobs, from audit review |
| Approver satisfaction | No approver reports a blocking usability issue | Kane, from feedback survey |
| Merge logic correctness | All rejection and rework scenarios produce correct outcomes per #35 spec | Kane, from case review |

### Tracked metrics (daily)

| Metric | Target | Alert if |
|---|---|---|
| Requests in parallel queue | Informational | Queue depth > 10 (suggests bottleneck) |
| Fork-to-merge time | < sequential Tier 2 + Tier 3 combined | Any request takes longer in parallel than sequential average |
| Timeout escalations triggered | 0-1 during pilot | More than 2 in a single week |
| Requests requiring manual intervention | 0 | Any request requires Michael Bolton to manually fix routing |
| Cancellation-on-rejection events | Informational | Cancelled task not marked correctly in audit trail |

---

## 6. Daily monitoring and issue escalation

### Daily standup (15 minutes, every business day during Phase 2)

**Attendees:** Kane, Michael Bolton
**Optional:** Samir, The Bobs (join if issues flagged)

**Standing agenda:**

1. Requests processed in last 24 hours (count, outcomes)
2. Any routing errors or platform warnings (Michael Bolton)
3. Any approver-reported issues (Kane)
4. Cycle time spot check: are parallel times tracking faster than baseline?
5. Open issues from the issue log (status update, owner, resolution ETA)

### Issue severity and escalation

| Severity | Definition | Response time | Escalation path |
|---|---|---|---|
| P1 — Critical | Request lost, routed to wrong person, audit trail missing entries, or data integrity issue | Immediate (within 1 hour) | Kane → Michael Bolton → rollback decision within 2 hours |
| P2 — High | Merge logic produces unexpected outcome, timeout not firing correctly, delegation fails in parallel mode | Same business day | Kane → Michael Bolton; fix before next business day |
| P3 — Medium | Notification timing is off, UI displays sequential status labels instead of parallel, cosmetic audit trail issues | Within 2 business days | Michael Bolton; tracked in issue log |
| P4 — Low | Minor UX suggestions from approvers, wording improvements | After pilot; added to backlog | Kane captures in feedback log |

### Issue log format

| Field | Description |
|---|---|
| Issue ID | Sequential (PILOT-001, PILOT-002, ...) |
| Date reported | Date and time |
| Reporter | Who found it |
| Severity | P1, P2, P3, or P4 |
| Description | What happened, what was expected |
| Request ID | Platform request number affected |
| Root cause | Filled in after investigation |
| Resolution | What was done to fix it |
| Status | Open, In Progress, Resolved, Deferred |

---

## 7. Approver feedback collection

### Structured feedback (collected at Day 8 pulse check and end of pilot)

**Survey questions for Samir (Tier 2) and Milton/compliance (Tier 3):**

1. When you received a parallel notification, was it clear that another tier was also reviewing the same request?
2. Did you ever start a review and then have it cancelled because the other path rejected? If so, was the cancellation notification clear?
3. Did the parallel flow change how you prioritize your review queue?
4. Did you experience any situations where you wanted to see the other tier's decision before making yours? (Tests whether tiers are truly independent)
5. Did you notice any difference in the information available to you compared to sequential routing?
6. On a scale of 1-5, how would you rate the parallel experience compared to sequential? (1 = much worse, 5 = much better)
7. What would you change about the parallel flow before it goes to all departments?

**Survey questions for Tier 1 (department heads):**

1. After you approve a request, did you notice any difference in what happens next?
2. Did you receive any unexpected notifications related to the parallel fork?
3. Any concerns about requests being processed by budget and compliance at the same time?

**Survey questions for requestors (brief, 3 questions max):**

1. Did you notice anything different about how your request was processed?
2. Did your request seem to complete faster, slower, or about the same as before?
3. Any issues or confusion?

### Feedback timeline

| Touchpoint | Timing | Format | Owner |
|---|---|---|---|
| Pulse check 1 | Day 8 | Async message (Slack/Teams/email) with 3 quick questions | Kane |
| Pulse check 2 | Day 12 | Async message | Kane |
| End-of-pilot survey | Day 15 | Full survey (questions above) | Kane |
| 1:1 debrief with Samir | Day 16 | 15-minute call | Kane |
| 1:1 debrief with Milton or compliance lead | Day 16 | 15-minute call | Kane |

---

## 8. Audit trail validation checklist (for The Bobs)

The Bobs validate that audit trail integrity is maintained when approvals run in parallel. This checklist applies to every pilot request reviewed.

### Per-request audit checks

| # | Check | Expected result | Pass/Fail |
|---|---|---|---|
| 1 | Request submission event recorded | Timestamp, requestor ID, request type, dollar amount | |
| 2 | Tier 1 approval event recorded | Timestamp, approver ID, decision (approved/rejected), comments | |
| 3 | Fork event recorded | Timestamp, fork type (AND-split), list of parallel paths dispatched | |
| 4 | Tier 2 task assignment recorded | Timestamp, assigned approver ID (Samir or delegate) | |
| 5 | Tier 3 task assignment recorded | Timestamp, assigned approver ID (Milton or compliance backup) | |
| 6 | Tier 2 and Tier 3 assignment timestamps are within 1 minute of fork event | Confirms simultaneous dispatch, not sequential | |
| 7 | Tier 2 decision event recorded | Timestamp, approver ID, decision, comments | |
| 8 | Tier 3 decision event recorded | Timestamp, approver ID, decision, comments | |
| 9 | Merge event recorded | Timestamp, merge type (AND-join), aggregated outcome | |
| 10 | If rejection occurred: cancellation event on the other path recorded | Timestamp, cancelled task ID, reason ("parallel path rejected") | |
| 11 | If rework requested: pause events on both paths recorded | Timestamps for both pauses; resubmission creates new fork event | |
| 12 | Final request status matches merge outcome | Approved, Rejected, or Returned for Rework | |
| 13 | Segregation of duties: Tier 2 and Tier 3 approver IDs are different people | No single person approved on both parallel paths | |
| 14 | All timestamps are in chronological order (no future-dated or backdated entries) | Sequence is logical | |
| 15 | Audit record is immutable (no edits or deletions to approval events) | Compare raw log to displayed audit trail | |

### Aggregate audit checks (end of pilot)

| # | Check | Expected result |
|---|---|---|
| A1 | Total pilot requests matches total audit records | No missing records |
| A2 | Every fork has a corresponding merge (or documented exception) | No orphaned forks |
| A3 | No approver ID appears on both Tier 2 and Tier 3 for any single request | Segregation of duties enforced |
| A4 | Timeout escalation events recorded correctly for any requests that hit the 3-day or 5-day threshold | Escalation audit trail is complete |

---

## 9. Go/no-go decision criteria

### Decision meeting

- **When:** Day 17 (or first business day after evaluation phase completes)
- **Attendees:** Kane, The Bobs, Michael Bolton, pilot department heads, Samir
- **Decision:** Full rollout, extend pilot, or revert to sequential

### Go criteria (all must be met)

| # | Criterion | Evidence required |
|---|---|---|
| G1 | Median cycle time improved by at least 20% over sequential baseline | Kane's cycle time analysis |
| G2 | Zero P1 or unresolved P2 issues | Issue log with all P1/P2 items resolved |
| G3 | The Bobs confirm audit trail integrity for 100% of pilot requests | Bobs' signed checklist (Section 8) |
| G4 | No approver reports a blocking usability concern | Feedback survey results |
| G5 | Merge logic handled all scenarios correctly (approval, rejection, rework, timeout) | Kane's case-by-case review |
| G6 | Platform stability: no unplanned outages caused by parallel routing | Michael Bolton's system logs |
| G7 | Segregation of duties maintained on every request | Audit check A3 |

### No-go triggers (any one is sufficient to block)

| # | Trigger | Consequence |
|---|---|---|
| N1 | Any request lost or routed to the wrong approver | Revert to sequential; root cause analysis required |
| N2 | Audit trail gaps on more than 1 request | Revert to sequential until logging is fixed |
| N3 | Cycle time is not faster than sequential | Investigate root cause; may extend pilot with adjustments |
| N4 | Approver reports they need to see the other tier's decision before deciding (tiers are not independent) | Revisit parallel routing design; may need conditional logic |
| N5 | Platform instability caused by parallel routing (crashes, deadlocks, queue corruption) | Revert immediately; engineering investigation required |

### Extend-pilot criteria

If results are promising but not conclusive (e.g., only 12 requests completed, or cycle time improvement is 15% instead of 20%), the decision meeting may elect to extend the pilot by 1 week with the same monitoring and feedback cadence.

---

## 10. Issue resolution process

### During the pilot

```
Issue detected
  │
  ▼
Kane logs in issue tracker (PILOT-XXX)
  │
  ▼
Severity assigned (P1-P4)
  │
  ├── P1/P2 → Michael Bolton notified immediately
  │            Kane and Michael Bolton triage within 1 hour (P1) or same day (P2)
  │            Fix deployed or workaround applied
  │            Affected request manually corrected if needed
  │            Root cause documented in issue log
  │
  ├── P3 → Michael Bolton picks up within 2 business days
  │         Fix deployed during pilot if straightforward
  │         Otherwise deferred to post-pilot backlog
  │
  └── P4 → Captured in feedback log
            Addressed in post-pilot improvements
```

### Post-pilot, pre-rollout

All resolved P1 and P2 issues are reviewed in the go/no-go meeting. The fix for each issue is validated (not just deployed, but confirmed working). Any P3 items that affect audit trail or routing accuracy are promoted to P2 and resolved before full rollout.

---

## 11. Rollback triggers and procedure

### When to rollback during pilot

| Trigger | Action | Who decides |
|---|---|---|
| Any P1 issue that cannot be resolved within 2 hours | Disable parallel routing for pilot departments; revert to sequential | Kane + Michael Bolton (immediate decision, no meeting required) |
| 2 or more P2 issues open simultaneously | Pause new requests from entering parallel flow; existing requests complete in parallel | Kane (consult Michael Bolton) |
| Approver reports data integrity concern (wrong data shown, missing fields) | Disable parallel routing immediately | Kane + Michael Bolton |
| The Bobs flag audit trail gaps on 2+ requests | Disable parallel routing; full audit review before resuming | Kane + The Bobs |
| Platform instability (queue corruption, deadlocks, crashes) | Emergency disable via circuit breaker (#35 Section 8) | Michael Bolton (immediate, no approval needed) |

### Rollback procedure

1. Michael Bolton disables the parallel routing feature flag for pilot departments
2. Requests currently in the parallel fork continue to completion (do not yank in-flight requests)
3. New requests from pilot departments revert to sequential routing (Tier 1 → Tier 2 → Tier 3)
4. Kane notifies pilot approvers and department heads: "We've paused the pilot to investigate an issue. Your requests will route sequentially until further notice."
5. Michael Bolton captures system state (logs, queue contents, error traces) before any cleanup
6. Kane documents the rollback in the issue log with timestamp, trigger, and affected requests
7. The Bobs review audit trail for all in-flight requests that were in parallel at the time of rollback
8. Resume decision requires Kane + Michael Bolton + The Bobs agreement

### What rollback does NOT do

- Does not cancel or reject any in-flight requests
- Does not delete any audit trail entries
- Does not change approver assignments or delegation rules
- Does not affect departments outside the pilot group

---

## Appendix A — Pilot timeline summary

```
Week 1 (Days 1-5): Ramp-up
├── Day 1: Feature flag enabled, approver briefing
├── Day 2-3: Shadow monitoring of first 3 requests
├── Day 4-5: Ramp-up exit criteria validated
│
Week 2 (Days 6-10): Full pilot — first half
├── Daily standups (Kane + Michael Bolton)
├── Day 8: Pulse check 1 (approver feedback)
├── Day 8: Bobs spot check 1
│
Week 3 (Days 11-17): Full pilot — second half + evaluation
├── Day 12: Pulse check 2
├── Day 13: Bobs spot check 2
├── Day 15: End-of-pilot survey distributed
├── Day 16: 1:1 debriefs with Samir and Milton
├── Day 17: Go/no-go decision meeting
```

## Appendix B — RACI for pilot activities

| Activity | Kane | Michael Bolton | Samir | Milton | The Bobs | Dept Heads |
|---|---|---|---|---|---|---|
| Pilot planning | A/R | C | I | I | C | I |
| Feature flag configuration | I | A/R | — | — | — | — |
| Approver briefing | A/R | C | I | I | — | I |
| Daily monitoring | A/R | R | — | — | — | — |
| Cycle time measurement | A/R | C | — | — | — | — |
| Audit trail validation | C | I | — | — | A/R | — |
| Feedback collection | A/R | — | R | R | — | R |
| Go/no-go decision | R | R | C | C | R | C |
| Rollback execution | R | A/R | I | I | C | I |

**A** = Accountable, **R** = Responsible, **C** = Consulted, **I** = Informed
