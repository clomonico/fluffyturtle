# Cycle Time Measurement and Reporting Plan

**Author:** Kane (Validator)
**Date:** 2026-03-02
**Issue:** #24
**Initiative:** Clear the Path — Epic 08: Tier Removal
**Status:** Draft

---

## 1. Current State Baseline — What We Know from Research

Cycle time is not formally tracked today. The estimates below are reconstructed from stakeholder interviews. This is itself a finding: the organization is making process decisions without performance data.

### 1.1 Overall Cycle Time Estimates

| Source | Stated Cycle Time | Context |
|--------|-------------------|---------|
| Lumbergh | "Two to three weeks for anything non-trivial" | His perception as executive sponsor |
| Joanna | "Eight to twelve business days" for non-trivial requests; sometimes three weeks | Requestor experience; same type/amount yields wildly different timelines |
| Joanna | 23 days for a cross-departmental resource request | Caused her team to abandon that category entirely |
| Lumbergh | 11 days for a "standard" vendor contract | Specific incident; Samir's review took 5 minutes once Lumbergh walked it over |
| The Bobs | "Under 3 days" is realistic if documentation is complete at intake | Compliance steps alone can complete in hours |
| Peter | 3-5 days for Samir's review alone (without Peter's workaround) | Peter's intervention drops this to ~1 day |

### 1.2 Per-Step Delay Contributions (Estimated from Interview Evidence)

| Step / Gate | Owner | Estimated Delay | Evidence Source |
|-------------|-------|-----------------|-----------------|
| Lumbergh's "loop me in" gate (Tier 6) | Lumbergh | 1-2 days per request over $5K | Peter: "sometimes that takes two days because he's in meetings or he forgot"; Lumbergh doesn't reject anything, just acknowledges |
| Samir's secondary budget review (Tier 3) | Samir | 3-5 days without intervention; ~1 day with Peter's workaround | Peter: "if I wait for Samir, it takes three to five days"; Samir: "I can turn it around in a few hours" if inputs are complete |
| Incomplete documentation rework | Multiple | Unknown per-cycle, but affects ~40-60% of requests | Samir: "40% of what lands on my desk is missing something"; Bobs: "60% of approved requests have incomplete documentation" |
| Tom's operational process check (Tier 4) | Tom | Unknown duration; skipped on 70% of requests with no consequences | Peter skips it routinely; Tom can't fully explain what it checks without consulting files |
| Tom's departmental review (Tier 1) | Dept. manager | Unknown duration; also skipped frequently | Added as a "quality gate" with no regulatory basis |
| Sequential routing (all tiers) | System design | Compounds all individual delays; no parallel execution | Platform supports parallel routing but it's not configured (Michael, Peter both confirmed) |
| Milton's exception routing (off-chart) | Milton | Unknown; 17 requests sat for a full week when Milton was sick | No SLA, no tracking, no visibility into this step |
| Queue time between steps | System | The Bobs: "The delay isn't in the controls. It's in the queue time between steps." | Single largest contributor per the Bobs |

### 1.3 Baseline Assumptions

**Validator note:** Current "performance" includes Peter's invisible interventions. Peter fabricates social proof to accelerate Samir's reviews, skips Tom's steps on 70% of requests, and manages Lumbergh's gate informally. If Peter's workarounds are removed or he leaves, true unassisted cycle time is likely 3-5 weeks, not 2-3. Any baseline measurement must distinguish between assisted performance (with Peter) and unassisted performance (without).

**Estimated current state:**

| Request Type | Estimated Current Cycle Time | Confidence |
|--------------|------------------------------|------------|
| Routine (< $5K, known vendor, renewal) | 3-5 business days | Low — Joanna says "sometimes three days" but also "sometimes three weeks" for same type |
| Mid-range ($5K-$50K, standard) | 8-15 business days | Medium — Joanna's "8-12 days" aligns with Lumbergh's "two to three weeks" |
| Complex (> $50K, new vendor, cross-dept) | 15-25+ business days | Low — Joanna's cross-dept request took 23 days; limited data points |
| Exception path (hits Milton) | Add unknown days; no SLA exists | Very low — completely untracked |

---

## 2. Measurement Methodology

### 2.1 What to Measure

| Metric | Definition | Why It Matters |
|--------|-----------|----------------|
| **End-to-end cycle time** | Calendar days from request submission to final approval (or rejection) | The number stakeholders care about; what Lumbergh reports upward |
| **Per-tier duration** | Time spent at each approval tier (clock starts at assignment, stops at action) | Identifies which tiers are bottlenecks vs. which are fast |
| **Queue time** | Time between one tier completing and the next tier starting work | The Bobs identified this as the largest delay contributor |
| **Rework cycles** | Number of times a request is returned for corrections before approval | Measures intake quality; 40-60% of requests currently need rework |
| **Rework duration** | Time added by each return-and-resubmit cycle | Quantifies the cost of incomplete submissions |
| **Bottleneck frequency** | How often each tier/step is the longest-duration step in a request's lifecycle | Tells us where to focus improvement effort |
| **First-pass yield** | Percentage of requests that complete without any rework or return | Intake gate effectiveness metric |
| **SLA compliance** | Percentage of requests completed within stated SLA per tier | Measures whether SLAs are realistic and being met |
| **Shadow process volume** | Number of requests bypassing the formal system (credit cards, informal routing) | Measures whether the formal process is being trusted |

### 2.2 How to Measure

**Phase 1: Baseline (Before tier removal — weeks 1-4)**

| Method | What It Captures | Limitations |
|--------|-----------------|-------------|
| System log extraction | Timestamps for submission, assignment, action, and completion events | Only captures the formal process; misses Peter's interventions and shadow volume |
| Manual time study (sampling) | 20-request sample tracked by hand through every step, including informal gates | Labor-intensive; only captures a snapshot |
| Requestor survey | Perceived cycle time and satisfaction from the last 5 requests per respondent | Perception may differ from actuals; still valuable for before/after comparison |
| Shadow process audit | Count of credit card purchases, informal arrangements, and off-system approvals in the last 90 days | One-time effort; establishes bypass volume baseline |

**Phase 2: Parallel Run (During transition — weeks 5-8)**

Both old (7-tier) and new (3-tier) processes run side by side for comparable request types. Measurement captures:
- Same request type processed through both paths
- Per-tier timing in both paths
- Rework rates in both paths
- Any requests that fail in the new path that would have been caught in the old

**Phase 3: Post-Cutover (After tier removal — weeks 9+)**

System logs only (manual sampling drops off). All metrics from Section 2.1 collected automatically. Requestor survey repeated at week 12 for comparison.

### 2.3 When to Measure

| Phase | Duration | Cadence | Purpose |
|-------|----------|---------|---------|
| Baseline | 4 weeks before removal | Daily collection, weekly report | Establish true current performance with hard data |
| Parallel run | 4 weeks | Daily collection, weekly report | Validate that tier removal doesn't break anything |
| Early post-cutover | 8 weeks after removal | Daily collection, weekly report | Detect regressions quickly |
| Steady state | Ongoing | Daily collection, monthly report | Long-term monitoring |

---

## 3. Expected Improvement Targets

### 3.1 Per-Change Impact Estimates

**Removing Lumbergh's gate (Tier 6):**
- Current impact: 1-2 days added to every request over $5K
- Evidence: Peter and Lumbergh's interviewer notes both confirm this range
- Expected reduction: **1.5 days average** on mid-range and complex requests
- Confidence: High — Lumbergh doesn't reject, doesn't review, doesn't add information. Dashboard fully replaces his need.

**Removing Samir's duplicate review (Tier 3):**
- Current impact: 3-5 days without Peter's workaround; ~1 day with it
- Evidence: Peter's testimony on timing; Samir's statement that he'd skip it with upstream visibility
- Expected reduction: **2 days average** (blended: some requests already get Peter's shortcut, some don't)
- Confidence: Medium — depends on upstream verification records actually being visible and trusted
- Validator flag: This improvement is contingent on Glass Floor (Initiative 3). Without verification visibility, removing Samir's tier could increase risk without reducing time, because Samir may reintroduce informal checks.

**Removing Tom's departmental review (Tier 1):**
- Current impact: Unknown duration but frequently skipped already
- Evidence: Peter skips it on most requests; no regulatory basis per the Bobs
- Expected reduction: **0.5-1 day average** (queue time and routing overhead even when fast)
- Confidence: Medium — the step is fast when it happens, but it adds queue time

**Removing Tom's operational process check (Tier 4):**
- Current impact: Unknown active review time; skipped on 70% of requests
- Evidence: Peter skips with no consequences; Tom can't explain what it checks
- Expected reduction: **0.5-1 day average** (mostly queue time savings)
- Confidence: Medium — limited data on actual duration

**Intake gate (documentation completeness enforcement):**
- Current impact: 40-60% of requests arrive incomplete, triggering rework cycles of unknown duration
- Evidence: Samir and the Bobs both confirmed this independently
- Expected reduction: Eliminates rework cycles entirely for properly submitted requests; may add initial friction (returns to requestor)
- Net effect: **1-3 days average reduction** from eliminated back-and-forth
- Confidence: Medium — depends on requestor compliance; 30-day grace period softens transition

### 3.2 Net Expected Improvement

| Request Type | Current Estimate | Expected Future | Improvement | Basis |
|--------------|-----------------|-----------------|-------------|-------|
| Routine (< $5K) | 3-5 days | 2-3 days | 1-2 days (~40%) | Removing Tiers 1, 3, 4; intake gate reduces rework |
| Mid-range ($5K-$50K) | 8-15 days | 4-7 days | 4-8 days (~50%) | Removing all 4 elective tiers including Lumbergh's gate; intake gate |
| Complex (> $50K) | 15-25 days | 7-10 days | 8-15 days (~55%) | Full impact of all removals; SLAs enforce per-tier time limits |

**Process design target (from 01-process-design.md):** Maximum 7 business days end-to-end (2 + 3 + 2 with SLAs). This is consistent with the estimates above for mid-range requests and achievable for complex requests if SLAs hold.

**Lumbergh's stated goal:** "Cut cycle times in half." The estimates above show 40-55% reduction, which meets or exceeds that target.

### 3.3 Assumptions That Must Hold

| Assumption | What Breaks If Wrong | How We'll Know |
|------------|---------------------|----------------|
| The 4 elective tiers add delay but not value | A control gap surfaces post-removal | Parallel run comparison; escalation volume spike |
| Intake gate eliminates rework cycles | Requestors submit incomplete packages anyway or bypass the system | First-pass yield metric; shadow process volume |
| SLAs are realistic (2, 3, 2 business days) | Reviewers can't meet them; auto-escalation fires constantly | SLA compliance rate; escalation frequency |
| Peter's workarounds become unnecessary | The new process is slower than Peter's shortcuts | End-to-end cycle time vs. Peter's informal benchmark |
| Samir trusts upstream verification records | Samir reintroduces informal checks outside the system | Samir's per-request duration stays elevated; qualitative check-in |

---

## 4. Reporting Template — Weekly Cycle Time Report

```
# Cycle Time Report — Week of [DATE]

## Summary
- Requests completed this week: [N]
- Average end-to-end cycle time: [X] business days
- Median end-to-end cycle time: [X] business days
- SLA compliance rate: [X]%
- First-pass yield (no rework): [X]%

## Cycle Time by Request Type
| Type | Count | Avg Cycle Time | Median | Target | Status |
|------|-------|---------------|--------|--------|--------|
| Routine (< $5K) | | | | 3 days | |
| Mid-range ($5K-$50K) | | | | 7 days | |
| Complex (> $50K) | | | | 7 days | |

## Per-Tier Performance
| Tier | Avg Duration | Median | SLA | Compliance | Bottleneck Count |
|------|-------------|--------|-----|------------|-----------------|
| Budget Verification | | | 2 days | | |
| Compliance Review | | | 3 days | | |
| Financial Controls | | | 2 days | | |

## Queue Time
| Transition | Avg Wait | Median Wait | Notes |
|-----------|----------|-------------|-------|
| Submission → Tier 1 | | | |
| Tier 1 → Tier 2 | | | |
| Tier 2 → Tier 3 | | | |

## Rework
- Requests returned for corrections: [N] ([X]%)
- Average days added per rework cycle: [X]
- Most common return reasons: [list]

## Escalations
- Auto-escalations triggered: [N]
- Manual escalations: [N]
- Escalation reasons: [list]

## Shadow Process Indicators
- Known credit card bypasses: [N]
- Off-system approvals reported: [N]
- Trend vs. last week: [up/down/flat]

## Flags
- [Any requests exceeding 2x the target cycle time]
- [Any tier consistently breaching SLA]
- [Any emerging patterns or concerns]
```

---

## 5. Dashboard Requirements

### 5.1 Real-Time Dashboard (Lumbergh Dashboard Extension)

The process design already specifies a Lumbergh Dashboard for visibility. Cycle time metrics should be integrated into it, not built as a separate tool.

| Dashboard Element | Data Source | Update Frequency |
|-------------------|-----------|------------------|
| Current average cycle time (rolling 30 days) | System logs | Daily |
| Cycle time trend chart (weekly averages over 12 weeks) | System logs | Weekly |
| Per-tier duration breakdown (current week) | System logs | Daily |
| SLA compliance gauge (% of requests within SLA) | System logs | Daily |
| First-pass yield (% completing without rework) | System logs | Daily |
| Active request heatmap (requests by age in queue) | System logs | Real-time |
| Before vs. after comparison (baseline period vs. current) | Stored baseline + current logs | Weekly |

### 5.2 Reporting Dashboard (Monthly Review)

| Element | Purpose |
|---------|---------|
| Month-over-month cycle time trend | Track sustained improvement or regression |
| Request volume by type | Identify shifts in demand patterns |
| Shadow process volume trend | Confirm formal process adoption is increasing |
| Escalation trend | Detect if the 3-tier process is missing edge cases |
| Requestor satisfaction score (quarterly survey) | Lagging indicator of process health |

---

## 6. Measurement Cadence

| Phase | Report Type | Frequency | Audience | Duration |
|-------|------------|-----------|----------|----------|
| Baseline | Weekly cycle time report | Weekly | Project team, Lumbergh, the Bobs | 4 weeks pre-removal |
| Parallel run | Weekly comparison report (old vs. new) | Weekly | Project team, Lumbergh, the Bobs, Samir | 4 weeks |
| Early post-cutover | Weekly cycle time report | Weekly | Project team, Lumbergh, the Bobs | 8 weeks post-removal |
| Steady state | Monthly cycle time report | Monthly | Lumbergh, the Bobs, process owner | Ongoing |
| Quarterly review | Quarterly trend report + requestor survey | Quarterly | Executive sponsor, all stakeholders | Ongoing |

**Transition criteria from weekly to monthly:** Two consecutive weeks where all three metrics below are met:
1. Average cycle time is within 10% of target (7 business days for mid-range)
2. SLA compliance is above 85%
3. Shadow process volume is trending flat or down

---

## 7. Validator Notes

**Evidence quality:** The baseline metrics in Section 1 are reconstructed from interviews, not system data. Confidence levels are low to medium. The first action item is extracting actual system logs to validate or correct these estimates before any removal begins. If system logs show cycle times significantly different from interview estimates, the improvement targets in Section 3 need recalibration.

**Peter dependency:** Current "performance" includes Peter's invisible labor. The baseline should be measured both with and without Peter's workarounds active. If the measurement window captures Peter still intervening, the baseline will look better than the true unassisted process. Recommend a 1-week sub-sample where Peter routes requests through official channels only.

**Cross-initiative dependency:** The cycle time reduction from removing Samir's tier (estimated 2 days) depends on Initiative 3 (Glass Floor) delivering upstream verification visibility. If Glass Floor is delayed, Samir's tier removal either delivers less improvement (because he reintroduces informal checks) or introduces risk (because nobody duplicates his review). The measurement plan should track Samir's behavior as a leading indicator.

**Milton's tier:** The process design removes Milton's vendor terms check (Tier 7) as elective. My earlier validation flagged that this step caught a $14K late fee in 2019. The cycle time plan should track vendor-related exceptions separately post-removal to confirm the vendor master database (Initiative 4) absorbs this function before the safety net is gone.
