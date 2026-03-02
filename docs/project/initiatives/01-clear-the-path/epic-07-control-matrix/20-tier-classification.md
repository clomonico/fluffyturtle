# Tier Classification: Mandatory vs. Elective

**Author:** Dallas (Process Designer)
**Date:** 2026-03-02
**Issue:** #20 — Classify each approval tier as mandatory or elective
**Status:** Draft for review
**Source:** Bobs' control matrix, 8 stakeholder interviews, process design (01-process-design.md)

---

## 1. Classification Criteria

Every approval tier in the current 7-tier chain was evaluated against two questions:

1. **Is there a regulatory, legal, or compliance requirement that mandates this control point?**
2. **Does the Bobs' published control matrix identify this tier as a required control?**

A tier is **mandatory** if the answer to either question is yes. Specifically:

| Criterion | What qualifies | Example |
|-----------|---------------|---------|
| **Regulatory mandate** | A law, regulation, or binding policy framework requires this control point for expenditure approvals | Segregation-of-duties requirements for financial transactions |
| **Control matrix inclusion** | The Bobs' published control matrix (governance folder, referenced in 2024 and 2025 audit reports) lists this tier as a required control | Budget authority confirmation, compliance review, financial controls sign-off |
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

| Tier | Name | Owner | What It Checks | Classification | Regulatory Basis | Evidence Summary |
|------|------|-------|----------------|----------------|-----------------|-----------------|
| 1 | Departmental Review | Requesting dept. manager | Manager confirms the request "makes sense" before it enters the chain | **Elective** | None | Tom added as a "quality gate" years ago. No regulatory mandate. Peter skips it. Intake gate replaces this function. |
| 2 | Budget Holder Verification | Budget holder (varies) | Funds availability, budget line alignment, spend authority, delegation limits | **Mandatory** | Control matrix: budget authority confirmation. Regulatory segregation-of-duties requirement. | The Bobs identify this as one of the 3 required control points. Verifies funds exist and spend is authorized. |
| 3 | Secondary Budget Review | Samir Nagheenanajar | Re-verifies everything the budget holder already checked at Tier 2 | **Elective** | None | Samir acknowledged this "isn't formally mandated." He does it because the system shows no upstream verification data. Self-imposed safety net. |
| 4 | Operational Process Check | Tom Smykowski | Tom's legacy workflow step; described as a "routing accuracy review" | **Elective** | None | Tom admitted 2 of his steps are "temporary patches" that became permanent. Peter skips this on 70% of requests with no consequences. Not in the control matrix. |
| 5 | Compliance and Regulatory Review | The Bobs (Internal Audit) | Regulatory requirements, conflict-of-interest rules, vendor compliance, consolidated spend thresholds | **Mandatory** | Control matrix: compliance/regulatory review. Required for segregation of duties and regulatory compliance. | The Bobs perform this step. This is the control that PO-splitting bypasses. Checks against regulatory requirements. |
| 6 | Executive Visibility Gate | Bill Lumbergh | Lumbergh acknowledges receipt of requests over $5K before they proceed | **Elective** | None | Lumbergh calls it a "courtesy copy." He doesn't reject or add review commentary. Peter identifies it as a 1-2 day pure delay. No compliance function. |
| 7 | Financial Controls Sign-off | Finance team (senior reviewer) | Documentation completeness, proper authorization chain, regulatory compliance markers | **Mandatory** | Control matrix: financial controls / segregation-of-duties sign-off. | Final financial controls check. Validates the full audit trail. Required by the Bobs' control matrix. Milton's original recommendation after the $14K late fee incident in 2019. |

**Summary:** 3 mandatory, 4 elective. This matches what the Bobs stated directly: "The regulatory requirement is three approval tiers... The current process has seven. Three are regulatory. Four are elective." (Bobs, Interview 01 and 07)

---

## 3. Mandatory Tiers — Regulatory and Compliance Basis

### Tier 2: Budget Holder Verification (Mandatory)

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

### Tier 5: Compliance and Regulatory Review (Mandatory)

**Regulatory basis:**
- The Bobs' control matrix requires compliance/regulatory review as a mandated control point
- Checks conflict-of-interest rules, vendor compliance status, and consolidated spend thresholds
- Regulatory requirement for segregation of duties between budget authority and compliance verification

**What it protects against:**
- Regulatory violations (transactions that breach applicable rules)
- Conflict-of-interest exposure (approver has undisclosed relationship with vendor)
- Threshold circumvention (PO-splitting to avoid consolidated amount triggers)
- Vendor compliance lapses (contracting with non-compliant vendors)

**Evidence from interviews:**
- Milton (Interview 01, 08): Peter's PO-splitting workaround "bypasses the conflict-of-interest check at step 7 [sic — Milton references step 7 but the compliance check is at Tier 5]. That check only triggers on consolidated amounts."
- The Bobs (Interview 01, 07): This is their direct control point. "We verify that controls are in place, that segregation of duties is maintained."
- Peter (Interview 02): Acknowledges "legal review on anything over $50K" as one of the steps that matters (this maps to the compliance review)

**Consequence of removal:** Material compliance gap. Conflict-of-interest violations go undetected. Regulatory findings in audit. PO-splitting becomes consequence-free.

---

### Tier 7: Financial Controls Sign-off (Mandatory)

**Regulatory basis:**
- The Bobs' control matrix requires a financial controls / segregation-of-duties sign-off as the third mandated control point
- Validates that the full authorization chain is intact and documentation is audit-complete
- Required for regulatory traceability (who approved what, when, with what authority)

**What it protects against:**
- Incomplete audit trails (approvals without documented rationale)
- Authorization chain breaks (steps skipped or approvals by unauthorized parties)
- Documentation gaps (the Bobs flagged 60% of approved requests have incomplete documentation)

**Evidence from interviews:**
- Milton (Interview 01, 08): This step exists because of the 2019 incident where a $120K PO went through with mismatched payment terms, resulting in a $14,000 late fee. Milton recommended the step; Tom took credit.
- The Bobs (Interview 01, 07): "Approximately 60% of approved requests have incomplete supporting documentation." This tier is supposed to catch that.
- Tom (Interview 01, 06): Identifies the audit trail and separation of duties as "non-negotiable" process elements

**Consequence of removal:** Loss of the final financial control checkpoint. Audit trail integrity breaks. The 2019 payment terms incident class of error becomes undetectable.

---

## 4. Elective Tiers — Origins, Ownership, and Evidence for Removability

### Tier 1: Departmental Review (Elective)

**How it was added:** Tom Smykowski added this as a "quality gate" when he designed the workflow in 2019. The intent was to catch "obviously wrong requests before they used approver time."

**Who added it:** Tom Smykowski (process owner)

**Why it's removable:**
- No regulatory mandate. Not in the Bobs' control matrix.
- The intake gate in the redesigned process (01-process-design.md, Section 3) replaces this function with automated validation: nothing enters the queue without complete documentation.
- Peter already skips this step on routine requests with no negative impact.
- The budget holder at Tier 2 (future state) reviews business justification, which is the substantive check this tier was meant to perform.

**Evidence:**
- Tom (Interview 01): Described the 9-step workflow but admitted some steps are "situational additions" and he'd need to "pull up my files" to explain the rationale for each one.
- Peter (Interview 02): "Step 4 is a 'routing accuracy review' — which is literally Tom reviewing whether I routed it to Tom correctly." (Peter conflates step numbers; this describes the pattern of Tom's self-referential review.)
- The Bobs (Interview 07): Confirmed the four elective tiers include "department head review."
- Process design (01-process-design.md): The intake gate enforces documentation completeness at submission, which handles the screening function this tier originally served.

---

### Tier 3: Secondary Budget Review — Samir's Layer (Elective)

**How it was added:** Samir self-imposed this additional review step after a 2022 incident where a vendor contract was approved without budget verification. Policy requires dual sign-off only above $50K, but Samir applies full review to everything over $10K (and often below).

**Who added it:** Samir Nagheenanajar (finance approver), on his own initiative

**Why it's removable:**
- Not policy-mandated. Samir acknowledged: "It's not formally mandated for everything, no." (Interview 01)
- Policy allows lighter touch for sub-$50K requests. Samir applies the same level of review regardless. (Interview 03: "I know the policy technically has a lighter touch for sub-$50K, but I do the same level of review regardless.")
- Samir described his own fix: "If I could see that the budget line was verified at step 2 with a timestamp and a name, I wouldn't need to re-do it at step 5." (Interview 01, 03)
- The redesigned process adds verification records at each tier. Downstream reviewers see what upstream approvers checked. This eliminates the visibility gap that caused Samir to duplicate work.

**Evidence:**
- Samir (Interview 01): Confirmed the secondary review is his personal safeguard, not policy.
- Samir (Interview 03): "My [secondary] check... is something I added because I don't want to be the person who signs off on something that puts us over budget."
- Peter (Interview 02): "Samir adds a second review on anything over $10K even though policy only requires it over $50K. Nobody told him to do that."
- Input Synthesis: "Samir designed his own fix without recognizing it."

---

### Tier 4: Operational Process Check — Tom's Step (Elective)

**How it was added:** Tom added this as a temporary patch during a period when requests had operational gaps. One instance (the 2020 duplicate vendor payments) was a specific fix. It became permanent because "nobody reviews the process design" and Tom considers the workflow his legacy contribution.

**Who added it:** Tom Smykowski (process owner)

**Why it's removable:**
- Tom himself admitted: "The exception path from 2020 — the duplicate vendor thing — that could probably be handled differently. It adds a step for every vendor-related request, not just the ones at risk of duplication." (Interview 06)
- Tom admitted 2 of his 9 steps started as temporary patches (2020, 2022) and became permanent without formal review. (Interview 06)
- Peter skips this step on 70% of requests with no negative consequences. (Interview 02: "I skip steps 4 and 6 on about 70% of requests because they're pointless.")
- The Bobs' control matrix does not include this as a required control point.
- Peter's description: "Step four — where the 'process owner' reviews for 'routing accuracy.' That's Tom checking that I filled out the form correctly. I fill out the form. I am the routing." (Interview 02)

**Evidence:**
- Tom (Interview 01, 06): Admitted the steps are "situational additions" and he can't explain the rationale for every step from memory.
- Peter (Interview 02): Skips this step routinely with zero consequence.
- The Bobs (Interview 01, 07): Confirmed this is one of the four elective tiers.
- Input Synthesis: "Tom's temporary patches that became permanent."

---

### Tier 6: Executive Visibility Gate — Lumbergh's Step (Elective)

**How it was added:** Lumbergh told Peter to "loop me in on everything over five grand." He frames it as a "courtesy copy" but nothing moves until he acknowledges receipt. This was an informal management request, not a policy or compliance decision.

**Who added it:** Bill Lumbergh (VP of Operations), informally

**Why it's removable:**
- Lumbergh doesn't reject requests, doesn't add review commentary, and doesn't perform any compliance or financial function.
- His need is visibility, not approval authority. He said: "I don't always need to formally approve, but I want to see it." (Interview 01)
- Peter identified this as a pure delay: "Nothing moves until Lumbergh replies 'Go ahead' — and sometimes that takes two days." (Interview 02)
- The Lumbergh Dashboard (01-process-design.md, Section 4) gives him better visibility than the current gate (real-time, with metrics, with alerts) without blocking anything.
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
| **Tier 1: Departmental Review** | Requests enter the queue without a manager gut-check. | Poorly justified requests reach Tier 2. | Intake gate enforces documentation completeness. Budget holder reviews business justification at Tier 2. Automated validation replaces the subjective manager review. |
| **Tier 3: Secondary Budget Review** | No one re-verifies what the budget holder checked. | If the budget holder makes an error, it won't be caught until Tier 7 (Financial Controls). | Verification records at each tier make upstream work visible. Tier 7 confirms the authorization chain is intact. Samir himself said he'd stop re-checking if he could see upstream verification. |
| **Tier 4: Operational Process Check** | Tom's routing accuracy review and exception path checks stop. | Duplicate vendor payments or routing errors could slip through. | The duplicate vendor check maps to compliance controls at Tier 5. The intake gate validates routing inputs. Peter already skips this 70% of the time with no issues. Tom acknowledged the 2020 exception path "could probably be handled differently." |
| **Tier 6: Executive Visibility Gate** | Lumbergh no longer acknowledges requests before they proceed. | Leadership loses real-time awareness of what's moving through the pipeline. | The Lumbergh Dashboard provides real-time visibility with alerts, SLA tracking, and drill-down into request details. Better visibility than the current gate, with zero blocking. |

**Net impact of removing all 4 elective tiers:**
- Estimated cycle time reduction: 4-8 business days per request (removing queue time at 4 non-value-adding steps)
- Current max end-to-end with 3 mandatory tiers: 7 business days (2+3+2 with SLAs)
- Compliance posture: unchanged or improved (the 3 regulatory control points remain, documentation enforced at intake, verification records added)
- Audit trail: stronger (verification records at each tier, immutable log, documentation-before-approval enforcement)

---

## 6. Request Type Matrix

Different request types flow through the same 7-tier chain today. The table below maps each request type to which tiers it actually needs.

| Request Type | Current Path (7 tiers) | Required Path (mandatory only) | Notes |
|-------------|----------------------|-------------------------------|-------|
| **Procurement (standard)** | All 7 tiers, sequential | Tier 2 (Budget) → Tier 5 (Compliance) → Tier 7 (Financial Controls) | Standard goods/services purchase. Full regulatory chain required. |
| **Procurement (>$50K)** | All 7 tiers + Milton's off-chart vendor terms check | Tier 2 → Tier 5 → Tier 7 + vendor terms validation (formalized from Milton's spreadsheet) | High-value. Currently also triggers Lumbergh's informal gate at $5K. Milton's vendor terms check must be formalized into Tier 5 or Tier 7. |
| **Vendor contract (new vendor)** | All 7 tiers | Tier 2 → Tier 5 → Tier 7 | New vendor compliance verification is critical at Tier 5. 47 vendors with non-standard terms tracked only by Milton. |
| **Vendor contract (renewal, unchanged terms)** | All 7 tiers (no differentiation) | Tier 2 → Tier 5 → Tier 7 (candidate for lighter review under Smart Routing, Initiative 2) | Joanna's $1,200 Slack renewal goes through full 7-tier review. Same vendor, same terms, same amount annually. |
| **Budget reallocation** | All 7 tiers | Tier 2 → Tier 7 | Internal fund movement. Compliance review (Tier 5) may not apply if no external vendor or regulatory trigger. |
| **Software license (new)** | All 7 tiers | Tier 2 → Tier 5 → Tier 7 | New software may have compliance implications (data handling, security). Full chain appropriate. |
| **Software license (renewal)** | All 7 tiers (no differentiation) | Tier 2 → Tier 7 (candidate for lighter review under Smart Routing) | Repeat purchase with no change. Compliance review at Tier 5 adds limited value for unchanged renewals. |
| **Capital expenditure (>$50K)** | All 7 tiers + exception routing | Tier 2 → Tier 5 → Tier 7 | Milton noted capex crossing fiscal year boundaries needs dual sign-off (added post-2019). Exception handling covers this. |
| **Capital expenditure (cross-fiscal-year)** | All 7 tiers + exception routing + dual sign-off | Tier 2 → Tier 5 → Tier 7 + exception escalation for dual sign-off | Milton's institutional knowledge: dual sign-off required "because of what happened." Must be codified into exception handling rules. |
| **Cross-departmental resource request** | All 7 tiers + both dept heads + resource allocation review | Tier 2 → Tier 7 (if no compliance trigger) | Joanna's team stopped submitting these 6 months ago. Last one took 23 days. Currently handled through informal handshake deals with no paper trail. |
| **Facilities request** | Separate SharePoint list (Linda in Facilities) — bypasses main system entirely | Must be absorbed into the main system (Initiative 4: One Front Door) | 15% of approval volume runs through this shadow system. No audit trail beyond SharePoint modified-by/modified-date. IT and compliance unaware. |
| **Credit card purchases (<$2,500)** | Bypasses all tiers entirely | Needs a lightweight approval path (Initiative 4: One Front Door) | Joanna's team uses credit cards to bypass procurement. Untracked financial exposure. |

---

## 7. Key Findings and Recommendations

### The control matrix is the source of truth
The Bobs published the mandatory/elective distinction 18 months ago. Every stakeholder operates as if all 7 tiers are required. Three are. The cost of not reading one document is potentially half the approval chain. Going forward, the control matrix should be treated as the literal configuration source for which tiers exist (brainstorm idea #10, #21).

### Elective tiers accumulated through three patterns
1. **One-time incident response:** Tom's 2020 exception path and the 2022 verification checkpoint were added as temporary fixes after specific failures. Neither was reviewed for ongoing necessity.
2. **Personal risk aversion:** Samir's secondary review exists because he's afraid of approving something that "blows up in front of the CFO." The policy doesn't require it; his anxiety does.
3. **Informal management preference:** Lumbergh's "courtesy copy" gate was never a formal policy decision. It was one verbal instruction to Peter that became a blocking step on every request over $5K.

### Removal is safe because mitigations already exist
Each elective tier's function is either replaced by the intake gate (Tier 1), made unnecessary by verification records and upstream visibility (Tier 3), already being skipped without consequence (Tier 4), or replaced by a non-blocking dashboard (Tier 6). No elective tier removal creates an unmitigated gap.

### Shadow processes need separate treatment
Two request types (facilities approvals, credit card purchases) bypass the entire approval chain. These are not tier classification issues; they are scope for Initiative 4 (One Front Door). However, their existence confirms that the current 7-tier process is so burdensome that 15%+ of volume has already routed around it entirely.

---

## Appendix: Evidence Index

| Source | Interview Date | Key Evidence |
|--------|---------------|--------------|
| The Bobs (Interview 01) | 2026-03-02 | "3 of 7 tiers are regulatory. Four are elective. We documented this in the control matrix." |
| The Bobs (Interview 07) | 2026-03-02 | "Three. That's the regulatory floor. Anything beyond that is organizational policy — internally imposed controls." |
| Tom Smykowski (Interview 01) | 2026-03-02 | Admitted 2 steps are temporary patches; can't recall all rationale from memory. |
| Tom Smykowski (Interview 06) | 2026-03-02 | "The exception path from 2020... could probably be handled differently." |
| Samir Nagheenanajar (Interview 01) | 2026-03-02 | "It's not formally mandated for everything, no." (re: his secondary review) |
| Samir Nagheenanajar (Interview 03) | 2026-03-02 | "If I could see that the budget line was verified at step 2... I wouldn't need to re-do it at step 5." |
| Peter Gibbons (Interview 02) | 2026-03-02 | Skips steps 4 and 6 on 70% of requests. "The other six steps are Tom making sure he has a reason to be in meetings." |
| Peter Gibbons (Engagement Interview) | 2026-03-02 | "Cut the chain from nine approvers to three. Legal, finance, department head." |
| Bill Lumbergh (Interview 01) | 2026-03-02 | "It's not a formal approval step. It's more of a... courtesy copy." |
| Joanna (Interview 01) | 2026-03-02 | "Half the steps in this workflow aren't there because a regulation requires them. They're there because somebody once got nervous." |
| Joanna (Interview 05) | 2026-03-02 | Uses credit cards for sub-$2,500 purchases. Handles cross-dept requests through informal deals. |
| Milton Waddams (Interview 01) | 2026-03-02 | 47 vendor exceptions in personal spreadsheet. Facilities SharePoint list handles 15% of volume. |
| Milton Waddams (Interview 08) | 2026-03-02 | PO-splitting bypasses conflict-of-interest check. "Nobody documents that I'm in the process." |
| Input Synthesis | 2026-03-02 | "Only 3 of 7 approval tiers are regulatory... The Bobs' control matrix is the Rosetta Stone nobody is reading." |
