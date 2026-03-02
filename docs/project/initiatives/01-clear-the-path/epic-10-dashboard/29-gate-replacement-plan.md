# Gate Replacement Plan — Replace Lumbergh's Informal Gate with Dashboard Access

**Date:** 2026-03-02
**Issue:** #29
**Epic:** 10 — Dashboard
**Initiative:** 1 — Clear the Path
**Author:** Ripley (Lead)

---

## Problem Statement

Lumbergh inserted himself as an informal approval gate on every request over $5K. He frames it as a "courtesy copy," but functionally nothing moves until he replies "Go ahead." Peter estimates this adds 1 to 2 days to every mid-size request. Lumbergh does not recognize this as a bottleneck. His actual need is visibility into request volume, status, and cycle time, not approval authority.

**Sources:** Lumbergh interview (unrecognized reveal), Peter interview (direct contradiction), Initiative Scope (elective tier #4), Input Synthesis Theme 1

---

## Transition Approach

The sequencing here matters. The dashboard must be live and Lumbergh must be trained on it before the gate is removed. Removing the gate without the replacement creates a vacuum Lumbergh will fill informally all over again.

### Phase 1 — Dashboard Deployment (Days 1 through 5)

| Step | Owner | Detail |
|---|---|---|
| Configure dashboard views | Lambert (Tech) | Real-time views showing request status, volume by category, cycle time averages, and stuck items (requests idle more than 2 business days) |
| Include Lumbergh-specific views | Lambert | Filter for requests over $5K, breakdown by requestor team, weekly trend lines |
| Deploy to staging | Lambert | Dashboard accessible via existing tooling, no new platform required |
| Smoke test with sample data | Kane (QA) | Verify all views render correctly, filters work, data refreshes on schedule |

### Phase 2 — Lumbergh Training and Buy-In (Days 6 through 8)

| Step | Owner | Detail |
|---|---|---|
| Walkthrough session with Lumbergh | Ripley + Dallas | Show the dashboard, demonstrate filters, explain what each view answers |
| Confirm Lumbergh's information needs are met | Ripley | Ask Lumbergh explicitly: "Does this give you what you need to stay informed without reviewing each request?" |
| Document Lumbergh's sign-off | Ripley | Written confirmation that the dashboard meets his visibility needs. This is the gate for Phase 3. |
| Provide self-service guide | Dallas | One-page reference for Lumbergh on how to access, filter, and interpret dashboard views |

**Exit criteria for Phase 2:** Lumbergh confirms in writing (email or issue comment) that the dashboard provides the visibility he needs. Phase 3 does not start without this.

### Phase 3 — Gate Removal (Days 9 through 12)

| Step | Owner | Detail |
|---|---|---|
| Notify Peter that Lumbergh loop-in is no longer required | Ripley | Peter stops routing requests through Lumbergh for informal approval. Clear, explicit instruction. |
| Update process documentation | Dallas | Remove the "loop in Lumbergh on requests over $5K" step from all routing guides, email templates, and workflow descriptions |
| Notify Joanna's team and other requestors | Parker | Brief communication that the routing path has changed and requests no longer require Lumbergh's informal sign-off |
| Monitor for informal re-insertion | Kane | Watch for 2 weeks post-removal to see if Lumbergh starts requesting loop-ins again or if Peter reverts to old behavior |

### Phase 4 — Measurement and Validation (Days 13 through 26)

| Step | Owner | Detail |
|---|---|---|
| Pull baseline cycle time data for $5K to $50K requests | Kane | Use data from the 30 days before dashboard go-live |
| Measure post-change cycle time | Kane | Same cohort of request types, measured over days 13 through 26 |
| Compare before and after | Ripley | Target is 1 to 2 day reduction in average cycle time for the $5K to $50K range |
| Document findings | Ripley | Include in parallel-run validation report for Initiative 1 |

---

## Peter's Role in the Transition

Peter is the person most affected day to day. He currently waits for Lumbergh's "Go ahead" on everything over $5K, even though it is not a formal approval step.

- **Before Phase 3:** Peter continues current routing. No changes until the dashboard is live and Lumbergh has signed off.
- **Phase 3 start:** Ripley explicitly tells Peter the gate is removed. Peter routes directly to formal approvers (budget holder, compliance, audit) without looping Lumbergh.
- **Post-transition:** Peter confirms the new process works. If Peter reverts to old habits (sending Lumbergh courtesy copies), that is a signal the transition was not clean.

Peter's informal workarounds (fabricated social proof for Samir, shadow routing) are a separate concern addressed by other initiatives. This plan only removes the Lumbergh loop-in.

---

## Process Documentation Updates

The following documents need revision to remove the informal gate:

1. **Routing guide** — Remove instruction to CC Lumbergh on requests over $5K
2. **Email templates** — Remove Lumbergh from default CC lists for mid-range requests
3. **Workflow diagrams** — Update to show direct routing from intake to formal approvers, with dashboard as a parallel information channel (not a gate)
4. **New hire onboarding docs** — Ensure no reference to Lumbergh as an informal checkpoint
5. **Peter's procedures** — Document the new routing path explicitly so it survives personnel changes

---

## Measurement Plan

### What We Measure

| Metric | Baseline Period | Post-Change Period | Target |
|---|---|---|---|
| Average cycle time for $5K to $50K requests | 30 days before dashboard go-live | 14 days after gate removal | 1 to 2 day reduction |
| Requests where Lumbergh is still CC'd informally | N/A (all of them currently) | 14 days after gate removal | Zero |
| Lumbergh dashboard login frequency | N/A | Ongoing | At least 3 times per week for first month |
| Requestor satisfaction (Joanna's team pulse) | Pre-survey | Post-survey at day 26 | Improvement on predictability and speed |

### How We Measure

- Cycle time pulled from the existing approval tool (timestamps on submission vs. final approval)
- CC tracking via email logs or routing system metadata
- Dashboard access tracked via built-in analytics
- Satisfaction via a brief 3-question pulse survey to Joanna's team

---

## Lumbergh Acceptance Criteria

Lumbergh agrees to the change when he can confirm all of the following:

1. The dashboard shows him real-time request status across all active approvals
2. He can filter by dollar amount, requestor team, and cycle time
3. He receives proactive alerts for high-value requests (configured in Issue #30)
4. He does not need to be "looped in" on individual requests because the dashboard gives him the same or better information
5. He can access the dashboard without IT assistance

If any of these are not met, the gate stays in place until they are. The dashboard must earn its replacement status.

---

## Risk Mitigation

| Risk | Mitigation |
|---|---|
| Lumbergh re-inserts himself informally after gate removal | 2-week monitoring period. If he starts requesting loop-ins, Ripley escalates with data showing the dashboard provides equivalent visibility. |
| Peter reverts to old routing habits | Explicit instruction and follow-up check at day 5 and day 14 post-removal. |
| Dashboard quality is insufficient | Phase 2 exit criteria require Lumbergh's explicit sign-off. No gate removal without it. |
| Other stakeholders assume Lumbergh still needs to approve | Proactive communication from Parker to all affected teams. |

---

## Timeline

| Phase | Days | Milestone |
|---|---|---|
| Dashboard deployment | 1 through 5 | Dashboard live in staging, smoke tested |
| Lumbergh training | 6 through 8 | Lumbergh sign-off obtained |
| Gate removal | 9 through 12 | Process docs updated, Peter routing directly |
| Measurement | 13 through 26 | Cycle time comparison complete, findings documented |

**Total estimated duration:** 26 business days, well within the Initiative 1 window.

---

## Acceptance Criteria Checklist

- [ ] Dashboard deployed and Lumbergh trained on its use
- [ ] Peter stops routing requests through Lumbergh for informal approval
- [ ] Process documentation updated to remove the informal gate
- [ ] Cycle time for $5K to $50K requests measured before and after (target: 1 to 2 day reduction)
- [ ] Lumbergh confirms he no longer needs to be "looped in" on individual requests
