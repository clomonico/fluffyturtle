# Quarterly Process Review Cadence

**Issue:** #42 — Establish quarterly process review cadence
**Initiative:** 02 — Glass Floor (Process Transparency)
**Epic:** 14 — Process Analytics
**Depends on:** #41 (Analytics Dashboard)
**Coordinates with:** #18 (Cross-Functional Process Governance, Initiative 3)
**Author:** Parker (Stakeholder Analyst)
**Date:** 2026-03-02

---

## 1. Purpose and Charter

The approval process grew through accumulated patches, informal gates, and individual risk aversion without intentional design. Tom's temporary fixes became permanent because nobody reviewed them. Lumbergh's "loop me in" became a gate nobody recognized as a gate. Samir added secondary review cycles that policy never required. Every stakeholder optimized independently for their own definition of "working" (Theme 6 from synthesis), and no feedback loop existed to catch drift or measure improvement.

The Quarterly Process Review exists to replace ad-hoc process additions with deliberate governance. Its charter:

- **Review** analytics dashboard metrics each quarter to understand actual process performance
- **Identify** bottlenecks, redundancies, and compliance gaps using data rather than intuition
- **Propose and evaluate** process changes through a structured improvement pipeline
- **Decide** which changes to implement, defer, or reject with documented rationale
- **Set targets** for the next quarter and track progress against prior targets
- **Enforce** that no process changes are made outside this review cycle without emergency authorization

This replaces the current state where anyone can add steps, gates, or workarounds without review and without sunset dates.

---

## 2. Review Committee Composition

Cross-functional representation is essential. The committee must include perspectives from operations, approvers, requestors, compliance, and institutional knowledge holders.

### Standing Members

| Role | Representative | Why They're Here |
|---|---|---|
| Executive Sponsor | Bill Lumbergh | Owns the budget and the mandate. Needs visible, measurable improvement. |
| Operations Lead | Peter Gibbons | Routes approvals daily. Knows every workaround. Represents the "real" process. |
| Finance Approver | Samir Nagheenanajar | Mid-level approver. Represents the approver experience and audit safety concerns. |
| Department Manager (Requestor) | Joanna | Submits requests on behalf of her team. Represents the "customer" of the process. |
| Compliance / Audit | The Bobs | Own the control matrix. Ensure regulatory requirements are met. |
| IT Systems Lead | Michael Bolton | Maintains tooling. Confirms technical feasibility of proposed changes. |

### Advisory Members (invited as needed)

| Role | Representative | When to Invite |
|---|---|---|
| Process Historian | Milton Waddams | Any discussion involving step removal, exception history, or vendor-related changes. Milton holds 47 vendor exception records and knows why steps were originally added. |
| Process Owner (Legacy) | Tom Smykowski | Discussions about legacy steps he authored. His resistance is valuable data about hidden dependencies. |

### Quorum

A meeting requires at least 4 standing members present, including at minimum one representative from each of these groups:
- Executive or Operations (Lumbergh or Peter)
- Approver or Requestor (Samir or Joanna)
- Compliance (The Bobs)

Decisions made without quorum are provisional and must be ratified at the next quorum meeting.

---

## 3. Standard Meeting Agenda Template

**Duration:** 90 minutes
**Cadence:** Quarterly (first Tuesday of Q2, Q3, Q4, Q1)
**Location:** Recorded video conference with shared screen on analytics dashboard

| Time | Section | Owner | Purpose |
|---|---|---|---|
| 0:00–0:05 | Opening and quorum check | Facilitator | Confirm attendance, declare quorum, state agenda |
| 0:05–0:25 | Metrics review | Operations Lead | Walk through analytics dashboard: cycle time, bottleneck ranking, rejection rate, escalation frequency. Compare to prior quarter targets. |
| 0:25–0:40 | Bottleneck analysis | Full committee | Discuss top 3 bottlenecks by data. Identify root causes. Distinguish systemic issues from one-time events. |
| 0:40–0:55 | Improvement proposals | Proposers | Present queued proposals (submitted in advance). Committee evaluates using criteria in Section 5. |
| 0:55–1:10 | Decision round | Full committee | Vote on proposals. Record decisions in decision log. Assign owners and timelines. |
| 1:10–1:20 | Target setting | Full committee | Set 2-3 measurable targets for next quarter based on current data and approved changes. |
| 1:20–1:25 | Urgent items and escalations | Facilitator | Review any emergency changes made between reviews. Ratify or reverse. |
| 1:25–1:30 | Wrap-up and action items | Facilitator | Summarize decisions, confirm owners, announce next meeting date. |

### Pre-Meeting Requirements

- Analytics dashboard data exported and circulated 5 business days before the meeting
- Any improvement proposals submitted through the proposal process (Section 5) at least 7 business days before
- Prior quarter decision log and target scorecard circulated with the agenda

---

## 4. Metrics Review Framework

All metrics are sourced from the Analytics Dashboard (#41). The quarterly review focuses on four primary metrics with defined thresholds that trigger committee discussion.

### Primary Metrics

| Metric | Definition | Green | Yellow (Discuss) | Red (Action Required) |
|---|---|---|---|---|
| **Cycle Time** | Median days from request submission to final approval | ≤ target set by committee | 10-25% above target | > 25% above target |
| **Bottleneck Concentration** | % of total wait time attributable to the single slowest step | < 30% | 30-50% | > 50% |
| **Rejection Rate** | % of submissions rejected or returned for rework | < 15% | 15-25% | > 25% |
| **Escalation Frequency** | % of requests requiring SLA breach escalation | < 5% | 5-10% | > 10% |

### Secondary Metrics (reviewed but no automatic trigger)

- **Workaround frequency:** Number of requests processed outside the standard system (shadow processes)
- **Documentation completeness:** % of approved requests with all required fields completed at submission
- **Tier utilization:** Volume distribution across the 3 mandatory and remaining elective tiers
- **Repeat request auto-approval rate:** % of eligible repeat requests processed without manual review

### Threshold Review

Thresholds themselves are reviewed annually. The committee can adjust thresholds at any quarterly review by majority vote, but changes must be documented in the decision log with rationale.

### Trend Analysis

Quarter-over-quarter trends matter more than single-quarter snapshots. A metric in "Green" that has worsened three consecutive quarters should be flagged for discussion even though it hasn't crossed a threshold. The analytics dashboard comparison mode (#41) supports this view.

---

## 5. Improvement Proposal Process

### Who Can Propose

Any employee can submit an improvement proposal. Proposals are not limited to committee members. This ensures that people closest to the work (approvers, requestors, administrators) can surface issues and ideas.

### Proposal Template

Each proposal must include:

| Field | Description |
|---|---|
| **Title** | Short description of the proposed change |
| **Proposer** | Name, role, contact |
| **Problem statement** | What is broken or suboptimal? Reference metrics or specific incidents. |
| **Proposed change** | What specifically would change in the process? |
| **Expected impact** | Which metrics should improve, and by how much? |
| **Risk assessment** | What could go wrong? Compliance implications? |
| **Affected stakeholders** | Who is impacted by this change? |
| **Reversibility** | Can this change be undone if it doesn't work? How? |
| **Evidence** | Data, interview quotes, incident reports supporting the proposal |

### Evaluation Criteria

The committee evaluates proposals against five criteria, each scored 1-5:

1. **Data support:** Is there measurable evidence that the problem exists? (Weight: 25%)
2. **Impact scope:** How many requests, users, or steps are affected? (Weight: 25%)
3. **Compliance safety:** Does this maintain or improve regulatory compliance? (Weight: 20%)
4. **Reversibility:** Can we undo this if it doesn't work? (Weight: 15%)
5. **Implementation effort:** How much work is required relative to expected benefit? (Weight: 15%)

Proposals scoring above 3.5 weighted average are approved. Proposals scoring 2.5-3.5 are deferred for more data. Proposals below 2.5 are rejected with documented rationale.

### Approval Outcomes

| Outcome | What Happens |
|---|---|
| **Approved** | Assigned an owner, timeline, and success metric. Tracked in decision log. |
| **Deferred** | Returned to proposer with specific questions or data requests. Re-evaluated next quarter. |
| **Rejected** | Documented in decision log with rationale. Proposer may resubmit with new evidence. |
| **Approved (pilot)** | Implemented for a subset of request types or departments. Full rollout contingent on pilot results. |

---

## 6. Change Management Gates

This is the mechanism that prevents the problem from recurring. Tom's temporary patches became permanent because nothing forced a review. Lumbergh's "loop me in" became a gate because nobody had authority to question it. These gates ensure that stops happening.

### Rule: No Process Changes Without Committee Review

Any modification to the approval process must go through the quarterly review. This includes:

- Adding, removing, or modifying approval steps
- Changing routing rules or approval thresholds
- Adding new required fields or documentation
- Creating new approval tiers or modifying existing tier criteria
- Introducing new tools, integrations, or shadow systems
- Modifying SLA targets or escalation rules

### Emergency Change Process

For urgent issues that cannot wait for the next quarterly review:

1. **Requestor** submits an emergency change request to any two standing committee members
2. **Two committee members** must agree the change qualifies as urgent (criteria: active compliance violation, system outage preventing approvals, or regulatory deadline within 30 days)
3. **Change is implemented** with a mandatory 90-day sunset clause
4. **Committee is notified** within 2 business days via email with full documentation
5. **Next quarterly review** must ratify, modify, or reverse the emergency change

Emergency changes that are not ratified at the next review are automatically reversed.

### Sunset Clause for All Changes

Every approved process change includes a review date (default: 4 quarters after implementation). At the review date, the committee evaluates whether the change achieved its intended impact. Changes that did not meet targets are modified or removed. This prevents the accumulation of well-intentioned patches that outlive their usefulness.

### Shadow Process Detection

The analytics dashboard tracks workaround frequency. Any quarter where shadow process volume exceeds 5% of total request volume triggers a mandatory agenda item to investigate root causes and determine whether the shadow process should be absorbed into the official workflow or the underlying friction should be addressed.

---

## 7. Decision Log and Tracking

### Decision Log Format

Every quarterly review produces a decision log entry:

```
Decision ID: QR-YYYY-QN-###
Date: YYYY-MM-DD
Proposal: [Title]
Decision: Approved / Deferred / Rejected / Approved (pilot)
Vote: For-Against-Abstain
Rationale: [2-3 sentences]
Owner: [Name]
Target Completion: [Date]
Success Metric: [What will be measured]
Review Date: [When this will be re-evaluated]
Status: Open / In Progress / Complete / Reversed
```

### Tracking Mechanism

- All decisions are stored in a shared, version-controlled decision log
- Open decisions are reviewed at the start of each quarterly meeting (agenda Section: Metrics Review includes prior target scorecard)
- Completed decisions have their actual impact recorded alongside the predicted impact
- The decision log is the single source of truth for "why does this step exist?" — replacing the current state where rationale lives in Tom's memory or Milton's spreadsheet

### Cumulative Scorecard

Each quarter, the committee maintains a running scorecard:

| Quarter | Targets Set | Targets Met | Targets Missed | Changes Approved | Changes Reversed | Emergency Changes |
|---|---|---|---|---|---|---|
| Q2 2026 | — | — | — | — | — | — |
| Q3 2026 | | | | | | |

This scorecard becomes the evidence base for whether the review process itself is working.

---

## 8. Quarterly Target-Setting Methodology

### How Targets Are Set

1. **Baseline from data:** Use the analytics dashboard to establish current performance for each primary metric
2. **Identify the biggest gap:** Which metric is furthest from acceptable? That gets the stretch target.
3. **Set 2-3 targets per quarter:** More than 3 dilutes focus. Fewer than 2 risks complacency.
4. **Each target must be specific and measurable:** "Reduce median cycle time from 12 days to 9 days" not "Improve speed"
5. **Each target must have an owner:** Someone on the committee is accountable for driving the improvement
6. **Each target must have a leading indicator:** Something measurable within the first month that suggests the target is on track

### Target Categories

| Category | Example | Tracked By |
|---|---|---|
| Cycle time reduction | Reduce median cycle time for requests under $10K from X to Y days | Operations Lead |
| Bottleneck relief | Reduce wait time at [specific step] by Z% | Step owner |
| Quality improvement | Increase first-submission acceptance rate from X% to Y% | Requestor representative |
| Compliance strengthening | Reduce shadow process volume from X% to Y% | Compliance |

### What Happens When Targets Are Missed

Missing a target is not a failure. It is data. When a target is missed:

1. The owner presents what was attempted and what happened
2. The committee discusses whether the target was realistic, the approach was wrong, or external factors intervened
3. The target is either adjusted, the approach is changed, or the target is retired with documented reasoning
4. No blame. The point is learning, not punishment. If people are punished for missed targets, they will set easy targets.

---

## 9. First Quarterly Review Plan

The first review is the validation milestone. It establishes baselines, tests the process, and sets the committee's working rhythm.

### Pre-Meeting (2 weeks before)

- [ ] Analytics dashboard (#41) is live and populated with at least 1 full quarter of data
- [ ] Committee members confirmed and calendar invitations sent
- [ ] Baseline metrics report generated from dashboard
- [ ] 2-3 pre-seeded improvement proposals prepared (drawn from known issues in synthesis)
- [ ] Decision log template created and shared
- [ ] Meeting recording and note-taking logistics confirmed

### First Meeting Agenda (Modified)

| Time | Section | Detail |
|---|---|---|
| 0:00–0:10 | Charter and ground rules | Review this document. Confirm committee roles. Establish decision-making norms (majority vote, quorum rules). |
| 0:10–0:30 | Baseline metrics walkthrough | First full review of the analytics dashboard. Establish current state for all primary and secondary metrics. No judgments yet, just measurement. |
| 0:30–0:45 | Known issues review | Walk through the top issues identified in the design thinking synthesis. Map them to current metric data. Which ones are confirmed by the numbers? |
| 0:45–1:00 | Pre-seeded proposals | Evaluate 2-3 proposals drawn from well-evidenced issues. Practice the evaluation process. |
| 1:00–1:15 | Initial target setting | Set 2-3 targets for Q3 2026 based on baseline data. Keep them achievable — the goal is to build the habit, not to transform the process in one quarter. |
| 1:15–1:25 | Emergency process review | Confirm the emergency change process. Ensure everyone knows how to raise urgent issues between reviews. |
| 1:25–1:30 | Retrospective and next steps | How did this meeting format work? What should we adjust for next quarter? |

### Suggested Pre-Seeded Proposals for First Review

These are drawn from validated research findings and are strong candidates for the committee's first decisions:

1. **Eliminate Samir's non-mandated secondary review for requests under $10K.** (Samir acknowledged this isn't policy-required. The Bobs' control matrix confirms only 3 of 7 tiers are regulatory.)

2. **Replace Lumbergh's "loop me in" gate with dashboard visibility.** (Lumbergh doesn't need to approve; he needs to see. The executive dashboard (#28) provides this without adding cycle time.)

3. **Set a formal sunset review for Tom's two admitted legacy patches.** (Tom acknowledged two steps are unreviewed temporary additions. The committee should evaluate whether they still serve a purpose.)

### Success Criteria for the First Review

The first review is successful if:

- [ ] Quorum was achieved
- [ ] Baseline metrics are documented and shared
- [ ] At least 2 targets are set for Q3 2026
- [ ] At least 1 proposal was evaluated using the standard process
- [ ] The decision log has its first entries
- [ ] The committee agreed on the next meeting date

---

## 10. Escalation Path for Urgent Issues Between Reviews

Not everything can wait 90 days. The escalation path provides a pressure valve while maintaining the discipline of quarterly governance.

### Severity Levels

| Level | Criteria | Response |
|---|---|---|
| **Critical** | Active compliance violation, system outage blocking all approvals, regulatory deadline within 30 days | Emergency change process (Section 6). Two committee members authorize. 90-day sunset. |
| **High** | Single bottleneck causing SLA breach rate above 20%, shadow process volume spike above 10% | Ad-hoc committee call within 5 business days. Minimum 3 standing members. |
| **Medium** | Metric crosses "Red" threshold but no immediate compliance or operational risk | Added to next quarterly review agenda as priority item. No interim action. |
| **Low** | Improvement idea, minor friction, feature request | Submit through standard proposal process for next quarterly review. |

### Escalation Contacts

- **Critical:** Any two standing committee members (fastest available)
- **High:** Operations Lead (Peter) or Executive Sponsor (Lumbergh)
- **Medium/Low:** Submitted to shared proposal queue

### Between-Review Communication

A brief monthly status email (no meeting) is sent by the Operations Lead summarizing:
- Current metric status (Green/Yellow/Red for each primary metric)
- Any emergency changes made since last review
- Count of proposals in the queue for the next review
- Progress against quarterly targets

This keeps the committee informed without adding meeting overhead.

---

## 11. Coordination with Initiative 3 — Cross-Functional Process Governance (#18)

Initiative 3 establishes permanent cross-functional governance for the approval process. This quarterly review cadence is one component of that broader structure. Coordination points:

### Relationship

- **This cadence (Issue #42)** defines the meeting structure, agenda, metrics framework, and change management gates for ongoing process review
- **Cross-Functional Process Governance (#18)** defines the broader governance model including the committee's charter, authority boundaries, escalation to executive leadership, and relationship to other organizational governance bodies

### Shared Elements

- The review committee composition defined here should align with the governance body defined in #18
- The decision log defined here becomes a primary input to the governance body's oversight reporting
- Change management gates defined here operate within the authority boundaries defined in #18

### Handoff Points

- Once #18 governance model is established, the quarterly review cadence may need to adjust its authority boundaries, quorum rules, or escalation paths to fit within the broader governance framework
- Any conflicts between this cadence and the #18 governance model are resolved in favor of #18 as the higher-level authority
- The committee defined here may evolve into a subcommittee of the #18 governance body

### Sequencing

This cadence can begin operating before #18 is fully established. The first quarterly review should proceed as defined here. When #18 governance is in place, the committee reviews this document and adjusts as needed to align.

---

## Appendix: Design Thinking Traceability

| Element | DT Source |
|---|---|
| No-change-without-review gates | Theme 1 (process is longer than it needs to be — because nobody questioned additions) |
| Cross-functional committee | Theme 6 (everyone solving for different success metrics) |
| Decision log replacing personal memory | Theme 5 (institutional knowledge lives in people, not the process) |
| Sunset clauses | Tom's temporary patches that became permanent (Theme 1, Tom interview) |
| Shadow process detection | Theme 2 (the "real" process isn't the official process) |
| Metrics-driven review | Theme 3 (approvers duplicate work due to no visibility — extends to no process-level visibility) |
| Emergency escalation path | Synthesis contradiction: need for governance discipline vs. operational reality that urgent issues arise |
