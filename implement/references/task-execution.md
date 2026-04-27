# Task Execution Reference

How to execute tasks from a slice's `plan.md` effectively.

## Execution Flow

```
1. Read slices/slice-N/plan.md → find first [ ] task
2. Read task description, files, dependencies
3. Read existing files (for context)
4. Implement the code
5. Mark task [x] in plan.md
6. Repeat steps 1-5 until all tasks are [x] AND all tests are written
7. Run Verification suite (the hard gate)
8. Pass? → transition to Review-slice
   Fail? → diagnose, fix, re-run (counts as a verification attempt)
9. After 3 failed verification attempts, escalate
```

## Checkbox Management

- **Always use the exact checkbox format**: `[ ]` and `[x]`.
- **Update in `slices/slice-N/plan.md`** — the source of truth for slice progress.
- **One task at a time** — mark complete before moving on.
- **If you can't complete a task** — leave it `[ ]` and explain why.

## Verification Attempts vs. Development Test Runs

These are different. The distinction matters because the budget (3) only applies to one of them.

### Development test runs — do not count

Tests you run during normal work:

- Running a unit test you just wrote to confirm it fails as expected (red phase of TDD).
- Running tests after fixing a bug in a single function.
- Running an end-to-end test that you know is incomplete to see how far it gets.
- Running tests after every save while iterating.

These are normal. Run them as much as you want.

### Verification attempts — count toward the budget

A run where you have:

1. Marked all tasks `[x]`.
2. Written all tests called out in the plan's Verification Steps.
3. Expect the suite to pass cleanly.

If a verification attempt fails:

- Diagnose: what failed, why.
- Fix: address the root cause, not the symptom.
- Run again — that's verification attempt 2.

After 3 failed verification attempts, stop. Don't grind. Escalate to the user with:

- Which verification command(s) failed.
- The failure output (relevant excerpt).
- Your best guess at the upstream cause (Plan-slice, Structure, Design).

## Handling Unexpected Files

### Task Requires a New File Not in Plan
- Create the file as needed.
- Note it in the task output: "Created `src/extra_util.py` — not in original plan, needed for task completion."
- Do not modify the plan.

### Existing File Has Different Structure Than Expected
- Adapt your implementation to the existing structure when reasonable.
- Note significant deviations in the task output.
- If the deviation suggests an architectural conflict, escalate (see Implement SKILL.md Triggers).

## Handling Blocked Tasks

### Task Is Blocked By Missing Information
- Identify what information is missing.
- Try to infer from context if reasonable.
- If you cannot, escalate — naming what's missing and which upstream phase is the likely cause.

### Task Depends On An Incomplete Task
- Verify the dependency was actually completed (read the code, not just the checkbox).
- If the checkbox was marked but the code is missing, this is a previous-execution issue — flag it in your task output.
- Try to work around the issue if possible; otherwise escalate.

## When to Stop and Ask vs. Proceed

The phase's [Triggers for Escalation to the User](../SKILL.md#triggers-for-escalation-to-the-user) is the authoritative list. Quick reference:

### Stop and Escalate
- Verification fails 3 times after all tests written.
- A required API/file/method doesn't exist.
- Architectural conflict with `design.md`.
- Required tooling missing.
- The slice's capability is not testable as written.

### Proceed with Best Effort
- Minor implementation details aren't specified (e.g., exact variable names).
- You can reasonably infer intent from context.
- The task has multiple valid implementations and you choose one.
- You spot a small issue in the plan but can work around it.

## Logging Progress

When you complete a task, briefly note:

- What was done.
- Files created or significantly modified.
- Decisions made that aren't in the plan.
- Any concerns to flag for Review.

Example:

> "Task 3 complete: Implemented `POST /register` handler with email/password validation and bcrypt hashing. Created `src/api/auth.py`, modified `src/api/__init__.py`. Note: used pydantic for request validation since `src/api/` already had pydantic models elsewhere — design.md didn't mandate this."

When all tasks are complete and you're about to run Verification, state it explicitly so it's clear which run is the verification attempt:

> "All tasks marked [x], all tests written. Running verification (attempt 1 of 3)."
