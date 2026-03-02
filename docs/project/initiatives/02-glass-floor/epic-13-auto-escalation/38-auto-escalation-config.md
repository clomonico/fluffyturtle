# Auto-Escalation Configuration Specification

**Issue:** #38
**Initiative:** Glass Floor (Init 2)
**Epic:** 13 — Auto-Escalation
**Author:** Lambert (Implementation Planner)
**Date:** 2026-03-02
**Status:** Draft — pending stakeholder review
**Dependencies:** #37 (SLA Thresholds), #34 (Feasibility Spike from Epic 12)

---

## Purpose

Configure the platform's built-in auto-escalation engine so that overdue approvals escalate automatically, with no human intervention required to trigger them. The platform already supports auto-escalation; it has never been turned on. This spec translates Dallas's SLA thresholds (#37) into concrete configuration rules, notification templates, visual indicators, and audit log entries.

**Problem this solves:** Joanna's vendor contract sat idle for 11 days because nobody knew it was stuck. Peter follows up manually via email. No escalation mechanism fires today, even though the platform supports it. After this configuration, idle requests escalate automatically at 75%, 100%, and 150% of their SLA window.

---

## Escalation Rule Definitions

### Escalation Levels

Every approval step uses three escalation thresholds, expressed as a percentage of the SLA target for that tier and complexity combination.

| Level | Trigger | Action Type | Description |
|-------|---------|-------------|-------------|
| **Warning** | 75% of SLA consumed | Reminder | Nudge the assigned approver that time is running short |
| **First Escalation** | 100% of SLA consumed | Manager notification | Approver's direct manager gains visibility; original approver retains the task |
| **Executive Escalation** | 150% of SLA consumed | Lumbergh override | Lumbergh can reassign the approval or approve directly within his delegation |

### Tier 1 — Requestor Verification

Tier 1 is mostly system-driven (intake validation from Epic 9). Escalation targets the requestor, not an approver.

| Complexity | SLA Target | 75% Warning | 100% Escalation | 150% Executive | 200% Auto-Cancel |
|------------|-----------|-------------|------------------|----------------|-------------------|
| Standard | 4 hours | 3 hours | 4 hours | 6 hours | 8 hours |
| Elevated | 8 hours | 6 hours | 8 hours | 12 hours | 16 hours |
| Complex | 8 hours | 6 hours | 8 hours | 12 hours | 16 hours |

**Tier 1 escalation chain:**
1. **75%:** Requestor receives an in-platform nudge and email reminding them to complete their submission.
2. **100%:** Requestor's manager is notified that a submission is incomplete.
3. **150%:** Not applicable (no executive override for requestor verification).
4. **200%:** Request is auto-cancelled with a "resubmit when ready" message. The requestor can resubmit at any time.

### Tier 2 — Budget Authority Confirmation

| Complexity | SLA Target | 75% Warning | 100% Escalation | 150% Executive |
|------------|-----------|-------------|------------------|----------------|
| Standard | 24 hours | 18 hours | 24 hours | 36 hours |
| Elevated | 48 hours | 36 hours | 48 hours | 72 hours |
| Complex | 24 hours | 18 hours | 24 hours | 36 hours |

**Tier 2 escalation chain:**
1. **75%:** Budget holder receives in-platform notification + email reminder.
2. **100%:** Budget holder's manager is notified. Original budget holder retains the task.
3. **150%:** Lumbergh is notified via executive alert (#30). Lumbergh can reassign to another qualified budget authority or approve directly if within his delegation.

### Tier 3 — Compliance / COI Check

| Complexity | SLA Target | 75% Warning | 100% Escalation | 150% Executive |
|------------|-----------|-------------|------------------|----------------|
| Standard | 24 hours | 18 hours | 24 hours | 36 hours |
| Elevated | 48 hours | 36 hours | 48 hours | 72 hours |
| Complex | 72 hours | 54 hours | 72 hours | 108 hours |

**Tier 3 escalation chain:**
1. **75%:** Samir receives in-platform notification + email reminder.
2. **100%:** Compliance team lead is notified. Samir retains the task.
3. **150%:** Lumbergh is notified via executive alert. Lumbergh can reassign to another compliance-qualified reviewer or approve directly if within his delegation.

---

## Notification Design

### Delivery Channels

All escalation notifications use two channels simultaneously:

| Channel | When Used | Latency |
|---------|-----------|---------|
| In-platform notification | Always (primary) | Immediate |
| Email | Always (backup) | Within 2 minutes |

Email is the backup channel because approvers do not always have the platform open. Both channels fire for every escalation event. No SMS or chat integrations in scope for Init 2.

### Notification Recipients by Level

| Level | Recipients | Purpose |
|-------|-----------|---------|
| 75% Warning | Assigned approver only | Gentle nudge, no visibility escalation |
| 100% First Escalation | Assigned approver + their direct manager | Manager gains visibility, approver retains ownership |
| 150% Executive Escalation | Assigned approver + their manager + Lumbergh | Executive override authority activated |

### Notification Content Templates

#### 75% Warning — Approver Nudge

> **Subject:** Action needed: [Request Type] #[ID] approaching SLA deadline
>
> **Body:**
> Request #[ID] ([Request Type], [Complexity]) has been in your queue for [elapsed time].
> The SLA target for this step is [SLA target]. You have [remaining time] to complete your review.
>
> **[Open request in platform →]**
>
> If you need more time, you can request a one-time SLA extension through the platform.
> If you need to delegate, reassign the task before the deadline.

#### 100% First Escalation — Manager Notification

> **Subject:** Escalation: [Request Type] #[ID] has exceeded SLA at [Tier Name]
>
> **Body:**
> Request #[ID] ([Request Type], [Complexity]) has exceeded its [SLA target] SLA at [Tier Name].
> The assigned approver is [Approver Name]. The request has been in queue for [elapsed time].
>
> **What this means:** The approval step is overdue. The original approver retains the task, but you now have visibility. If the approver is unable to act, please coordinate directly.
>
> **[View request details →]**
>
> This escalation is logged in the decision trail for audit purposes.

#### 150% Executive Escalation — Lumbergh Alert

> **Subject:** Executive escalation: [Request Type] #[ID] at 150% SLA
>
> **Body:**
> Request #[ID] ([Request Type], [Complexity]) has reached 150% of its SLA at [Tier Name].
> Assigned approver: [Approver Name]. Approver's manager: [Manager Name]. Elapsed time: [elapsed time].
>
> **Available actions:**
> - **Reassign** to another qualified approver
> - **Approve directly** (if within your delegation authority)
> - **Request additional time** with documented rationale
>
> **[Take action on request →]**
>
> This escalation is logged in the decision trail for audit purposes.

---

## Visual Indicators for Escalated Items

Approver queues display escalation status through color coding and badge overlays. These align with the requestor-facing SLA indicators defined in #37.

### Queue Display Elements

| SLA Status | Color | Badge | Queue Sort Priority |
|------------|-------|-------|---------------------|
| Under 75% | Green | None | Normal (by submission date) |
| 75%–99% | Amber | ⚠ Warning | Elevated — sorted above green items |
| 100%–149% | Red | 🔴 Escalated | High — sorted to top |
| 150%+ | Red (pulsing) | 🔴 Executive | Highest — pinned to top with distinct styling |

### Additional Visual Cues

- **Escalated items show the escalation chain** in an expandable detail panel: who was notified, when, and whether any action was taken.
- **Manager view:** Managers see escalated items from their direct reports in a dedicated "Escalated to You" section of their queue-visible only when they have active escalations.
- **Lumbergh's dashboard:** Executive-escalated items appear in the Epic 10 dashboard with a dedicated "Overdue Approvals" widget, consistent with the executive alerts spec (#30).

---

## Decision Trail Integration

Every escalation event writes a record to the decision log (Story #32) for audit. This creates an unbroken chain of accountability.

### Logged Events

| Event | Fields Recorded |
|-------|----------------|
| 75% Warning sent | Timestamp, request ID, tier, approver, SLA target, elapsed time |
| 100% Escalation triggered | Timestamp, request ID, tier, approver, manager notified, SLA target, elapsed time |
| 150% Executive escalation triggered | Timestamp, request ID, tier, approver, manager, Lumbergh notified, SLA target, elapsed time |
| Escalation resolved (approver acts) | Timestamp, request ID, tier, who acted, resolution type (approved/rejected/reassigned), time from escalation to resolution |
| Reassignment via escalation | Timestamp, request ID, tier, original approver, new approver, who authorized reassignment |
| SLA extension requested | Timestamp, request ID, tier, approver, extension duration, reason |
| SLA extension applied | Timestamp, request ID, tier, new SLA deadline, original deadline |

### Audit Query Support

The decision log supports these reporting queries:

- "Show all escalation events for request #X" — full escalation history for a single request
- "Which approvers triggered the most escalations in the past 30 days?" — identifies patterns
- "Average time from escalation to resolution by tier" — measures escalation effectiveness
- "How many requests reached executive escalation last quarter?" — Lumbergh workload indicator
- "Escalation events during system outages" — validates clock-pause behavior

---

## Configuration Steps for Michael Bolton

Michael is the sole platform administrator. These steps configure escalation rules in the platform's admin console.

### Pre-Configuration Checklist

- [ ] Feasibility spike (#34) confirms auto-escalation feature works as documented
- [ ] SLA thresholds (#37) are finalized and approved by stakeholders
- [ ] Approver org chart is current in the platform (manager relationships for escalation targets)
- [ ] Lumbergh's account has executive override permissions enabled
- [ ] Decision log integration (#32) is active so escalation events can be recorded
- [ ] Test environment is available (not production)

### Configuration Sequence

Perform all configuration in the test environment first, then promote to production after testing.

**Step 1: Enable the escalation engine**
- Admin Console → Workflow Settings → Escalation → Enable Auto-Escalation
- Set global escalation mode to "Active"
- Set clock basis to "Business hours" (Mon–Fri, 8am–6pm local)

**Step 2: Configure SLA targets per tier and complexity**
- Admin Console → Approval Steps → [Select Tier] → SLA Configuration
- Enter the SLA target hours from #37 for each complexity band (Standard, Elevated, Complex)
- Verify that the clock-pause rules from #37 are configured: requestor action needed, clarification requested, system outage

**Step 3: Configure escalation thresholds**
- For each tier's SLA configuration, add three escalation rules:
  - Rule 1: At 75% of SLA → Send notification to assigned approver (Warning level)
  - Rule 2: At 100% of SLA → Send notification to assigned approver + direct manager (Escalation level)
  - Rule 3: At 150% of SLA → Send notification to assigned approver + manager + Lumbergh (Executive level)
- For Tier 1 only: Add Rule 4: At 200% of SLA → Auto-cancel with resubmission option

**Step 4: Configure notification templates**
- Admin Console → Notifications → Escalation Templates
- Create three templates using the content from the Notification Design section above
- Map each template to the corresponding escalation rule
- Enable both in-platform and email delivery for all templates

**Step 5: Configure escalation targets**
- Admin Console → Escalation Routing
- Tier 2: First escalation → budget holder's direct manager; Executive → Lumbergh
- Tier 3: First escalation → compliance team lead; Executive → Lumbergh
- Verify the org chart lookups resolve correctly for each approver

**Step 6: Configure visual indicators**
- Admin Console → Queue Display → Escalation Indicators
- Enable color-coded SLA status (green/amber/red)
- Enable escalation badges
- Enable priority sorting (escalated items sorted to top)
- Enable the "Escalated to You" manager view

**Step 7: Enable decision log integration**
- Admin Console → Audit → Escalation Logging
- Enable "Log all escalation events to decision trail"
- Map escalation event types to decision log entry fields (see Decision Trail Integration above)
- Verify log entries appear in the decision log query interface

**Step 8: Configure parallel routing interaction**
- Admin Console → Workflow Settings → Parallel Routing → Escalation Behavior
- Set to "Independent" — each parallel branch escalates based on its own SLA clock
- Verify that an escalation in Tier 2 does not affect Tier 3's SLA clock or status, and vice versa

### Estimated Configuration Time

| Step | Estimated Effort |
|------|-----------------|
| Steps 1–3 (engine + SLA + thresholds) | 4 hours |
| Step 4 (notification templates) | 2 hours |
| Steps 5–6 (routing + visual) | 2 hours |
| Steps 7–8 (audit + parallel) | 2 hours |
| **Total configuration** | **10 hours** |

This does not include testing time, which is estimated separately below.

---

## Parallel Routing Considerations

Tier 2 and Tier 3 run in parallel after Tier 1 completes (Epic 12). Auto-escalation must respect branch independence.

### Rules

1. **Independent SLA clocks.** Each branch starts its clock when Tier 1 completes. They are independent timers.
2. **Independent escalation.** An escalation in one branch has no effect on the other branch's SLA clock, escalation status, or notifications.
3. **No cross-branch notification.** If Tier 3 escalates, the Tier 2 approver is not notified about it (and vice versa). Each branch's escalation chain is self-contained.
4. **Both branches must complete.** The request moves to "Complete" only after both branches render a decision. If one branch is escalated and the other is already done, the completed branch simply waits.
5. **Rejection in one branch.** If Tier 2 rejects while Tier 3 is still in progress, Tier 3 is notified but continues its review (the compliance record still matters per #37). The request is marked rejected, but Tier 3's SLA clock and escalation rules remain active until Tier 3 also reaches a decision.
6. **End-to-end SLA.** The end-to-end SLA is measured from Tier 1 submission to both parallel branches completing. Escalation for individual branches uses that branch's SLA, not the end-to-end target.

### Scenario: One Branch Fast, One Branch Slow

- Request enters Tier 2 and Tier 3 simultaneously at 9:00 AM Monday.
- Tier 2 (budget) approves at 2:00 PM Monday (5 hours — well within 24-hour SLA).
- Tier 3 (compliance) for a complex new vendor has a 72-hour SLA.
- Tier 3's 75% warning fires at 54 hours (Wednesday 3:00 PM).
- Tier 3's 100% escalation fires at 72 hours (Thursday 9:00 AM).
- The request remains in "pending" until Tier 3 decides, even though Tier 2 finished days earlier.
- The requestor sees "Tier 2: Approved ✓ | Tier 3: In Review (SLA: Amber)" in their status view.

---

## Edge Cases

### Approver on Leave

| Scenario | System Behavior |
|----------|----------------|
| Approver designated a delegate before leave | Request auto-routes to delegate. SLA clock does not pause. Escalation targets the delegate (not the absent approver). |
| Approver marked out-of-office, no delegate set | System auto-assigns to the next person in the role (budget holder backup for Tier 2; compliance team member for Tier 3). SLA clock does not pause. |
| Approver disappeared (no OOO, no delegate, not responding) | Normal escalation fires: 75% warning, 100% to manager, 150% to Lumbergh. Manager or Lumbergh can reassign. |

### System Outage During Escalation Window

| Scenario | System Behavior |
|----------|----------------|
| Outage occurs before escalation fires | SLA clock pauses for outage duration. Escalation thresholds shift accordingly. Example: if a 24-hour SLA had 18 hours elapsed and a 4-hour outage occurs, the clock resumes at 18 hours with 6 hours remaining. |
| Outage occurs during the exact moment escalation should fire | Escalation fires immediately when the system comes back online. The log records the original scheduled time and the actual fire time. |
| Outage occurs after escalation but before the approver acts on it | The approver's response time clock pauses during the outage. The escalation record notes the outage. |

### SLA Extension Interaction

- An approved SLA extension shifts all three escalation thresholds proportionally. Example: a 24-hour SLA extended by 12 hours becomes a 36-hour SLA. The 75% warning moves from 18 hours to 27 hours, the 100% escalation moves from 24 to 36 hours, and the 150% executive moves from 36 to 54 hours.
- Extensions are limited to 1x the original SLA per #37.
- Extension requests are logged in the decision trail.

### Rapid Successive Escalations

- If an approver does not act and the request moves from 100% to 150% quickly (no action at first escalation), both the 100% and 150% notifications will have fired. The system does not suppress the 150% notification just because the 100% was recently sent.
- Each notification is a distinct event with its own decision log entry.

### Request Reclassification Mid-Escalation

- If an approver reclassifies a request (e.g., Standard to Complex) while an escalation is active, the SLA clock resets to the new complexity's threshold from the point of reclassification.
- Any prior escalation notifications remain in the log but are annotated as "superseded by reclassification."
- New escalation thresholds apply from the reclassification timestamp forward.

---

## Testing Plan

All testing takes place in the test environment before production deployment. Michael Bolton executes the test plan; Dallas (Process Designer) validates results against SLA threshold expectations.

### Test Scenarios

| # | Scenario | Setup | Expected Outcome | Pass Criteria |
|---|----------|-------|-------------------|---------------|
| 1 | Standard request, Tier 2, no action taken | Submit standard request. Do not act on Tier 2 approval. | 75% warning at 18h, 100% escalation at 24h, 150% executive at 36h. | All three notifications fire at correct times to correct recipients. |
| 2 | Standard request, Tier 3, approved at 80% | Submit standard request. Approve Tier 3 at 80% mark (~19.2h). | 75% warning fires at 18h. No 100% escalation. | Warning fires; escalation does not fire; decision log shows warning + approval. |
| 3 | Complex request, Tier 3, full escalation chain | Submit complex new-vendor request. Do not act on Tier 3. | 75% at 54h, 100% at 72h, 150% at 108h. | Correct timing for complex SLA; compliance team lead and Lumbergh receive correct notifications. |
| 4 | Parallel routing: Tier 2 fast, Tier 3 slow | Submit elevated request. Approve Tier 2 at 12h. Do not act on Tier 3. | Tier 2 completes without escalation. Tier 3 escalates independently. Request stays pending until both complete. | Tier 2 completion does not affect Tier 3 clock or suppress Tier 3 escalation. |
| 5 | Approver delegates before leave | Set Tier 2 approver on leave with delegate. Submit standard request. | Request routes to delegate. SLA clock does not pause. Escalation targets delegate. | Delegation works; clock uninterrupted; notifications go to delegate. |
| 6 | Approver absent, no delegate | Remove Tier 2 approver's delegate. Mark out-of-office. Submit request. | Auto-assigns to backup budget holder. SLA clock does not pause. | Backup assignment happens; correct escalation chain applies to backup. |
| 7 | System outage mid-SLA | Submit standard request. Simulate 4-hour platform outage at 16h mark. | SLA clock pauses for 4 hours. 75% warning fires at 22h (18h active + 4h pause). | Clock correctly pauses and resumes; escalation timing adjusts. |
| 8 | SLA extension requested | Submit standard request. At 20h, approver requests 12h extension. | New SLA becomes 36h. 75% warning shifts to 27h (from reclassified baseline). | Extension logged in decision trail; escalation thresholds recalculated. |
| 9 | Reclassification during active escalation | Submit standard request for Tier 3. At 20h (past 75% warning), reclassify to Complex. | SLA resets to 72h from reclassification point. Prior warning annotated as superseded. | New escalation thresholds apply; prior events preserved with annotation. |
| 10 | Executive escalation with Lumbergh reassignment | Submit standard request. Let it reach 150% in Tier 2. Lumbergh reassigns. | Lumbergh notification fires. Reassignment succeeds. New approver sees the request. Reassignment logged. | Full chain fires; reassignment works; decision log records all events. |
| 11 | Rejection in one parallel branch | Submit elevated request. Tier 2 rejects at 12h. Tier 3 is at 10h (no escalation yet). | Tier 3 continues. Tier 3 SLA clock and escalation remain active. Request marked rejected but Tier 3 completes review. | Tier 3 is not cancelled; its SLA/escalation operates to completion. |
| 12 | Notification delivery verification | Trigger each notification type. | Both in-platform and email notifications delivered with correct content and recipients. | All templates render correctly; no missing fields; both channels fire. |

### Test Environment Requirements

- Dedicated test tenant or sandbox (not production)
- Ability to simulate time advancement (fast-forward SLA clocks without waiting real-time hours)
- Test accounts for: requestor, budget holder, compliance reviewer, managers, Lumbergh
- Email delivery verification (test mailboxes or mail-trap)
- Decision log query access to validate audit entries

### Test Execution Timeline

| Phase | Duration | Activities |
|-------|----------|------------|
| Environment setup | 1 day | Create test accounts, configure time simulation, verify sandbox isolation |
| Configuration deployment | 1 day | Michael applies all configuration steps in test environment |
| Individual scenario testing | 3 days | Run scenarios 1–12, document results, fix configuration issues |
| Regression testing | 1 day | Re-run all scenarios after any fixes to confirm nothing broke |
| Stakeholder validation | 1 day | Dallas reviews test results against SLA expectations; Samir confirms compliance logging |
| **Total testing** | **7 business days** |

---

## Rollback Procedure

The platform is brittle. If auto-escalation causes unexpected behavior in production, Michael needs a clear, fast path to revert.

### Rollback Steps

1. **Disable the escalation engine** (immediate — under 1 minute)
   - Admin Console → Workflow Settings → Escalation → Set Auto-Escalation to "Disabled"
   - This stops all future escalation events from firing. In-flight requests return to manual-only escalation.
   - Notifications already sent cannot be recalled, but no new ones will fire.

2. **Verify the disable took effect** (under 5 minutes)
   - Check the escalation event log for any events firing after the disable timestamp.
   - Confirm that queued escalation jobs (if any) have been cancelled by the engine.

3. **Communicate the rollback** (within 30 minutes)
   - Notify stakeholders (Lumbergh, Samir, budget holders) that auto-escalation has been temporarily disabled.
   - Notify the requestor-facing team that SLA countdown indicators may show stale state until re-enabled or cleared.

4. **Diagnose the issue** (timeline varies)
   - Review the decision log for the escalation event that caused the problem.
   - Identify whether the issue is: misconfigured threshold, incorrect escalation target, notification template error, clock calculation bug, or platform defect.
   - Document findings.

5. **Fix and re-test in sandbox** (before re-enabling)
   - Apply the fix in the test environment.
   - Re-run the relevant test scenario(s) to confirm the fix.
   - Only re-enable in production after the fix is validated.

### Rollback Decision Authority

- **Michael Bolton** can disable the escalation engine at any time without prior approval if it is causing active harm (incorrect escalations, notification storms, data corruption).
- **Lumbergh** must approve re-enabling after a rollback.
- Any rollback event is logged in the decision trail and reported in the next status update.

---

## Monitoring and Alerting

Once auto-escalation is live in production, the system itself needs monitoring to ensure it continues working correctly.

### Health Checks

| Check | Frequency | Method | Alert Threshold |
|-------|-----------|--------|-----------------|
| Escalation engine status | Every 5 minutes | Platform health API | Engine status != "Active" |
| Escalation queue depth | Every 15 minutes | Admin dashboard metric | Queue depth > 50 pending escalation jobs |
| Notification delivery rate | Hourly | Email delivery logs + platform notification log | Delivery failure rate > 5% in any hour |
| SLA clock accuracy | Daily | Compare calculated SLA times against wall-clock expectations | Drift > 15 minutes on any request |
| Decision log write success | Every 15 minutes | Decision log error count | Any write failure |

### Operational Alerts

| Alert | Recipient | Channel | Severity |
|-------|-----------|---------|----------|
| Escalation engine stopped unexpectedly | Michael Bolton | Email + platform alert | Critical — investigate immediately |
| Notification delivery failures | Michael Bolton | Email | High — check email gateway and platform notification service |
| SLA clock drift detected | Michael Bolton + Dallas | Email | Medium — investigate before next business day |
| Decision log write failures | Michael Bolton | Email + platform alert | High — escalation events not being recorded for audit |
| Unusual escalation volume (>2x daily average) | Michael Bolton + Lumbergh | Email | Medium — may indicate a process bottleneck or misconfiguration |

### Monthly Reporting

Michael provides a monthly escalation health report including:

- Total escalation events by tier and level (75%, 100%, 150%)
- Average time from escalation to resolution
- Approvers with the most escalation events (pattern detection)
- Notification delivery success rate
- Any rollback events or configuration changes
- SLA clock accuracy summary

---

## Implementation Sequence

| Order | Dependency | Activity |
|-------|-----------|----------|
| 1 | #34 (feasibility spike) | Confirm platform escalation features work as documented in sandbox |
| 2 | #37 (SLA thresholds) | Finalize SLA targets — these are the inputs to escalation rules |
| 3 | #32 (decision log) | Confirm decision log integration is ready to receive escalation events |
| 4 | This spec (#38) | Michael configures escalation rules in test environment |
| 5 | Testing plan above | Execute all 12 test scenarios in test environment |
| 6 | Stakeholder sign-off | Dallas and Samir validate test results |
| 7 | Production deployment | Michael promotes configuration to production |
| 8 | Monitoring activation | Enable all health checks and alerts |
| 9 | 2-week observation period | Monitor escalation behavior with heightened alerting before declaring stable |
