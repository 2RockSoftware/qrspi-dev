---
name: qrspi-dev-questions
description: Refine rough project ideas into clarified requirements through open dialogue. Use in the Questions phase of QRSPI development. Produces questions.md.
---

# Questions Phase

Convert rough ideas into clarified requirements through open, freeform dialogue. Think of it as two teammates fleshing out their ideas until the foundation is solid enough to build on.

## How It Works

1. **Start from the user's input** — what they described when launching the orchestrator, or their initial idea
2. **Ask open-ended questions** to uncover the full picture — requirements, constraints, edge cases, success criteria, integrations, deployment context, scope boundaries
3. **Follow the threads** — answers may spawn new questions. That's fine and expected.
4. **Write `questions.md`** as you go, capturing clarified requirements, resolved decisions, and remaining uncertainties

## What to Ask About

Use the [question guide](references/question-guide.md) for categories and examples. At minimum, cover:

- **Functional requirements** — what should the system do?
- **Non-functional requirements** — performance, scalability, reliability, security expectations
- **Integrations** — does it connect to existing systems, APIs, databases?
- **Constraints** — tech stack preferences, budget, timeline, platform requirements
- **Edge cases** — unusual inputs, failure modes, error handling expectations
- **Success criteria** — how will you know when this is done and working?
- **Scope boundaries** — what is explicitly out of scope?

## The Transition Signal

**Know when to stop.** The Questions phase is complete when the conversation shifts from:

> ❌ "What should this do?" / "How should it work?"

to:

> ✅ "Where does this connect to the existing codebase?" / "How should we implement X?" / "What technology should we use?"

That shift means you've clarified the *what* and *why*. The *how* is research and design territory.

## Output Artifact

Write `questions.md` with:

- **Clarified requirements** — organized by category
- **Resolved decisions** — choices that were made during the dialogue
- **Remaining uncertainties** — anything still unclear (pass to Research to investigate)

## Human Checkpoint

After writing `questions.md`, **stop and wait for approval**. Say something like:

> "Questions phase is complete. I've written `questions.md` with our clarified requirements. Review it and let me know if we're ready to move on to Research, or if anything needs adjustment."

Do not proceed to Research until the human explicitly approves or requests changes.
