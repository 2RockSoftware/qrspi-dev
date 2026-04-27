---
name: qrspi-pi-design
description: Make architectural decisions by surfacing 2-3 viable approaches with tradeoffs, converging with the user, then synthesizing the chosen approach into design.md. Use in the Design phase of QRSPI development.
model_tier: frontier-top
inputs: questions.md, research.md
outputs: design.md
forbidden_inputs: none
---

# Design Phase

Make the architectural decisions: *where we're going.* Design says what to build and why. Structure (next phase) decomposes that into phased, vertical-slice work — Design does **not** produce a phased breakdown.

## Phase Contract

- **Reads:** `questions.md` (decisions and remaining uncertainties), `research.md` (facts and open questions for Design).
- **Writes:** `design.md` at the project root.
- **Does not read:** there are no forbidden inputs at this phase. Design is allowed to reach back into anything upstream.
- **Tier:** `frontier-top`. Design is where the engineer makes the real decisions; the discussion at this phase catches the bulk of architectural mistakes. Frontier reasoning is most valuable here.

## How It Works

1. **Read `questions.md`** — the clarified requirements, resolved options, and open items.
2. **Read `research.md`** — the facts and the Open Questions surfaced for Design.
3. **Surface 2-3 viable approaches with tradeoffs in dialogue.** Do not jump straight to a recommendation. For each major decision (architecture pattern, data layer, auth, key integrations, observability, …), present candidates side-by-side with tradeoffs. Use the [Approach Comparison Template](references/design-patterns.md#approach-comparison-template).
4. **Converge with the user.** The user picks (or asks for more options). Design is where humans should weigh in most.
5. **Synthesize the chosen approach into `design.md`** — the architecture blueprint, with rejected alternatives noted briefly.

## What to Include

### High-Level Architecture
- Components and modules that make up the system.
- How they relate to each other (text diagrams or clear descriptions).
- The overall architectural pattern (layered, event-driven, modular, etc.).

### Component Responsibilities
- What each component does.
- What it doesn't do.
- Its dependencies and dependents.

### Data Flow
- How data enters the system.
- How it moves between components.
- How data is stored and retrieved.
- How data exits the system.

### Key Interfaces
- External APIs (what the system exposes).
- Internal interfaces (how components communicate).
- Integration points (connections to existing systems).

### Technology Choices
- What technologies are used and why, grounded in `research.md`.
- Alternatives considered and the tradeoff that decided it.

### Cross-Cutting Concerns
- Authentication and authorization approach.
- Error handling strategy.
- Logging and observability.
- Configuration management.
- Testing strategy.

## What to Avoid

- **Implementation details** — no code, no file paths, no function signatures.
- **Task-level decisions** — that's the Plan phase's job.
- **Phased breakdown of work** — that's **Structure's** job. Design says *where we're going*; Structure says *how we get there*.
- **UI mockups or wireframes** — describe the interface conceptually, not visually.
- **Over-specification** — keep it broad enough that multiple implementations are possible.

## Output Artifact

`design.md` contains:

- **Architecture overview** — high-level system description.
- **Component diagram** — text-based or markdown description of component relationships.
- **Component descriptions** — what each component does and its responsibilities.
- **Data flow description** — how data moves through the system.
- **Interface specification** — key APIs and integration points.
- **Technology decisions** — choices with the rejected-alternatives delta.
- **Cross-cutting concerns** — auth, error handling, logging, etc.

## Human Checkpoint

After writing `design.md`:

> "Design phase is complete. I've written `design.md` with the architecture blueprint. Review it and let me know if we're ready to move on to Structure, or if anything needs adjustment."

Do not proceed to Structure until the human explicitly approves or requests changes.

## Triggers for Escalation to the User

Stop the phase and surface to the user when:

- **`questions.md` and `research.md` contradict on a load-bearing requirement** — likely upstream cause: Questions. Suggested user action: re-run Questions to resolve the conflict.
- **No viable approach exists given the stated constraints** (the constraints are mutually exclusive, or no candidate technology meets them) — likely upstream cause: Questions (constraints) or Research (facts wrong). Suggested user action: relax a constraint, or re-run Research with the gap named.
- **The user wants to skip the options-with-tradeoffs step and just be told what to build** — likely upstream cause: process mismatch. Suggested user action: confirm — Design is the most consequential phase, and skipping option comparison is a known way to lock in the wrong architecture. Proceed only if the user explicitly accepts that risk.
