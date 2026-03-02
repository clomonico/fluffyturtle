# Tier Classification: Mandatory vs. Elective

**Author:** Dallas (Process Designer)
**Date:** 2026-03-02
**Issue:** #20 — Classify each approval tier as mandatory or elective
**Status:** Revised — aligned with approved control matrix audit (PR #57, Kane review)
**Source:** Bobs' control matrix, 8 stakeholder interviews, process design (01-process-design.md), control matrix audit (19-control-matrix-audit.md)

> **Revision note:** Tier numbering, names, and mandatory/elective classifications in this document now match the approved control matrix audit (issue #19, PR #57). Previous draft used a different tier ordering; this revision adopts the canonical 7-tier numbering from the audit.

---

## 1. Classification Criteria

Every approval tier in the current 7-tier chain was evaluated against two questions:

1. **Is there a regulatory, legal, or compliance requirement that mandates this control point?**
2. **Does the Bobs' published control matrix identify this tier as a required control?**

A tier is **mandatory** if the answer to either question is yes. Specifically:

| Criterion | What qualifies | Example |
|-----------|---------------|---------|
| **Regulatory mandate** | A law, regulation, or binding policy framework requires this control point for expenditure approvals | Segregation-of-duties requirements for financial transactions |
| **Control matrix inclusion** | The Bobs' published control matrix (governance folder, referenced in 2024 and 2025 audit reports) lists this tier as a required control | Budget authority confirmation, compliance review, requestor verification |
| **Audit dependency** | Removal would create a gap that internal or external audit would flag as a finding | Traceability of who approved what, when, with what authority |

A tier is **elective** if:

| Criterion | What qualifies | Example |
|-----------|---------------|---------|
| **No regulatory basis** | No law, regulation, or binding policy requires this step | Departmental "quality gate" added by a process owner |
| **Not in the control matrix** | The Bobs' control matrix does not list it as a mandatory control | Executive visibility acknowledgment, secondary budget re-verification |
| **Organizational habit** | Added in response to a one-time incident, personal risk aversion, or management preference without formal policy review | Tom's temporary patches that became permanent, Samir's self-imposed secondary review |
| **Safely bypassed today** | Stakeholders already skip this tier routinely without negative consequences | Peter skips Tier 4 on 70% of requests with no impact |

**Source for the mandatory/elective distinction:** The Bobs confirmed in both interview rounds that "the regulatory framework requires three control points for expenditure approvals: requestor verification, budget authority confirmation, and segregation-of-duties sign-off" (Interview 01, Interview 07). Their control matrix, published 18 months ago, delineates mandatory versus elective controls. Nobody reads it (Bob 1: "Nobody reads it." Bob 2: "Nobody reads it.").

---

## 2. Tier Classification Table

> **Numbering follows the approved control matrix audit (19-control-matrix-audit.md).** Tiers 1-3 are the Bobs' three mandatory regulatory control points. Tiers 4-7 are organizationally added tiers classified as elective.

| Tier | Name | Owner | What It Checks | Classification | Regulatory Basis | Evidence Summary |
|------|------|-------|----------------|----------------|-----------------|-----------------|
| 1 | Requestor Verification | Department head (varies) | Confirms the requestor has authority to submit the request and that it aligns with departmental need | **Mandatory** | Regulatory control point. First of the Bobs' three mandatory controls. Ensures segregation of duties at the point of origin. | The Bobs identify this as one of the 3 required control points. |
| 2 | Budget Authority Confirmation | Finance approver (Samir's function) | Verifies budget codes, allocation, cost center, available funding. Confirms spend is within approved budget and properly categorized. | **Mandatory** | Regulatory control point. Second of the Bobs' three mandatory controls. | The Bobs identify this as one of the 3 required control points. Samir references the 2022 $40K overrun. |
| 3 | Compliance / Conflict-of-Interest Check | Compliance function (Bobs' audit trail) | Segregation of duties, conflict-of-interest conditions, vendor compliance on consolidated amounts. Includes Milton's vendor terms verification function. | **Mandatory** | Regulatory control point. Third of the Bobs' three mandatory controls. Triggers on POs above $50K or flagged vendors. | Milton performs vendor terms verification (47 vendors) but operates off-system. PO-splitting bypasses this check. Must be formalized. |
| 4 | Tom's Routing Accuracy Review | Tom Smykowski | Reviews whether requests were routed correctly and whether the process was followed | **Elective** | None. Added by Tom in 2019 when he built the original workflow. | Peter skips this on 70% of requests with no consequences. Self-referential. Not in the control matrix. |
| 5 | Tom's Exception Routing Path | Tom Smykowski (added it), Milton (actually performs checks) | Temporary exception path from 2020 for duplicate vendor payments, applied to all vendor requests regardless of duplication risk | **Elective** | None. Originated as a temporary fix in 2020. | Over-scoped. Even Tom says it should be narrower. Real vendor checks done by Milton informally outside this tier. |
| 6 | Samir's Secondary Review | Samir Nagheenanajar | Re-verifies everything the budget holder already checked at Tier 2. Peer review on anything over $10K despite policy only requiring dual sign-off above $50K. | **Elective** | None. Self-imposed by Samir after a 2022 incident. Policy allows lighter review sub-$50K. | Samir acknowledged this "isn't formally mandated." Adds ~1 day per request. Root cause is no upstream visibility. |
| 7 | Lumbergh's Informal Visibility Gate | Bill Lumbergh | Lumbergh acknowledges receipt of requests over $5K before they proceed. Framed as "courtesy copy" but functions as a gate. | **Elective** | None. Informal executive request, never documented as a formal approval step. | Adds 1-2 days per request. Peter identifies it as a pure delay. No compliance function. |

**Summary:** 3 mandatory (Tiers 1-3), 4 elective (Tiers 4-7). This matches what the Bobs stated directly: "The regulatory requirement is three approval tiers... The current process has seven. Three are regulatory. Four are elective." (Bobs, Interview 01 and 07)

---

## 3. Mandatory Tiers — Regulatory and Compliance Basis

### Tier 1: Requestor Verification (Mandatory)

**Regulatory basis:**
- The Bobs' control matrix requires "requestor verification" as one of the three mandated control points
- Ensures the requestor has authority to submit the request and that it aligns with departmental need
- Segregation of duties at the point of origin

**What it protects against:**
- Unauthorized requests entering the approval chain
- Requests submitted without proper departmental authority
- Segregation-of-duties violation at the point of origin

**Evidence from interviews:**
- The Bobs (Interview 01): "The regulatory framework requires three control points for expenditure approvals: requestor verification, budget authority confirmation, and segregation-of-duties sign-off."
- The Bobs (Interview 07): "Three. That's the regulatory floor."
- Peter (Interview 02): Independently arrived at the same minimum: "Give me three approvers max: budget holder, compliance if it hits the threshold, and done."

**Consequence of removal:** Direct regulatory violation. Loss of first control point in the segregation-of-duties chain.

---

### Tier 2: Budget Authority Confirmation (Mandatory)

**Regulatory basis:**
- The Bobs' control matrix requires "budget authority confirmation" as one of the three mandated control points
- Segregation-of-duties requirement: the person requesting funds cannot be the same person verifying budget availability
- Financial governance frameworks require verification that funds exist and the spend falls within delegated authority limits

**What it protects against:**
- Unauthorized spend (spending without budget holder knowledge)
- Budget overruns (committing funds that don't exist in the budget line)
- Delegation breaches (approving spend above one's authority threshold)

**Evidence from interviews:**
- Samir (Interview 03): "Two years ago someone approved a vendor contract without checking that we had budget remaining in that line item. We were $40,000 over by the time anyone noticed."
- The Bobs (Interview 01, 07): Identified budget authority confirmation as one of three required control points
- Peter (Interview 02): Acknowledges "finance sign-off on anything hitting a new budget line" as one of the steps that "actually matters"

**Consequence of removal:** Direct regulatory and compliance violation. Budget overruns, unauthorized spend, audit findings on segregation of duties.

---

### Tier 3: Compliance / Conflict-of-Interest Check (Mandatory)

**Regulatory basis:**
- The Bobs' control matrix requires compliance/regulatory review as a mandated control point
- Checks conflict-of-interest rules, vendor compliance status, and consolidated spend thresholds
- Regulatory requirement for segregation of duties between budget authority and compliance verification
- Includes Milton's vendor terms verification function (47 vendors with non-standard terms, triggers on POs above $50K or flagged vendors)

**What it protects against:**
- Regulatory violations (transactions that breach applicable rules)
- Conflict-of-interest exposure (approver has undisclosed relationship with vendor)
- Threshold circumvention (PO-splitting to avoid consolidated amount triggers)
- Vendor compliance lapses (contracting with non-compliant vendors)
- Payment terms mismatch ($14K late fee incident in 2019 that Milton's check was designed to prevent)

**Evidence from interviews:**
- Milton (Interview 01, 08): Peter's PO-splitting workaround "bypasses the conflict-of-interest check at step 7. That check only triggers on consolidated amounts." Milton performs vendor terms verification for 47 vendors with non-standard terms using a personal spreadsheet. He is not in the flowchart, not in the RACI, not in the system.
- The Bobs (Interview 01, 07): This is their direct control point. "We verify that controls are in place, that segregation of duties is maintained."
- Peter (Interview 02): Acknowledges "legal review on anything over $50K" as one of the steps that matters

**Structural gap:** Milton performs a critical function (vendor terms verification) that maps to this tier, but he operates entirely off-system through informal email routing from Peter. Any process redesign must formalize Milton's role into this tier or reopen the $14K late-fee exposure he originally prevented.

**Consequence of removal:** Material compliance gap. Conflict-of-interest violations go undetected. Regulatory findings in audit. PO-splitting becomes consequence-free.

---

## 4. Elective Tiers — Origins, Ownership, and Evidence for Removability

### Tier 4: Tom's Routing Accuracy Review (Elective)

**How it was added:** Tom Smykowski added this when he built the original workflow in 2019. Originally designed as a quality gate over the routing logic.

**Who added it:** Tom Smykowski (process owner)

**Why it's removable:**
- No regulatory mandate. Not in the Bobs' control matrix.
- Self-referential: "Tom reviewing whether Peter routed it to Tom correctly" (Peter, Interview 02)
- Peter already skips this step on 70% of requests with no negative impact
- The intake gate in the redesigned process replaces the screening function

**Evidence:**
- Tom (Interview 01): Described the 9-step workflow but admitted some steps are "situational additions" and he'd need to "pull up my files" to explain the rationale.
- Peter (Interview 02): "Step four, where the 'process owner' reviews for 'routing accuracy.' That's Tom checking that I filled out the form correctly. I fill out the form. I am the routing."
- The Bobs (Interview 01, 07): Confirmed this is one of the four elective tiers.
- Peter (Interview 02): Skips this step routinely with zero consequence.

---

### Tier 5: Tom's Exception Routing Path (Elective)

**How it was added:** Added as a temporary exception path in 2020 after a duplicate vendor payment incident. Routes all vendor-related requests through an extra check. Originally scoped to catch duplications, but applied to all vendor requests regardless of duplication risk.

**Who added it:** Tom Smykowski (took credit for the fix)

**Why it's removable:**
- Tom himself admitted: "The exception path from 2020... could probably be handled differently. It adds a step for every vendor-related request, not just the ones at risk of duplication." (Interview 06)
- Tom admitted 2 of his steps started as temporary patches (2020, 2022) and became permanent without formal review
- Peter skips this step on 70% of requests with no negative consequences
- The actual vendor verification is done by Milton outside the system anyway
- The Bobs' control matrix does not include this as a required control point

**Evidence:**
- Tom (Interview 06): Admitted the 2020 exception path "could probably be handled differently"
- Milton (Interview 08): "The 2020 exception routing? That was also... I told him about the duplicate vendor payments in September. He added the exception path in November and presented it as his quarterly improvement initiative."
- Peter (Interview 02): Skips this step routinely with zero consequence
- The Bobs (Interview 01, 07): Confirmed this is one of the four elective tiers

---

### Tier 6: Samir's Secondary Review (Elective)

**How it was added:** Samir self-imposed this additional review step after a 2022 incident where a vendor contract was approved without budget verification. Policy requires dual sign-off only above $50K, but Samir applies full review to everything over $10K (and often below).

**Who added it:** Samir Nagheenanajar (finance approver), on his own initiative

**Why it's removable:**
- Not policy-mandated. Samir acknowledged: "It's not formally mandated for everything, no." (Interview 01)
- Policy allows lighter touch for sub-$50K requests. Samir applies the same level of review regardless. (Interview 03)
- Samir described his own fix: "If I could see that the budget line was verified at step 2 with a timestamp and a name, I wouldn't need to re-do it at step 5." (Interview 01, 03)
- The redesigned process adds verification records at each tier, eliminating the visibility gap
- Root cause is no upstream visibility, addressed by Initiative 3 (Glass Floor)

**Evidence:**
- Samir (Interview 01): Confirmed the secondary review is his personal safeguard, not policy.
- Samir (Interview 03): "My check... is something I added because I don't want to be the person who signs off on something that puts us over budget."
- Peter (Interview 02): "Samir adds a second review on anything over $10K even though policy only requires it over $50K. Nobody told him to do that."
- Input Synthesis: "Samir designed his own fix without recognizing it."

---

### Tier 7: Lumbergh's Informal Visibility Gate (Elective)

**How it was added:** Lumbergh told Peter to "loop me in on everything over five grand." He frames it as a "courtesy copy" but nothing moves until he acknowledges receipt. This was an informal management request, not a policy or compliance decision.

**Who added it:** Bill Lumbergh (VP of Operations), informally

**Why it's removable:**
- Lumbergh doesn't reject requests, doesn't add review commentary, and doesn't perform any compliance or financial function
- His need is visibility, not approval authority. He said: "I don't always need to formally approve, but I want to see it." (Interview 01)
- Peter identified this as a pure delay: "Nothing moves until Lumbergh replies 'Go ahead' — and sometimes that takes two days." (Interview 02)
- The Lumbergh Dashboard gives him better visibility than the current gate (real-time, with metrics, with alerts) without blocking anything
- Lumbergh himself would accept the dashboard: his stated need is "visibility" and "a dashboard that shows me where everything is." (Interview 01)

**Evidence:**
- Lumbergh (Interview 01): "It's not a formal approval step. It's more of a... courtesy copy."
- Peter (Interview 02): "Lumbergh doesn't know how the process actually works. He thinks his 'just loop me in' emails are a courtesy. They're not. They're a gate."
- Input Synthesis: "Lumbergh created a gate he doesn't see as a gate. Peter sees it as a 1-2 day delay on every request over $5K."
- The Bobs (Interview 07): Confirmed this is one of the four elective tiers ("the executive 'visibility' loop that Lumbergh requested").

---

## 5. Impact Assessment: What Happens If Each Elective Tier Is Removed?

| Elective Tier | What Happens If Removed | Residual Risk | Mitigation Already in Place |
|---------------|------------------------|---------------|---------------------------|
| **Tier 4: Tom's Routing Review** | Tom's routing accuracy review stops. | Routing errors could slip through. | The intake gate validates routing inputs. Peter already skips this 70% of the time with no issues. |
| **Tier 5: Tom's Exception Path** | The over-scoped vendor exception routing stops. | Duplicate vendor payments could recur. | The vendor check maps to compliance controls at Tier 3. Milton performs the actual vendor verification off-system. Tom acknowledged the 2020 exception path "could probably be handled differently." |
| **Tier 6: Samir's Secondary Review** | No one re-verifies what the budget authority confirmed at Tier 2. | If the budget authority makes an error, it won't be caught until downstream. | Verification records at each tier make upstream work visible. Samir himself said he'd stop re-checking if he could see upstream verification. Initiative 3 (Glass Floor) delivers the visibility fix. |
| **Tier 7: Lumbergh's Visibility Gate** | Lumbergh no longer acknowledges requests before they proceed. | Leadership loses real-time awareness of the pipeline. | The Lumbergh Dashboard provides real-time visibility with alerts, SLA tracking, and drill-down. Better visibility than the current gate, with zero blocking. |

**Net impact of removing all 4 elective tiers:**
- Estimated cycle time reduction: 4-8 business days per request (removing queue time at 4 non-value-adding steps)
- Current max end-to-end with 3 mandatory tiers: 7 business days (2+3+2 with SLAs)
- Compliance posture: unchanged or improved (the 3 regulatory control points remain, documentation enforced at intake, verification records added)
- Audit trail: stronger (verification records at each tier, immutable log, documentation-before-approval enforcement)

---

## 6. Request Type Matrix

Different request types flow through the same 7-tier chain today. The table below maps each request type to which mandatory tiers it actually needs.

| Request Type | Current Path (7 tiers) | Required Path (mandatory only) | Notes |
|-------------|----------------------|-------------------------------|-------|
| **Procurement (standard)** | All 7 tiers, sequential | Tier 1 (Requestor) -> Tier 2 (Budget) -> Tier 3 (Compliance) | Standard goods/services purchase. Full regulatory chain required. |
| **Procurement (>$50K)** | All 7 tiers + Milton's off-chart vendor terms check | Tier 1 -> Tier 2 -> Tier 3 + vendor terms validation (formalized from Milton's spreadsheet) | High-value. Milton's vendor terms check must be formalized into Tier 3. |
| **Vendor contract (new vendor)** | All 7 tiers | Tier 1 -> Tier 2 -> Tier 3 | New vendor compliance verification is critical at Tier 3. 47 vendors with non-standard terms tracked only by Milton. |
| **Vendor contract (renewal, unchanged terms)** | All 7 tiers (no differentiation) | Tier 1 -> Tier 2 -> Tier 3 (candidate for lighter review under Smart Routing, Initiative 2) | Joanna's $1,200 Slack renewal goes through full 7-tier review. Same vendor, same terms, same amount annually. |
| **Budget reallocation** | All 7 tiers | Tier 1 -> Tier 2 | Internal fund movement. Compliance review (Tier 3) may not apply if no external vendor or regulatory trigger. |
| **Software license (new)** | All 7 tiers | Tier 1 -> Tier 2 -> Tier 3 | New software may have compliance implications (data handling, security). Full chain appropriate. |
| **Software license (renewal)** | All 7 tiers (no differentiation) | Tier 1 -> Tier 2 (candidate for lighter review under Smart Routing) | Repeat purchase with no change. Compliance review at Tier 3 adds limited value for unchanged renewals. |
| **Capital expenditure (>$50K)** | All 7 tiers + exception routing | Tier 1 -> Tier 2 -> Tier 3 | Milton noted capex crossing fiscal year boundaries needs dual sign-off. Exception handling covers this. |
| **Capital expenditure (cross-fiscal-year)** | All 7 tiers + exception routing + dual sign-off | Tier 1 -> Tier 2 -> Tier 3 + exception escalation for dual sign-off | Milton's institutional knowledge: dual sign-off required "because of what happened." Must be codified. |
| **Cross-departmental resource request** | All 7 tiers + both dept heads + resource allocation review | Tier 1 -> Tier 2 (if no compliance trigger) | Joanna's team stopped submitting these 6 months ago. Last one took 23 days. |
| **Facilities request** | Separate SharePoint list (Linda in Facilities), bypasses main system entirely | Must be absorbed into the main system (Initiative 4: One Front Door) | 15% of approval volume runs through this shadow system. No audit trail. |
| **Credit card purchases (<$2,500)** | Bypasses all tiers entirely | Needs a lightweight approval path (Initiative 4: One Front Door) | Joanna's team uses credit cards to bypass procurement. Untracked financial exposure. |

---

## 7. Key Findings and Recommendations

### The control matrix is the source of truth
The Bobs published the mandatory/elective distinction 18 months ago. Every stakeholder operates as if all 7 tiers are required. Three are. The cost of not reading one document is potentially half the approval chain. Going forward, the control matrix should be treated as the literal configuration source for which tiers exist.

### Elective tiers accumulated through three patterns
1. **One-time incident response:** Tom's 2020 exception path and the 2022 verification checkpoint were added as temporary fixes after specific failures. Neither was reviewed for ongoing necessity.
2. **Personal risk aversion:** Samir's secondary review exists because he's afraid of approving something that "blows up in front of the CFO." The policy doesn't require it; his anxiety does.
3. **Informal management preference:** Lumbergh's "courtesy copy" gate was never a formal policy decision. It was one verbal instruction to Peter that became a blocking step on every request over $5K.

### Removal is safe because mitigations already exist
Each elective tier's function is either already being skipped without consequence (Tier 4), a duplicate of Tier 3's actual function (Tier 5), made unnecessary by upstream visibility (Tier 6), or replaced by a non-blocking dashboard (Tier 7). No elective tier removal creates an unmitigated gap.

### Milton's function must be formalized into Tier 3
Milton performs vendor terms verification for 47 vendors with non-standard terms using a personal spreadsheet. This function maps to the mandatory compliance check at Tier 3 but operates entirely off-system. Any process redesign must bring Milton's check into the system or reopen the $14K late-fee exposure he originally prevented.

### Shadow processes need separate treatment
Two request types (facilities approvals, credit card purchases) bypass the entire approval chain. These are not tier classification issues; they are scope for Initiative 4 (One Front Door). However, their existence confirms that the current 7-tier process is so burdensome that 15%+ of volume has already routed around it entirely.

---

## Appendix: Evidence Index

| Source | Interview Date | Key Evidence |
|--------|---------------|--------------|
| The Bobs (Interview 01) | 2026-03-02 | "3 of 7 tiers are regulatory. Four are elective. We documented this in the control matrix." |
| The Bobs (Interview 07) | 2026-03-02 | "Three. That's the regulatory floor. Anything beyond that is organizational policy, internally imposed controls." |
| Tom Smykowski (Interview 01) | 2026-03-02 | Admitted 2 steps are temporary patches; can't recall all rationale from memory. |
| Tom Smykowski (Interview 06) | 2026-03-02 | "The exception path from 2020... could probably be handled differently." |
| Samir Nagheenanajar (Interview 01) | 2026-03-02 | "It's not formally mandated for everything, no." (re: his secondary review) |
| Samir Nagheenanajar (Interview 03) | 2026-03-02 | "If I could see that the budget line was verified at step 2... I wouldn't need to re-do it at step 5." |
| Peter Gibbons (Interview 02) | 2026-03-02 | Skips steps 4 and 6 on 70% of requests. |
| Peter Gibbons (Engagement Interview) | 2026-03-02 | "Cut the chain from nine approvers to three. Legal, finance, department head." |
| Bill Lumbergh (Interview 01) | 2026-03-02 | "It's not a formal approval step. It's more of a... courtesy copy." |
| Joanna (Interview 01) | 2026-03-02 | "Half the steps in this workflow aren't there because a regulation requires them. They're there because somebody once got nervous." |
| Joanna (Interview 05) | 2026-03-02 | Uses credit cards for sub-$2,500 purchases. Handles cross-dept requests through informal deals. |
| Milton Waddams (Interview 01) | 2026-03-02 | 47 vendor exceptions in personal spreadsheet. Facilities SharePoint list handles 15% of volume. |
| Milton Waddams (Interview 08) | 2026-03-02 | PO-splitting bypasses conflict-of-interest check. "Nobody documents that I'm in the process." |
| Input Synthesis | 2026-03-02 | "Only 3 of 7 approval tiers are regulatory... The Bobs' control matrix is the Rosetta Stone nobody is reading." |
