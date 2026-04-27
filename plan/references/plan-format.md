# Plan Format

Specification for writing per-slice `plan.md` documents (one per slice, at `slices/slice-N/plan.md`).

## Overall Structure

```markdown
# Plan: Slice <N> — <slice name from structure.md>

## Context

**Capability:** <what the user can do after this slice ships, from structure.md>
**Stack coverage:** <layers/components, from structure.md>
**Footprint:** <coarse directories, from structure.md>

**Design anchors:**
- [<short reference>](../../design.md#<section>) — <one-line summary>
- ...

## Tasks

- [ ] **1. Task Title** — One-line summary
  - **Files:** `src/file1.py`, `src/file2.py`
  - **Description:** What to do, in enough detail for execution without ambiguity.
  - **Dependencies:** (or "none" for the first task)

- [ ] **2. Task Title** — ...

- [ ] **N. Run verification** — execute the verification steps below.

## Verification Steps

The slice is complete when all of the following pass:

```bash
pytest slices/slice-<N>/tests/ -v
ruff check src/
mypy src/
```

These commands run after all other tasks are marked `[x]` and all tests are written. See implement/SKILL.md for the verification-attempt budget.

End-to-end test for this slice:
- `tests/e2e/test_slice_<N>.py::test_<capability>` — exercises <user capability described in structure.md>.
```

## Task Naming Conventions

- Use **flat numbering** within a slice: 1, 2, 3, … No phases or sub-phases (one slice = one task list).
- Use **descriptive titles**: "Implement user registration handler" not "Task 1."
- Use **action verbs**: Create, Implement, Configure, Wire up, Write, Add.

## Writing Task Descriptions

Good task descriptions include:

### What
- What should be created or modified.
- The expected outcome of the task.
- Any specific behaviors the code should have.

### Where
- Which files to create or modify.
- Which existing files to read for context.

### How (high-level)
- Patterns or conventions to follow (referencing design.md).
- Any specific algorithms or approaches to use.

### Dependencies
- Which prior tasks (in this slice) must be complete first.

## Example Task

```markdown
- [ ] **3. Implement registration handler**
  - **Files:** `src/api/auth.py`, `src/api/__init__.py`
  - **Description:** Implement `POST /register` endpoint. Accepts `{email, password}` JSON body. Validates email format and password length (≥ 8 chars). Hashes password with bcrypt. Inserts a `User` row. Returns `201` with the user's id, or `400` with a validation error. Wire into the app's URL routing in `src/api/__init__.py`.
  - **Dependencies:** Task 2 (User model exists), Task 1 (bcrypt installed).
```

## Verification Steps Subsection

Every plan ends with explicit, runnable verification commands. The Implement phase runs these to determine when the slice is done.

Required:

- At least one **end-to-end test** that exercises the slice's user-visible capability (the one named in `structure.md`).
- Lint and typecheck commands appropriate to the language.

Optional:

- Unit tests for individual components touched by this slice.
- Integration tests that span multiple components.

The verification commands are part of the slice's commitment. If verification can't be expressed as a runnable command, the slice was under-specified — escalate to Structure.

## Task Granularity

**Too granular:**
- [ ] Create file `src/utils.py`
- [ ] Add import for `requests` in `src/utils.py`
- [ ] Define function `fetch_data()` in `src/utils.py`

**Just right:**
- [ ] Create utility module with `fetch_data()` function that handles HTTP requests with retry logic.

**Too broad:**
- [ ] Build the entire API layer.

## Checkbox Management

- All tasks start with `[ ]` (empty checkbox).
- Implement marks them `[x]` as work progresses, in `slices/slice-N/plan.md`.
- Do not pre-mark any tasks as complete.
- A slice is complete when all tasks are `[x]` **and** the verification steps pass cleanly.

## What Was Removed from a Previous Version

A previous version of this format included a "Phase Grouping" section with examples like *"Data layer → API layer → Integration → Polish"* and *"Foundation → Features → Integration → Testing."* Those are horizontal-layer phasings — the exact anti-pattern Structure prevents. They are intentionally absent now. One slice = one flat task list = no internal phases.
