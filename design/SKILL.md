---
name: qrspi-dev-design
description: Create a broad architecture document that synthesizes requirements and research findings into a design blueprint. Use in the Design phase of QRSPI development. Produces design.md.
---

# Design Phase

Create a broad architecture document that synthesizes requirements (from Questions) and research findings into a concrete design blueprint.

## How It Works

1. **Read `questions.md`** — understand the clarified requirements and constraints
2. **Read `research.md`** — understand the findings, patterns, and technology recommendations
3. **Design the architecture** — synthesize everything into a coherent system design
4. **Write `design.md`** — the architecture blueprint

## What to Include

### High-Level Architecture
- Components and modules that make up the system
- How they relate to each other (use text diagrams or clear descriptions)
- The overall architectural pattern (layered, event-driven, modular, etc.)

### Component Responsibilities
- What each component does
- What it doesn't do
- Its dependencies and dependents

### Data Flow
- How data enters the system
- How it moves between components
- How data is stored and retrieved
- How data exits the system (responses, output, notifications)

### Key Interfaces
- External APIs (what the system exposes)
- Internal interfaces (how components communicate)
- Integration points (connections to existing systems)

### Technology Choices
- What technologies are used and why
- Ground choices in the research findings
- Note alternatives considered and why they were rejected

### Cross-Cutting Concerns
- Authentication and authorization approach
- Error handling strategy
- Logging and observability
- Configuration management
- Testing strategy

## What to Avoid

- **Implementation details** — no code, no file paths, no function signatures
- **Task-level decisions** — that's the Plan phase's job
- **UI mockups or wireframes** — describe the interface conceptually, not visually
- **Over-specification** — keep it broad enough that multiple implementation approaches are possible

## Output Artifact

Write `design.md` with:

- **Architecture overview** — high-level system description
- **Component diagram** — text-based or markdown description of component relationships
- **Component descriptions** — what each component does and its responsibilities
- **Data flow description** — how data moves through the system
- **Interface specification** — key APIs and integration points
- **Technology decisions** — choices with justification
- **Cross-cutting concerns** — auth, error handling, logging, etc.

## Human Checkpoint

After writing `design.md`, **stop and wait for approval**. Say:

> "Design phase is complete. I've written `design.md` with the architecture blueprint. Review it and let me know if we're ready to move on to Outline, or if anything needs adjustment."

Do not proceed to Outline until the human explicitly approves or requests changes.
