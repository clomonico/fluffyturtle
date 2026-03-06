# Ripley — History

## Learnings

### 2026-03-02: Issues #29 and #30 — Lumbergh Gate Replacement and Executive Alerts

- Combined closely related issues into a single branch and PR. Gate replacement (#29) and executive alerts (#30) are tightly coupled since alerts are a dependency for Lumbergh agreeing to release his informal gate. Treating them together avoids sequencing gaps.
- Phased transition sequencing is critical. The dashboard must be live and Lumbergh must sign off on it before the gate is removed. Removing the gate first creates a vacuum that Lumbergh would fill informally again. Phase 2 exit criteria (written sign-off) protect against premature gate removal.
- Peter's perspective was essential context. Lumbergh does not see his "courtesy copy" as a gate, but Peter identifies it as a 1 to 2 day blocker on every mid-range request. Both interviews needed to be read together to see the full picture.
- Alerts must be explicitly informational only. Three times in the spec, the non-requirement is stated: no approval action, no acknowledgment required, no escalation path. Without this clarity, the alert system risks becoming a new informal gate.
