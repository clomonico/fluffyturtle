# Implementation Options — Approval Process

**Date:** 2026-03-02
**Source:** HMW Statements + SCAMPER Brainstorm (2026-03-02-03-hmw-and-brainstorm.md)
**Original request:** "Our approval process is killing us..." — VP of Operations

---

## Option 1: "Clear the Path"

**What it does:** Strip the approval chain down to what compliance actually requires and fix the inputs. Drop all 4 elective tiers, keep the 3 regulatory control points from the Bobs' control matrix, enforce documentation completeness at intake so nothing enters the queue incomplete, and replace Lumbergh's informal gate with a visibility dashboard. Process surgery — no new tools, no new platform, just a sharper version of what exists.

**Brainstorm ideas incorporated:** #8 (combine submission + docs), #9 (dashboard instead of gate), #10 (control matrix as config), #21 (control matrix as blueprint), #26 (eliminate elective tiers), #27 (eliminate Lumbergh's gate), #34 (fewer steps + better documentation)

**Primary HMW addressed:** HMW 1 — Make the distinction between regulatory requirements and organizational habits visible

**Key risk or assumption to prove out:** That the 4 elective tiers are truly elective. The Bobs say they are, but organizational muscle memory may resist dropping controls — even non-mandatory ones — without evidence that nothing breaks. Requires a parallel-run period to validate.

---

## Option 2: "Smart Routing"

**What it does:** Replace the flat, one-size-fits-all approval chain with dynamic routing based on risk. Low-risk requests (renewals, small amounts, known vendors) get auto-approved with documentation. Mid-risk requests go to a single approver. High-risk requests get the full chain. The system calculates risk at intake based on dollar amount, vendor history, request type, and requestor track record — then routes accordingly. Trusted requestors with clean history get a fast lane.

**Brainstorm ideas incorporated:** #1 (rule-based routing), #3 (risk score routing), #12 (trusted traveler model), #15 (triage at intake), #16 (depth scales with amount), #18 (auto-renew unchanged purchases), #19 (shrinking chains for repeat patterns), #28 (eliminate approval for low-risk)

**Primary HMW addressed:** HMW 4 — Close compliance gaps without reintroducing friction

**Key risk or assumption to prove out:** That a risk-scoring model can be calibrated accurately enough that the Bobs trust it. If auto-approved requests produce even one audit finding, confidence collapses and the organization reverts to reviewing everything. Needs a pilot with tight monitoring.

---

## Option 3: "Glass Floor"

**What it does:** Solve the visibility and duplication problem without changing the process structure. Every approval step records what was verified, by whom, with a timestamp — creating a verification chain that downstream approvers can see. Samir stops re-checking budgets because he can see the budget holder already verified. Enable parallel routing so approvers work simultaneously instead of sequentially. Add auto-escalation so nothing sits idle past 48 hours. All of this uses features already built into the current platform that Michael confirmed are just not configured.

**Brainstorm ideas incorporated:** #2 (verification receipts), #4 (living decision log), #17 (enforced SLAs with auto-escalation), #24 (turn on existing unused features), #25 (approval data as feedback loop), #29 (eliminate sequential routing)

**Primary HMW addressed:** HMW 3 — Give approvers evidence of what upstream already verified

**Key risk or assumption to prove out:** That configuring existing platform features actually works as advertised. Michael says the platform is fragile and poorly integrated — toggling on parallel routing and auto-escalation in a brittle system could create new failure modes. Needs a technical feasibility spike before committing.

---

## Option 4: "One Front Door"

**What it does:** Absorb every shadow process into a single, auditable system. Peter's routing spreadsheet becomes formalized routing rules. Milton's 47-vendor exception list becomes a vendor master database that the system checks at intake. Joanna's credit card purchases get a lightweight approval path so they're tracked. The Facilities SharePoint list gets migrated into the main system. Milton joins the redesign team as institutional memory. One system, one audit trail, reflecting how work actually moves.

**Brainstorm ideas incorporated:** #5 (vendor master database), #7 (unified intake), #22 (formalize workarounds as the process), #23 (Milton as transition advisor), #20 (cross-functional process ownership)

**Primary HMW addressed:** HMW 2 — Absorb shadow processes into a single system without losing what makes them work

**Key risk or assumption to prove out:** That formalizing workarounds doesn't kill their effectiveness. Peter's shortcuts work *because* they're informal — adding governance to them could reintroduce the friction they were designed to avoid. Requires careful co-design with the people who built the workarounds.

---

## Option 5: "Flip the Default"

**What it does:** Invert the entire model. Instead of "nothing moves until approved," everything within policy guardrails is pre-approved by default. Spending policies, vendor pre-qualification, and budget thresholds replace human gates for 80% of transactions. AI-driven anomaly detection flags the exceptions — unusual amounts, new vendors, pattern breaks — and only those go to human review. Post-hoc audit replaces pre-hoc approval for routine spend. Redesigns accountability from scratch instead of optimizing the existing approval paradigm.

**Brainstorm ideas incorporated:** #11 (CI/CD approval gates), #13 (anomaly detection), #14 (edit-then-review model), #30 (spending policies + audit), #31 (pre-approve then flag exceptions), #35 (redesign accountability from scratch)

**Primary HMW addressed:** HMW 6 — Align on a shared definition of "working"

**Key risk or assumption to prove out:** That the organization's risk tolerance can accommodate a "default yes" model. The Bobs, Samir, and Tom are culturally wired for pre-approval. Moving to post-hoc review requires a fundamental shift in how accountability is understood — and if a significant error slips through during the transition, the entire model gets scrapped. Needs executive air cover and a very controlled rollout.

---

## Comparison

| Option | Name | Approach | Speed to Impact | Organizational Risk | HMW |
|---|---|---|---|---|---|
| 1 | Clear the Path | Cut the process to regulatory minimum | Fast (weeks) | Medium — people resist losing controls | HMW 1 |
| 2 | Smart Routing | Route dynamically by risk | Medium (months) | Medium — risk model must earn trust | HMW 4 |
| 3 | Glass Floor | Make existing steps transparent + parallel | Fast (weeks) | Low — uses existing platform features | HMW 3 |
| 4 | One Front Door | Absorb all shadow systems into one | Medium (months) | Medium — formalizing workarounds is tricky | HMW 2 |
| 5 | Flip the Default | Replace approvals with policies + monitoring | Slow (quarters) | High — cultural and structural shift | HMW 6 |
