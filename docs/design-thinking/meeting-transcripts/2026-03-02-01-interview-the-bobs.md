# Simulated Stakeholder Interview — The Bobs

**Date:** 2026-03-02
**Persona:** The Bobs (Internal Audit / Compliance)
**Interviewer:** Discovery Team
**Source request:** "Our approval process is killing us..." — VP of Operations

---

## The Bobs — Internal Audit / Compliance

---

**Interviewer:** Thanks for making time. We're looking at the approval process end to end and talking to everyone who touches it. Can you start by telling me what the current approval process looks like from your perspective?

**Bob 1:** Sure. From our side, the approval process is a control surface. Every step in that workflow corresponds to a segregation-of-duties requirement, a documentation checkpoint, or an escalation threshold.

**Bob 2:** — or all three. We don't run approvals day to day, but we audit the outputs quarterly. We need to see who approved what, when, with what authority, and whether the supporting documentation was attached at the time of decision.

**Bob 1:** Not after the fact. At the time. That's a distinction people miss.

---

**Interviewer:** When you say you audit quarterly, what are you actually looking for? What does a clean audit trail look like versus a messy one?

**Bob 2:** Clean means every approval has a timestamp, an identity, a reason code, and the backing artifact. Purchase over $25K? We need the three-bid summary attached before the approval, not uploaded a week later because someone asked.

**Bob 1:** Messy looks like what we have now about 40% of the time. Missing attachments. Approvals by people outside the delegation-of-authority matrix. Timestamps that suggest someone bulk-approved 15 items on a Friday afternoon without reviewing them.

**Bob 2:** We flagged 23 instances of that last quarter alone. Twenty-three. That's not a tool problem. That's a behavior problem the tool is enabling.

---

**Interviewer:** That's a great segue. Some of the other folks we've talked to say the process has too many steps and that's what causes people to rubber-stamp things. What's your take?

**Bob 1:** We've heard that argument. "If you reduce the steps, people will pay more attention to each one." That's a theory. What we've *observed* is that when you reduce control points, people skip the remaining ones faster.

**Bob 2:** Joanna's team, for example — and we have data on this — their approval requests have a 60% documentation deficiency rate on first submission. Sixty percent. If we remove the review step that catches that, those deficiencies flow downstream.

**Bob 1:** And then we catch them in audit. Which is worse for everyone, because now it's a finding, not a correction.

**Bob 2:** Right. A checkpoint that adds two days up front saves a six-week remediation cycle on the back end. The math isn't complicated.

---

**Interviewer:** So it sounds like you'd want to keep the current number of steps, or even add more?

**Bob 1:** We want the *right* controls, not necessarily *more* controls. There's a difference.

**Bob 2:** For instance — and this is just factual — the **regulatory requirement is three approval tiers** for expenditures under $100K. Three. The current process has seven.

**Bob 1:** Four of those were added internally over the years. Department head review, Tom's process checkpoint, the secondary finance sign-off, and the executive "visibility" loop that Lumbergh requested.

**Bob 2:** From a pure compliance standpoint, those four steps are elective. We don't require them. We've never required them.

**Bob 1:** But we're also not going to be the ones who recommend removing them, because if something goes wrong after a reduction, the first question in any investigation is "who approved weakening the controls?"

---

**Interviewer:** That's really interesting. So nearly half the steps aren't actually compliance-driven. Has that ever come up before?

**Bob 1:** It's in our control matrix documentation. Anyone can read it.

**Bob 2:** Nobody reads it.

**Bob 1:** Nobody reads it.

---

**Interviewer:** Fair enough. Let me ask this differently. If someone were building a new system from scratch, what would you need it to do to keep audit happy?

**Bob 2:** Three things. Immutable audit log — no one can alter a record after the fact. Identity verification at every approval node, meaning no shared accounts, no "approved on behalf of." And real-time documentation attachment, meaning the artifact must be present before the system allows the approval to proceed.

**Bob 1:** That third one is the big one. The current system allows approvals with missing attachments. It sends a reminder email. Nobody reads those either.

**Bob 2:** If the new system enforced attachment-before-approval, we'd probably eliminate 80% of our quarterly findings overnight.

**Bob 1:** Which, candidly, **"would be the single most impactful improvement anyone has proposed in the five years we've been auditing this process."** We've said this in three separate audit reports. It's never been prioritized.

---

**Interviewer:** Lumbergh mentioned wanting to cut the average approval cycle from 12 days to under 3. Is that realistic from a compliance standpoint?

**Bob 1:** The compliance steps — the real ones, not the elective ones — can be completed in hours if the submitter has their documentation ready at the point of request.

**Bob 2:** The delay isn't in the controls. It's in the queue time between steps and in the rework cycles when submissions come in incomplete.

**Bob 1:** So yes, three days is achievable. Possibly less. But only if the system enforces completeness at intake. If people can still submit half-finished requests and "add the rest later," you'll get the same cycle times in a prettier interface.

**Bob 2:** We've seen that pattern before. New tool, same behavior, same findings, new vendor to blame.

---

**Interviewer:** Last question. What's the one thing that keeps you up at night about this process changing?

**Bob 2:** That someone frames this as "removing bureaucracy" and strips out controls that exist for regulatory reasons because they can't tell the difference between a compliance requirement and something Tom added in 2019 because he wanted to feel involved.

**Bob 1:** And that nobody asks us until the system is already built. We've seen three modernization efforts in other departments. Two of them had to be rolled back after our review because they eliminated segregation of duties.

**Bob 2:** Include us in the design. Not at the end for a sign-off. At the beginning, when the workflows are being drawn.

**Bob 1:** That's all we ask.

---

## Key Takeaways (Interviewer Notes)

- **Pain quote:** *"[Enforcing attachment-before-approval] would be the single most impactful improvement anyone has proposed in the five years we've been auditing this process."*
- **Contradiction with other stakeholders:** The Bobs push back directly on Joanna's position that fewer steps equals better outcomes, citing a 60% documentation deficiency rate on her team's submissions. Also challenge Lumbergh's assumption that speed and compliance are in tension.
- **Unintentional reveal:** Only 3 of the 7 current approval tiers are regulatory requirements. The other 4 are internally added and elective. The Bobs know this, documented it, and have never pushed to change it because they don't want to own the risk of recommending a reduction.
- **Design implication:** A system that enforces completeness at intake (attachment-before-approval, no partial submissions) would address both compliance concerns *and* cycle time, making this a rare alignment point across stakeholders.
