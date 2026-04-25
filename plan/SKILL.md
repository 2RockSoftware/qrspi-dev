---
name: qrspi-dev-plan
description: Write a detailed implementation plan with checkbox task list for the Implement phase. Use in the Plan phase of QRSPI development. Produces plan.md.
---

# Plan Phase

Write the full, detailed implementation plan. Humans skim it; agents rely on it during implementation.

## How It Works

1. **Read `outline.md`** — understand the broad-stroke plan and structure
2. **Read `design.md`** — understand the architecture and technical decisions
3. **Write `plan.md`** — a detailed, actionable plan with checkbox tasks

## What to Include

### Context Section
A brief reminder of the project and its key decisions. This is for the agent executing the plan to understand the "why" behind the tasks. Include:
- Project summary (1-2 paragraphs)
- Key technology choices and why they were made
- Important constraints from the requirements
- Any cross-cutting concerns that apply to all tasks

### Task List
A numbered, sequential list of tasks with `[ ]` checkboxes. Each task should include:
- **Title** — what the task accomplishes
- **Description** — what to do, in enough detail for an agent to execute without ambiguity
- **Files** — which file(s) to create, modify, or reference
- **Dependencies** — which tasks must be completed first
- **Decisions to apply** — which design/research decisions inform this task

Tasks must be:
- **Granular** — each task should be completable without splitting
- **Ordered** — each builds on previous work
- **Self-contained** — each task has clear start and end states
- **Actionable** — an agent can execute them without additional clarification

### Worktree Guidance
Rough guidance for what the Worktree phase should produce. This helps the Worktree skill understand what scaffolding and tooling to set up. Include:
- Expected directory structure
- Technology stack for tooling setup
- Any special tooling considerations

### What Not to Include
- **Long blocks of code** — describe what to write, don't write it
- **Implementation minutiae** — specific function bodies, exact variable names
- **UI designs** — describe interface conceptually, don't design pixels

## Output Artifact

Write `plan.md` with:

- **Context** — project summary, tech choices, constraints, cross-cutting concerns
- **Phase 1: [Phase Name]** — tasks for the first implementation phase
  - [ ] Task 1 — description, files, dependencies
  - [ ] Task 2 — description, files, dependencies
  - [ ] ...
- **Phase 2: [Phase Name]** — next set of tasks
  - [ ] ...
- **Worktree Guidance** — scaffolding and tooling expectations

## Human Checkpoint

After writing `plan.md`, **stop and wait for approval**. Humans will skim this — they want to verify the plan makes sense, not review every task. Say:

> "Plan phase is complete. I've written `plan.md` with the detailed implementation plan. Take a quick look — if it looks right, we'll move on to Worktree. Let me know if any changes are needed."

Incorporate any feedback and re-prompt for approval before proceeding. Do not move to the Worktree phase until the plan is approved.
