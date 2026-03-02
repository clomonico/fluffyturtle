# Parallel Routing Feasibility Spike Plan

**Issue:** #34
**Initiative:** Glass Floor (Init 2)
**Epic:** 12 — Parallel Routing
**Owner:** Michael Bolton (platform lead)
**Spike Lead:** Ripley (squad lead)
**Duration:** 1 week (5 business days), time-boxed
**Start:** Immediately upon approval — no blockers

---

## 1. Objectives and success criteria

### Objectives

- Validate that the platform's parallel routing feature works reliably for splitting Tier 2 (Budget Authority Confirmation) and Tier 3 (Compliance / COI Check) into simultaneous paths
- Identify configuration requirements, limitations, and failure modes before committing to full Epic 12 implementation
- Produce a go/no-go recommendation backed by test evidence

### Success criteria

| Criterion | Measure |
|---|---|
| Parallel send confirmed | Two approvers receive the same request within 60 seconds of each other |
| Merge logic validated | Request advances only after both parallel paths complete |
| Conflict handling documented | System behavior when one approver rejects while the other approves is predictable and repeatable |
| Performance acceptable | Parallel route completes in less time than sequential route for the same request type |
| Audit trail intact | Every action on both parallel paths appears in the request history with correct timestamps |
| Configuration documented | Step-by-step setup that a second person could follow without Michael |

---

## 2. Test scenarios

### 2.1 Happy path

| # | Scenario | Expected result |
|---|---|---|
| H1 | Submit request; Tier 2 and Tier 3 receive simultaneously | Both approvers see the request within 60 seconds |
| H2 | Both approvers approve | Request advances to next stage (or completes) |
| H3 | Approver A approves first, Approver B approves second | Order doesn't matter; request advances after both complete |
| H4 | Approver B approves first, Approver A approves second | Same result as H3 |

### 2.2 Conflict scenarios

| # | Scenario | Expected result |
|---|---|---|
| C1 | Tier 2 approves, Tier 3 rejects | Request should follow rejection path — document what the platform actually does |
| C2 | Tier 3 approves, Tier 2 rejects | Same expectation as C1; confirm symmetry |
| C3 | Both reject simultaneously | Request rejected; both rejection reasons captured |
| C4 | Approver A rejects while Approver B's review is still open | Does the platform recall/cancel the pending approval? Or does B still see it? |

### 2.3 Edge cases

| # | Scenario | Expected result |
|---|---|---|
| E1 | One approver is out of office / never responds | Timeout behavior documented; does the other path stall? |
| E2 | Approver delegates to someone else mid-review | Delegation works within a parallel branch without breaking the merge |
| E3 | Request is recalled/cancelled by requestor while both paths are active | Both pending approvals cancelled cleanly |
| E4 | Same person assigned to both parallel paths | Platform rejects or handles as single approval (document actual behavior) |
| E5 | Approver reopens/changes decision after initial response | Does it re-trigger the merge logic? |
| E6 | Network interruption during parallel send | Both tasks still created when connectivity returns |

### 2.4 Volume and stress

| # | Scenario | Expected result |
|---|---|---|
| V1 | 10 requests enter parallel routing simultaneously | All 10 route correctly with no cross-contamination |
| V2 | 50 requests in parallel routing at once | Platform remains responsive; no queue backlog |

---

## 3. Sandbox environment setup

### Requirements

| Item | Detail |
|---|---|
| Environment | Sandbox or staging instance of the current approval platform |
| Test accounts | Minimum 4: 1 requestor, 2 parallel approvers, 1 admin/observer |
| Parallel routing feature | Enabled (Michael confirms this is available but unconfigured) |
| Test workflow | Clone of production workflow with Tier 2 and Tier 3 configured as parallel branches |
| Data | Synthetic test requests across 3 request types (procurement, vendor onboarding, access) |
| Monitoring | Audit log access, platform admin console, email/notification logs |
| Rollback plan | Sandbox only — no production impact; reset workflow to sequential if needed |

### Setup steps

1. Michael provisions sandbox access for all test participants
2. Michael clones current sequential workflow into sandbox
3. Configure Tier 2 and Tier 3 as parallel branches with AND-merge (both must complete)
4. Create test request templates with varying dollar values
5. Validate that audit logging captures parallel branch events
6. Confirm notification delivery to both parallel approvers

---

## 4. Day-by-day schedule

| Day | Focus | Activities | Deliverable |
|---|---|---|---|
| **Day 1 (Mon)** | Setup and configuration | Sandbox provisioned. Michael configures parallel workflow. Team walk-through of test plan. | Working parallel workflow in sandbox |
| **Day 2 (Tue)** | Happy path testing | Run scenarios H1–H4. Verify notifications, timing, merge behavior. | Happy path test results logged |
| **Day 3 (Wed)** | Conflict and edge case testing | Run scenarios C1–C4, E1–E6. Document actual vs. expected behavior for each. | Conflict/edge case findings documented |
| **Day 4 (Thu)** | Volume testing and performance | Run scenarios V1–V2. Measure response times. Compare parallel vs. sequential elapsed time. | Performance comparison data |
| **Day 5 (Fri)** | Analysis and recommendation | Compile findings. Draft go/no-go recommendation. Review with Michael. | Final spike report with go/no-go decision |

### Time-box enforcement

- If Day 1 setup fails (can't configure parallel routing at all), spike ends with a "no-go — platform limitation" finding
- No scope creep: this spike tests only Tier 2 / Tier 3 parallelization, not conditional branching or other features
- If critical blockers appear mid-week, document them and shift to alternative assessment (see Section 10)

---

## 5. Platform capabilities to test

### 5.1 Parallel routing

- Can the platform send one request to two approvers at the same time?
- Does it support AND-merge (wait for all) vs. OR-merge (first response wins)?
- Can we configure which merge type to use per split point?

### 5.2 Conditional branching

- Can routing rules vary by request type or dollar amount?
- Example: requests under $5K skip Tier 3 entirely
- Is this configurable without code changes?

### 5.3 Merge logic

- What happens at the convergence point?
- How does the platform aggregate responses from parallel branches?
- Does it report which branch completed first?

### 5.4 Notification and visibility

- Do both approvers see the request independently?
- Are notifications sent simultaneously or queued?
- Does the requestor see status for both branches?

### 5.5 Audit trail

- Are parallel branch events logged with distinct identifiers?
- Can an auditor reconstruct the timeline of parallel approvals?
- Do timestamps accurately reflect parallel (not sequential) execution?

---

## 6. Performance benchmarks

### Measurements

| Metric | Sequential baseline | Parallel target | How measured |
|---|---|---|---|
| **Routing time** (request visible to approver) | Tier 2 → wait → Tier 3 | Both within 60 seconds | Timestamp in approval queue |
| **Total elapsed time** (submit to fully approved) | Tier 2 wait + Tier 3 wait | MAX(Tier 2, Tier 3) — not SUM | Timestamp delta |
| **Platform response time** | Current page load / API speed | No degradation (< 10% slower) | Admin performance dashboard |
| **Notification delivery** | Sequential notification times | Both notifications within 60 seconds | Email/notification timestamps |
| **Throughput under load** | N/A (new test) | 50 concurrent parallel requests without errors | Batch test results |

### Expected improvement

Sequential routing adds wait times: if Tier 2 takes 2 days and Tier 3 takes 1 day, sequential = 3 days. Parallel should reduce to 2 days (the longer of the two). This spike validates that the platform delivers that improvement without introducing errors.

---

## 7. Risk scenarios and what failure looks like

### Risk matrix

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Parallel routing feature doesn't work as documented | Medium | High — spike fails on Day 1 | Michael tests basic config before Day 1; fast-fail to alternatives |
| Merge logic drops one approval silently | Low | Critical — data integrity issue | Every test includes audit trail verification |
| Performance degrades under parallel load | Medium | Medium — may need platform tuning | Run at low volume first; escalate if > 20% degradation |
| Sandbox doesn't match production behavior | Low | Medium — results may not transfer | Document sandbox vs. production differences; retest critical cases in staging if available |
| Michael unavailable during spike week | Low | High — only person with platform knowledge | Pre-schedule Michael's time blocks on Day 1; document everything for knowledge transfer |
| Conflict handling is unpredictable | Medium | High — can't deploy parallel routing if reject behavior is random | C1-C4 scenarios run repeatedly; must be deterministic to pass |

### What "failure" looks like

The spike produces a no-go if any of these occur:

1. **Platform can't configure parallel routing at all** — feature is documented but broken or unavailable
2. **Merge logic is unreliable** — different results for the same test scenario on repeat runs
3. **Audit trail gaps** — parallel actions not logged or timestamps inaccurate
4. **Conflict handling is undefined** — platform has no predictable behavior when one path rejects and the other approves
5. **Performance degradation > 25%** — parallel routing makes the platform noticeably slower
6. **Configuration requires code-level changes** — can't be maintained by non-developers

---

## 8. Go/no-go decision framework

### Decision criteria

| Category | Go | No-go |
|---|---|---|
| **Parallel send** | Both approvers receive within 60 seconds, every time | Inconsistent delivery or sequential behavior persists |
| **Merge logic** | AND-merge works reliably across 10+ test runs | Merge behavior is inconsistent or drops approvals |
| **Conflict handling** | Reject-while-approve behavior is deterministic and documented | Behavior varies between runs or produces data corruption |
| **Audit trail** | All parallel actions logged with accurate timestamps | Missing events, wrong timestamps, or no branch identifiers |
| **Performance** | Parallel ≤ sequential response time; < 10% platform overhead | > 25% performance degradation under parallel load |
| **Configurability** | Admin-configurable without code changes | Requires platform vendor engagement or code deployment |
| **Knowledge transfer** | Second team member can configure parallel routing from documentation | Only Michael can do it and configuration isn't documentable |

### Decision process

1. Michael and Ripley review all test results on Day 5
2. Score each category as Go, Conditional Go (fixable issues), or No-Go
3. Overall decision:
   - **Go** — All categories green. Proceed with Epic 12 stories.
   - **Conditional Go** — Some categories have fixable issues. Proceed with mitigations; document what needs follow-up.
   - **No-Go** — Any critical category (merge logic, audit trail, conflict handling) is red. Pivot to alternatives.

---

## 9. Michael Bolton's role and time commitment

### Why Michael is essential

Michael is the single point of failure for platform knowledge. He confirmed the platform supports parallel routing, conditional branching, and delegation but flagged it as "brittle" and poorly integrated. No one else has configured advanced routing features.

### Time commitment

| Day | Hours | Activities |
|---|---|---|
| Day 1 | 4–6 hrs | Sandbox setup, parallel workflow configuration, team walkthrough |
| Day 2 | 2–3 hrs | Support happy path testing, troubleshoot configuration issues |
| Day 3 | 2–3 hrs | Support edge case testing, explain platform behavior |
| Day 4 | 1–2 hrs | Review volume test results, assess platform limits |
| Day 5 | 2–3 hrs | Co-author findings, review go/no-go recommendation |
| **Total** | **11–17 hrs** | |

### Knowledge transfer goal

By end of spike, at least one other team member should be able to:
- Configure a basic parallel routing workflow
- Read and interpret audit logs for parallel branches
- Reset the workflow to sequential if needed

This directly addresses the single-point-of-failure risk Michael represents.

---

## 10. Alternative approaches if spike fails

If the spike produces a no-go on native parallel routing, these alternatives preserve the Glass Floor initiative's goals:

### Option A: Manual parallel notification

- **How:** Requestor or system sends notification to both Tier 2 and Tier 3 simultaneously via email. Approvals tracked manually or in a shared tracker.
- **Effort:** Low (process change only)
- **Risk:** No merge logic enforcement; depends on people following the process
- **Time savings:** Partial — approvers can review in parallel, but tracking is manual

### Option B: Process redesign — combine Tier 2 and Tier 3

- **How:** One reviewer checks both budget authority and compliance. Eliminate the parallel routing need entirely.
- **Effort:** Medium (role redefinition, training)
- **Risk:** Concentration of responsibility; may not satisfy separation-of-duties requirements
- **Time savings:** Full — removes one approval step

### Option C: Platform upgrade or replacement

- **How:** Upgrade to a supported version with working parallel routing, or evaluate alternative platforms
- **Effort:** High (procurement, migration, testing)
- **Risk:** Significant timeline impact; may delay Init 2 by months
- **Time savings:** Full — if new platform works properly

### Option D: Lightweight orchestration layer

- **How:** Build a simple orchestration script (webhook-based) that sends parallel requests and tracks responses outside the platform
- **Effort:** Medium (development, testing, maintenance)
- **Risk:** Adds a custom integration point; increases Michael's maintenance burden
- **Time savings:** Full — but adds technical debt

### Recommendation if no-go

Start with **Option A** (manual parallel) as an interim measure — it delivers partial time savings with minimal effort. Simultaneously evaluate **Option C** timeline, since a brittle platform is a risk regardless of this spike's outcome.

---

## Appendix: Traceability

| Field | Value |
|---|---|
| DT Source | 2026-03-02 Design Thinking Session |
| Scenario | Samir's 23-step approval — sequential routing as largest elapsed-time contributor |
| Route | Glass Floor — bottom-up improvement |
| Interview evidence | Michael Bolton (platform capabilities), Samir Nagheenanajar (pain of sequential waits) |
| Related issues | #33 (upstream evidence display), #35+ (Epic 12 stories dependent on this spike) |
