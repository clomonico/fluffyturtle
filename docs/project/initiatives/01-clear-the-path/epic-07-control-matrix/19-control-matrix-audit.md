# Control Matrix Audit — 7-Tier Approval Chain

**Date:** 2026-03-02
**Author:** Ripley (Lead)
**Issue:** #19
**Initiative:** 01 — Clear the Path

---

## Purpose

The Bobs' control matrix distinguishes mandatory from elective controls in the current 7-tier approval chain. Nobody reads it. This audit reconstructs the full 7-tier chain from interview evidence, classifies each tier, cross-references the matrix against operational reality, and documents the discrepancies.

The goal is a validated record of what each tier does, why it exists, whether it's regulatory or organizational habit, and whether it's functioning as intended.

---

## Reconstructed 7-Tier Approval Chain

### Tier 1: Requestor Verification (Department Head Approval)

**What it does:** Confirms the requestor has authority to submit the request and that it aligns with departmental need.

**Who:** Department head (varies by org unit).

**Why it exists:** Regulatory requirement. The first of the Bobs' three mandatory control points. Ensures segregation of duties at the point of origin.

**Classification:** MANDATORY (regulatory)

**Evidence:**
- The Bobs (Interview 1): "The regulatory framework requires three control points for expenditure approvals: requestor verification, budget authority confirmation, and segregation-of-duties sign-off."
- The Bobs (Interview 2): "Three. That's the regulatory floor."
- Peter: "Give me three approvers max: budget holder, compliance if it hits the threshold, and done." (Independently arrived at the same minimum.)

---

### Tier 2: Budget Authority Confirmation (Finance Sign-off)

**What it does:** Verifies budget codes, allocation, cost center, and available funding. Confirms the spend is within the approved budget and properly categorized.

**Who:** Finance approver (Samir Nagheenanajar's function).

**Why it exists:** Regulatory requirement. The second of the Bobs' three mandatory control points.

**Classification:** MANDATORY (regulatory)

**Evidence:**
- The Bobs (Interview 1): Budget authority confirmation is one of three regulatory control points.
- Samir (Interview 1): "I check the budget codes, verify the amounts against the current allocation, cross-reference the cost center, and then I check whether the requestor has the right delegation of authority."
- Samir references the 2022 incident where a vendor contract was approved without remaining budget, resulting in a $40K overrun.

---

### Tier 3: Compliance / Conflict-of-Interest Check (Segregation-of-Duties Sign-off)

**What it does:** Ensures segregation of duties, checks for conflict-of-interest conditions (e.g., department head also being budget holder), validates vendor relationships on consolidated amounts.

**Who:** Compliance function (Bobs' audit trail), triggered by threshold and vendor flags.

**Why it exists:** Regulatory requirement. The third of the Bobs' three mandatory control points. Also includes Milton's step 7 payment terms/contract deviation check (triggers on POs above $50K or flagged vendors).

**Classification:** MANDATORY (regulatory)

**Evidence:**
- The Bobs (Interview 1): "Segregation-of-duties sign-off" is one of three regulatory control points.
- Milton (Interview 1): Step 7 exists because of a $14K late fee incident in 2019 where payment terms weren't verified. "He wasn't here when we ate that $14,000 penalty."
- Milton (Interview 2): Peter's PO-splitting bypasses this check entirely. "When you split a PO, it also bypasses the conflict-of-interest check at step 7. That check only triggers on consolidated amounts."
- The Bobs (Interview 1): "Immutable audit log, identity verification at every approval node, and real-time documentation attachment" are the three requirements.

**Note on Milton's role:** Milton performs the vendor terms verification (47 vendors with non-standard terms in a personal spreadsheet), but he is not in the flowchart, not in the RACI, and not in the system. His function maps to Tier 3 but operates entirely through informal email routing from Peter. This is a structural gap in the current process.

---

### Tier 4: Tom's Process Checkpoint / Routing Accuracy Review

**What it does:** Tom reviews whether requests were routed correctly and whether the process was followed. Originally designed as a quality gate over the routing logic.

**Who:** Tom Smykowski (Process Owner).

**Why it exists:** Added by Tom in 2019 when he built the original workflow. Not driven by a regulatory requirement. Functions as Tom reviewing whether Peter routed things to Tom correctly.

**Classification:** ELECTIVE (organizational)

**Evidence:**
- The Bobs (Interview 1): "Four of those were added internally over the years. Department head review, Tom's process checkpoint, the secondary finance sign-off, and the executive 'visibility' loop that Lumbergh requested."
- The Bobs (Interview 2): "Three are regulatory. Four are elective. We documented this in the control matrix we published eighteen months ago."
- Peter (Interview 1): "Step 4 is a 'routing accuracy review' — which is literally Tom reviewing whether I routed it to Tom correctly."
- Peter (Interview 1): "I skip steps 4 and 6 on about 70% of requests because they're pointless."
- Tom (Interview 2): Acknowledged he'd need to consult his files to explain the rationale for every step. "I could give you the gist. For the detailed rationale — the original decision documents, the exception logs — I'd need to pull up my files."

**Operational reality:** Peter skips this step on 70% of requests with no adverse consequences. Nobody has flagged a routing error that this step would have caught.

---

### Tier 5: Tom's Exception Routing Path (Duplicate Vendor Payments)

**What it does:** Added as a temporary exception path in 2020 after a duplicate vendor payment incident. Routes all vendor-related requests through an extra check. Originally scoped to catch duplications, but applied to all vendor requests regardless of duplication risk.

**Who:** Tom Smykowski (added it), Milton (actually performs vendor checks informally).

**Why it exists:** Temporary patch from 2020 that became permanent. Tom admitted it was supposed to be temporary. The underlying issue (duplicate vendor payments) was flagged by Milton, not Tom. Tom took credit for the fix.

**Classification:** ELECTIVE (organizational patch, over-scoped)

**Evidence:**
- Tom (Interview 2): "In 2020, we had a procurement issue — duplicate vendor payments — and I added an exception routing path to catch that. It was supposed to be temporary, but it turned out to be useful, so we kept it."
- Tom (Interview 2, when asked what he'd change): "The exception path from 2020 — the duplicate vendor thing — that could probably be handled differently. It adds a step for every vendor-related request, not just the ones at risk of duplication."
- Milton (Interview 2): "The 2020 exception routing? That was also... I told him about the duplicate vendor payments in September. He added the exception path in November and presented it as his quarterly improvement initiative."
- Peter (Interview 1): Skips this step on 70% of requests.
- Input Synthesis: Two of Tom's steps are "unreviewed temporary patches that became permanent through inertia."

**Operational reality:** Tom himself admits this step is over-scoped. It applies to every vendor request when it should only apply to duplication-risk scenarios. Peter skips it routinely. The actual vendor verification is done by Milton outside the system anyway.

---

### Tier 6: Samir's Self-Imposed Secondary Review

**What it does:** Samir performs a secondary cross-reference of every request against the live budget (not just allocated amounts), and seeks peer review on anything over $10K. Policy only requires dual sign-off above $50K.

**Who:** Samir Nagheenanajar, plus a colleague he informally consults.

**Why it exists:** Samir added this after a 2022 incident where an AP clerk miscoded an expense. It is not required by policy. Samir applies full review uniformly to all requests regardless of dollar amount, including sub-$50K requests that policy allows a lighter treatment for.

**Classification:** ELECTIVE (self-imposed risk mitigation)

**Evidence:**
- Samir (Interview 1): "It's not formally mandated for everything, no. The policy requires dual sign-off above $50K. But I do it for anything over $10K, sometimes lower."
- Samir (Interview 2): "The formal financial review always existed. But after that incident, I started doing a secondary cross-reference against the live budget, not just the allocated amount. It's not in the written policy — it's something I added."
- Samir (Interview 2): "The policy technically has a lighter touch for sub-$50K, but I do the same level of review regardless."
- Peter (engagement interview): "Samir's a good guy. He's also terrified. He adds a second review on anything over $10K even though policy only requires it over $50K. Nobody told him to do that."
- The Bobs (Interview 1): Explicitly listed "the secondary finance sign-off" as one of the four elective tiers.
- Samir's own fix (Interview 2): "If I could see that the budget line was verified at step 2 with a timestamp and a name, I wouldn't need to re-do it at step 5."

**Operational reality:** Adds approximately one day per request across all requests. Samir re-verifies everything from scratch because the system shows him nothing about upstream decisions. He designed his own solution (upstream visibility) without recognizing it would eliminate the need for this tier. Initiative 3 (Glass Floor) directly addresses the root cause.

---

### Tier 7: Lumbergh's Informal "Loop Me In" Gate

**What it does:** Lumbergh reviews all requests over $5K before they route to formal approvers. He replies "Go ahead" before anything moves forward. He frames it as a "courtesy copy" but it functions as a gate.

**Who:** Bill Lumbergh (VP of Operations / Executive Sponsor).

**Why it exists:** Lumbergh asked Peter to "just loop me in on everything over five grand." Not a formal approval step. Not in the workflow documentation. Driven by Lumbergh's desire for visibility, not by a regulatory or operational requirement.

**Classification:** ELECTIVE (informal executive gate)

**Evidence:**
- Lumbergh (Interview): "I told Peter a while back, 'Yeah, just loop me in on everything over five grand.' It's not a formal approval step. It's more of a... courtesy copy. Doesn't slow anything down."
- Peter (Interview 1): "Nothing moves until Lumbergh replies 'Go ahead' — and sometimes that takes two days because he's in meetings or he forgot. But if I skip him and route straight to Samir, Lumbergh finds out later and gets weird about it."
- The Bobs (Interview 1): Listed "the executive 'visibility' loop that Lumbergh requested" as one of the four elective tiers.
- Input Synthesis: "Lumbergh created a gate he doesn't see as a gate. Peter sees it as a 1-2 day delay on every request over $5K."

**Operational reality:** Adds 1-2 days to every request over $5K. Lumbergh doesn't recognize it as a gate. His actual need is visibility (what's moving, how fast, what's stuck), not approval authority. A real-time dashboard would give him what he actually wants without inserting him into the approval chain.

---

## Cross-Reference: Matrix vs. Operational Reality vs. Regulatory Requirements

| # | Tier | Matrix Classification | Regulatory Basis | Operational Reality | Status |
|---|---|---|---|---|---|
| 1 | Requestor Verification | Mandatory | Regulatory control point — requestor auth | Functioning as designed | Active, required |
| 2 | Budget Authority | Mandatory | Regulatory control point — financial auth | Functioning, but inputs are poor (60% incomplete docs) | Active, required |
| 3 | Compliance / COI Check | Mandatory | Regulatory control point — segregation of duties | Partially functioning. Milton does the actual vendor check off-system. PO-splitting bypasses the threshold trigger entirely. | Active but has gaps |
| 4 | Tom's Routing Review | Elective | None | Skipped on 70% of requests by Peter. No consequences from skipping. | Vestigial |
| 5 | Tom's Exception Path | Elective | None (originated from a temp fix) | Over-scoped. Even Tom says it should be narrower. Peter skips it. Real vendor checks done by Milton informally. | Over-scoped, redundant |
| 6 | Samir's Secondary Review | Elective | None (self-imposed below $50K) | Applied uniformly at all dollar amounts despite policy allowing lighter review sub-$50K. Adds ~1 day per request. Root cause is no upstream visibility. | Self-imposed, removable with visibility fix |
| 7 | Lumbergh's Visibility Gate | Elective | None | Functions as a gate despite being framed as "courtesy." Adds 1-2 days per request over $5K. | Informal gate, replaceable with dashboard |

---

## Discrepancies Between What The Bobs Say and What Actually Happens

### Discrepancy 1: The Bobs say 3 tiers are mandatory. The organization operates as if all 7 are.

The Bobs published a control matrix eighteen months ago that clearly delineates mandatory vs. elective controls. Both Bobs confirmed in separate interviews that the regulatory requirement is three control points. Nobody read the matrix. Every stakeholder — including Tom, Samir, and Lumbergh — operates as if their tier is mandatory. The Bobs know the distinction exists but won't recommend removing elective tiers because they don't want to own the risk of that recommendation.

**Bob 1:** "We wouldn't oppose simplification as long as the three mandatory controls remain intact."
**Bob 1 (same interview):** "But we're also not going to be the ones who recommend removing them, because if something goes wrong after a reduction, the first question in any investigation is 'who approved weakening the controls?'"

### Discrepancy 2: The Bobs' audit focuses on documentation completeness, but the system allows incomplete submissions.

The Bobs' top recommendation for five consecutive years has been enforcing documentation completeness at intake. 60% of approved requests have incomplete documentation. The system allows approvals with missing attachments and sends reminder emails that nobody reads. The Bobs have flagged this in three separate audit reports. It has never been prioritized.

**Bob 1:** "If the new system enforced attachment-before-approval, we'd probably eliminate 80% of our quarterly findings overnight."

### Discrepancy 3: The Bobs' compliance check (Tier 3) is being systematically bypassed.

Peter's PO-splitting workaround breaks a single purchase above the threshold into multiple smaller requests, each of which individually falls below the compliance trigger. This bypasses the conflict-of-interest check that only activates on consolidated amounts. The Bobs don't know this is happening. Milton is the only person aware of it, and he's not in any official review chain.

**Milton:** "When you split a PO, it also bypasses the conflict-of-interest check at step 7. That check only triggers on consolidated amounts."

### Discrepancy 4: The Bobs are comfortable with parallel routing and auto-escalation, but neither is configured.

Both Bobs confirmed that parallel routing is acceptable as long as segregation of duties is maintained, and that auto-escalation would help. Michael confirmed the current platform supports both features but none are configured. The Bobs have no objection to modernization — their requirements are "three control points, an audit trail, and documentation."

### Discrepancy 5: Joanna's team bypasses the entire approval chain for sub-$2,500 purchases.

Credit card purchases bypass the approval workflow entirely. No approval trail, no budget verification, no segregation of duties. The Bobs flagged this as a significant concern in Interview 2 but were unaware of the scale until informed during the interview.

**Bob 1:** "That's exactly the kind of thing that keeps us up at night — not that the process is too slow, but that people are routing around it entirely."

---

## Summary Table

| Tier | Origin | Regulatory Basis | Current Status | Recommendation |
|---|---|---|---|---|
| **1. Requestor Verification** | Regulatory framework | Yes — requestor authorization | Active, functioning | KEEP. No changes needed. |
| **2. Budget Authority** | Regulatory framework | Yes — financial authorization | Active, but 60% incomplete docs on input | KEEP. Enforce documentation completeness at intake to improve input quality. |
| **3. Compliance / COI Check** | Regulatory framework | Yes — segregation of duties | Active but has bypass gaps (PO-splitting, Milton off-system) | KEEP. Close the PO-splitting bypass. Formalize Milton's vendor check into the system. |
| **4. Tom's Routing Review** | Tom (2019, original build) | None | Skipped on 70% of requests, no consequences | REMOVE. No evidence this tier catches anything. Peter skips it routinely with zero adverse outcomes. |
| **5. Tom's Exception Path** | Tom (2020, temporary patch) | None | Over-scoped to all vendor requests, even Tom says it should be narrower | REMOVE. The vendor verification function that matters is performed by Milton outside this tier anyway. Formalize Milton's check into Tier 3 instead. |
| **6. Samir's Secondary Review** | Samir (post-2022 incident) | None (policy allows lighter review sub-$50K) | Self-imposed, adds ~1 day/request, root cause is no upstream visibility | REMOVE. Samir described his own fix: upstream visibility receipts. Initiative 3 (Glass Floor) delivers this. Once Samir can see what was verified upstream, this tier has no justification. |
| **7. Lumbergh's Visibility Gate** | Lumbergh (informal request to Peter) | None | Adds 1-2 days/request over $5K, Lumbergh doesn't recognize it as a gate | REMOVE. Replace with a real-time dashboard showing volume, cycle time, and stuck items. Gives Lumbergh what he actually needs (visibility) without the approval chain delay. |

---

## Key Findings

1. **The control matrix is accurate but unused.** The Bobs correctly identified 3 mandatory and 4 elective tiers eighteen months ago. Nobody read it. The cost of ignoring one document is approximately half the approval chain.

2. **All 4 elective tiers are removable, but Tier 3 needs reinforcement.** The three mandatory tiers have real regulatory grounding. However, Tier 3 has active compliance gaps (PO-splitting bypass, Milton operating off-system) that must be closed as part of any simplification.

3. **Tom's two tiers (4 and 5) are the weakest.** Tier 4 is self-referential (Tom reviewing routing to Tom). Tier 5 is an over-scoped temporary patch that even Tom says should be narrower. Peter skips both on 70% of requests with no adverse outcomes.

4. **Samir's tier (6) is a symptom, not a root cause.** He re-verifies upstream work because the system gives him no visibility. Fix the visibility problem (Initiative 3) and this tier removes itself. Samir essentially described this solution in his own interview.

5. **Lumbergh's tier (7) is a gate disguised as a courtesy.** He wants visibility, not approval authority. A dashboard solves his actual need without the 1-2 day delay.

6. **Milton is invisible but critical.** He performs a real function (vendor terms verification) that maps to the regulatory Tier 3, but he's not in the flowchart, not in the RACI, not in the system. Any process redesign must account for his role or reopen the $14K late-fee exposure he originally prevented.

---

## Validation Next Steps

This audit should be shared with The Bobs for confirmation that:
1. The 3 mandatory / 4 elective classification still matches their current control matrix
2. The PO-splitting compliance gap is acknowledged and prioritized
3. Milton's vendor terms function is recognized as part of the Tier 3 compliance control
4. They support the parallel-run approach to validate that elective tier removal produces no compliance divergence
