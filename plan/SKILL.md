---
name: qrspi-pi-plan
description: Write a detailed, mechanical implementation plan for one slice. Per-slice plans run in fresh contexts and are suitable for smaller models. Use in the Plan phase of QRSPI development. Produces slices/slice-N/plan.md.
model_tier: local
inputs: structure.md, design.md, current slice's section
outputs: slices/slice-N/plan.md
forbidden_inputs: questions.md, research.md, other slices' plans
---

# Plan Phase

Write the implementation plan for **one slice**. Plan-slice runs once per slice from `structure.md`. By the time Plan runs, all hard decisions are made — Plan just fills in the mechanical details.

If Plan is doing real thinking, something upstream was skipped. The output should be boring; that's the goal.

## Phase Contract

- **Reads:** `structure.md` (the full slice list, to know context and ordering) and `design.md` (the architecture being implemented). Reads the **current slice's section only** — not other slices'.
- **Writes:** `slices/slice-N/plan.md` for the current slice.
- **Does not read:** `questions.md`, `research.md`, or other slices' `plan.md` files. Those decisions are already encoded into `structure.md` and `design.md`. If Plan needs them directly, Structure or Design under-specified the slice.
- **Tier:** `local`. Once the slice is well-scoped, planning is mechanical task enumeration. A smaller local model can handle this in fresh context.

## How It Works

1. **Read `structure.md`** — find the current slice's section. Note its capability, stack coverage, verification, and footprint.
2. **Read `design.md`** — load the architecture decisions relevant to the slice's footprint.
3. **Enumerate tasks** — break the slice into ordered, self-contained tasks. Each task is granular enough to complete in one go.
4. **Define verification** — name the exact tests / lint / typecheck commands that confirm the slice is done.
5. **Write `slices/slice-N/plan.md`.**

## What to Include

### Context Section
A brief reminder of the slice and its anchors. Includes:

- **Slice summary** — capability, stack coverage. Restated from `structure.md` for fresh-context legibility.
- **Design anchors** — the design decisions the tasks rest on (one-line references; don't re-derive).
- **Slice 1 only:** any project-wide bootstrap tasks live here (initial dependency install, lint/format/test config, etc.). Subsequent slices skip this.

### Task List

Numbered, sequential tasks with `[ ]` checkboxes. Each task includes:

- **Title** — what the task accomplishes.
- **Files** — which files to create, modify, or reference.
- **Description** — what to do, in enough detail to execute without ambiguity.
- **Dependencies** — which prior tasks (in this slice's plan) must be complete first.

Tasks must be:

- **Granular** — each completable without further splitting.
- **Ordered** — each builds on previous tasks.
- **Self-contained** — clear start and end states.
- **Actionable** — executable without additional clarification.

### Verification Steps

A concrete list of commands the executing model runs to confirm the slice is done. Examples:

- `pytest slices/slice-1/tests/ -v` — all tests pass.
- `ruff check src/` — no lint errors.
- `mypy src/` — no type errors.

The verification steps are the precondition for marking the slice complete. They must include at least one **end-to-end test** that exercises the slice's user-visible capability.

### What Not to Include

- **Long blocks of code** — describe what to write, don't write it.
- **Implementation minutiae** — specific function bodies, exact variable names.
- **UI mockups** — describe the interface conceptually.
- **Other slices' tasks** — Plan-slice covers one slice only.
- **Worktree / scaffolding setup** — that's the worktree phase's job. (Slice 1 may include project-bootstrap tasks like `npm install` or `pyproject.toml` creation, but not `git worktree add`.)

## Output Artifact

Write `slices/slice-N/plan.md` using the format in [plan-format.md](references/plan-format.md).

## Mode-Aware Transition

Read the `mode:` field at the top of `structure.md`:

- **`mode: autonomous`** (default) — Plan-slice transitions automatically to Implement-slice. No human checkpoint.

  > "Plan for slice N is complete at `slices/slice-N/plan.md`. Moving to Implement."

- **`mode: gated`** — Plan-slice waits for human approval before Implement-slice starts.

  > "Plan for slice N is complete at `slices/slice-N/plan.md`. Review and let me know if it's ready for Implement."

Do not transition in gated mode without explicit approval.

## Triggers for Escalation to the User

Stop the phase and surface to the user when:

- **The slice as defined in `structure.md` is incoherent** (capability not stack-spanning, verification not testable, footprint vague enough that planning is guessing) — likely upstream cause: Structure. Suggested user action: re-run Structure for this slice.
- **The slice depends on a decision not made in `design.md`** (Plan would have to invent an architectural choice to enumerate tasks) — likely upstream cause: Design. Suggested user action: re-run Design with the missing decision named.
- **The slice's verification cannot be defined as a deterministic command** (no clear test-pass criteria) — likely upstream cause: Structure. Suggested user action: re-run Structure to add a concrete verification checkpoint.
