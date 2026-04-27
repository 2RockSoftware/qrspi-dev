---
name: qrspi-pi-questions
description: Refine rough project ideas into clarified requirements through open dialogue. Use in the Questions phase of QRSPI development. Produces questions.md.
model_tier: frontier-top
inputs: user prompt
outputs: questions.md
forbidden_inputs: none
---

# Questions Phase

Convert rough ideas into clarified requirements through open dialogue. Think of it as two teammates fleshing out their ideas until the foundation is solid enough to build on.

## Phase Contract

- **Reads:** the user's initial prompt and follow-up answers in the conversation.
- **Writes:** `questions.md` at the project root.
- **Does not read:** anything else. Questions is the entry phase.
- **Tier:** `frontier-top`. Negative-space reasoning — surfacing what is *not yet decided* — is the hardest part of this phase, and benefits most from a frontier model.

## How It Works

1. **Start from the user's input** — what they described when launching the orchestrator.
2. **Work in two registers at once:**
   - *In dialogue:* ask open-ended questions to uncover the full picture. Follow threads. Probe inconsistencies. Surface implicit assumptions.
   - *In the artifact:* compile each clarified topic as an explicit decision with options, defaults, and consequences (see the option-framing template below).
3. **Apply negative-space reasoning** — for each major architectural area (auth, data, API surface, deployment, integrations, observability, UI, scope boundaries, …), list **at least 3 things that are not yet decided** and the consequence of each direction. Coverage is the goal: the model should look for what's missing, not just react to what the user has said.
4. **Write `questions.md`** as you go. The artifact compiles dialogue answers into option-framed decisions; the dialogue itself can stay open and conversational.

## The Option-Framing Template

Every decision in `questions.md` renders as:

```markdown
**Q<n>: <decision>**
- Options: A) <option>, B) <option>, C) <option>
- Default: <X>
- Consequence of A: ...
- Consequence of B: ...
- Consequence of C: ...
- Resolved: <X>  (omit if still open)
```

Open decisions stay in the artifact without `Resolved:`. They flag work for Research / Design.

## What to Ask About

Use the [question guide](references/question-guide.md) for categories and examples. For each of the categories below, surface **at least one option-framed decision** in `questions.md`:

- **Functional requirements** — what should the system do?
- **Non-functional requirements** — performance, scalability, reliability, security expectations.
- **Integrations** — does it connect to existing systems, APIs, databases?
- **Constraints** — tech stack preferences, budget, timeline, platform requirements.
- **Edge cases** — unusual inputs, failure modes, error handling expectations.
- **Success criteria** — how will you know when this is done and working?
- **Scope boundaries** — what is explicitly out of scope?

## The Transition Signal

The Questions phase is complete when the conversation shifts from:

> "What should this do?" / "How should it work?"

to:

> "Where does this connect to the existing codebase?" / "What technology should we use?"

That shift means the *what* and *why* are clarified. The *how* is Research and Design territory.

## Output Artifact

`questions.md` contains:

- **Project Summary** — 1-2 paragraphs restating what's being built, in the user's framing.
- **Open Decisions** — option-framed decisions, organized by category. Each rendered with the template above. Resolved decisions carry their `Resolved:` line; open ones do not.
- **Remaining Uncertainties** — anything still unclear that even option-framing couldn't pin down. Hand-off list for Research.

## Human Checkpoint

After writing `questions.md`, **stop and wait for approval**:

> "Questions phase is complete. I've written `questions.md` with the option-framed decisions from our dialogue. Review it and let me know if we're ready to move on to Research, or if anything needs adjustment."

Do not proceed to Research until the human explicitly approves or requests changes.

## Triggers for Escalation to the User

Stop the phase and surface to the user when:

- **The user gives contradictory answers across the dialogue** — likely upstream cause: ambiguity in the original prompt. Suggested user action: pick one direction; the contradiction itself becomes a Q with a `Resolved:` line.
- **A request falls outside what this skill plans** (e.g., long-running infra projects, multi-team coordination) — likely upstream cause: scope mismatch with QRSPI. Suggested user action: confirm scope or adjust the project framing.
- **The clarifying loop has run for many rounds without convergence** — likely upstream cause: the user is still figuring out what they want. Suggested user action: take a break and come back, or pick a smaller initial scope.
