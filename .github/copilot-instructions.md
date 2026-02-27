# Copilot Instructions — fluffyturtle

This is a **live workshop demo repo** for an HVE Design Thinking session. There is no application code. All meaningful work lives in `.github/` (agents, instructions, prompts, skills) and `docs/engagement/design-thinking/`.

---

## Architecture

```plaintext
.github/
├── agents/          ← Copilot agent definitions (.agent.md) — tool declarations, variables, system prompts
├── instructions/    ← Scoped instruction files applied via `applyTo` frontmatter glob
├── prompts/         ← Reusable slash-command prompts (.prompt.md)
├── skills/          ← Domain skill files (SKILL.md per subdirectory)
└── ISSUE_TEMPLATE/  ← Issue templates with DT traceability fields

docs/engagement/design-thinking/
├── context/         ← Active DT session state (agents read this first to restore context)
├── interview-questions/
├── meeting-agendas/
└── meeting-transcripts/
```

The two primary agents are `dt-facilitator-lean.agent.md` and `dt-facilitator-beast.agent.md`. Beast has terminal tool access; Lean does not.

---

## File Conventions

- **All dated files** use prefix `yyyy-mm-dd-xx-description.md` — e.g. `2026-02-27-01-synthesis-notes.md`
- **Agent files** use a `chatagent` fenced block with YAML frontmatter (name, description, target, tools, variables)
- **Instruction files** use an `instructions` fenced block; `applyTo` frontmatter governs scope as a glob
- **Skill files** use a `skill` fenced block and live at `.github/skills/<name>/SKILL.md`
- **Prompt files** use a `prompt` fenced block with `name`, `description`, `agent`, and `tools` frontmatter

---

## Key Paths (used as agent variables)

| Variable | Path |
|---|---|
| `CONTEXT_FOLDER` | `docs/engagement/design-thinking/context` |
| `MEETING_TRANSCRIPTS_FOLDER` | `docs/engagement/design-thinking/meeting-transcripts` |
| `INTERVIEW_QUESTIONS_FOLDER` | `docs/engagement/design-thinking/interview-questions` |
| `MEETING_AGENDAS_FOLDER` | `docs/engagement/design-thinking/meeting-agendas` |
| `PROJECT_FOLDER` | `docs/engagement/engagement` |

---

## Issue Templates

Five templates under `.github/ISSUE_TEMPLATE/`: `feature.md`, `story.md`, `task.md`, `bug.md`, `risk.md`. All feature and story issues must include a **DT Source** section tracing back to the design thinking session, scenario name, route, and date.

---

## Communication Style (agents and instructions)

See [`.github/instructions/communication-guidelines.instructions.md`](instructions/communication-guidelines.instructions.md). Short version: think like a coach, speak like a colleague, always end with options. Avoid "comprehensive", "thorough", hyphens, and colons in suggestions.
