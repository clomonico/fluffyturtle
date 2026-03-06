# SLA Threshold Definitions for Approval Steps

**Issue:** #37
**Initiative:** Glass Floor (Init 2)
**Epic:** 13 — Auto-Escalation
**Author:** Dallas (Process Designer)
**Date:** 2026-03-02
**Status:** Draft — pending stakeholder review

---

## Purpose

Define explicit SLA thresholds for each approval step so that every request has a measurable target completion time, idle requests trigger automatic escalation, and requestors can see where their submission stands. These SLAs apply only to Tiers 1–3 (the retained steps after Init 1 tier simplification) and account for parallel routing introduced in Epic 12.

**Problem this solves:** Joanna's vendor contract sat idle for 11 days with no escalation. Peter follows up manually via email when things stall. Lumbergh guesses "2–3 weeks" for total cycle time. No escalation mechanism exists today, even though the platform supports it.

---

## Tier Structure (Post–Init 1)

| Tier | Step | Approver | What's Verified |
|------|------|----------|-----------------|
| 1 | Requestor Verification | Requestor / System | Submission completeness, documentation attached, correct request type |
| 2 | Budget Authority Confirmation | Samir (Budget Authority) | Funds available, budget code valid, spend within delegation |
| 3 | Compliance / COI Check | Compliance team | Regulatory requirements met, COI disclosures current, vendor eligibility |

**Routing model (Epic 12):** Tier 1 → [Tier 2 + Tier 3 in parallel] → Complete

---

## Request Type Classification

SLA thresholds vary by request type. Five categories, grouped into three complexity bands.

| Complexity | Request Types | Characteristics |
|------------|--------------|-----------------|
| Standard | Standard purchase, Renewal | Known vendor, within budget norms, no new risk factors |
| Elevated | High-value ($50K+) | Larger dollar amount, may require additional budget verification |
| Complex | New vendor, Vendor exception | New relationship or policy deviation, requires deeper compliance review |

---

## SLA Thresholds by Tier and Complexity

### Tier 1 — Requestor Verification

| Complexity | SLA Target | Stretch Goal |
|------------|-----------|--------------|
| Standard | 4 hours | 2 hours |
| Elevated | 8 hours | 4 hours |
| Complex | 8 hours | 4 hours |

Tier 1 is largely system-driven (intake validation from Epic 9). The clock measures time from submission to the request entering the approval queue. If the requestor needs to fix incomplete documentation, the clock pauses (see Clock Rules below).

### Tier 2 — Budget Authority Confirmation

| Complexity | SLA Target | Stretch Goal |
|------------|-----------|--------------|
| Standard | 24 hours | 16 hours |
| Elevated | 48 hours | 24 hours |
| Complex | 24 hours | 16 hours |

Samir reviews spend against available budget. High-value requests get 48 hours because they may require cross-checking against quarterly commitments or consulting with finance.

### Tier 3 — Compliance / COI Check

| Complexity | SLA Target | Stretch Goal |
|------------|-----------|--------------|
| Standard | 24 hours | 16 hours |
| Elevated | 48 hours | 24 hours |
| Complex | 72 hours | 48 hours |

New vendor and vendor exception requests require the most compliance effort: vendor eligibility research, COI disclosure review, and sometimes regulatory database checks. The 72-hour window for complex requests reflects compliance team feedback that new vendor vetting genuinely takes time.

### End-to-End Targets (with Parallel Routing)

Because Tier 2 and Tier 3 run in parallel, the end-to-end SLA is Tier 1 + max(Tier 2, Tier 3).

| Complexity | End-to-End Target | Current State |
|------------|-------------------|---------------|
| Standard | 28 hours (~1.2 business days) | 2–3 weeks (Lumbergh estimate) |
| Elevated | 56 hours (~2.3 business days) | 2–3 weeks |
| Complex | 80 hours (~3.3 business days) | 2–3 weeks+ |

These targets replace the vague "2–3 weeks" with measurable commitments.

---

## Clock Rules

### When the Clock Starts

- **Tier 1:** Clock starts when the requestor clicks Submit.
- **Tier 2 and Tier 3:** Clock starts when Tier 1 is complete and the request enters the parallel approval queue. Both clocks start simultaneously.

### What Pauses the Clock

| Pause Reason | Applies To | How It Works |
|--------------|-----------|--------------|
| Requestor action needed | Tier 1 | System sends the request back for missing info; clock pauses until resubmission |
| Clarification requested | Tier 2, Tier 3 | Approver formally requests additional information from the requestor via the platform; clock pauses until the requestor responds |
| Planned approver absence | Tier 2, Tier 3 | Delegated approver is auto-assigned (see Exception Handling); clock does not pause — it transfers |
| System outage | All | Platform-declared outage pauses all SLA clocks; resumes when service is restored |

### What Does NOT Pause the Clock

- Approver being busy with other work
- Approver not seeing the notification
- Weekends and holidays (SLAs are measured in business hours, Mon–Fri, 8am–6pm local)
- Internal discussion between approvers

---

## Escalation Targets

Each tier has a defined escalation path. Escalation is automatic — the platform triggers it when the SLA target is reached without a decision.

### Escalation Chain

| Tier | Step 1: Reminder | Step 2: Escalation | Step 3: Executive Escalation |
|------|-----------------|-------------------|------------------------------|
| **Tier 1** | Requestor gets a nudge at 75% of SLA | Requestor's manager notified at 100% | Request auto-cancelled at 200% with option to resubmit |
| **Tier 2** | Samir gets a reminder at 75% of SLA | Samir's manager notified at 100% | Lumbergh notified at 150%; can override or reassign |
| **Tier 3** | Compliance approver gets a reminder at 75% of SLA | Compliance team lead notified at 100% | Lumbergh notified at 150%; can override or reassign |

### Escalation Timing Examples (Standard Request)

| Tier | 75% Reminder | 100% Escalation | 150% Executive |
|------|-------------|-----------------|----------------|
| Tier 1 | 3 hours | 4 hours | 8 hours |
| Tier 2 | 18 hours | 24 hours | 36 hours |
| Tier 3 | 18 hours | 24 hours | 36 hours |

### Escalation Actions

- **Reminder (75%):** In-platform notification + email to the assigned approver. Informational only.
- **Escalation (100%):** Notification to the approver's manager. The original approver retains the task but their manager has visibility.
- **Executive escalation (150%):** Lumbergh receives an alert (per Epic 10 dashboard and #30 executive alerts spec). Lumbergh can reassign the approval to another qualified person or approve directly if within his delegation.

---

## Interaction with Parallel Routing (Epic 12)

Parallel routing means Tier 2 and Tier 3 run concurrently. SLA implications:

1. **Independent clocks.** Each parallel branch has its own SLA clock. Tier 2 hitting its SLA does not affect Tier 3's clock, and vice versa.
2. **Request completes when both finish.** The request moves to Complete only after both Tier 2 and Tier 3 have rendered a decision.
3. **One branch stalling doesn't block the other.** If Tier 3 escalates, Tier 2 may already be done. The escalation applies only to the stalled branch.
4. **End-to-end SLA is measured from Tier 1 submission to both parallel branches completing.** This is the number reported to requestors and tracked on the dashboard.
5. **Rejection in either branch.** If Tier 2 or Tier 3 rejects, the other branch is notified but allowed to complete its review (the compliance record still matters). The request is marked rejected with the rejecting tier's rationale.

---

## Exception Handling

### Planned Absence

- Approvers must designate a delegate in the platform before going on leave.
- If no delegate is set and the approver is marked out-of-office, the system auto-assigns to the next person in the role (Samir's backup for Tier 2; compliance team member for Tier 3).
- The SLA clock does not pause for planned absence. The delegation mechanism is the mitigation.

### System Outage

- Platform-declared outages (logged by IT) pause all SLA clocks for the duration.
- The pause is automatic and retroactive — no manual intervention needed.
- Outage time is excluded from all SLA reporting.

### Legitimate Complexity

- Approvers can request a one-time SLA extension through the platform by documenting the reason.
- Extensions are limited to 1x the original SLA (e.g., a 24-hour SLA can be extended by up to 24 additional hours).
- Extensions are visible to the requestor and logged for reporting.
- Extensions do not suppress escalation notifications — they shift the escalation timeline.
- If an extension is insufficient, the approver should escalate to their manager proactively.

### Disputed Requests

- If an approver believes a request is misclassified (e.g., marked Standard but should be Complex), they can reclassify it in the platform.
- Reclassification resets the SLA clock to the new complexity tier's threshold from the point of reclassification.
- Reclassifications are auditable and included in reporting.

---

## SLA Visibility for Requestors

Requestors can see:

- **Current step:** Which tier(s) their request is in, including parallel branches
- **Time elapsed:** How long the request has been at the current step
- **SLA target:** The target completion time for each active step
- **Countdown:** A visual indicator (green / amber / red) showing SLA status
  - Green: under 75% of SLA consumed
  - Amber: 75–100% of SLA consumed
  - Red: SLA exceeded, escalation triggered
- **Estimated completion:** Based on the remaining SLA time for all pending steps

Requestors cannot see:
- The identity of the approver (privacy — they see the role, not the person)
- Internal escalation details (they see that escalation occurred, not who was notified)

---

## Measurement and Reporting

### Key Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| SLA compliance rate | % of approvals completed within SLA target | ≥ 90% within 90 days of launch |
| Average cycle time | Mean time from submission to completion | Reduction of 60%+ vs. current state |
| Escalation rate | % of requests that trigger any escalation | ≤ 15% (indicates SLAs are feasible) |
| Extension rate | % of requests where an approver requests an extension | ≤ 10% |
| Tier-level compliance | SLA compliance broken down by tier | Per-tier visibility for targeted improvement |
| Complexity-level compliance | SLA compliance by request type | Ensures complex requests aren't unfairly penalized |

### Reporting Cadence

- **Real-time:** Dashboard (Epic 10) shows live SLA status for all active requests
- **Weekly:** Automated summary to Lumbergh and team leads — compliance rate, escalation count, average cycle time
- **Monthly:** Trend report comparing current month vs. prior months, with breakdown by tier and request type
- **Quarterly:** SLA threshold review (see Threshold Adjustment Process below)

---

## Threshold Adjustment Process

SLAs are not permanent. They should be tuned based on evidence.

### Review Triggers

- **Scheduled:** Quarterly review of all thresholds using the prior quarter's data
- **Metric-driven:** If SLA compliance for any tier/complexity combination drops below 75% for two consecutive weeks, an ad-hoc review is triggered
- **Feedback-driven:** Any approver or requestor can request a threshold review through their manager

### Review Process

1. Pull SLA compliance, escalation, and extension data for the period
2. Identify tiers or request types where thresholds are consistently too tight or too loose
3. Propose adjustments with supporting data
4. Review with affected approvers (Samir for Tier 2, compliance team for Tier 3)
5. Lumbergh approves changes as executive sponsor
6. Updated thresholds are configured in the platform and communicated to all users

### Guardrails

- No single adjustment can change a threshold by more than 50% in either direction
- Threshold changes require at least 2 weeks of data after the prior change before another adjustment is permitted
- All threshold changes are version-controlled and auditable

---

## Stakeholder Review and Approval

These thresholds require review and sign-off before going live.

| Stakeholder | Role in Review | Sign-off Required |
|-------------|---------------|-------------------|
| Samir | Validate Tier 2 SLAs are feasible for budget authority work | Yes |
| Compliance team | Validate Tier 3 SLAs are feasible for compliance work | Yes |
| Lumbergh | Executive sponsor approval of all thresholds | Yes |
| Peter | Validate end-to-end targets against operational expectations | Advisory |
| The Bobs | Confirm SLAs don't compromise audit trail requirements | Advisory |

### Approval Sequence

1. Dallas circulates draft thresholds to Samir and compliance team for feasibility feedback
2. Incorporate feedback, adjust thresholds if warranted
3. Present revised thresholds to Lumbergh for executive sign-off
4. Share final thresholds with the Bobs for audit trail confirmation
5. Publish to all users with a 1-week notice period before enforcement begins

---

## Dependencies

| Dependency | Relationship |
|------------|-------------|
| Init 1 — Tier simplification (Epic 7, Epic 8) | SLAs are defined for the 3 retained tiers only |
| Epic 9 — Intake validation | Tier 1 SLA assumes intake validation rules are enforced |
| Epic 10 — Dashboard | SLA countdown and status visibility displayed on the dashboard |
| Epic 12 — Parallel routing | SLA clock rules account for parallel Tier 2 + Tier 3 execution |
| #30 — Executive alerts | Lumbergh's 150% escalation alert uses the executive alert mechanism |
| Epic 13 — Auto-escalation (this epic) | Platform configuration to trigger reminders and escalation at SLA thresholds |

---

## Open Questions

1. Should business hours be standardized across time zones, or should each department use local business hours?
2. Does the 72-hour complex SLA for Tier 3 need a carve-out for vendor exceptions that require external regulatory database checks (which may have their own response times)?
3. Should the quarterly threshold review be a standing meeting or async review with a meeting only when changes are proposed?
