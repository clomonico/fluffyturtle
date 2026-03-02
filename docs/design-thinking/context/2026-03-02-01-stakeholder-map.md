# Stakeholder Map — Approval Process Overhaul

**Date:** 2026-03-02
**Source request:** "Our approval process is killing us..." — VP of Operations
**Persona universe:** Office Space

---

## Primary Stakeholders (Decision Makers & Daily Users)

| Persona | Character | Role | Core Motivation | Blind Spot |
|---|---|---|---|---|
| **Bill Lumbergh** | VP of Operations | Executive sponsor who made the request. Owns the budget and the mandate. | Wants visible, measurable improvement he can report up the chain. Needs to look decisive. | Assumes the problem is the tool, not the process he helped design. Doesn't see how his own layer of "just loop me in" adds cycle time. |
| **Peter Gibbons** | Senior Process Analyst | The person who actually routes approvals day to day. Knows every workaround. | Wants to stop doing pointless work. Deeply unmotivated by a process he sees as theater. | Has mentally checked out enough that he's stopped escalating real issues. His workarounds mask how broken things really are. |
| **Samir Nagheenanajar** | Finance Approver | Mid-level approver who signs off on budget-related requests. | Wants accuracy and audit readiness. Afraid of approving something that blows up later. | Over-approves cautiously — adds review cycles "just in case," not because policy requires it. Doesn't realize he's a bottleneck. |

## Secondary Stakeholders (Influencers & Supporters)

| Persona | Character | Role | Core Motivation | Blind Spot |
|---|---|---|---|---|
| **Michael Bolton** | IT Systems Lead | Maintains the current approval tooling and integrations. | Wants modern tech stack and less maintenance burden. Tired of duct-taping legacy systems. | Sees every problem as a technology problem. Will push for a rebuild when a config change might suffice. |
| **Joanna** | Department Manager (Requesting Side) | Submits approval requests on behalf of her team. The "customer" of the process. | Wants fast, predictable turnaround so her team isn't blocked. | Doesn't understand why approvals take multiple steps. Sees all governance as friction, even the parts that protect her. |
| **Tom Smykowski** | Middle Manager / Process Owner | Wrote the original approval workflow. Considers it his legacy contribution. | Wants to remain relevant. The process is his justification for existing. | Will frame any change as risky because simplifying the process threatens his role. Won't say this out loud. |

## Hidden Stakeholders (Compliance, Risk & Quiet Influence)

| Persona | Character | Role | Core Motivation | Blind Spot |
|---|---|---|---|---|
| **The Bobs** | Internal Audit / Compliance | Don't interact with the process daily but will shut down anything that weakens controls. | Need to demonstrate regulatory compliance and audit trail integrity. | Only see the process through the lens of risk mitigation. Don't weigh productivity cost against compliance cost. |
| **Milton Waddams** | Long-tenured Specialist | Has been in the org forever. Knows the history of every approval exception and edge case. | Wants to be heard and acknowledged. Holds institutional memory no one asks for. | Has critical context about why certain steps exist, but nobody thinks to include him. If the process changes without his input, unexpected failures will surface. |

---

## Power vs. Interest Matrix

```
           HIGH INTEREST
                │
   Lumbergh     │    Peter, Joanna
   (high power, │    (high interest,
    high interest)    lower power)
                │
HIGH POWER ─────┼───── LOW POWER
                │
   The Bobs     │    Milton, Tom
   (high power, │    (low interest/power
    low interest)│    but high impact)
                │
           LOW INTEREST
```

## Key Dynamics to Watch

- **Lumbergh ↔ Tom:** Lumbergh wants to modernize; Tom's identity is tied to the current process. This tension will play out as passive resistance disguised as "risk concerns."
- **Samir ↔ Joanna:** Samir adds review cycles for safety; Joanna sees them as pointless delays. Neither is wrong — they're optimizing for different things.
- **Peter ↔ Everyone:** Peter's workarounds are so embedded that the current process appears more functional than it actually is. If we map the "real" workflow vs. the "official" one, Peter is the key informant.
- **Milton:** The wildcard. He knows why step 7 exists, why the exception for procurement over $50K was added in 2019, and what happened the last time someone tried to simplify this. Ignore him at your peril.
