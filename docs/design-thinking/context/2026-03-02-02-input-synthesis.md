# Input Synthesis — Approval Process

**Date:** 2026-03-02
**Source material:** 8 simulated stakeholder interviews (Lumbergh, Peter, Samir, Michael, Joanna, Tom, The Bobs, Milton)
**Original request:** "Our approval process is killing us. It takes too long, too many people are involved, and things fall through the cracks. We need a modern system to replace what we have. Can you build it?" — VP of Operations

---

## Theme 1: The Process Is Longer Than It Needs to Be — and Nobody Knows Why

The Bobs confirmed that only **3 of 7 approval tiers are regulatory**. The other 4 were added internally over time — Tom's temporary patches that became permanent, Samir's self-imposed secondary review, Lumbergh's informal "loop me in" gate. Nobody questioned whether these additions were still necessary because nobody read the control matrix that distinguishes mandatory from elective controls.

**Cited by:** The Bobs (control matrix), Tom (admitted 2 steps are legacy patches), Samir (acknowledged his secondary review isn't policy-mandated), Lumbergh (doesn't recognize his "courtesy copy" as a gate)

---

## Theme 2: The "Real" Process Isn't the Official Process

Peter built a shadow workflow — spreadsheets, fabricated soft approvals, strategic routing — that is the only reason things move at a tolerable pace. Joanna's team uses credit cards and informal request bundling to bypass the system entirely. Milton performs a critical vendor terms check that isn't on any flowchart. 15% of all approvals run through a separate SharePoint list that IT and compliance don't know about.

The process everyone is trying to "fix" isn't the process that's actually running. Any redesign built on the official workflow is targeting a fiction.

**Cited by:** Peter (shadow routing, fabricated approvals), Joanna (credit cards, informal bundling), Milton (off-chart vendor checks, Facilities SharePoint list)

---

## Theme 3: Approvers Duplicate Each Other's Work Because They Can't See Each Other's Work

Samir re-verifies everything from scratch because the system shows him nothing about what upstream approvers already checked. He said it himself: "If I could see that the budget line was verified at step 2 with a timestamp and a name, I wouldn't need to re-do it at step 5." The Bobs flagged that 60% of approved requests have incomplete documentation — approvers are clicking "approve" without recording what they actually checked. The result is that every approver treats every request as if nobody has touched it before them.

This isn't a trust problem between people. It's a visibility problem in the system.

**Cited by:** Samir (no upstream visibility), The Bobs (60% incomplete documentation), Peter (no audit trail for his workarounds)

---

## Theme 4: Workarounds Have Created Invisible Compliance Gaps

Peter's PO-splitting workaround — designed to skip slow steps — also bypasses the conflict-of-interest check that only triggers on consolidated amounts. Joanna's credit card purchases sit entirely outside the approval trail. The Facilities SharePoint list has no integration with the main system and minimal audit logging. These aren't hypothetical risks. They're active, unmonitored gaps that neither the Bobs nor anyone else in the organization is currently tracking.

**Cited by:** Milton (PO-splitting bypasses compliance control), Joanna (credit card bypass), Milton (Facilities shadow system)

---

## Theme 5: Institutional Knowledge Lives in People, Not in the Process

Milton holds 47 vendor exception records in a personal spreadsheet. Tom can't explain his own 9-step workflow without consulting his files. Peter's workarounds are undocumented and known only to three people. The process appears functional because specific individuals compensate for its gaps — if any of them leave, the system breaks in ways nobody will understand until it's too late.

**Cited by:** Milton (vendor spreadsheet, historical rationale for steps), Tom (can't recall all step rationales), Peter (undocumented workarounds)

---

## Theme 6: Everyone Is Solving for Different Success Metrics

Lumbergh wants speed and dashboards. Samir wants audit safety. Joanna wants predictability. Tom wants relevance. The Bobs want documentation completeness. Michael wants a modern stack. Milton wants to be heard. Nobody is wrong — but nobody is aligned, and the current process tries to satisfy all of them by adding steps instead of making tradeoffs.

**Cited by:** All 8 personas — each optimizing independently for their own definition of "working"

---

## Significant Contradictions

| Clash | What's Really Happening |
|---|---|
| **Lumbergh vs. Peter** on the "loop me in" step | Lumbergh created a gate he doesn't see as a gate. Peter sees it as a 1-2 day delay on every request over $5K. |
| **Michael vs. Peter** on whether the tool is the problem | The platform supports parallel routing, auto-escalation, and delegation — none configured. Michael wants to replace it; Peter says the tool is fine. The gap is process design and configuration, not technology. |
| **Samir vs. Joanna** on whether extra reviews add value | Samir applies full review to sub-$50K requests policy allows lighter treatment. Joanna sees re-reviewing a $1,200 renewal as theater. The process doesn't differentiate. |
| **Tom vs. Everyone** on whether the process is solid | Tom insists every step is load-bearing. Two steps are unreviewed temporary patches. Peter skips two on 70% of requests with no consequences. |

---

## Hidden Insights

1. **Samir designed his own fix without recognizing it.** He said if the system showed him what prior approvers verified, he'd skip his redundant check. That's the solution to his own bottleneck — upstream transparency.

2. **The Bobs' control matrix is the Rosetta Stone nobody is reading.** It already distinguishes mandatory from elective controls. Every stakeholder operates as if all 7 tiers are required. Three are. The cost of not reading one document is potentially half the approval chain.

---

## Reframed Problem Statement

The VP asked for a modern approval system. What the research reveals is that **this is not a tooling problem — it's a process trust and visibility problem.**

The organization has an approval workflow that grew through accumulated patches, informal gates, and individual risk aversion — not through intentional design. Nobody sees the full picture: approvers duplicate each other's work because the system shows them nothing about upstream decisions; workaround artists keep things moving through shadow processes that create untracked compliance gaps; and the distinction between regulatory requirements and organizational anxiety has been lost.

**The real problem:** The organization doesn't have a shared, transparent model for who needs to approve what, why, and what "approved" actually means at each step. A new tool on top of the current process would automate the dysfunction faster. The path forward requires process redesign grounded in the Bobs' control matrix (3 mandatory tiers, not 7), upstream visibility so approvers stop duplicating work, and absorption of the shadow processes into a single auditable system.
