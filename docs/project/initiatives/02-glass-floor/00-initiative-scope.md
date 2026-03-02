# Initiative 2: Glass Floor — Make Approvals Transparent and Parallel

**Date:** 2026-03-02  
**Status:** Scoped  
**Speed to Impact:** Fast (weeks)  
**Organizational Risk:** Low (uses existing platform features)  
**Primary HMW Addressed:** HMW 3 — Give approvers evidence of what upstream already verified

---

## Executive Summary

Glass Floor solves two compounding problems in the approval process: approvers duplicating upstream work because they can't see what was already verified, and requests sitting idle because steps run sequentially when they don't need to.

Every approval step will record what was verified, by whom, and when, creating a verification chain visible to downstream approvers. Independent approval steps will run in parallel instead of sequentially. Requests that stall past 48 hours will auto-escalate. And for the first time, the organization will have actual data on how the process performs.

All of this uses features already built into the current platform but never configured. Michael Bolton confirmed the platform supports parallel routing, conditional branching, and delegation. None of it is turned on.

The motivating cases from research:
- Samir said, "If I could see that the budget line was verified at step 2 with a timestamp and a name, I wouldn't need to re-do it at step 5"
- Joanna's vendor contract sat idle for 11 days until Lumbergh personally walked it to Samir's desk
- The Bobs flagged that 60% of approved requests have incomplete documentation because approvers click "approve" without recording what they checked
- Sequential routing is the single largest contributor to elapsed time

---

## Epics and Stories

### Epic 11: Verification Chain and Decision Logging

Add verification receipts and a living decision log so downstream approvers see upstream evidence. Eliminates redundant re-verification across the 3 mandatory tiers.

| Story | Title | Description |
|---|---|---|
| #31 | Verification receipt fields | Add structured verification fields to each mandatory tier so approvers record what they checked, with timestamp and approver name |
| #32 | Living decision log | Create a running log visible to all approvers showing the full verification chain for each request |
| #33 | Downstream visibility configuration | Configure downstream approver views so Tier 2 (Samir) sees Tier 1 evidence and Tier 3 sees both Tier 1 and Tier 2 evidence |

**Research anchor:** Samir's re-verification problem. He re-checks budget lines at Tier 2 because he can't see what Tier 1 already confirmed. The Bobs found 60% of approvals lack documentation of what was actually reviewed.

---

### Epic 12: Parallel Routing Configuration

Enable parallel routing using existing platform capabilities so independent approvers work simultaneously instead of sequentially.

| Story | Title | Description |
|---|---|---|
| #34 | Technical feasibility spike | Validate that parallel routing, conditional branching, and delegation features work as expected in the current platform. Identify failure modes and configuration risks |
| #35 | Parallel routing configuration | Configure parallel routing for tiers that can run simultaneously after the post-Init 1 approval chain is finalized |
| #36 | Routing fallback and error handling | Define fallback behavior when parallel routes conflict or the platform encounters configuration errors |

**Research anchor:** Michael Bolton confirmed the platform supports parallel routing but flagged it as "brittle." The platform's dormant features have never been exercised in production. Sequential routing is the single largest contributor to elapsed time.

**Hard dependency:** Story #34 (spike) must complete before Stories #35 and #36 can begin. The spike outcome may reshape the scope of this entire epic.

---

### Epic 13: Auto-Escalation and SLA Enforcement

Configure auto-escalation after 48 hours and approaching-deadline notifications so requests never sit idle without visibility.

| Story | Title | Description |
|---|---|---|
| #37 | SLA threshold configuration | Define and configure SLA thresholds per tier. 48-hour default with configurable overrides for specific request types |
| #38 | Auto-escalation rules | Configure auto-escalation paths when SLA thresholds are breached. Define who gets notified and what happens to the request |
| #39 | Approaching-deadline notifications | Configure advance warning notifications (e.g., 24 hours before SLA breach) so approvers can act before escalation triggers |

**Research anchor:** Joanna's vendor contract sat for 11 days with no visibility into where it was stuck or who was responsible. The platform supports auto-escalation after SLA thresholds but it's never been configured.

---

### Epic 14: Process Analytics Feedback Loop

Instrument data collection, build a cycle time dashboard, and establish a quarterly review cadence. Nobody has real data on process performance today.

| Story | Title | Description |
|---|---|---|
| #40 | Approval process instrumentation | Configure data collection for cycle time, time-per-tier, escalation frequency, and verification completeness |
| #41 | Cycle time dashboard | Build a dashboard showing end-to-end cycle time, time spent at each tier, escalation rates, and SLA compliance |
| #42 | Quarterly review cadence | Establish a recurring review process where stakeholders assess process performance data and decide on adjustments |

**Research anchor:** When asked for data on process performance, nobody had any. Every pain point was described anecdotally. Without instrumentation, the organization can't distinguish real bottlenecks from perceived ones and can't measure whether changes actually helped.

---

## Dependencies on Initiative 1 (Clear the Path)

Initiative 2 operates on the approval chain that Initiative 1 produces. Several dependencies are structural.

| Dependency | Why It Matters | Affected Epics |
|---|---|---|
| Tier classification complete (Init 1, Epic 7) | Verification fields are added to the 3 mandatory tiers only. We need to know which tiers survive | Epic 11 |
| Elective tier removal complete (Init 1, Epic 8) | Parallel routing is configured on the post-reduction chain. Routing a 7-tier chain vs. a 3-tier chain is a different problem | Epic 12 |
| Intake validation live (Init 1, Epic 9) | Verification receipts depend on complete intake data. If requests enter incomplete, verification fields are meaningless | Epic 11 |
| Dashboard deployed (Init 1, Epic 10) | Process analytics (Epic 14) extends the dashboard built in Init 1 rather than creating a separate one | Epic 14 |

**Canonical tier numbering after Init 1:**
- Tier 1: Requestor Verification (Dept Head) — mandatory
- Tier 2: Budget Authority Confirmation (Samir) — mandatory
- Tier 3: Compliance / COI Check (includes Milton's vendor terms) — mandatory
- Tiers 4 through 7: removed by Initiative 1

---

## Key Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Platform fragility | High | High | The feasibility spike (Story #34) is a gate. If parallel routing destabilizes the platform, Epic 12 scope is reduced or deferred. No configuration without spike results |
| Samir's behavior change | Medium | Medium | Verification chain visibility reduces the need to re-check, but Samir may continue re-verifying out of habit. Needs change management support and explicit sign-off that upstream verification is sufficient |
| Spike discovers blocking issues | Medium | High | If the platform can't reliably support parallel routing, Epic 12 pivots to sequential optimization (reducing time-per-step instead of running steps simultaneously). The other 3 epics are unaffected |
| Incomplete Init 1 delivery | Medium | High | Epics 11 and 12 depend on Init 1 tier reductions. If Init 1 is delayed, Glass Floor can begin with Epic 13 (auto-escalation) and Epic 14 (analytics) which have no tier structure dependencies |
| Approver resistance to transparency | Low | Medium | Some approvers may resist having their verification activity logged. Frame as protection (audit trail shows they did the work) rather than surveillance |

---

## Success Metrics

| Metric | Baseline | Target | Measurement |
|---|---|---|---|
| End-to-end approval cycle time | Unknown (no data exists) | 50% reduction from first measured baseline | Cycle time dashboard (Story #41) |
| Redundant re-verification rate | Anecdotal (Samir's description) | Near zero for data verified at upstream tiers | Verification log analysis |
| Requests idle > 48 hours | Unknown (Joanna's 11-day case is the benchmark) | Zero (auto-escalation catches all) | SLA compliance dashboard |
| Verification completeness | 40% (Bobs' finding: 60% incomplete) | 95% of approvals include structured verification | Instrumentation data (Story #40) |
| Stakeholder satisfaction | Anecdotal frustration | Positive trend over 2 quarterly reviews | Quarterly review feedback (Story #42) |

---

## Timeline Estimate

Glass Floor is rated "Fast (weeks)" in the implementation options comparison, with low organizational risk because it uses existing platform features.

| Phase | Duration | Activities |
|---|---|---|
| Phase 1: Spike and planning | 1 to 2 weeks | Technical feasibility spike (#34). Verification field design (#31). SLA threshold definition (#37) |
| Phase 2: Core configuration | 2 to 3 weeks | Verification receipts (#31, #32, #33). Auto-escalation (#38, #39). Instrumentation (#40). Parallel routing if spike clears (#35, #36) |
| Phase 3: Dashboard and rollout | 1 to 2 weeks | Cycle time dashboard (#41). Validation with live requests. Stakeholder training |
| Phase 4: Steady state | Ongoing | Quarterly review cadence (#42). Metric tracking. Continuous adjustment |

**Total estimated delivery:** 4 to 7 weeks after Init 1 dependencies are met.

**Note:** Phase 1 can begin in parallel with Init 1's later stages. The spike and SLA threshold design don't depend on tier reductions being complete.

---

## Stakeholder Map

| Stakeholder | Role in Init 2 | Key Concern |
|---|---|---|
| Samir Nagheenanajar | Primary beneficiary of verification chain. Budget Authority at Tier 2 | Wants to stop re-checking what upstream already verified. Needs to trust upstream evidence |
| Michael Bolton | Platform configuration owner. Executes the spike and all feature enablement | Worried about platform stability. Needs clear rollback procedures and staging environment |
| Joanna | Requestor and SLA case study. Validates that auto-escalation solves idle-request problem | Wants to know where her requests are and that they won't sit idle for days |
| The Bobs | Compliance stakeholders. Must approve that verification logging meets audit requirements | Want structured evidence that approvers are doing real review, not just clicking approve |
| Bill Lumbergh | Receives executive alerts from Init 1 dashboard. Visibility into SLA breaches | Transition from gatekeeper to observer. Auto-escalation reduces need for manual intervention |
| Milton Waddams | Vendor terms included in Tier 3 compliance check. Verification fields capture his domain | Institutional knowledge of vendor exceptions must be codified into verification requirements |
| Tom Smykowski | Process documentation owner. Documents the new verification and routing procedures | Needs clarity on what changed so documentation stays current |
| Peter Gibbons | End user and requestor. Benefits from faster cycle times and transparency | Wants approval status visibility and faster turnaround |

---

## Cross-Initiative Coordination

**Initiative 1 → Initiative 2 handoffs:**
- When Init 1 finalizes the 3-tier mandatory chain, Init 2 begins verification field design for those specific tiers
- When Init 1 completes tier removal, Init 2 configures parallel routing on the reduced chain
- The Lumbergh dashboard from Init 1 (Epic 10) becomes the foundation for the cycle time dashboard in Init 2 (Epic 14)

**Sequencing:**
- Init 2 Phase 1 (spike + planning) can run in parallel with Init 1's later stages
- Init 2 Phases 2 and 3 require Init 1 tier reductions to be complete
- Epic 13 (auto-escalation) and Epic 14 (analytics) have lighter Init 1 dependencies and can start earlier if needed

**Shared risks:**
- If Init 1 tier reductions are contested or delayed, Init 2's scope narrows to what can be configured on the current 7-tier chain (less value, more complexity)
- Platform fragility is a shared concern. Michael is the bottleneck for both initiatives' platform work
