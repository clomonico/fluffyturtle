```skill
---
name: "dt-workshop-walkthrough"
description: "Step-by-step guided walkthrough of the HVE Design Thinking workshop — one stage at a time, with suggested prompts at each step. The user controls the pace."
domain: "design-thinking"
confidence: "high"
source: "earned"
---

# dt-workshop-walkthrough

Guide the user through the HVE Design Thinking workshop scenario one stage at a time. Never advance to the next stage unless the user explicitly asks you to. At the end of each stage, provide the suggested prompt for the next step and ask the user if they want to continue or try a different angle.

## Persistence Rule

> **IMPORTANT — applies to every stage without exception.**
>
> Any artifact produced during this session must be saved to the correct location in the workspace immediately upon creation. Do not wait for the user to ask. Do not summarize in chat and skip the file. If you generated it, write it to the context.
>
> Use the date-prefixed naming convention `yyyy-mm-dd-nn-[descriptor].md` for all files. Resolve the actual date and sequence number at the time of creation. Each stage below specifies the correct target folder for its artifacts — follow those exactly.

## The Scenario

Every stage in this walkthrough is grounded in this request:

> "Our approval process is killing us. It takes too long, too many people are involved, and things fall through the cracks. We need a modern system to replace what we have. Can you build it?"
> — VP of Operations

## Pacing Rule

After completing each stage, always close with:

1. A one-sentence summary of what was just accomplished.
2. The exact suggested prompt for the next step (in a code block).
3. A gate — "Want to continue to Stage [N], or would you like to try a different angle here?" — and wait for the user to respond before proceeding.

Do not include presenter notes, audience cues, or narration. Focus only on what to do and what to say.

---

## Stage 1 — Scope Conversations

**What this stage does:** Classifies the VP's request and surfaces the questions you need to ask before scoping anything.

**Send this to the agent:**

```
Hey I just got handed a request from a VP, its a vague one but it needs attention.

They said: "Our approval process is killing us.
It takes too long, too many people are involved, and things fall through the cracks.
We need a modern system to replace what we have. Can you build it?"

We haven't started any scoping yet. Where do I begin?
```

**What to expect:** The agent classifies this as a Frozen Request and routes to Method 1: Scope Conversations.

**Next — discovery questions:**
```
Help me generate some discovery questions that we
should be asking before we scope anything.
```

> **File output:** Save the generated discovery questions to `docs/engagement/design-thinking/interview-questions/` using the date prefix and a short descriptive name (e.g. `yyyy-mm-dd-nn-discovery-questions.md`).

**Next — stakeholder map:**
```
Good. Now let's map the stakeholders.
Who do we need to consider for something like this?
```

> **Persona rule:** Map all stakeholder personas using characters from the *Office Space* universe. Assign each character a role, their core motivation, and their blind spot.

> **File output:** Save the completed stakeholder map to `docs/engagement/design-thinking/context/` using the date prefix and a short descriptive name (e.g. `yyyy-mm-dd-nn-stakeholder-map.md`).

**Stage 1 complete** — request classification, discovery questions, and a stakeholder map are in hand.

---

**Suggested prompt to continue:**
```
Good. Now let's map the stakeholders.
Who do we need to consider for something like this?
```

Want to continue to Stage 2, or would you like to try a different angle here?

---

## Stage 2 — Design Research + SME Simulation

**What this stage does:** Simulates interviews with each stakeholder persona to surface contradictions and hidden insights.

**Send this to the agent:**
```
I want you to simulate interviews with each of the stakeholders we mapped.
Embody each persona and answer as that person would —
with their blind spots,
their priorities,
and at least one thing they reveal
without realizing they've said something important.
```

**What to expect:** Each persona gives a different version of the same problem, with at least one direct contradiction between them.

> **File output:** Save each persona's interview individually to `docs/engagement/design-thinking/meeting-transcripts/` using the date prefix, a sequential number, and the character name (e.g. `yyyy-mm-dd-nn-interview-[character-name].md`). Do not combine personas into a single file.

**If the output isn't deep enough:**
```
Simulate realistic interviews for each persona in our stakeholder map. For each persona:
- Answer through the lens of their role, incentives, and blind spots
- Include at least one direct pain quote
- Include at least one moment where this persona contradicts what another persona would say
- Include one thing they reveal without realizing its significance
Format as short Q&A exchanges — 6 to 8 per persona, not essay responses.
```

**Stage 2 complete** — you have simulated interviews with competing perspectives across all personas.

---

**Suggested prompt to continue:**
```
Let's do the synthesis. Look at everything we got from those
interviews and find the patterns.
What's the actual problem underneath the
request we started with?
```

Want to continue to Stage 3, or would you like to go deeper on any of the personas first?

---

## Stage 3 — Input Synthesis

**What this stage does:** Finds patterns across the interview data and produces a reframed problem statement.

> Stay in the same conversation. Synthesis runs against the Stage 2 interview history.

**Send this to the agent:**
```
Let's do the synthesis. Look at everything we got from those
interviews and find the patterns.
What's the actual problem underneath the
request we started with?
```

**What to expect:** 4 to 6 named themes, 2 to 3 contradictions, and a reframed problem statement that identifies whether this is a tooling problem, a process problem, or something else.

**If the output is too shallow:**
```
Perform a full Input Synthesis using the simulated interviews from all personas. Produce:
1. 4 to 6 primary themes — named, described in 1 to 2 sentences, with persona citations
2. 2 to 3 significant contradictions — conflicting needs, assumptions, or definitions of success
3. 1 to 2 hidden insights — things a persona revealed without recognizing its significance
4. A reframed problem statement that names whether this is a tooling problem, a process problem, or something else
Be explicit about the real problem beneath the original request.
```

**Stage 3 complete** — you have a reframed problem statement grounded in research.

---

**Suggested prompt to continue:**
```
Generate How Might We statements from our synthesis.
They should be open enough to invite multiple solution directions,
but specific enough to stay anchored to the real problem
we just identified.
```

Want to continue to Stage 4, or would you like to revisit any of the synthesis themes first?

---

## Stage 4 — How Might We + Brainstorming

**What this stage does:** Reframes the problem into HMW statements, then runs a SCAMPER brainstorm and clusters the output into distinct implementation options.

> Stay in the same conversation. This runs against the synthesis from Stage 3.

**Step 1 — HMW statements:**
```
Generate How Might We statements from our synthesis.
They should be open enough to invite multiple solution directions,
but specific enough to stay anchored to the real problem
we just identified.
```

**Step 2 — divergent brainstorm:**
```
Let's brainstorm.
Use SCAMPER against those HMW statements.
Give me a wide range. Include obvious ideas,
surprising ones,
ideas that would probably fail,
and at least one idea that reframes the problem entirely.
Don't filter yet.
```

**Step 3 — converge into options:**
```
Now cluster those ideas into between 3 and 5 distinct implementation options
that I can review.
Each option needs a short memorable name
(a direction, not a feature name),
a brief but clear description of
what it builds or does, the HMW statement
it addresses, and the key risk or assumption
we would need to prove out.
```

**If options aren't distinct enough:**
```
Cluster the brainstormed ideas into exactly 5 implementation options. Options must be meaningfully distinct — no overlapping purpose or approach. For each option:
1. A short memorable name (direction, not feature)
2. What it builds or does in 2 sentences
3. Which brainstorm ideas it incorporates
4. The primary HMW statement it addresses
5. The key risk or assumption that could invalidate it
```

**Stage 4 complete** — you have 3 to 5 named implementation options, each tied to a HMW statement and a named risk.

---

**Suggested prompt to continue:**
```
Using the implementation options we generated,
create a GitHub Feature issue for each one in the
repository using the gh CLI.
Use the template in .github/ISSUE_TEMPLATE/feature.md
```

Want to continue to Stage 5 to confirm the options before voting, or would you like to refine any of the options first?

---

## Stage 5 — Confirm the Options

**What this stage does:** No agent needed. Confirm all option names are clearly visible before the vote.

**What to do:**
- Scroll back through the conversation and confirm all option names and 1-sentence descriptions are visible.
- Copy them somewhere easy to reference before moving forward.

**Stage 5 complete** — options are locked and documented.

---

Want to continue to Stage 6 to vote, or do you need a moment to confirm the options are captured?

---

## Stage 6 — Vote

**What this stage does:** No agent. No terminal. Choose one option to move forward with.

**What to do:**
- All option names and 1-sentence descriptions should be visible.
- Pick one. This is the direction the backlog will be built from.

**Stage 6 complete** — you have a winning option.

---

Want to continue to Stage 7 to create the GitHub issues, or would you like to revisit any of the options?

---

## Stage 7 — Feature-Level GitHub Issues

**What this stage does:** The agent reads all context built throughout the session and creates a GitHub Feature issue for each implementation option using the gh CLI.

**Send this to the agent:**
```
Using the implementation options we generated,
create a GitHub Feature issue for each one in the
repository using the gh CLI.
Use the template in .github/ISSUE_TEMPLATE/feature.md
```

**What to expect:** One issue per option, each carrying the description, the HMW statement it addresses, and the risk named during brainstorming.

**Stage 7 complete** — the backlog is live in GitHub, rooted in the research this session produced.

---

Want to wrap up with Stage 8, or would you like to review any of the issues first?

---

## Stage 8 — Closing

**What this stage does:** Reflect on what was built — from one vague request to a research-backed GitHub backlog.

**Three things to hold onto:**

1. The original request was to build a modern approval system. The backlog direction that won came from what the research actually surfaced — not from the original ask.
2. The session moved from one sentence to discovery, synthesis, ideation, and live issue creation — without a single pre-written prompt.
3. The backlog isn't a task list. It carries the reasoning that produced it.

**Session complete.**
```
