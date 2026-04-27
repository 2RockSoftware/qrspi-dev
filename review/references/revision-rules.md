# Revision Rules

How Review handles findings. The trichotomy is auto-fix / defer / escalate. Review does **not** auto-iterate with Implement.

## Decision Tree

```
Review finds issue
        │
        ▼
  Is the issue minor?
        │
   ┌────┴────┐
   Yes       No
   │          │
   ▼          ▼
 Auto-fix   Is it scoped to this slice / fixable
 in place   by re-running Implement on this slice?
            │
        ┌───┴───┐
        Yes    No (cross-cutting / systemic)
        │      │
        ▼      ▼
   Defer     Escalate
   to        to user
   review    with named
   .md       upstream cause
```

There is no "automatically loop back to Implement" path. Whether the user wants to re-run Implement on a slice with the deferred findings as input is the user's call after reading the review.

## Minor → Auto-Fix

Fix immediately without writing to the issues list. Examples:

- Missing import.
- Typo in string or comment.
- Wrong comparison operator (`=` vs. `==`).
- Formatting / linting issues.
- Missing closing bracket or quote.
- Small error-handling addition (e.g., adding `except FileNotFoundError` where it was clearly intended).
- Wrong default parameter value.

After auto-fixing, list the change under **Minor Fixes Applied** in the review for transparency.

## Major → Defer to review.md

Write to the appropriate review file (`slices/slice-N/review.md` for per-slice mode, `review.md` for final mode) with severity. Do **not** auto-fix.

Major issues require decisions beyond simple corrections:

- Logic changes that affect behavior.
- Missing features.
- Architecture mismatches.
- Security fixes that need design changes.
- Test additions that need new test structure.

Each deferred issue has:

- **Title** (with severity).
- **File** and location (function name, line range).
- **What:** description of the problem.
- **Recommended Fix:** what should be done — but don't implement it.

The user reads the review and decides:

- Re-run Implement-slice with the review as context (user's call, not auto).
- Edit the slice's `plan.md` to add tasks for the fixes (manual).
- Re-run an upstream phase if the issue points there.
- Accept the issue (e.g., for a low-severity nice-to-have).

## Systemic / Cross-Cutting → Escalate

Stop and surface to the user when the issue is bigger than "Implement should fix this on this slice."

Systemic indicators:

- Multiple slices show the same problem — points at upstream (Plan template, Design, Structure).
- Architecture as built doesn't match `design.md` in a fundamental way.
- Cross-slice integration is broken even though individual slices reviewed Green.
- The codebase needs significant rework, not patches.

When escalating:

1. Summarize the issue clearly.
2. Show the evidence (file:line references, multiple instances).
3. Name the most likely upstream cause (Design, Structure, Plan, Implement).
4. Suggest a user action (re-run X, or accept and move on, or take a different path).

Example:

> "Final review found a systemic issue: the auth implementation across slices 1, 3, and 5 silently bypasses the authorization checks `design.md` specified for admin endpoints. This is consistent across slices, which suggests the issue is upstream — Plan-slice templates didn't surface auth checks, or Design under-specified them. Recommend re-running Design with the auth-enforcement requirement explicit, then regenerating affected slices' Plans. Alternatively, manually add the auth checks if the architectural intent matches what's in design.md."

## What Was Removed from a Previous Version

A previous version of this skill had Review automatically hand findings to Implement and iterate up to 3 times. That auto-loop is gone. Review writes findings; the user decides next steps. This keeps Review's role clear (find issues; don't fix everything) and avoids the kind of runaway agent loops where the same issue keeps surfacing across iterations.

## Writing Review Entries

Good deferred-issue entry:

```markdown
#### [High] User registration silently accepts empty email

- **File:** `src/api/auth.py`
- **Location:** `register()` at line 42
- **What:** Empty string passes the email validator. Should be rejected with a 400.
- **Recommended Fix:** Add `if not email.strip(): raise ValidationError("email required")` in `validate_email()`. Add a regression test in `tests/api/test_auth.py`.
```

Bad deferred-issue entry:

```markdown
#### [High] Validation issue
- **File:** somewhere in auth
- **What:** doesn't work right
- **Recommended Fix:** fix it
```

## Picking Up After a Previous Review

If a review file already exists from a prior run:

1. Read the previous review.
2. Check whether previously-deferred items have been addressed in the code.
3. Update the review file:
   - Remove items that are now fixed.
   - Keep items still present.
   - Add anything new that surfaced.
4. Note in the review what was checked and what changed since last time.

The review file is a living artifact during iteration; the user reads it to decide whether the project is ready or needs more work.
