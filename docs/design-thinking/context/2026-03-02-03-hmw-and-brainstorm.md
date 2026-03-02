# HMW Statements + SCAMPER Brainstorm — Approval Process

**Date:** 2026-03-02
**Source:** Input Synthesis (2026-03-02-02-input-synthesis.md)
**Original request:** "Our approval process is killing us..." — VP of Operations

---

## How Might We Statements

### HMW 1 — From Theme 1: Process Is Longer Than It Needs to Be

How might we make the distinction between regulatory requirements and organizational habits visible — so the team can consciously choose which controls to keep instead of treating all 7 tiers as mandatory?

### HMW 2 — From Theme 2: The "Real" Process Isn't the Official Process

How might we absorb the shadow processes — Peter's workarounds, Joanna's credit card bypass, Milton's vendor spreadsheet, the Facilities SharePoint list — into a single system without losing what makes them work?

### HMW 3 — From Theme 3: Approvers Duplicate Work Due to No Visibility

How might we give each approver clear evidence of what upstream approvers already verified — so they can build on prior work instead of starting from scratch?

### HMW 4 — From Theme 4: Workarounds Created Compliance Gaps

How might we close the compliance gaps created by workarounds — PO-splitting, credit card bypasses, untracked Facilities approvals — without reintroducing the friction that caused people to work around the process in the first place?

### HMW 5 — From Theme 5: Knowledge Lives in People, Not the Process

How might we capture Milton's vendor exceptions, Tom's historical rationale, and Peter's routing logic into something the organization owns — so the process doesn't collapse when any one person leaves?

### HMW 6 — From Theme 6: Everyone Optimizes for Different Metrics

How might we align stakeholders around a shared definition of "working" — balancing Lumbergh's speed, Samir's audit safety, Joanna's predictability, and the Bobs' documentation — instead of adding steps to satisfy each one independently?

---

## SCAMPER Brainstorm (Unfiltered)

### S — Substitute

1. **Substitute human routing with rule-based routing.** Peter manually routes every request. Replace the routing function with rules: amount → approver chain, department → budget holder, risk tier → compliance gate. Peter becomes exception handler instead of traffic cop.

2. **Substitute Samir's re-verification with a verification receipt.** Instead of Samir checking everything from scratch, each upstream approver stamps what they verified. Samir only checks what's uncovered. Substitute redundant review with incremental review.

3. **Substitute the approval chain with a risk score.** Don't route by type — route by calculated risk. A $1,200 renewal with 3 years of history is low risk, auto-approve. A $50K new vendor engagement is high risk, full chain. Let the math decide who's needed, not a static flowchart.

4. **Substitute Tom's institutional process knowledge with a living decision log.** Every approval step records *why* it approved or flagged — not just that it did. The log replaces Tom's memory as the system of record for rationale.

5. **Substitute Milton's spreadsheet with a vendor master database.** The 47 vendor exceptions he tracks in a personal spreadsheet become a shared, queryable data source that the system checks automatically at intake.

### C — Combine

6. **Combine budget verification and compliance check into one step.** Samir and the Bobs are checking overlapping things. Create a single "financial and compliance review" step that covers both, done by one qualified reviewer with a checklist.

7. **Combine all shadow systems into one intake point.** Peter's spreadsheets, Joanna's credit card purchases, Milton's vendor list, and the Facilities SharePoint list all feed into a single approval queue. One system, one audit trail, all volume visible.

8. **Combine the approval request with its supporting documentation at submission.** The Bobs said 60% of requests have incomplete docs. Don't let a request enter the queue without required attachments. Combine submission and documentation into an atomic action.

9. **Combine Lumbergh's visibility need with a dashboard instead of a gate.** He doesn't need to approve — he needs to see. Give him a real-time dashboard and remove him from the approval chain. Combine his oversight need with a reporting tool instead of an approval step.

10. **Combine the control matrix with the routing logic.** The Bobs already published what's mandatory vs. elective. Use that document as the literal configuration for the approval engine. The compliance framework *becomes* the process design.

### A — Adapt

11. **Adapt CI/CD pipeline approval gates.** In DevOps, deployments go through automated checks with human review only on exception. Adapt that pattern: automated validation for routine requests, human gates only when something fails a check or exceeds a threshold.

12. **Adapt airline pre-check / trusted traveler model.** Frequent requestors with clean track records get a pre-approved "fast lane." First-time or flagged requests get full review. Adapt security's risk-tiering model to approvals.

13. **Adapt how banks handle transaction monitoring.** Banks don't review every transaction — they flag anomalies. Adapt anomaly detection: approve most things automatically, surface only the statistical outliers for human review.

14. **Adapt Wikipedia's "edit then review" model.** Instead of approving before action, let low-risk actions proceed and audit afterward. Adapt post-hoc review for categories where the cost of delay exceeds the cost of reversal.

15. **Adapt how hospitals handle triage.** Not every patient needs a surgeon. Not every approval needs seven steps. Adapt triage logic: assess at intake, route by severity, fast-track the routine.

### M — Modify

16. **Modify approval depth based on dollar amount.** Under $2,500: auto-approve with documentation. $2,500–$10K: budget holder only. $10K–$50K: budget holder + compliance. Over $50K: full chain. Modify the process to scale with risk instead of being flat.

17. **Modify the SLA to be visible and enforced.** Add a clock to every approval step. If an approver doesn't act within 48 hours, it auto-escalates. Modify the process from "wait and hope" to "act or it moves without you."

18. **Modify renewal approvals to be automatic.** Same vendor, same terms, same amount as last year? Auto-renew. No human touch needed. Modify the process to recognize that re-approving an unchanged purchase is waste.

19. **Modify the process to shrink for repeat patterns.** First time buying from a vendor: full review. Third time, same terms: abbreviated review. Tenth time: auto-approve. Let the process learn from its own history. Modify the chain length dynamically.

20. **Modify who "owns" the process.** Tom owns it now and can't see beyond it. Modify ownership to a cross-functional process council — Joanna (requestor), Samir (approver), the Bobs (compliance), Peter (operations) — that reviews and adjusts quarterly. No single gatekeeper.

### P — Put to Another Use

21. **Put the Bobs' control matrix to use as the approval routing config.** It already defines the 3 mandatory control points. Stop treating it as an audit artifact and start treating it as the process blueprint.

22. **Put Peter's workarounds to use as the *actual* process design.** His shadow workflow is what's keeping things moving. Instead of eliminating workarounds, formalize the best ones. His routing spreadsheet becomes v2 of the process.

23. **Put Milton to use as process historian and transition advisor.** He knows why every step exists. Instead of ignoring him, put him on the redesign team as institutional memory. His knowledge becomes the migration guide.

24. **Put the existing platform's unused features to use.** Parallel routing, auto-escalation, delegation — already built in, never configured. Before building anything new, turn on what's already there and measure the impact.

25. **Put approval data to use as a feedback loop.** Track cycle time, bottleneck location, rejection rates, and workaround frequency. Use it to continuously tune the process — put the data to work as a self-improvement mechanism.

### E — Eliminate

26. **Eliminate all 4 elective approval tiers.** The Bobs said 3 are regulatory. Keep those. Drop the rest. See what happens. This is the most aggressive option and the fastest to implement.

27. **Eliminate Lumbergh's informal gate entirely.** He said it's a "courtesy copy." It's a 1-2 day delay on every mid-size request. Eliminate the gate, give him the dashboard, move on.

28. **Eliminate the concept of "approval" for low-risk categories.** Sub-$2,500 renewals, pre-approved vendors, standard software licenses — these don't need approval. They need documentation. Eliminate the human step and replace it with a confirmation receipt.

29. **Eliminate sequential routing.** Every request goes to all required approvers simultaneously. Nobody waits in line. If you need budget holder + compliance, both review at the same time. Eliminate the queue.

30. **Eliminate the approval process entirely and replace it with spending policies + audit.** *(Would probably fail.)* Give every department a budget, set policy guardrails, and audit quarterly instead of approving every transaction. Works in some orgs. Would terrify the Bobs in this one.

### R — Reverse

31. **Reverse the flow: pre-approve, then flag exceptions.** Instead of "nothing moves until approved," flip it: everything is pre-approved within policy guardrails. Only exceptions get routed for review. Reverse the default from "blocked" to "cleared."

32. **Reverse who initiates the review.** Instead of routing to approvers and waiting, let approvers pull from a visible queue when they're ready. Reverse from push-based (wait for assignment) to pull-based (grab next item). Removes routing as a bottleneck entirely.

33. **Reverse the approval chain direction.** Start with compliance (the Bobs), then budget, then operations — instead of operations first. Reverse so that the hardest "no" comes first, not last. Stop wasting six steps of review time on something compliance will reject at step 7.

34. **Reverse the assumption that more steps = more control.** What if fewer steps with better documentation at each one produces a *stronger* audit trail than seven steps with 60% incomplete docs? Reverse the relationship between quantity and quality.

35. **Reverse the problem entirely.** *(The reframe.)* Stop asking "how do we fix the approval process" and ask "what would we build if approvals didn't exist and we were designing accountability from scratch?" The entire mental model is anchored in "approvals" — sequential human permission gates. What if the answer isn't a better approval process but a different accountability architecture entirely? Spending policies + real-time monitoring + anomaly detection + post-hoc audit might replace the concept of "approval" altogether for 80% of transactions.

---

## Summary

| SCAMPER Lens | Ideas | Range |
|---|---|---|
| Substitute | 1–5 | Rule engines, verification receipts, risk scores, decision logs, vendor databases |
| Combine | 6–10 | Merged review steps, unified intake, submission+docs, dashboard visibility, control matrix as config |
| Adapt | 11–15 | CI/CD gates, trusted traveler, anomaly detection, edit-then-review, triage |
| Modify | 16–20 | Scaled depth, enforced SLAs, auto-renewal, shrinking chains, shared ownership |
| Put to Another Use | 21–25 | Control matrix as blueprint, workarounds as design, Milton as advisor, unused features, data as feedback |
| Eliminate | 26–30 | Drop elective tiers, kill informal gates, remove low-risk approvals, end sequencing, eliminate approvals entirely |
| Reverse | 31–35 | Pre-approve by default, pull-based queues, compliance-first ordering, fewer+better steps, redesign accountability from scratch |
