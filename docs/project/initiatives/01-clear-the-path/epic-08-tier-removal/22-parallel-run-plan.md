# Parallel-Run Plan — Tier Removal (7-Tier to 3-Tier)

**Date:** 2026-03-02
**Author:** Lambert (Implementation Planner)
**Issue:** #22
**Initiative:** Clear the Path — Strip the approval chain to regulatory minimum
**Companion documents:** [02-implementation-plan.md](../02-implementation-plan.md) (Phase 3), [01-process-design.md](../01-process-design.md)

---

## Executive Summary

This document defines the parallel-run strategy for validating the transition from the current 7-tier approval process to the new 3-tier regulatory-only workflow. During the parallel run, every incoming request is processed through both the old and new paths simultaneously. The purpose is to prove the new process produces equivalent or better outcomes before cutting over permanently.

The parallel run is manual dual-tracking, not technical dual-routing. Michael Bolton flagged the platform as fragile. Running two routing configurations simultaneously in production is riskier than having Peter Gibbons compare outcomes manually using his existing process expertise.

---

## 1. Parallel-Run Approach

### What Runs in Parallel

| Path | Description | How It Operates |
|------|-------------|-----------------|
| **New path (primary)** | 3-tier regulatory flow: Budget Verification → Compliance & Regulatory Review → Financial Controls Sign-off. Intake gate enforces documentation completeness. Lumbergh Dashboard provides visibility. | Live in the system. All new requests route through this path. |
| **Old path (reference)** | 7-tier flow: all original tiers including the 4 elective ones (Departmental Review, Secondary Budget Review, Operational Process Check, Executive Visibility Gate). | Tracked manually by Peter Gibbons in a comparison spreadsheet. Peter evaluates each request against the old routing rules and records what each deactivated tier would have done. |

### Why Manual Dual-Tracking

Michael Bolton described the platform as "fragile and poorly integrated" with hardcoded routing logic, string-match configs, and flat-file audit logs. Running two parallel routing configurations in this environment risks:

- Silent routing failures (the West Region incident where approvals failed for 3 weeks without detection)
- Audit log corruption from two concurrent write paths
- Configuration conflicts between active and shadow routing rules

Manual tracking using Peter's domain knowledge is lower risk and produces richer data. Peter can explain *why* the old path would have diverged, not just *that* it diverged.

---

## 2. Duration and Justification

### Recommended Duration: 2 Weeks (10 Business Days)

| Week | Focus | Rationale |
|------|-------|-----------|
| **Week 1** | Primary validation. Process all incoming requests through both paths. Daily variance checks. Identify any immediate issues with the new 3-tier flow, intake form, or dashboard. | First week catches configuration errors, intake form design problems, and obvious control gaps. Based on historical volume, expect 15-25 requests per week to flow through the process. |
| **Week 2** | Confirmation and edge cases. Continue dual-tracking. Focus on requests that hit edge conditions: high-value items, new vendors, vendor exceptions from Milton's list, cross-department requests. Validate that Week 1 findings (if any) were addressed. | One week is not enough to encounter the full range of request types. The 47 vendor exceptions, high-value thresholds, and cross-department routing scenarios may not all appear in a single week. Two weeks provides a larger sample size and catches patterns that one-time observations miss. |

### Why Not Shorter

A single week (the original Phase 3 duration in the implementation plan) risks an insufficient sample. If historical volume is 15-25 requests per week, one week may produce only 15 data points. At that volume, a single unusual request can skew metrics and an infrequent request type might not appear at all.

### Why Not Longer

Beyond two weeks, the cost of dual-tracking outweighs the marginal data value. Peter spends roughly 40% of his time on the comparison exercise. The Bobs invest 20-30 minutes daily on variance reviews. Extending past two weeks burns resources without proportional risk reduction, and creates change fatigue that could undermine adoption.

### Extension Trigger

If Week 1 produces any compliance-relevant variance, the parallel run automatically extends by one additional week after remediation. Maximum total duration: 4 weeks. If variance persists after 4 weeks, the initiative pauses for root cause analysis.

---

## 3. Success Metrics

Each metric has a specific target. All "Must Pass" metrics must be met before proceeding to cutover.

### Must-Pass Metrics

| # | Metric | Target | Measurement Method | Owner |
|---|--------|--------|--------------------|-------|
| S1 | **Compliance variance** | Zero requests where the old 7-tier path would have caught a compliance issue that the new 3-tier path missed | Peter's comparison spreadsheet cross-referenced with the Bobs' daily variance review | The Bobs |
| S2 | **Audit trail completeness** | 100% of requests processed through the new path have complete verification records at all 3 mandatory tiers (who checked, what, when) | Spot-check by the Bobs on every request during parallel run | The Bobs |
| S3 | **Documentation completeness at intake** | ≥ 90% of submissions pass the intake gate on first attempt (compared to 40% baseline under the old process) | Intake exception log: count of returned vs. accepted submissions | Joanna, Peter Gibbons |
| S4 | **Configuration stability** | Zero platform incidents (silent routing failures, audit log gaps, notification failures) caused by the new configuration | Michael Bolton's system monitoring and incident log | Michael Bolton |

### Should-Pass Metrics

| # | Metric | Target | Measurement Method | Owner |
|---|--------|--------|--------------------|-------|
| S5 | **Cycle time reduction** | ≥ 30% average reduction in end-to-end approval time compared to historical average under the old process | Compare new-path timestamps (submission to final approval) against Peter's estimated old-path duration for each request | Peter Gibbons |
| S6 | **Intake form usability** | ≤ 3 requests per week blocked by form design errors (as opposed to genuinely incomplete submissions) | Intake exception log: categorize each rejection as "legitimate" or "form design issue" | Joanna |
| S7 | **Dashboard accuracy** | Lumbergh confirms dashboard data matches or exceeds the information he previously received through his "loop me in" gate | Lumbergh's daily dashboard accuracy check | Lumbergh |
| S8 | **Stakeholder satisfaction** | No formal escalation or complaint from any approver or requestor about the new process that cannot be resolved within the parallel run period | Feedback collected by Lambert during daily standups | Lambert |

---

## 4. Failure Criteria — Rollback Triggers

Any single item below triggers an immediate rollback discussion. Items marked **automatic** halt the parallel run without waiting for the Go/No-Go meeting.

| # | Failure Condition | Severity | Response |
|---|-------------------|----------|----------|
| F1 | A compliance-relevant request is approved under the new process that would have been flagged or rejected under the old process | **Automatic rollback** | Re-activate all 4 deactivated tiers. Conduct root cause analysis. Remediate and restart parallel run. |
| F2 | More than 3 requests per day are blocked by intake validation due to form design errors (not genuinely incomplete submissions) | **Automatic pause** | Freeze intake form changes. Fix form rules in sandbox. Resume parallel run once fixes are validated. Timeline extends. |
| F3 | Platform instability: silent routing failure, audit log corruption, or notification breakdown caused by the new configuration | **Automatic rollback** | Revert to pre-change configuration backup. Michael Bolton investigates. No resumption until root cause is resolved. |
| F4 | Lumbergh's dashboard fails to surface a request that his previous informal gate would have caught | **Risk discussion** | Evaluate whether the gap is a dashboard design issue (fixable) or a structural problem. Adjust dashboard and continue if fixable. |
| F5 | Any approver at a mandatory tier reports they cannot perform their review because the new process removed information they need | **Risk discussion** | Assess whether the missing information came from an elective tier. If yes, evaluate whether it should be added to the intake gate or a mandatory tier's checklist. |
| F6 | Cycle time does not improve (new path is equal to or slower than old path) | **Risk discussion** | Does not block cutover, but triggers investigation into bottlenecks. May indicate SLA enforcement or workload issues. |
| F7 | Peter identifies more than 5 requests where his shadow workflow handled a routing scenario the new process cannot | **Risk discussion** | Document the gap. Evaluate whether it requires a process design change before cutover or can be addressed post-cutover in Initiative 4. |

### Rollback Procedure

1. Re-activate the 4 deactivated tiers in production (set from "Inactive" back to "Active")
2. Revert intake form to pre-change version from configuration backup
3. Restore original routing rules from backup
4. Keep the Lumbergh Dashboard live (it adds value regardless of process structure)
5. Notify all stakeholders within 2 hours of rollback decision
6. Conduct root cause analysis within 48 hours

**Rollback time estimate:** Under 4 hours, assuming Michael Bolton's configuration backups are in place. The dashboard stays.

---

## 5. Roles and Responsibilities — RACI Matrix

### Parallel-Run RACI

| Activity | Peter Gibbons | The Bobs | Michael Bolton | Lumbergh | Joanna | Tom Smykowski | Milton Waddams | Lambert |
|----------|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Dual-track requests (old vs. new comparison) | **R** | C | I | I | I | I | I | A |
| Daily variance review | C | **R** | I | I | I | I | I | A |
| System monitoring and incident response | I | I | **R** | I | I | I | I | A |
| Dashboard accuracy checks | I | I | I | **R** | I | I | I | A |
| Intake form feedback and testing | I | I | I | I | **R** | I | I | A |
| Vendor exception scenario tracking | C | C | I | I | I | I | **R** | A |
| Elective tier risk documentation | I | C | I | I | I | **R** | I | A |
| Daily standup facilitation | C | C | C | I | I | I | I | **R** |
| Go/No-Go recommendation | C | **R** | C | A | I | I | I | C |
| Rollback execution (if needed) | C | I | **R** | I | I | I | I | A |
| Final parallel-run summary report | C | C | C | I | I | I | I | **R** |

**Legend:** R = Responsible (does the work), A = Accountable (owns the outcome), C = Consulted, I = Informed

### Key Role Details

- **Peter Gibbons** is the dual-tracking lead. He has the deepest knowledge of both the official and shadow workflows. His comparison spreadsheet is the primary data source for variance analysis.
- **The Bobs** own the compliance judgment. They decide whether a variance is compliance-relevant or cosmetic. Their sign-off is required for Go/No-Go.
- **Michael Bolton** is on-call for any platform issues. He monitors system logs, routing behavior, and audit trail integrity. He is the single point of failure for technical rollback.
- **Lumbergh** is the Go/No-Go decision maker. He has final authority on whether to proceed to cutover, based on the Bobs' recommendation and Lambert's data.
- **Lambert** (Implementation Planner) facilitates daily standups, compiles the summary report, and is accountable for the overall parallel-run process.

---

## 6. Data Collection Plan

### What Gets Measured

| # | Data Point | Collection Method | Frequency | Collected By | Stored Where |
|---|-----------|-------------------|-----------|-------------|-------------|
| D1 | Request outcomes (approved/rejected) under both paths | Peter's comparison spreadsheet: one row per request, columns for old-path outcome and new-path outcome | Per request | Peter Gibbons | Shared spreadsheet (access: Peter, Bobs, Lambert) |
| D2 | Cycle time per request (submission to final approval) | System timestamps for new path; Peter's estimate for old path based on historical averages | Per request | Peter Gibbons | Comparison spreadsheet |
| D3 | Variance details: any case where old and new paths diverge | Peter documents the divergence, the tier that caused it, and whether it was compliance-relevant | Per occurrence | Peter Gibbons, reviewed by The Bobs | Variance log (tab in comparison spreadsheet) |
| D4 | Intake gate rejections: count, reason, and classification (legitimate vs. form error) | Intake exception log in the system | Daily tally | Joanna | Intake exception log |
| D5 | Dashboard data accuracy: does the dashboard match Lumbergh's expectations? | Lumbergh compares dashboard view to his previous "loop me in" information for each request | Daily | Lumbergh | Dashboard accuracy log (simple checklist) |
| D6 | Platform incidents: routing failures, audit log gaps, notification issues | Michael's system monitoring alerts and manual log review | Continuous | Michael Bolton | Incident log |
| D7 | Stakeholder feedback: concerns, complaints, suggestions from any participant | Captured during daily standups and ad-hoc conversations | Daily | Lambert | Standup notes |
| D8 | Vendor exception interactions: requests involving Milton's 47-vendor list | Peter flags any request that involves a vendor from Milton's exception list; Milton reviews | Per occurrence | Peter Gibbons, Milton Waddams | Exception tracking tab in comparison spreadsheet |

### Data Flow

```
Request enters system
    ↓
New 3-tier path processes it (system timestamps recorded)
    ↓
Peter evaluates same request against old 7-tier rules (manual)
    ↓
Peter logs both outcomes + cycle times in comparison spreadsheet
    ↓
If variance detected → Peter documents in variance log → Bobs review within 24 hours
    ↓
Daily standup: Lambert reviews all data points, flags issues
    ↓
End of each week: Lambert compiles weekly summary
    ↓
End of parallel run: Lambert produces final Go/No-Go report
```

---

## 7. Communication Plan

### Phases of Communication

#### Pre-Parallel-Run (3 Days Before Start)

| Audience | Message | Channel | Owner |
|----------|---------|---------|-------|
| All approvers (current 7-tier chain) | Parallel run begins on [date]. Your role during the next two weeks. What stays the same, what changes. | Email + brief in-person walkthrough | Lambert |
| Requestors (department managers, Joanna's team) | New intake form goes live. What to expect, who to contact if issues arise. One-page guide attached. | Email + department Slack/Teams channel | Joanna |
| Lumbergh | Dashboard goes live. Here's your access, here's what each panel shows, here's what alerts you'll get. | 1:1 briefing with walkthrough | Lambert |
| Michael Bolton | Monitoring expectations, escalation path for platform incidents, backup schedule confirmed. | 1:1 technical briefing | Lambert |
| Tom Smykowski | Your input during Phase 1 helped shape this. During the parallel run, your role is to monitor for risks you flagged and report anything the team missed. | 1:1 conversation | Lambert |

#### During Parallel Run

| Communication | Frequency | Participants | Format | Owner |
|---------------|-----------|-------------|--------|-------|
| **Daily standup** | Every morning, 15 minutes | Peter, The Bobs, Michael, Lambert | Stand-up meeting (in-person or video) | Lambert (facilitates) |
| **Variance alert** | As needed (within 2 hours of detection) | Peter → The Bobs → Lambert | Email with variance log entry attached | Peter Gibbons |
| **Platform incident alert** | As needed (immediate) | Michael → Lambert → Lumbergh | Phone/IM for critical; email for non-critical | Michael Bolton |
| **Weekly summary** | End of Week 1, End of Week 2 | All participants + Lumbergh | Written report (1-2 pages) distributed by email | Lambert |
| **Lumbergh dashboard check-in** | Twice per week | Lambert + Lumbergh | 10-minute walkthrough of dashboard data | Lambert |

#### Post-Parallel-Run (Go/No-Go Decision)

| Communication | Audience | Format | Owner |
|---------------|----------|--------|-------|
| Go/No-Go recommendation report | Lumbergh, The Bobs, Lambert | Written report with data summary, metrics, recommendation | Lambert |
| Go/No-Go decision meeting | Lumbergh (decides), The Bobs (recommend), Lambert (presents), Peter (available for questions) | 30-minute meeting | Lambert (facilitates), Lumbergh (decides) |
| Cutover notification (if Go) | All approvers, all requestors, IT | Email: cutover date, what changes, who to contact | Lambert |
| Pause/rollback notification (if No-Go) | All approvers, all requestors, IT | Email: what's happening, what stays the same, revised timeline | Lambert |

### Escalation Path During Parallel Run

```
Issue detected
    ↓
Is it a platform/technical issue?
    → Yes → Michael Bolton → Lambert → Lumbergh (if rollback candidate)
    → No ↓
Is it a compliance variance?
    → Yes → Peter → The Bobs → Lambert → Lumbergh (if rollback candidate)
    → No ↓
Is it a process/usability issue?
    → Yes → Raise at daily standup → Lambert tracks resolution
```

---

## 8. Risk Matrix — Parallel-Run-Specific Risks

| # | Risk | Likelihood | Impact | Mitigation |
|---|------|-----------|--------|-----------|
| PR1 | **Insufficient request volume during parallel run.** If fewer than 20 requests flow through during the 2-week window, the data set may be too small to validate the new process confidently. | Medium | Medium | If volume is below 10 requests in Week 1, extend the parallel run by one week. Consider routing historical requests through the new path as additional test cases. |
| PR2 | **Peter's dual-tracking introduces bias.** Peter knows which outcome is "expected" (new path should match or beat old path). His manual evaluation may unconsciously favor the new process. | Low | Medium | The Bobs independently review every variance Peter flags AND spot-check 3 randomly selected "no variance" cases per day to validate Peter's assessments. |
| PR3 | **Tom Smykowski undermines the parallel run.** Tom may surface manufactured risks or escalate minor concerns to slow the process, since tier removal threatens his perceived relevance. | High | Medium | Give Tom a defined monitoring role (Activity in RACI). Document his concerns visibly. If his concerns are legitimate, they strengthen the process. If not, the data shows it. Either way, he's included. |
| PR4 | **Intake form frustrates requestors, creating workaround pressure.** If the new intake gate rejects too many submissions, requestors may pressure Peter to bypass it (recreating shadow workflows). | Medium | High | 48-hour correction window during parallel run (grace period). Joanna's team is the first point of contact for intake issues. Track rejection reasons daily and adjust form rules in real time if design errors emerge. |
| PR5 | **Michael Bolton becomes unavailable.** He is the single point of failure for all technical changes and platform monitoring. If he is sick, on vacation, or pulled to another priority, the parallel run has no technical support. | Medium | High | Before the parallel run starts, Michael documents the monitoring checklist and rollback procedure in enough detail that a second IT team member can execute them. Identify a backup operator during Phase 1. |
| PR6 | **Dashboard data latency creates false negatives.** If the dashboard refreshes too slowly, Lumbergh may see stale data and report accuracy issues that are actually timing problems. | Low | Low | Confirm dashboard refresh interval with Michael before parallel run. Set Lumbergh's expectations on data freshness. If refresh interval is more than 1 hour, note it in the dashboard accuracy log. |
| PR7 | **Vendor exception requests expose gaps.** Requests involving Milton's 47-vendor exception list may require approver judgment that previously lived in the elective tiers. | Medium | High | Milton reviews every request that hits a vendor on his exception list during the parallel run. Any gap is documented and assessed for resolution before cutover. |
| PR8 | **Parallel-run fatigue.** Two weeks of dual-tracking is demanding. Peter and the Bobs may lose rigor in the second week. | Medium | Medium | Lambert monitors data quality daily. If Week 2 entries are less detailed than Week 1, Lambert flags it at standup and reallocates comparison work if needed. |

---

## 9. Go/No-Go Decision Criteria

The Go/No-Go meeting happens at the end of Week 2 (or end of any extension period). Lumbergh makes the final call based on the Bobs' recommendation and Lambert's data.

### Go/No-Go Checklist

#### Must Pass — Any Failure Blocks Cutover

| # | Criterion | Evidence Required | Owner |
|---|-----------|-------------------|-------|
| G1 | Zero compliance-relevant variance between old and new paths across the full parallel-run period | Variance log reviewed and signed off by the Bobs, showing no compliance gaps | The Bobs |
| G2 | Audit trail at all 3 mandatory tiers captures required data (who, what, when) for 100% of parallel-run requests | Bobs' spot-check results confirm complete records at every tier for every request | The Bobs |
| G3 | Production configuration backup is verified and restorable | Michael confirms backup exists, has been tested, and can be restored in under 4 hours | Michael Bolton |
| G4 | All in-flight requests at deactivated tiers have been resolved (fast-tracked through old path or rerouted to next active tier) | In-flight inventory shows zero requests stranded at a deactivated tier | Peter Gibbons |
| G5 | Rollback procedure reviewed and executable in under 4 hours | Written procedure reviewed by Michael and Lambert; dry-run completed in sandbox | Michael Bolton, Lambert |
| G6 | No unresolved automatic-rollback conditions (F1, F2, F3) during the parallel run | Incident log and variance log show no unresolved triggers | Lambert |

#### Should Pass — Failure Triggers Risk Discussion, Not Automatic Block

| # | Criterion | Target | Evidence Required | Owner |
|---|-----------|--------|-------------------|-------|
| G7 | Cycle time improvement | ≥ 30% average reduction vs. historical | Comparison spreadsheet data | Peter Gibbons |
| G8 | Intake form rejection rate driven by genuinely incomplete submissions, not form design | ≤ 3 form-design rejections per week | Intake exception log categorized by reason | Joanna |
| G9 | Dashboard provides Lumbergh with equal or better visibility | Lumbergh's written confirmation | Dashboard accuracy log | Lumbergh |
| G10 | No stakeholder has raised a formal, unresolved complaint about the new process | Standup notes and feedback log | Lambert |

### Decision Outcomes

| Decision | Criteria | Next Step |
|----------|----------|-----------|
| **Go** | All Must-Pass criteria met. Should-Pass criteria met or risk accepted. | Proceed to Phase 4 (Cutover and Monitoring). Deploy new configuration to production. Begin 5-day post-cutover monitoring. |
| **Conditional Go** | All Must-Pass criteria met. One or more Should-Pass criteria not met, but risk is understood and accepted by Lumbergh. | Proceed to cutover with documented risk acceptance. Monitor the unmet Should-Pass items during post-cutover period. |
| **No-Go: Extend** | Must-Pass variance found but remediable. Root cause identified and fixed. | Extend parallel run by 1 week. Re-evaluate at end of extension. Maximum 2 extensions (4 weeks total). |
| **No-Go: Pause** | Systemic issue identified (platform instability, fundamental process gap). | Roll back to 7-tier process. Conduct root cause analysis. Revise implementation plan. Re-enter at Phase 2 or Phase 3 after remediation. |

---

## Appendix A: Comparison Spreadsheet Template

Peter's dual-tracking spreadsheet should include these columns:

| Column | Description |
|--------|-------------|
| Request ID | System-generated ID |
| Submission date | When the request entered the system |
| Requestor | Who submitted |
| Department | Requesting department |
| Amount | Dollar value |
| Vendor | Vendor name |
| On Milton's exception list? | Yes/No |
| New-path outcome | Approved / Rejected / Pending |
| New-path cycle time (days) | Submission to final approval |
| Old-path estimated outcome | What Peter believes would have happened under the 7-tier flow |
| Old-path estimated cycle time (days) | Peter's estimate based on historical patterns |
| Variance detected? | Yes/No |
| Variance description | If yes, what differed and why |
| Compliance-relevant? | Yes/No (assessed by the Bobs) |
| Tier(s) that would have caught it | Which of the 4 elective tiers would have flagged this |
| Notes | Additional context |

---

## Appendix B: Daily Standup Agenda (15 Minutes)

1. **Requests processed since last standup** — Peter: count, any variance flagged (2 min)
2. **Variance review** — Bobs: any new compliance assessments, outstanding items (3 min)
3. **Platform status** — Michael: any incidents, monitoring observations (2 min)
4. **Intake form status** — Joanna (or proxy): rejections, form issues (2 min)
5. **Dashboard status** — Lumbergh update (via Lambert if Lumbergh not present) (1 min)
6. **Blockers and risks** — open floor (3 min)
7. **Action items** — Lambert records and assigns (2 min)
