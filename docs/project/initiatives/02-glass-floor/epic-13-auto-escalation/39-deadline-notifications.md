# Approaching-Deadline Notification System

**Issue:** #39
**Initiative:** Glass Floor (Init 2)
**Epic:** 13 — Auto-Escalation
**Author:** Parker (Stakeholder Analyst)
**Date:** 2026-03-02
**Status:** Draft — pending stakeholder review
**Dependencies:** #38 (Auto-Escalation Configuration), #37 (SLA Thresholds), #30 (Executive Alerts)

---

## Purpose

Give approvers timely, non-punitive nudges when approvals in their queue are approaching SLA deadlines, and give requestors real-time visibility into where their request stands. Today Peter follows up manually via email. Requestors have zero visibility — Joanna's vendor contract sat idle for 11 days and she had no way to know it was stalled. This system replaces both of those gaps.

**What this is not:** This is not the auto-escalation engine (#38). Notifications here are the friendly "heads up" layer that fires *before* escalation kicks in. The timeline is: notification at 24 hours remaining → urgent notification at 4 hours remaining → auto-escalation at 100% SLA per #38.

---

## 1. Notification Types and Triggers

Three notification types fire based on time remaining before the SLA deadline for a given approval step.

| Type | Trigger | Audience | Tone |
|------|---------|----------|------|
| **Warning** | 24 hours before SLA breach | Assigned approver | Friendly nudge — "this is coming up" |
| **Urgent** | 4 hours before SLA breach | Assigned approver | Firm but supportive — "this needs attention soon" |
| **Breach** | SLA deadline reached (0 hours remaining) | Assigned approver + requestor | Factual — "the SLA window has closed, escalation is underway" |

### Trigger Calculation

- SLA deadlines are calculated from the auto-escalation configuration (#38), which defines SLA targets per tier and complexity band.
- The 24-hour and 4-hour triggers count backward from the SLA deadline in **business hours** (Mon–Fri, 8am–6pm local time), consistent with the SLA clock in #37 and #38.
- If the SLA target is shorter than 24 hours (e.g., Tier 1 Standard at 4 hours), only the triggers that fit within the window fire. A 4-hour SLA fires the urgent notification at the start (since 4 hours remaining = the entire window) but skips the 24-hour warning entirely.

### Trigger Fit by SLA Target

| SLA Target | 24h Warning Fires? | 4h Urgent Fires? | Breach Fires? |
|------------|---------------------|-------------------|---------------|
| 4 hours | No (SLA shorter than 24h) | Yes (at SLA start) | Yes |
| 8 hours | No | Yes (at 4h remaining) | Yes |
| 24 hours | Yes (at SLA start) | Yes (at 4h remaining) | Yes |
| 48 hours | Yes (at 24h remaining) | Yes (at 4h remaining) | Yes |
| 72 hours | Yes (at 24h remaining) | Yes (at 4h remaining) | Yes |

---

## 2. Notification Content Design

Each notification includes enough context for the approver to act without hunting for information.

### Warning Notification (24 Hours Remaining)

> **Subject:** Heads up — [Request Type] #[ID] due in 24 hours
>
> **Body:**
> Hi [Approver First Name],
>
> Request #[ID] ([Request Type], [Complexity]) is in your approval queue and due within 24 business hours.
>
> | Detail | Value |
> |--------|-------|
> | Requestor | [Requestor Name] |
> | Submitted | [Submission Date] |
> | Your step | [Tier Name] |
> | SLA deadline | [Deadline Date/Time] |
> | Time remaining | ~24 hours |
>
> **[Open request →]**
>
> If you need to delegate, you can reassign from the request detail page.

### Urgent Notification (4 Hours Remaining)

> **Subject:** Urgent — [Request Type] #[ID] due in 4 hours
>
> **Body:**
> Hi [Approver First Name],
>
> Request #[ID] is due in approximately 4 business hours. After the deadline, this will escalate to [Escalation Target Name] per the auto-escalation policy.
>
> | Detail | Value |
> |--------|-------|
> | Requestor | [Requestor Name] |
> | Submitted | [Submission Date] |
> | Your step | [Tier Name] |
> | SLA deadline | [Deadline Date/Time] |
> | Time remaining | ~4 hours |
> | Escalates to | [Manager Name] at deadline |
>
> **[Open request →]** &nbsp; **[Reassign →]**
>
> If you need more time, you can request a one-time SLA extension from the request detail page.

### Breach Notification (SLA Deadline Reached)

> **Subject:** SLA breach — [Request Type] #[ID] has escalated
>
> **Body (to approver):**
> Hi [Approver First Name],
>
> Request #[ID] has passed its SLA deadline at [Tier Name]. Auto-escalation is now active per policy.
> [Manager Name] has been notified. You still have the task — please complete your review as soon as possible.
>
> **[Open request →]**
>
> **Body (to requestor):**
> Hi [Requestor First Name],
>
> Your request #[ID] ([Request Type]) has passed the expected review window at [Tier Name].
> The system has automatically escalated review to keep things moving. No action is needed from you.
> You can track status on your request dashboard.
>
> **[View request status →]**

### Content Principles

- **Name the next step.** Every notification tells the approver what happens if they don't act, without being threatening about it.
- **Include a direct link.** One click to open the request. No searching.
- **Keep it short.** The table format puts key details front and center. No walls of text.
- **No blame language.** "Due in 4 hours" not "You have failed to act for 20 hours."

---

## 3. Channel Options and Per-User Preferences

### Available Channels

| Channel | Description | Default State |
|---------|-------------|---------------|
| **Email** | Standard email to the approver's work address | On |
| **Microsoft Teams** | Adaptive card posted to the approver's Teams chat | On |
| **In-platform** | Banner notification within the approval platform | Always on (cannot be disabled) |

In-platform notifications are always delivered regardless of user preferences. Email and Teams are the configurable channels.

### Per-User Preference Model

Each approver can configure their own notification preferences. The system respects individual choices.

| Setting | Options | Default |
|---------|---------|---------|
| Email notifications | On / Off | On |
| Teams notifications | On / Off | On |
| Warning notification (24h) | On / Off | On |
| Urgent notification (4h) | On / Off (cannot disable if email and Teams are both off) | On |
| Breach notification | Always on | Always on |

**Guardrails:**
- Approvers cannot disable all channels. If both email and Teams are turned off, in-platform remains active and the system displays a note: "You will only receive deadline notifications within the platform. Consider enabling at least one external channel."
- Breach notifications cannot be turned off. They are informational and tied to the escalation audit trail.
- Managers cannot override individual approver preferences. If a manager wants to ensure their team is reachable, that's a conversation, not a system control.

### Teams Adaptive Card Format

Teams notifications use an adaptive card with action buttons so the approver can respond without leaving Teams.

**Card elements:**
- Header with notification type (Warning / Urgent / Breach) and color coding (amber / red / dark red)
- Request summary table (same fields as the email body)
- Action buttons: "Open Request" (deep link), "Reassign" (deep link to reassignment page)
- For urgent notifications: additional "Request Extension" button

---

## 4. Requestor-Facing SLA Countdown Display

Requestors currently have no visibility into where their request is or when it might move. This changes that.

### Request Status Dashboard

Every requestor sees a "My Requests" dashboard showing all their active and completed requests. Each active request displays:

| Element | Description |
|---------|-------------|
| **Current step** | Which tier is currently reviewing (e.g., "Budget Authority Review") |
| **Status indicator** | Color-coded: Green (on track), Amber (approaching deadline), Red (overdue) |
| **Time remaining** | Countdown showing business hours remaining before the SLA deadline for the current step |
| **Progress bar** | Visual bar showing how far through the overall approval flow the request is |
| **Parallel branch status** | For requests in Tier 2 + Tier 3 parallel review, each branch shown separately |

### Countdown Display Rules

| SLA Status | Display | Color |
|------------|---------|-------|
| More than 24 hours remaining | "[X] days, [Y] hours remaining" | Green |
| 24 hours or less remaining | "[X] hours remaining" | Amber |
| 4 hours or less remaining | "[X] hours, [Y] minutes remaining" | Red (with subtle pulse) |
| Overdue | "Review window exceeded — escalation active" | Red |
| Completed | "Approved ✓" or "Returned for revision" | Gray |

### Parallel Routing Display

When Tier 2 and Tier 3 run in parallel, the requestor sees both branches with independent countdowns:

```
┌─────────────────────────────────────────────────┐
│  Request #4521 — Software License Renewal       │
│  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│                                                  │
│  Tier 1: Requestor Verification    ✓ Complete   │
│                                                  │
│  ┌──────────────────┐ ┌──────────────────┐      │
│  │ Tier 2: Budget   │ │ Tier 3: Compliance│     │
│  │ ██████████░░ 78% │ │ ████░░░░░░░░ 33% │      │
│  │ 🟡 5h remaining  │ │ 🟢 48h remaining │      │
│  └──────────────────┘ └──────────────────┘      │
│                                                  │
│  Overall: Waiting on both reviews                │
└─────────────────────────────────────────────────┘
```

### What Requestors Do NOT See

- Approver names (privacy — the requestor sees the role, not the person)
- Escalation status (they see "Review window exceeded" but not "Your approver's manager was notified")
- Internal SLA targets (they see time remaining and color coding, not the raw SLA hours)

---

## 5. Notification Frequency Management

Alert fatigue is the fastest way to make people ignore notifications entirely. This system is designed to be useful, not noisy.

### Deduplication Rules

| Rule | Behavior |
|------|----------|
| **One warning per step** | An approver receives at most one 24-hour warning per request per approval step. No repeats. |
| **One urgent per step** | An approver receives at most one 4-hour urgent notification per request per approval step. No repeats. |
| **No stacking** | If an approver has 5 requests all hitting the 24-hour mark within the same hour, they receive 5 individual notifications (one per request), not a batch. However, see digest mode below. |
| **SLA extension resets** | If an approver requests and receives an SLA extension, the notification timers reset to the new deadline. They may receive a second warning if the extended deadline approaches. |

### Digest Mode (Optional Per User)

Approvers who handle high volumes can opt into digest mode:

| Setting | Behavior |
|---------|----------|
| **Digest frequency** | Twice daily (9:00 AM and 2:00 PM local) or once daily (9:00 AM local) |
| **Digest content** | Summary of all pending items with their SLA status, sorted by urgency |
| **Urgent override** | Even in digest mode, 4-hour urgent notifications are delivered immediately (not batched) |
| **Breach override** | Breach notifications are always delivered immediately |

Digest mode is off by default. Approvers who handle fewer than 5 active items at a time should not need it.

### Daily Volume Caps

| Notification Type | Maximum per Approver per Day | Behavior at Cap |
|-------------------|------------------------------|-----------------|
| Warning (24h) | 15 | After 15, remaining warnings are held for the next digest or next business morning |
| Urgent (4h) | No cap | Always delivered — these are time-critical |
| Breach | No cap | Always delivered — tied to escalation |

If an approver is consistently hitting the daily cap, that's a signal about workload distribution, not a notification problem. The monthly reporting in #38 will surface this pattern.

---

## 6. Parallel Routing — Independent Countdowns

Parallel routing (Tier 2 and Tier 3 running simultaneously) means two independent SLA clocks and two independent notification streams.

### Rules

1. **Independent timers.** When Tier 1 completes, Tier 2 and Tier 3 each start their own SLA clock. Each clock runs independently per #38.
2. **Independent notifications.** The Tier 2 approver's notifications are based solely on the Tier 2 SLA clock. Same for Tier 3. No cross-pollination.
3. **No cross-branch alerting.** If Tier 3 is approaching deadline, the Tier 2 approver is not notified. Each branch owns its own notification stream.
4. **Requestor sees both.** The requestor dashboard shows both branches with their own countdowns (see Section 4).
5. **Rejection does not cancel notifications.** If Tier 2 rejects while Tier 3 is still active, Tier 3's notifications continue per #38 parallel routing rules. The compliance review still matters.

### Scenario Walkthrough

Request #4521, elevated complexity. Tier 2 SLA: 48 hours. Tier 3 SLA: 48 hours. Both start at 9:00 AM Monday.

| Time | Tier 2 (Budget) | Tier 3 (Compliance) |
|------|-----------------|---------------------|
| Mon 9:00 AM | Clock starts | Clock starts |
| Tue 9:00 AM (24h) | ⚠ Warning notification to budget holder | ⚠ Warning notification to Samir |
| Tue 5:00 PM | Budget holder approves. Tier 2 done. | Clock continues. |
| Wed 5:00 AM (44h) | — | ⚠ Urgent notification to Samir (4h remaining) |
| Wed 9:00 AM (48h) | — | 🔴 Breach. Escalation fires per #38. |

The requestor's dashboard updates at each step. After Tier 2 approves, the requestor sees "Tier 2: Approved ✓ | Tier 3: 🟡 24h remaining."

---

## 7. Quiet Hours and Scheduling

Notifications should arrive when people can act on them, not at 3:00 AM on a Saturday.

### Business Hours Definition

Business hours for notification delivery follow the same definition used by SLA clocks in #37 and #38:
- **Monday through Friday, 8:00 AM to 6:00 PM** in the approver's local time zone
- Weekends and company-observed holidays are excluded

### Quiet Hours Behavior

| Scenario | Behavior |
|----------|----------|
| Warning trigger falls outside business hours | Notification is queued and delivered at the start of the next business window (8:00 AM next business day) |
| Urgent trigger falls outside business hours | Notification is delivered immediately regardless of quiet hours (4 hours is time-critical) |
| Breach trigger falls outside business hours | Notification is delivered immediately (escalation is active) |
| Warning trigger falls on a holiday | Queued for next business day |

### Approver Time Zone Handling

- Each approver's time zone is pulled from their platform profile.
- If no time zone is set, the system defaults to the organization's primary time zone.
- Quiet hours are calculated per approver, not globally. An approver in Pacific time has different quiet hours than one in Eastern time.
- SLA clocks use the organization's business hours, but notification delivery uses the approver's local quiet hours.

### Per-User Quiet Hours Override

Approvers can customize their quiet hours within the preference UI:

| Setting | Options | Default |
|---------|---------|---------|
| Quiet hours start | Configurable (5:00 PM – 11:00 PM) | 6:00 PM |
| Quiet hours end | Configurable (6:00 AM – 10:00 AM) | 8:00 AM |
| Weekend delivery | On / Off | Off |
| Urgent override | Always on (cannot disable) | Always on |

---

## 8. Notification Preference Management UI

Approvers manage their notification settings through a dedicated preferences page in the platform.

### Preferences Page Layout

```
┌─────────────────────────────────────────────────────────┐
│  Notification Preferences                                │
│  ─────────────────────────────────────────────────────── │
│                                                          │
│  CHANNELS                                                │
│  ┌────────────────────────────────────────────────┐      │
│  │ ☑ Email         notifications@company.com      │      │
│  │ ☑ Teams         @approver.name                 │      │
│  │ ☐ In-platform   Always on                      │      │
│  └────────────────────────────────────────────────┘      │
│                                                          │
│  NOTIFICATION TYPES                                      │
│  ┌────────────────────────────────────────────────┐      │
│  │ ☑ Warning (24h before deadline)                │      │
│  │ ☑ Urgent (4h before deadline)                  │      │
│  │ ☐ Breach (always on)                           │      │
│  └────────────────────────────────────────────────┘      │
│                                                          │
│  DELIVERY MODE                                           │
│  ┌────────────────────────────────────────────────┐      │
│  │ ○ Individual notifications (default)           │      │
│  │ ○ Digest mode — Twice daily (9am, 2pm)        │      │
│  │ ○ Digest mode — Once daily (9am)              │      │
│  └────────────────────────────────────────────────┘      │
│                                                          │
│  QUIET HOURS                                             │
│  ┌────────────────────────────────────────────────┐      │
│  │ Start:  [6:00 PM ▾]    End:  [8:00 AM ▾]      │      │
│  │ ☐ Deliver on weekends                          │      │
│  │ Note: Urgent and breach notifications are      │      │
│  │ always delivered immediately.                   │      │
│  └────────────────────────────────────────────────┘      │
│                                                          │
│  [Save Preferences]                                      │
└─────────────────────────────────────────────────────────┘
```

### Default Settings (New Approvers)

New approvers start with sensible defaults. No configuration required to start receiving notifications.

| Setting | Default |
|---------|---------|
| Email | On |
| Teams | On |
| Warning (24h) | On |
| Urgent (4h) | On |
| Breach | Always on |
| Delivery mode | Individual |
| Quiet hours | 6:00 PM – 8:00 AM |
| Weekend delivery | Off |

### Admin Visibility

Michael Bolton (platform admin) can view notification preference summaries across all approvers for troubleshooting:
- Which approvers have disabled email or Teams
- Which approvers are using digest mode
- Delivery failure rates per channel per approver

Michael cannot change individual preferences. He can only view them and reach out to the approver if there's a delivery issue.

---

## 9. Integration with Auto-Escalation

This notification system operates **upstream** of the auto-escalation engine (#38). Notifications are the early warning; escalation is the enforcement.

### Timeline Relationship

For a standard Tier 2 approval (24-hour SLA):

```
SLA Clock:  0h ──────────────── 18h ── 20h ──── 24h ──── 36h ──── 48h
                                 │      │        │        │        │
Notifications (this spec):       │      │        │        │        │
                        Warning ─┘      │        │        │        │
                              Urgent ───┘        │        │        │
                                        Breach ──┘        │        │
                                                 │        │        │
Auto-escalation (#38):                           │        │        │
                              75% warning (18h) ─┘        │        │
                              100% escalation (24h) ──────┘        │
                              150% executive (36h) ────────────────┘
```

**Wait — doesn't #38 already have a 75% warning at 18 hours?**

Yes. The #38 warning at 75% and this spec's 24-hour warning are designed to complement, not duplicate. Here's how they differ:

| Aspect | #38 Escalation Warning (75% SLA) | #39 Deadline Warning (24h before) |
|--------|----------------------------------|----------------------------------|
| Target audience | Approver (internal system nudge) | Approver (user-facing notification) |
| Channel | In-platform + email (fixed) | In-platform + email + Teams (configurable) |
| User can disable | No | Yes (except in-platform) |
| Tone | System-level — "SLA at 75%" | Human-level — "due tomorrow" |
| Content | SLA percentage, tier data | Plain-language deadline, requestor name, link |

For SLA targets where the 75% mark and the 24-hour-remaining mark coincide (like the standard 24-hour SLA, where 75% = 18h and 24h before = 0h start), the system sends the #38 75% warning and this spec's 24-hour warning at different times. For a 24-hour SLA, this spec's "24 hours remaining" fires at the start (0h), and #38's "75% consumed" fires at 18h.

### Handoff at Breach

When the SLA deadline is reached:
1. This spec fires the **breach notification** to the approver and requestor.
2. Simultaneously, #38 fires the **100% escalation** to the approver and their manager.
3. Both events are logged independently in the decision trail.

The approver may receive two notifications at the breach point (one from this spec, one from #38). This is intentional. The breach notification is informational ("the window closed"). The escalation notification is operational ("your manager has been notified, here's what to do").

---

## 10. Testing and Validation Plan

### Test Scenarios

| # | Scenario | Setup | Expected Outcome | Pass Criteria |
|---|----------|-------|-------------------|---------------|
| 1 | Warning fires at 24h before deadline | Submit standard Tier 2 request (24h SLA). Observe at 0h. | Warning notification delivered to approver via all enabled channels. | Notification content correct; delivery to email, Teams, and in-platform. |
| 2 | Urgent fires at 4h before deadline | Submit standard Tier 2 request (24h SLA). Advance clock to 20h. | Urgent notification delivered. Content includes escalation target name. | Notification content correct; "Reassign" and "Request Extension" links functional. |
| 3 | Breach fires at deadline | Submit standard Tier 2 request (24h SLA). Advance clock to 24h. | Breach notification to approver and requestor. #38 escalation fires simultaneously. | Both notifications fire; decision trail logs both; no duplication. |
| 4 | Short SLA skips warning | Submit standard Tier 1 request (4h SLA). | No 24h warning (SLA too short). Urgent fires at start (4h remaining = SLA start). | Only urgent and breach fire; warning correctly skipped. |
| 5 | User disables email | Approver turns off email in preferences. Submit request. | Notifications via Teams and in-platform only. No email. | Email not sent; Teams and in-platform delivered. |
| 6 | User enables digest mode | Approver selects "Twice daily" digest. Submit multiple requests. | Warning notifications batched in 9am/2pm digest. Urgent delivered immediately. | Digest contains all pending warnings; urgent bypasses digest. |
| 7 | Quiet hours — warning queued | Warning triggers at 7:00 PM (after quiet hours start). | Warning queued; delivered at 8:00 AM next business day. | No notification during quiet hours; delivered at next window. |
| 8 | Quiet hours — urgent overrides | Urgent triggers at 10:00 PM. | Urgent delivered immediately despite quiet hours. | Urgent notification delivered; quiet hours bypassed. |
| 9 | Parallel routing — independent countdowns | Submit elevated request. Tier 2 and Tier 3 start simultaneously. Tier 2 approves quickly. | Tier 3 notifications continue independently. Tier 2 approver gets no Tier 3 notifications. | Independence confirmed; no cross-branch alerts. |
| 10 | Requestor countdown display | Submit request. View requestor dashboard at various SLA stages. | Countdown updates correctly; color changes at 24h and 4h thresholds. | Green → Amber → Red transitions correct; parallel branches display independently. |
| 11 | SLA extension resets notifications | Submit request. At 20h, approver requests 12h extension (SLA becomes 36h). | New 24h warning fires at 12h remaining (24h mark from new deadline). Previous warning logged as superseded. | Notification timers recalculated; new warning fires at correct time. |
| 12 | Daily volume cap | Approver has 20 requests all hitting 24h warning in one day. | First 15 warnings delivered immediately. Remaining 5 held for next digest/morning. | Cap enforced; held warnings delivered next business morning. |
| 13 | Teams adaptive card actions | Trigger urgent notification. Click "Open Request" button in Teams card. | Deep link opens the request in the platform. | All action buttons in Teams card are functional. |
| 14 | Requestor privacy | View requestor dashboard during escalation. | Requestor sees "Review window exceeded" but not approver name or escalation details. | Privacy rules enforced; no approver identity or escalation chain visible. |

### Test Environment Requirements

- Same test environment used for #38 testing (shared sandbox)
- Ability to simulate time advancement (fast-forward SLA clocks)
- Test accounts with configurable notification preferences
- Test email mailboxes and Teams test channels for delivery verification
- Requestor test accounts to validate dashboard display

### Test Execution Timeline

| Phase | Duration | Activities |
|-------|----------|------------|
| Environment setup | 0.5 days | Leverage #38 test environment; add notification preference test data |
| Notification delivery testing | 2 days | Scenarios 1–8 (core notification triggers, channels, quiet hours) |
| Parallel routing and countdown | 1 day | Scenarios 9–10 (parallel independence, requestor display) |
| Edge cases and integration | 1 day | Scenarios 11–14 (extensions, caps, Teams cards, privacy) |
| Stakeholder validation | 0.5 days | Peter confirms notifications replace his manual process; Joanna confirms countdown visibility |
| **Total testing** | **5 business days** |

---

## 11. Accessibility and Inclusivity

### Notification Accessibility

| Requirement | Implementation |
|-------------|---------------|
| Screen reader compatibility | All email notifications use semantic HTML. Subject lines are descriptive. Tables use proper headers. |
| Color-blind safe indicators | SLA status uses color + icon + text (not color alone). Green ✓, Amber ⚠, Red 🔴, with text labels always visible. |
| Keyboard navigation | In-platform notifications are focusable and dismissible via keyboard. Teams adaptive cards support keyboard interaction natively. |
| Text sizing | Email templates use relative font sizes. No fixed-pixel fonts below 14px equivalent. |
| Reduced motion | The "subtle pulse" on urgent countdown display respects the user's prefers-reduced-motion OS setting. No pulse if motion is reduced. |

### Language and Tone

- Notifications use plain, non-technical language. "Due in 4 hours" not "SLA at 83.3% consumption."
- No blame, urgency theater, or guilt. "This needs attention soon" not "OVERDUE — IMMEDIATE ACTION REQUIRED."
- Consistent voice across all notification types. The system sounds like a helpful teammate.

### Inclusivity Considerations

- Notification preferences are individual. The system does not assume that all approvers work the same hours, use the same tools, or process information the same way.
- Quiet hours default to standard business hours but are fully customizable for approvers working non-standard schedules.
- Digest mode exists for approvers who are overwhelmed by individual notifications. The system adapts to the person, not the other way around.
- The requestor dashboard uses universal iconography (checkmarks, warning triangles, circles) alongside text. Nothing relies solely on cultural or language-specific symbols.

---

## Implementation Sequence

| Order | Dependency | Activity |
|-------|-----------|----------|
| 1 | #38 (auto-escalation config) | Auto-escalation must be configured first — notification triggers depend on SLA clock data from #38 |
| 2 | #37 (SLA thresholds) | SLA targets determine when notifications fire |
| 3 | This spec (#39) | Michael configures notification templates, channel integrations, and preference UI |
| 4 | Testing plan above | Execute all 14 test scenarios in test environment |
| 5 | Stakeholder validation | Peter, Joanna, and Samir confirm the system meets their needs |
| 6 | Production deployment | Deploy alongside or after #38 production deployment |
| 7 | 1-week observation period | Monitor notification volumes, delivery rates, and user feedback before declaring stable |

---

## Stakeholder Impact Summary

| Stakeholder | Before | After |
|-------------|--------|-------|
| **Peter Gibbons** | Manually emails approvers when requests are stuck. No system support. | Automated notifications replace his follow-up emails. He can focus on other work. |
| **Joanna** | Had no idea her vendor contract was sitting idle for 11 days. Zero visibility. | SLA countdown on her requestor dashboard shows exactly where her request is and when it will move. |
| **Approvers (Samir, budget holders)** | No warning before deadlines. Escalation comes as a surprise. | 24-hour and 4-hour heads up, configurable channels, digest option for high-volume approvers. |
| **Bill Lumbergh** | Gets escalations after the fact with no warning trail. | Escalation chain has a clear paper trail. Breach notifications document the timeline. |
| **Michael Bolton** | No notification infrastructure to manage. | Configures templates and monitors delivery health. Preference admin view for troubleshooting. |
