---
name: qrspi-dev-implement
description: Build one slice by following its plan. Write tests, run them until they pass, then mark the slice complete. Use in the Implement phase of QRSPI development.
model_tier: local
inputs: slices/slice-N/plan.md, code under the slice's footprint
outputs: code, updated checkboxes in slices/slice-N/plan.md
forbidden_inputs: other slices' plans, design.md
---

# Implement Phase

Build one slice by following its plan. Implement runs once per slice. Each slice ships with passing tests; running them is part of the work, not a final check.

## Phase Contract

- **Reads:** `slices/slice-N/plan.md` (the current slice's plan), and the code under that slice's footprint (the directories named in the plan's Context).
- **Writes:** code in the project tree; updates checkboxes in the slice's `plan.md`.
- **Does not read:** other slices' plans, `design.md`, `questions.md`, `research.md`. Plan should already encode every design decision the slice needs. If Implement finds itself wanting to read `design.md`, that's a Plan-quality issue — escalate.
- **Tier:** `local`. Once a slice is well-scoped by its plan, implementation is mechanical task execution. The smaller model writes code; a larger model can spot-check at Review.

## How It Works

1. **Read `slices/slice-N/plan.md`** — the primary guide. Understand the context, tasks, and verification steps.
2. **Find the first unchecked task** (`[ ]`). If none remain, jump to Verification (below).
3. **Execute the task:**
   - Read the task description, files, dependencies.
   - Read existing files in the slice's footprint for context.
   - Write or modify code.
   - Mark the task `[x]` in `plan.md`.
4. **Repeat** until all tasks are `[x]`.
5. **Run Verification** (below).
6. **Transition to Review-slice** (light correctness pass).

## Verification — The Hard Gate

A slice is **not complete** until all of the following hold:

- All tasks marked `[x]`.
- All tests for the slice are written.
- Verification commands from `plan.md` all pass cleanly.

A **verification attempt** is the run after you have marked all tasks complete and written all tests, and you expect the verification suite to pass. Test runs *during* development — while iterating on a task, debugging a function, or watching a unit test fail and fixing it — do **not** count as verification attempts. Those are normal work.

The verification-attempt budget is **3 attempts**. After 3 failed verification attempts, escalate to the user (see Triggers for Escalation below). Don't keep grinding.

```
Verification attempt:  marked all [x] + all tests written + run verification suite
                       ↓
                  pass?  → slice complete, transition to Review-slice
                  fail?  → diagnose, fix, run again (this is attempt 2 of 3)
```

This contrasts with the soft "ensure tests pass if applicable" framing of earlier QRSPI iterations. Tests-must-pass is a hard precondition for "slice done." Writing tests is part of Implement; running them until they pass is part of Implement.

## Coding Standards

Apply [coding-standards.md](references/coding-standards.md). Highlights:

- **Clean, readable code** — meaningful names, logical structure, no clever tricks.
- **Follow language/ecosystem conventions** — idiomatic code for the stack.
- **Handle errors appropriately** — don't swallow them, don't let unhandled errors crash unexpectedly.
- **Each slice ships with tests** that exercise the capability it delivers. The plan's verification step names them. Writing tests is part of Implement; running them until they pass is part of Implement.
- **Don't over-engineer** — follow the plan, not your imagination.
- **Comments for counter-intuitive code** — if a solution isn't obvious, comment the why.

## Task Execution Rules

- **One task at a time** — complete it fully before starting the next.
- **If a task needs a new file the plan didn't mention** — create it and note in the task output.
- **If a task is blocked or unclear** — escalate (see Triggers below).
- **If you discover the plan is wrong or incomplete** — note it in the task output and continue with best effort. Don't modify upstream artifacts.

## Frozen-Artifacts Rule

The executing model **does not edit upstream artifacts** (`questions.md`, `research.md`, `design.md`, `structure.md`, the slice's `plan.md` other than checkboxes). Those capture decisions the user already approved.

This rule binds the phase, not the user. The user may re-invoke any upstream phase manually; downstream artifacts then become stale and the user decides what to keep. Implement should never silently mutate those files.

## Picking Up Mid-Slice

If you start Implement and some tasks in `slices/slice-N/plan.md` are already `[x]`:

1. Read the plan to see what's been completed.
2. Read the implemented files to understand current state.
3. Continue from the first unchecked task.
4. If you find that completed work doesn't match the task descriptions, note it but don't redo completed work — flag it in Review.

## Auto-Transition

After verification passes, transition to Review-slice (the light correctness pass for this slice — not the heavy final Review):

> "Slice <N> implementation complete. Verification passed. Moving to Review-slice."

## Triggers for Escalation to the User

Stop the phase and surface to the user when:

- **Verification fails 3 times after writing all tests** — likely upstream cause: Plan-slice (the plan missed something or wrote an unverifiable test). Suggested user action: re-run Plan-slice with the failure diagnosis attached, or edit the plan directly.
- **A plan task references an API/file/method that doesn't exist** — likely upstream cause: Plan-slice. Suggested user action: re-run Plan-slice; the planner's mental model was wrong.
- **An architectural conflict surfaces** (the slice cannot be built without contradicting `design.md`) — likely upstream cause: Design. Suggested user action: re-run Design with the conflict surfaced; Structure and Plan downstream may also need re-runs.
- **Required tooling is missing** (interpreter, package manager, system dependency) — likely upstream cause: external. Suggested user action: install tooling, then re-run Implement.
- **Tests cannot be written for the slice's capability** (no obvious assertion target, capability is purely visual or external-state-dependent) — likely upstream cause: Structure (verification was under-specified). Suggested user action: re-run Structure with a concrete verification checkpoint, then Plan-slice.
