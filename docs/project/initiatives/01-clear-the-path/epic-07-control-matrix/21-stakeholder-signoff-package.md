# Stakeholder Sign-Off Package — Tier Classifications

**Date:** 2026-03-02
**Author:** Parker (Stakeholder Analyst)
**Initiative:** Clear the Path (Initiative 1)
**Status:** Draft — Pending Stakeholder Review

---

## Executive Summary

The current approval process has 7 tiers. Only 3 are regulatory. The Bobs confirmed this in their control matrix, published 18 months ago and referenced in two consecutive audit reports. Nobody read it.

The 4 elective tiers grew through accumulated patches: Tom's temporary fixes from 2019-2020 that became permanent, Samir's self-imposed secondary review he acknowledged isn't policy-mandated, and Lumbergh's informal "loop me in" gate that adds 1-2 days to every request over $5K without anyone recognizing it as an approval step.

**What changes:** The 4 elective tiers are removed. The 3 mandatory control points (budget holder verification, compliance/conflict-of-interest check, segregation-of-duties sign-off) stay intact and are strengthened through intake completeness enforcement. Lumbergh's gate is replaced with a real-time dashboard that gives him better visibility than the courtesy copy ever provided.

**Why now:** Average cycle time is 12 days. The Bobs confirmed the 3 regulatory steps can complete in hours with proper documentation. The organization is paying a time tax of roughly 9 extra days per request for controls that compliance never required. Meanwhile, workarounds (PO-splitting, credit card bypass, shadow SharePoint lists) have created untracked compliance gaps that are worse than the risks the elective tiers were meant to prevent.

**What this document asks:** Each stakeholder reviews their tier classification section, confirms understanding of how the change affects their work, and signs off with any conditions or reservations recorded.

---

## Tier Classification Summary

| Tier | Description | Classification | Basis | Status |
|---|---|---|---|---|
| **Tier 1: Budget Holder Verification** | Financial authorization by the budget owner for the spend | **Mandatory** | Bobs' control matrix — regulatory requirement | Retained, strengthened with intake completeness |
| **Tier 2: Compliance / Conflict-of-Interest Check** | Review that triggers on consolidated amounts to verify vendor relationships and COI | **Mandatory** | Bobs' control matrix — regulatory control | Retained, PO-splitting bypass to be closed |
| **Tier 3: Segregation-of-Duties Sign-Off** | Independent sign-off confirming requestor and approver are not the same party | **Mandatory** | Bobs' control matrix — regulatory requirement | Retained |
| **Tier 4: Tom's Routing Accuracy Review** | Tom reviews whether Peter routed to the correct approver | **Elective** | Added by Tom in 2019; Peter skips on 70% of requests with no consequence | Removed |
| **Tier 5: Tom's 2020 Exception Patch** | Broad exception routing added after a duplicate vendor payment; applies to all vendor requests, not just at-risk ones | **Elective** | Temporary patch from 2020 that became permanent; Tom acknowledged it could be scoped down | Removed |
| **Tier 6: Samir's Secondary Finance Review** | Secondary cross-reference against live budget, applied to all requests regardless of size | **Elective** | Self-imposed by Samir; not policy-mandated; he acknowledged he wouldn't need it with upstream visibility | Removed |
| **Tier 7: Lumbergh's "Loop Me In" Gate** | Informal review where nothing moves until Lumbergh replies "Go ahead" on requests over $5K | **Elective** | Never documented as an approval step; adds 1-2 days per request; Lumbergh doesn't recognize it as a gate | Replaced with dashboard |

**Source:** Bobs' control matrix (mandatory/elective distinction), stakeholder interviews (8 personas, 14 interview sessions), Input Synthesis document, Initiative 1 scope (Dallas).

---

## Per-Stakeholder Sections

### The Bobs — Internal Audit / Compliance

**How the 3 mandatory tiers align with your control requirements:**

The three retained tiers map directly to the control matrix you published 18 months ago. This initiative is, in effect, implementing the framework you've been recommending for five years.

| Your Requirement | How It's Addressed |
|---|---|
| Three regulatory control points | Retained as the complete approval chain — budget verification, COI check, segregation of duties |
| Immutable audit log | Preserved in current system; no architectural change |
| Identity verification at every node | Maintained — no shared accounts, no "approved on behalf of" |
| Documentation completeness at time of decision | **New: intake enforcement** — requests cannot be submitted without required documentation attached |

**Pre-addressing your likely concerns:**

- *"Will removing 4 tiers weaken controls?"* — No. You confirmed these 4 tiers are elective, not regulatory. Your own data shows approvers are clicking "approve" on 60% of requests with incomplete documentation. The current 7-tier chain isn't preventing problems; it's creating an illusion of control while real gaps (PO-splitting, credit card bypass) go untracked. Intake enforcement addresses your top audit finding directly.
- *"What if something goes wrong after the reduction?"* — The control matrix is the foundation. You designed it. The 3 mandatory tiers remain intact. The risk isn't in what's being removed; it's in continuing to operate with workarounds that bypass all 7 tiers entirely.
- *"Were we consulted early enough?"* — Yes. This initiative is built on your framework. Your intellectual contribution is the basis for every design decision. You are being credited publicly because that's what happened.

**What we need from you:** Confirm that the 3 mandatory tier classifications match your control matrix requirements. Flag any conditions under which the 4 elective tiers would need to remain.

---

### Bill Lumbergh — VP of Operations / Executive Sponsor

**The dashboard trade-off: what you gain vs. what you lose**

You asked for speed and visibility. This delivers both. Here's the honest breakdown.

| | Current (Gate) | Proposed (Dashboard) |
|---|---|---|
| **Visibility** | You see requests one at a time, when Peter sends them | Real-time view of all active requests, cycle times, bottleneck locations |
| **Response time** | Your "Go ahead" reply adds 1-2 days per request | Requests move immediately; you monitor outcomes, not individual items |
| **Reporting** | Anecdotal ("it feels like 2-3 weeks") | Hard data — cycle time trends, approver throughput, exception rates |
| **Control** | You decide when something moves forward | You see everything, intervene on exceptions, report aggregate performance upward |
| **Executive value** | You're a checkpoint | You're the person with the data your boss wants |

**What you gain:** The dashboard gives you the metrics you've been telling your boss you need. Cycle time data, bottleneck identification, real-time status across all requests. You become the person who can answer "how's operations doing?" with numbers instead of anecdotes. That's a stronger position than being one approval step in a chain.

**What you lose:** The feeling of "nothing moves without my say-so." That's real. But consider: your courtesy copy was already being worked around. Peter routes around it when timing matters. Your gate was slowing things down without giving you complete information. The dashboard gives you more information, faster, on everything — not just the items that come through your inbox.

**Sequencing matters:** The dashboard goes live before the gate comes down. You'll see it working, experience the visibility it provides, and then the informal gate step is retired. No gap.

**What we need from you:** Confirm that real-time dashboard visibility is an acceptable replacement for the informal gate. Identify any specific request types where you'd want to retain direct approval authority (these can be routed to you as exceptions).

---

### Tom Smykowski — Process Owner

**Your work matters. This section is written for you specifically because your situation is different from everyone else's.**

You built the approval workflow in 2019. That was four months of work, and it was the right solution at the time. Before your process, approvals ran on sticky notes and "OK" emails. You brought structure when the organization needed it. That contribution is documented, it's real, and it's not being erased.

**What's changing and why:**

The Bobs' control matrix distinguishes 3 mandatory regulatory tiers from 4 elective organizational tiers. Two of the elective tiers are the patches you added in 2019 and 2020. You've acknowledged that the 2020 exception path could be scoped down. You've also acknowledged that you'd need to consult your files to recall the rationale for every step. That's not a criticism. It's a sign that the process grew beyond what any one person can carry in their head, which is exactly why the Bobs' framework becomes the single source of truth.

**What's staying:**

Your institutional knowledge of why steps were added, how exception paths work, and what breaks when things change. That knowledge exists in you and in your documentation. It doesn't disappear when the tier count changes.

**Your role going forward — Process Transition Lead:**

This isn't a consolation title. There is specific, necessary work that only you can do:

- **Exception Path Mapping:** You know every exception condition and legacy routing rule. The 3 remaining tiers need to be validated against those edge cases. If you don't map them, someone will miss one.
- **Transition Risk Assessment:** You're worried about unintended consequences. Channel that into a structured deliverable. Identify the top 10 scenarios where dropping the elective tiers could cause a real problem. The team addresses each one. You get to be the person who prevented failures.
- **Process Historian:** Pair with Milton to create a documented record of every process decision, why each step was added, and what it was meant to prevent. This captures the knowledge that lives only in people's heads and turns it into organizational memory.

**Addressing your concern directly:**

You've said "people don't understand what I do here." You're right that many didn't. This initiative changes that by making your knowledge visible and necessary, not by eliminating it. The process you built served the organization for 7 years. The organization's needs have evolved. You have the context to guide that evolution safely, and that's what we're asking you to do.

**What we need from you:** Acknowledge the mandatory/elective distinction from the Bobs' control matrix. Confirm you'll lead the exception path mapping as Process Transition Lead. Flag any specific scenarios where you believe a removed tier is the only control preventing a real (not hypothetical) failure.

---

### Peter Gibbons — Senior Process Analyst

**Your workarounds told us the truth about the process.**

You built a shadow workflow because the official one was too broken to use. You skip steps 4 and 6 on 70% of requests because they're pointless. You fabricate soft approvals to accelerate Samir's sign-off. You route around Lumbergh's gate when timing matters. Those aren't behavior problems. They're rational responses to a process that doesn't work.

**What changes for you:**

The process is being simplified to what you've been saying it should be: three tiers, not nine. The steps you skip become officially unnecessary. The Lumbergh gate goes away. The routing simplifies.

**What this means in practice:** Less time fabricating workarounds. Less time chasing approvals. Less time maintaining shadow spreadsheets. The system starts working the way you've been making it work manually.

**One thing to address:** Your PO-splitting workaround (breaking requests over $10K into smaller pieces to avoid slow steps) also bypasses the conflict-of-interest check. That's a compliance gap that needs to close during transition, not because you did anything wrong, but because the process design left you no good option. In the new 3-tier model, the compliance check will work on consolidated amounts and the threshold games won't be necessary because there are fewer steps to route around.

**What we need from you:** Confirm that the 3-tier model reflects what you've been saying the process should be. Flag any workarounds you currently run that might still be needed after the elective tiers are removed — those would indicate a gap in the new design.

---

### Samir Nagheenanajar — Finance Approver

**Your regulatory tier stays. Your inputs get better.**

Your role as the financial review checkpoint is one of the 3 mandatory control points in the Bobs' framework. That doesn't change. What changes is the quality of what lands on your desk.

**The improvement:**

You've said that 40% of submissions arrive incomplete, forcing rework cycles that add 3-4 days. Intake completeness enforcement means requests can't be submitted without required documentation. Budget codes, cost center verification, vendor quotes — all required before the request enters the approval chain. Your rework cycles drop because the rework shifts to the submitter, where it belongs.

**Your secondary review:**

You've acknowledged that your sub-$50K peer review isn't policy-mandated. You've also said that if you could see what upstream approvers verified — with a timestamp and a name — you wouldn't need to re-verify at your step. That upstream visibility comes in later initiatives (Glass Floor). For now, the elective tier that formalized your secondary review is removed from the organizational process, but nobody is telling you to change your personal review practices. The expectation is that as input quality improves, you'll naturally find less reason to duplicate upstream work.

**What we need from you:** Confirm that the 3 mandatory tiers preserve your regulatory review authority. Identify any specific request types where you believe intake enforcement would be insufficient and your secondary review remains necessary.

---

### Milton Waddams — Long-tenured Specialist

**You have been heard. This section exists because you've been overlooked before and we're not doing that again.**

You've been in this organization since 2011. You process vendor exception requests that aren't on any flowchart. You hold 47 vendor exception records in a personal spreadsheet that the system doesn't have. You recommended step 7 after you predicted the $14,000 late fee. You flagged the duplicate vendor payment issue months before Tom added his exception path. You know about the Facilities SharePoint list that 15% of approval volume runs through. You know about Peter's PO-splitting and the compliance gap it creates.

**Your knowledge is a strategic asset.** The initiative can't succeed without it.

**Your role going forward — Transition Advisor:**

- Your vendor exception spreadsheet gets integrated into the system. Those 47 records become part of the official vendor master, not a personal file.
- You're on the transition review team. By name. On the calendar invite. Not forwarded by Joanna after the fact.
- You pair with Tom on exception path mapping, because between the two of you, every edge case and historical rationale is documented.
- At day 14 post-launch, you lead an exception review to catch anything the new process missed.

**Your vendor terms check (step 7):** The underlying check stays. The mechanism changes. Instead of Peter emailing you informally and waiting 1-2 days, the vendor terms validation becomes part of the intake enforcement layer, populated with the data from your spreadsheet. Your knowledge gets built into the system instead of being routed around it.

**What we need from you:** Confirm you'll participate on the transition review team. Provide your vendor exception spreadsheet for integration into the vendor master. Flag any exception conditions you believe won't be captured by intake enforcement.

---

### Joanna — Department Manager (Requesting Side)

**Fewer steps, faster turnaround, predictable timelines.**

You've been the loudest voice for change, and for good reason. Your team averages 8-12 business days per approval. You've resorted to credit cards and informal request bundling because the process couldn't keep up.

**What changes for you:**

- 7 tiers become 3. Fewer people touching your requests, fewer places for them to stall.
- Intake completeness enforcement means you submit everything upfront. That's new — your team will need to include budget codes, justifications, and supporting docs at submission time. But once a request is in, it moves.
- Status tracking becomes visible. No more Slacking Peter to find out where your request is.
- Routine renewals (same vendor, same terms, same amount) get flagged for expedited review.

**The trade-off:**

Your team's documentation deficiency rate is 60% on first submission (per the Bobs' data). Intake enforcement means incomplete requests get rejected at the door instead of entering the system and stalling downstream. Your team does more work upfront. The payoff is that "submit and wait 12 days" becomes "submit complete and done in days."

**Credit card workaround:** As cycle times drop, the pressure to bypass the process drops with it. Nobody is coming after your team for past workarounds. The goal is to make the official path fast enough that the unofficial path isn't worth the effort.

**What we need from you:** Confirm that predictable SLAs and status visibility address your core needs. Identify any request types where your team would still feel pressure to bypass the process.

---

### Michael Bolton — IT Systems Lead

**No full rebuild. Platform configuration instead.**

You've been pitching a rip-and-replace for two years. This initiative doesn't do that. Here's why, and here's what it does instead.

**Why not a rebuild:**

The current platform supports parallel routing, auto-escalation, conditional branching, and delegation rules. None of them are configured. You said this yourself. The gap isn't the technology — it's that nobody set it up properly when the system launched, and "we'll optimize later" never happened.

**What you're being asked to do:**

- Configure intake validation rules — documentation completeness enforcement that prevents submission without required attachments. This is the change the Bobs have wanted for 5 years.
- Build the dashboard data layer — real-time cycle time metrics, request status tracking, and bottleneck identification for Lumbergh's executive dashboard.
- Configure the parallel routing and auto-escalation features that are already in the platform but inactive.

This is real engineering work. It's "making the current platform do what it should have done from day one." If the platform genuinely can't handle these configurations after you dig in, that becomes the evidence base for the rebuild conversation in a later initiative. Right now, the hypothesis is that the tool is fine and the process was the problem. You get to prove or disprove that.

**What we need from you:** Confirm your understanding of the configuration scope. Flag any technical constraints in the current platform that would prevent intake enforcement or dashboard delivery.

---

## Sign-Off Tracking Matrix

| Stakeholder | Role | Approval Status | Conditions / Reservations | Date |
|---|---|---|---|---|
| The Bobs | Internal Audit / Compliance | Pending | | |
| Bill Lumbergh | VP Operations / Executive Sponsor | Pending | | |
| Tom Smykowski | Process Owner | Pending | | |
| Peter Gibbons | Senior Process Analyst | Pending | | |
| Samir Nagheenanajar | Finance Approver | Pending | | |
| Milton Waddams | Long-tenured Specialist | Pending | | |
| Joanna | Department Manager (Requesting Side) | Pending | | |
| Michael Bolton | IT Systems Lead | Pending | | |

**Process:** Each stakeholder reviews their section, discusses with the project lead, and records their sign-off status. Conditions and reservations are documented here rather than buried in email or meeting notes.

---

## Frequently Anticipated Objections and Prepared Responses

### "Removing tiers weakens our controls."

The Bobs' control matrix — the same framework that governs internal audit — distinguishes 3 mandatory tiers from 4 elective ones. The mandatory tiers stay. Intake completeness enforcement adds a control that didn't exist before. The net control posture improves because documentation deficiency drops and workaround-driven compliance gaps (PO-splitting, credit card bypass) get addressed. Four elective steps that people already skip or route around aren't providing real control. They're providing the appearance of control while actual risk goes unmonitored.

### "This is moving too fast."

The Bobs published their control matrix 18 months ago. The distinction between mandatory and elective tiers has been documented for over a year. Peter has been skipping 2 of these tiers on 70% of requests with no consequences. The process is already operating without these tiers in practice. This initiative makes the official process match reality and closes the gaps that reality created.

### "What if an edge case falls through the cracks?"

Tom and Milton are mapping every exception path as part of the transition. Between them, they hold the complete institutional knowledge of why every step was added and what it was designed to catch. The 3 mandatory tiers are validated against those edge cases before go-live. Post-launch exception reviews at day 14 and day 30 catch anything that was missed.

### "I need to keep my extra review step because I don't trust upstream work."

That's Samir's position, and it's rational given the current system. The system shows approvers nothing about what upstream reviewers checked. Upstream visibility (Initiative 3) solves this structurally. In the meantime, intake completeness enforcement raises the quality bar on what reaches every approver. The redundant review is a symptom of a visibility problem, not a permanent need.

### "My role is being eliminated."

No roles are being eliminated. Tom becomes Process Transition Lead with specific deliverables that use his institutional knowledge. Milton becomes a Transition Advisor whose vendor exception data gets integrated into the system. The work changes — the people don't.

### "The tool can't handle this — we need a new platform."

Michael confirmed the current platform supports parallel routing, conditional branching, auto-escalation, and delegation rules. None are configured. This initiative configures them. If the platform genuinely can't deliver after configuration, that's evidence for a rebuild conversation in a later initiative. The hypothesis gets tested, not assumed.

### "We tried something like this before and it failed."

The initiative Lumbergh referenced added MORE routing rules to a process that was already too long. This does the opposite — it removes the rules that compliance never required. Different approach, different basis (the Bobs' control matrix vs. Tom's routing logic), different outcome.

### "What about the Facilities SharePoint list?"

Milton identified that 15% of approval volume runs through a separate SharePoint list IT doesn't know about. This initiative doesn't consolidate that system, but it's documented and scoped for a later initiative. The sign-off package acknowledges the gap so nobody is surprised.

---

## Appendix: Evidence Sources

| Source | Path | Relevance |
|---|---|---|
| Bobs' Control Matrix | Referenced in Bobs' interviews and Input Synthesis | Mandatory vs. elective tier distinction |
| Input Synthesis | `docs/design-thinking/context/2026-03-02-02-input-synthesis.md` | 6 themes, contradictions, hidden insights |
| Stakeholder Map | `docs/design-thinking/context/2026-03-02-01-stakeholder-map.md` | Power-interest matrix, key dynamics |
| Initiative 1 Scope | `docs/project/initiatives/01-clear-the-path/00-initiative-scope.md` | What's being removed, what stays, scope boundaries |
| Stakeholder Impact Analysis | `docs/project/initiatives/01-clear-the-path/03-stakeholder-impact.md` | Impact matrix, resistance map, adoption strategy, Tom strategy |
| Interview Transcripts | `docs/design-thinking/meeting-transcripts/` | 14 transcripts, 8 personas — direct quotes and context |
| Tier Classification (Dallas) | Referenced from Initiative 1 Scope — Dallas's process design work | Tier mapping to Bobs' control matrix |
