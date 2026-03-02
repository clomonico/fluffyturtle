# 33 — Upstream Verification Evidence Display

**Initiative:** Glass Floor (Init-2)
**Epic:** 11 — Verification Chain
**Story:** As an approver (Samir), I want to see exactly what upstream approvers already verified before I start my review, so that I can focus on what's uncovered instead of re-checking everything from scratch.
**Dependencies:** Verification receipts (#31), Decision log (#32)
**Date:** 2026-03-02

---

## 1. User Experience Design

### Samir's Problem (in his words)

> "If I could see that the budget line was verified at step 2 with a timestamp and a name, I wouldn't need to re-do it at step 5."

Today, downstream approvers open a request and start from scratch. They have no visibility into what upstream tiers already validated. This creates redundant effort and longer cycle times.

### Design Principle

Upstream evidence is the first thing an approver sees when opening a request — not after they click through menus, not at the bottom of a page, and not in a separate tab they have to discover. Evidence shows up front, before the review begins.

### Post-Init-1 Approval Tiers

| Tier | Role | Responsibility |
|------|------|----------------|
| 1 | Dept Head | Requestor Verification — confirms requestor identity, need, and departmental authority |
| 2 | Samir (Budget Authority) | Budget Confirmation — validates funding source, amount, and budget availability |
| 3 | Compliance / COI Check | Compliance review including Milton's vendor terms, conflict-of-interest screening |

### What Each Tier Sees

- **Tier 1 (Dept Head):** No upstream evidence — this is the originating tier. Sees the raw request only.
- **Tier 2 (Samir):** Sees Tier 1's verification receipt before starting budget review.
- **Tier 3 (Compliance):** Sees Tier 1 and Tier 2 verification receipts before starting compliance review.

---

## 2. Verification Chain Display Layout

### 2.1 Evidence Panel Placement

The upstream evidence panel appears at the top of the approval screen, above the request details and above the approver's own action buttons. The panel is expanded by default on first load.

```
┌─────────────────────────────────────────────────────┐
│  📋 UPSTREAM VERIFICATION EVIDENCE                  │
│  2 of 3 tiers complete · Last verified 14 min ago   │
│                                                     │
│  ┌─ Tier 1: Requestor Verification ──────────────┐  │
│  │  ✅ Verified by: J. Martinez (Dept Head)      │  │
│  │  📅 2026-03-02 09:14 AM                       │  │
│  │  Checked: Requestor identity, departmental    │  │
│  │  authority, business need documented           │  │
│  │  Evidence: [Purchase justification form]       │  │
│  │  Receipt ID: VR-20260302-0914-MART            │  │
│  └───────────────────────────────────────────────┘  │
│                                                     │
│  ┌─ Tier 2: Budget Confirmation ─────────────────┐  │
│  │  ✅ Verified by: Samir Nagheenanajar          │  │
│  │  📅 2026-03-02 10:31 AM                       │  │
│  │  Checked: Budget line 4200-OPS confirmed,     │  │
│  │  $12,400 available of $15,000 allocation      │  │
│  │  Evidence: [Budget snapshot] [GL extract]      │  │
│  │  Receipt ID: VR-20260302-1031-NAGH            │  │
│  └───────────────────────────────────────────────┘  │
│                                                     │
│  ─── You are reviewing as Tier 3 (Compliance) ───   │
│                                                     │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│  REQUEST DETAILS                                    │
│  ...                                                │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│  YOUR REVIEW ACTIONS                                │
│  [ ] Mark checks complete   [Approve] [Return]      │
└─────────────────────────────────────────────────────┘
```

### 2.2 Receipt Card Anatomy

Each upstream receipt card contains:

| Field | Description | Source |
|-------|-------------|--------|
| Tier name and number | Which approval tier this receipt represents | System config |
| Verification status | Approved, Returned, Flagged, or Partial | Verification receipt (#31) |
| Verified by | Full name and role of the approver | Verification receipt (#31) |
| Timestamp | Date and time the verification was completed | Verification receipt (#31) |
| What was checked | Plain-language summary of verification items | Verification receipt (#31) |
| Evidence links | Clickable links to attached evidence documents | Verification receipt (#31) |
| Receipt ID | Unique identifier for audit trail cross-reference | Decision log (#32) |

### 2.3 Timeline View (Alternative Layout)

For requests with many tiers, a compact timeline view is available as a toggle:

```
● Tier 1 — J. Martinez — 09:14 AM — Requestor Verification ✅
│
● Tier 2 — Samir N. — 10:31 AM — Budget Confirmation ✅
│
◌ Tier 3 — (You) — Pending — Compliance / COI Check
```

The timeline defaults to card view (detailed) unless the approver has toggled it to timeline in their preferences.

---

## 3. Information Hierarchy

Information is layered so the approver can get the signal they need at a glance and drill into detail only when something looks wrong.

### Layer 1: Summary Bar (always visible)

- Total tiers complete out of total
- Time since last upstream verification
- Any flags or issues from upstream (red indicator if present)

### Layer 2: Receipt Cards (expanded by default on first visit)

- Full receipt details as described in Section 2.2
- Collapsible after first review — system remembers the approver's preference per session

### Layer 3: Evidence Documents (click to expand)

- Linked evidence opens in a side panel or new tab
- Evidence includes: attached files, form snapshots, GL extracts, vendor documents
- Evidence is read-only — downstream approvers cannot modify upstream evidence

### Layer 4: Audit Metadata (expandable footer per card)

- Decision log entry ID
- Hash chain reference (per #32 spec)
- Full receipt JSON for technical verification

---

## 4. Per-Tier View Requirements

### 4.1 Tier 2 View (Samir — Budget Authority)

Samir sees Tier 1's receipt before starting his review.

**What's shown:**
- Tier 1 receipt card with requestor verification details
- Whether the Dept Head confirmed requestor identity and authority
- Business need documentation link
- Any notes or flags from Tier 1

**What Samir can act on:**
- If Tier 1 verified requestor identity, Samir can skip re-verifying the requestor and focus on budget validation
- If Tier 1 flagged an issue, Samir sees a warning banner before proceeding

**Samir's workflow change:**
| Before | After |
|--------|-------|
| Open request, verify requestor identity, verify budget | Open request, see Tier 1 already verified requestor, verify budget only |
| ~25 min per request | ~15 min per request (estimated) |

### 4.2 Tier 3 View (Compliance / COI)

Tier 3 sees both Tier 1 and Tier 2 receipts.

**What's shown:**
- Tier 1 receipt card (requestor verification)
- Tier 2 receipt card (budget confirmation, including budget line and amount)
- Chronological order, Tier 1 first

**What Tier 3 can act on:**
- If requestor identity and budget are both verified upstream, compliance reviewer can proceed directly to vendor terms, COI screening, and regulatory checks
- If either upstream tier flagged an issue, a consolidated flag summary appears at the top

---

## 5. Skip-Justification Workflow

When an approver decides that upstream evidence is sufficient for a check item on their own tier, they can mark it as "covered by upstream" instead of re-performing it.

### 5.1 Workflow Steps

1. Approver opens request and reviews upstream evidence panel
2. Approver sees that a specific check (e.g., "requestor identity") was already verified at Tier 1
3. Approver clicks the check item on their own review checklist
4. System shows two options:
   - **"Verify independently"** — perform the check from scratch
   - **"Accept upstream verification"** — mark as covered, referencing the upstream receipt
5. If "Accept upstream verification" is selected:
   - System prompts for a one-line justification (free text, required)
   - System auto-populates the upstream receipt ID being referenced
   - Approver submits
6. Decision is logged in the decision log (#32) as an "upstream-accepted" entry

### 5.2 Skip-Justification Record

```json
{
  "decision_type": "upstream-accepted",
  "approver": "samir.nagheenanajar",
  "tier": 2,
  "check_item": "requestor_identity_verification",
  "upstream_receipt_id": "VR-20260302-0914-MART",
  "upstream_tier": 1,
  "justification": "Dept Head confirmed requestor identity and authority at Tier 1. No reason to re-verify.",
  "timestamp": "2026-03-02T10:32:00Z",
  "decision_log_entry_id": "DL-20260302-1032-NAGH-001"
}
```

### 5.3 Guardrails

- Approvers cannot skip checks that are unique to their tier (e.g., Samir cannot skip "budget line validation" by citing Tier 1 — Tier 1 doesn't check budget)
- The system validates that the referenced upstream receipt actually covers the check being skipped
- Compliance-critical checks (flagged in system config) require independent verification regardless of upstream evidence
- An approver can always choose to verify independently even when upstream evidence exists

---

## 6. Audit Trail Implications

### 6.1 What the Bobs Need

The Bobs require a complete, unbroken verification chain for every approved request. When an approver accepts upstream evidence instead of re-verifying, the audit trail must clearly show:

1. **What was skipped** — the specific check item
2. **Why it was skipped** — the approver's justification
3. **What upstream evidence was relied upon** — the receipt ID and tier
4. **Who made the decision** — the approver's identity
5. **When** — timestamp
6. **That the upstream evidence was valid at the time** — hash chain integrity verification

### 6.2 Audit Report Views

Two new report views for the Bobs (extending the decision log report views from #32):

**Upstream Acceptance Report**
- All instances where an approver accepted upstream evidence instead of verifying independently
- Filterable by: approver, tier, check item, date range
- Shows: request ID, approver, check skipped, upstream receipt referenced, justification

**Verification Coverage Report**
- For any given request: which checks were performed independently vs. accepted from upstream
- Shows the complete verification chain with no gaps
- Flags any check items that were neither independently verified nor accepted from upstream (should be zero for approved requests)

### 6.3 Hash Chain Integration

Each "upstream-accepted" decision is appended to the decision log hash chain (per #32 spec). The hash includes:

- The upstream receipt ID being referenced
- The upstream receipt's hash (creating a cross-reference link in the chain)
- The skip-justification record

This means any tampering with the upstream receipt after it was relied upon would be detectable through hash chain verification.

---

## 7. Edge Cases

### 7.1 Upstream Tier Flagged an Issue

**Scenario:** Tier 1 approved the request but flagged a concern (e.g., "requestor is new hire, authority not yet confirmed in HR system").

**Behavior:**
- Flag appears as a yellow warning banner at the top of the evidence panel
- The flagged check item cannot be accepted via "Accept upstream verification" — must be verified independently
- Warning text: "Tier 1 flagged a concern on this item. Independent verification required."

### 7.2 Upstream Check Was Partial

**Scenario:** Tier 1 verified requestor identity but marked "business need documentation" as partial (e.g., "justification memo attached but missing cost breakdown").

**Behavior:**
- Partial checks are displayed with an orange indicator instead of green
- The "Accept upstream verification" option is unavailable for partial checks
- Downstream approver must either verify the partial item independently or return the request for completion

### 7.3 Upstream Receipt Missing or Corrupted

**Scenario:** Decision log shows a Tier 1 approval but the verification receipt cannot be retrieved or fails hash validation.

**Behavior:**
- Evidence panel shows the tier with a red "Evidence unavailable" indicator
- All checks from that tier must be verified independently by the downstream approver
- System generates an integrity alert to the Bobs
- The request can still proceed but the integrity incident is logged

### 7.4 Request Returned and Re-submitted

**Scenario:** Samir returns a request to Tier 1 for corrections. Tier 1 re-verifies and re-submits.

**Behavior:**
- The evidence panel shows only the most recent verification receipts
- Previous (superseded) receipts are available in an "Earlier versions" expandable section
- The re-verification receipt includes a note explaining what changed
- Previous "upstream-accepted" decisions by downstream tiers are invalidated — downstream must re-review

### 7.5 Concurrent Approvers at Same Tier

**Scenario:** Two people share Tier 2 responsibility and both open the same request.

**Behavior:**
- The first approver to submit their verification creates the receipt
- The second approver sees a notification that the tier has already been reviewed
- If the second approver disagrees with the first, they can add a supplemental review (logged separately)

---

## 8. Samir Adoption Plan

### 8.1 Why Samir's Buy-In Matters

Samir described the upstream evidence display himself during interviews. This is his solution. Adoption risk is lower than typical feature rollouts because the user already wants the capability. The risk is in execution: if the display is clunky, slow, or incomplete, Samir will revert to checking everything manually.

### 8.2 Adoption Phases

**Phase 1: Shadow Mode (Week 1-2)**
- Enable the upstream evidence panel alongside Samir's existing workflow
- Samir reviews requests as he does today, but can also see what Tier 1 verified
- No "Accept upstream verification" option yet — observation only
- Samir provides feedback on information completeness and layout

**Phase 2: Opt-In Skip (Week 3-4)**
- Enable the "Accept upstream verification" workflow
- Samir can choose to use it or ignore it
- Track which checks Samir accepts from upstream vs. verifies independently
- Weekly check-in: is the evidence sufficient? Is anything missing?

**Phase 3: Full Adoption (Week 5+)**
- Samir uses the upstream evidence display as his primary workflow
- Measure time savings against the baseline (~25 min → target ~15 min per request)
- Extend to Tier 3 if not already active

### 8.3 Samir's Confirmation Criteria

Samir confirms adoption when:
- [ ] He can see Tier 1's verification before starting his review
- [ ] The evidence panel answers his question: "Was the budget line already verified?"
- [ ] He can skip redundant checks without feeling like he's cutting corners
- [ ] The justification workflow is fast (under 30 seconds per skip)
- [ ] He trusts the upstream receipts are accurate and tamper-evident

### 8.4 Fallback

If Samir finds the evidence display insufficient after Phase 1, the team pauses Phase 2 and iterates on the display based on his feedback. The existing approval workflow remains unchanged — this feature adds a layer on top, it does not remove existing capabilities.

---

## 9. Implementation Requirements

### 9.1 UI Changes

| Component | Description | Priority |
|-----------|-------------|----------|
| Evidence Panel | New panel at top of approval screen, above request details | P0 |
| Receipt Cards | Formatted cards with status, approver, timestamp, evidence links | P0 |
| Timeline Toggle | Compact timeline view as alternative to cards | P1 |
| Skip-Justification Modal | Dialog for "Accept upstream verification" with justification field | P0 |
| Flag/Warning Banners | Yellow/red banners for upstream flags, partial checks, missing evidence | P0 |
| Preference Persistence | Remember panel collapsed/expanded and card/timeline preference | P2 |

### 9.2 Data Access Patterns

**Read path (on approval screen load):**
1. Fetch the request's approval chain configuration (which tiers apply)
2. For each upstream tier: query the verification receipt store (#31) for the latest receipt
3. For each receipt: validate hash chain integrity against the decision log (#32)
4. Assemble the evidence panel payload and render

**Write path (on skip-justification submit):**
1. Validate that the referenced upstream receipt exists and is valid
2. Validate that the check item is eligible for upstream acceptance (not flagged, not partial, not compliance-critical)
3. Create the skip-justification record
4. Append to the decision log hash chain (#32)
5. Update the approver's review checklist state

**Performance targets:**
- Evidence panel load time: under 2 seconds (including hash validation)
- Skip-justification submit: under 1 second
- Receipt card rendering: progressive — show cards as data arrives, don't wait for all tiers

### 9.3 Data Model Additions

```
upstream_evidence_view:
  request_id: string
  viewer_tier: int
  viewer_approver: string
  upstream_receipts: VerificationReceipt[]  # from #31
  panel_state: "expanded" | "collapsed"
  view_mode: "cards" | "timeline"
  loaded_at: timestamp

skip_justification:
  id: string
  request_id: string
  approver: string
  tier: int
  check_item: string
  upstream_receipt_id: string
  upstream_tier: int
  justification: string
  timestamp: datetime
  decision_log_entry_id: string
  hash: string  # SHA-256, chained per #32 spec
```

### 9.4 Access Control

- Approvers can only see upstream evidence for requests assigned to their tier
- Evidence is read-only — no downstream modification of upstream receipts
- Skip-justification records are append-only (per decision log #32 immutability spec)
- The Bobs have read access to all evidence views and skip-justification records

---

## 10. Testing and Validation Plan

### 10.1 Functional Tests

| Test | Expected Result |
|------|----------------|
| Tier 2 approver opens request with completed Tier 1 | Evidence panel shows Tier 1 receipt card with all fields |
| Tier 3 approver opens request with completed Tiers 1 and 2 | Evidence panel shows both receipt cards in chronological order |
| Tier 1 approver opens request | No evidence panel shown (no upstream tiers) |
| Approver clicks "Accept upstream verification" | Skip-justification modal appears with receipt ID pre-populated |
| Approver submits skip-justification | Record created in decision log, check item marked as covered |
| Upstream tier flagged an issue | Warning banner appears, "Accept upstream" option disabled for flagged item |
| Upstream check is partial | Orange indicator shown, "Accept upstream" option disabled |
| Upstream receipt fails hash validation | Red "Evidence unavailable" indicator, independent verification required |
| Request returned and re-submitted | Only latest receipts shown, previous in "Earlier versions" section |

### 10.2 Audit Integrity Tests

| Test | Expected Result |
|------|----------------|
| Skip-justification is recorded in decision log | Entry appears with correct type, receipt reference, and hash |
| Hash chain remains valid after skip-justification | Chain verification passes end-to-end |
| Upstream receipt is tampered with after being referenced | Hash chain verification detects the inconsistency |
| Bobs run Upstream Acceptance Report | All skip-justifications appear with correct filters |
| Bobs run Verification Coverage Report | Every check item shows as independently verified or upstream-accepted |

### 10.3 Samir's Confirmation Tests

These tests are performed with Samir directly:

| Scenario | Samir confirms |
|----------|---------------|
| Open a $12,000 procurement request where Tier 1 verified requestor | Samir can see Tier 1's verification and skips requestor re-check |
| Open a request where Tier 1 flagged a concern | Samir sees the warning and verifies independently |
| Complete a full review using upstream evidence | Samir reports time savings and no loss of confidence |
| Review the skip-justification record after the fact | Samir confirms the audit trail accurately reflects his decision |

### 10.4 Performance Tests

| Test | Target |
|------|--------|
| Evidence panel load with 1 upstream tier | < 1.5 seconds |
| Evidence panel load with 2 upstream tiers | < 2 seconds |
| Skip-justification submit round-trip | < 1 second |
| Evidence panel with 10+ evidence document links | No rendering lag |

### 10.5 The Bobs' Acceptance

The Bobs confirm:
- [ ] The verification chain maintains audit integrity when approvers accept upstream evidence
- [ ] Every skip-justification is fully traceable (who, what, when, why, which upstream receipt)
- [ ] The hash chain cross-references upstream receipts correctly
- [ ] Audit reports cover all upstream acceptance activity
- [ ] No verification gaps exist in approved requests (every check is either independently verified or upstream-accepted with justification)
