# Executive Alerts Specification — High-Value Request Notifications

**Date:** 2026-03-02
**Issue:** #30
**Epic:** 10 — Dashboard
**Initiative:** 1 — Clear the Path
**Author:** Ripley (Lead)

---

## Purpose

Lumbergh's informal gate exists because he wants visibility into high-value requests. The dashboard (Issue #29) gives him passive visibility. Executive alerts give him proactive visibility. Together, they replace the gate without leaving an information vacuum.

These alerts are informational only. They do not require any action from Lumbergh. No approval, no reply, no sign-off. He reads them, he knows what is happening, and the request continues moving through the formal approval chain without waiting for him.

**Sources:** Lumbergh interview ("I want to see where everything is"), Initiative Scope (dashboard replaces informal gate), Brainstorm ideas #9 and #27

---

## Alert Types

### 1. New High-Value Request

Triggered when a request enters the approval queue above a configurable dollar threshold.

| Field | Detail |
|---|---|
| **Trigger** | Request submitted with estimated value at or above the configured threshold |
| **Default threshold** | $25,000 |
| **Content** | Request summary, requestor name, requestor team, dollar amount, request category, expected approval timeline based on current average cycle time |
| **Delivery** | Email and dashboard notification (configurable) |
| **Action required** | None. Informational only. |

### 2. SLA Warning

Triggered when a high-value request approaches its target approval timeline without completing all approval steps.

| Field | Detail |
|---|---|
| **Trigger** | Request is at 75% of its target cycle time and has not received final approval |
| **Default timing** | 75% of target cycle time (e.g., if target is 5 business days, alert fires at end of day 3.75, rounded to day 4) |
| **Content** | Request summary, requestor name, dollar amount, current step in the approval chain, name of current approver, days elapsed, days remaining |
| **Delivery** | Email and dashboard notification |
| **Action required** | None. Informational only. Lumbergh may choose to follow up, but the alert does not create an obligation. |

### 3. SLA Breach

Triggered when a high-value request exceeds its target approval timeline.

| Field | Detail |
|---|---|
| **Trigger** | Request has exceeded 100% of its target cycle time without final approval |
| **Content** | Request summary, requestor name, dollar amount, current step, current approver, total days elapsed, days overdue |
| **Delivery** | Email and dashboard notification (marked as urgent/high-priority in email subject) |
| **Action required** | None required from Lumbergh. The alert is a signal, not an escalation path. Formal escalation follows existing process rules. |

---

## Alert Content Format

All alerts follow a consistent format:

```
SUBJECT: [Alert Type] — [Request Category] — $[Amount]

Request: [Title/Summary]
Requestor: [Name] ([Team])
Amount: $[Dollar Amount]
Category: [Procurement / Budget Reallocation / Vendor Contract / Other]
Submitted: [Date]
Expected Timeline: [X business days based on category average]
Current Status: [Step N of M — Awaiting [Approver Name]]
```

For SLA Warning and SLA Breach alerts, the following additional fields are included:

```
Days Elapsed: [N]
Days Remaining: [M] (or "OVERDUE by [X] days" for breaches)
Current Approver: [Name]
Hold Duration at Current Step: [N days]
```

---

## Threshold Configuration

Lumbergh must be able to adjust alert thresholds without IT involvement. This is a hard requirement.

### Configurable Parameters

| Parameter | Default | Range | Who Can Change |
|---|---|---|---|
| Dollar threshold for new request alerts | $25,000 | $1,000 to $500,000 | Lumbergh (self-service) |
| SLA warning percentage | 75% | 50% to 90% | Lumbergh (self-service) |
| Delivery channel (email, Teams, dashboard only) | Email + Dashboard | Any combination | Lumbergh (self-service) |
| Alert frequency cap (max alerts per day) | No cap | 1 to unlimited | Lumbergh (self-service) |
| Quiet hours (no email alerts during off-hours) | Off | Configurable start/end times | Lumbergh (self-service) |

### Configuration Interface

- Accessible from the dashboard settings panel
- Changes take effect immediately, no deployment required
- Audit log of threshold changes (who changed what, when) for compliance purposes
- Reset to defaults option available

---

## Delivery Channels

Three delivery options, configurable per alert type:

### Email
- Standard email to Lumbergh's inbox
- Subject line includes alert type and dollar amount for quick scanning
- Body follows the content format above
- SLA Breach emails marked with high-priority flag

### Microsoft Teams
- Adaptive card notification in a dedicated channel or direct message (Lumbergh's choice)
- Card includes key fields with a "View in Dashboard" link
- Does not require reply or interaction

### Dashboard Notification
- Bell icon / notification badge on the dashboard
- Alert history view showing all alerts with filters by type, date range, and dollar amount
- Read/unread tracking

---

## Integration with Issue #29 (Gate Replacement)

These alerts are a dependency for the gate replacement. Lumbergh's acceptance criteria for removing the informal gate include receiving proactive alerts for high-value requests. The timeline alignment:

| Gate Replacement Phase | Alert Milestone |
|---|---|
| Phase 1 (Dashboard deployment, days 1 through 5) | Alert system configured and integrated with dashboard |
| Phase 2 (Lumbergh training, days 6 through 8) | Lumbergh walks through alert settings, configures thresholds, sees sample alerts |
| Phase 3 (Gate removal, days 9 through 12) | Alerts are live. Lumbergh receives real notifications as requests enter the queue. |
| Phase 4 (Measurement, days 13 through 26) | Alert delivery and Lumbergh engagement tracked as part of validation. |

---

## Testing Plan

### Pre-Go-Live Testing (During Phase 1 and 2)

| Test | Method | Pass Criteria |
|---|---|---|
| Alert triggers correctly at threshold | Submit simulated requests at $24,999, $25,000, and $25,001 | Alert fires for $25,000 and $25,001 only |
| Alert content is accurate | Compare alert content against source request data | All fields match, no missing data |
| SLA Warning timing is correct | Submit request with 5-day target, verify alert fires at day 4 | Alert fires within 1 hour of the 75% mark |
| SLA Breach timing is correct | Allow simulated request to exceed target | Alert fires within 1 hour of the 100% mark |
| Threshold change takes effect immediately | Lumbergh changes threshold from $25K to $10K, submit $15K request | Alert fires for the $15K request |
| Email delivery works | Check Lumbergh's inbox after simulated trigger | Email arrives within 5 minutes |
| Teams delivery works | Check Teams channel/DM after simulated trigger | Adaptive card appears within 5 minutes |
| Dashboard notification works | Check dashboard after simulated trigger | Badge appears, alert visible in history |
| Quiet hours respected | Set quiet hours, trigger alert during quiet period | Email held until quiet hours end, dashboard notification still appears |
| Alert frequency cap works | Set cap to 3/day, trigger 5 alerts | Only 3 emails sent, all 5 appear in dashboard |

### Post-Go-Live Validation (Phase 4)

| Check | Frequency | Owner |
|---|---|---|
| Lumbergh receiving alerts as expected | Daily for first week, then weekly | Kane (QA) |
| Alert data accuracy spot-check | Weekly for first month | Kane |
| Lumbergh threshold adjustment test | Once during Phase 4 | Lumbergh (with Kane observing) |
| False positive/negative review | End of Phase 4 | Kane + Ripley |

---

## Edge Cases

| Scenario | Handling |
|---|---|
| Request amount is updated after submission (crosses threshold after initial entry) | Re-evaluate against threshold. Fire alert if new amount qualifies. |
| Request is withdrawn after alert fires | No follow-up alert. Dashboard shows request as withdrawn. |
| Multiple requests from same team in same day | Each triggers independently. Frequency cap applies to total alerts, not per-request. |
| Lumbergh is on vacation | Alerts queue in dashboard. Email delivery optional (quiet hours or delegation). No impact on approval flow. |
| Dollar threshold set to $0 | Effectively alerts on every request. System allows it but shows a warning that alert volume may be high. |

---

## Non-Requirements

To be clear about what this system does NOT do:

- **Does not create an approval step.** Alerts are informational. They do not block or pause requests.
- **Does not require Lumbergh to acknowledge alerts.** Read receipts are tracked for analytics, not for process enforcement.
- **Does not escalate to Lumbergh's manager.** SLA Breach alerts go to Lumbergh. Formal escalation follows existing process rules and is not part of this spec.
- **Does not replace the formal approval chain.** The 3 mandatory tiers (budget holder, compliance, audit) are unchanged.

---

## Acceptance Criteria Checklist

- [ ] Alert thresholds configurable by dollar amount (e.g., notify on requests over $25K)
- [ ] Alerts are informational only, no approval action required
- [ ] Alert format includes request summary, requestor, dollar amount, and expected approval timeline
- [ ] Lumbergh can adjust thresholds without IT involvement
- [ ] Alerts tested and confirmed working
