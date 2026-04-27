---
name: qrspi-dev-review
description: Review completed work in two modes — light per-slice correctness pass and heavy final thoroughness pass with optional PR description. Use in the Review phase of QRSPI development.
model_tier: frontier-mid
inputs: per-slice mode = slices/slice-N/plan.md + slice diff; final mode = all artifacts + full diff
outputs: per-slice mode = slices/slice-N/review.md; final mode = review.md, optional pr.md
forbidden_inputs: none
---

# Review Phase

Review runs in two distinct modes:

- **Per-slice review** (light, correctness only) — runs after each Implement-slice. Catches bugs and plan mismatches against one slice's diff. Tier: `frontier-mid`.
- **Final review** (heavy, correctness + thoroughness, optional PR description) — runs once after all slices are complete. Tier: `frontier-top` for the thoroughness pass.

The point of Review is to look back at the work and catch things humans shouldn't have to. PR creation is a side product of doing the final review well, not the reason for the phase.

## Phase Contract

### Per-slice mode

- **Reads:** `slices/slice-N/plan.md` and the slice's diff (the code added/modified during Implement-slice).
- **Writes:** `slices/slice-N/review.md`.
- **Tier:** `frontier-mid`.

### Final mode

- **Reads:** all upstream artifacts (`questions.md`, `research.md`, `design.md`, `structure.md`, every `slices/slice-N/plan.md` and `review.md`), the full project diff, and the working code.
- **Writes:** `review.md` at the project root. Optionally `pr.md`.
- **Tier:** `frontier-top` — the thoroughness pass benefits from frontier reasoning.

The two modes use the same checklist tiers from [review-checklist.md](references/review-checklist.md): per-slice runs **Correctness Tier** only; final runs **Correctness + Thoroughness Tiers**.

## How It Works

### Per-slice (light) mode

1. **Read `slices/slice-N/plan.md`** — what was supposed to be built for this slice.
2. **Read the slice's diff** — what was actually built.
3. **Run the Correctness Tier checklist** ([review-checklist.md](references/review-checklist.md#correctness-tier)).
4. **Auto-fix** minor issues (typos, missing imports, trivial bugs).
5. **Write `slices/slice-N/review.md`** with the assessment, auto-fixes applied, and any issues found.
6. **Transition** based on assessment (see Transition below).

### Final (heavy) mode

1. **Read all upstream artifacts** — `questions.md`, `research.md`, `design.md`, `structure.md`, every slice's `plan.md` and `review.md`.
2. **Read the full project state** — the working code, tests, configuration.
3. **Run the Correctness Tier checklist** for any cross-slice integration concerns.
4. **Run the Thoroughness Tier checklist** — test coverage gaps, type design, simplification opportunities, performance considerations, dead code, documentation.
5. **Auto-fix** minor issues.
6. **Write `review.md`** with the full assessment.
7. **PR mode (optional):**
   - Run `git remote -v`. If a remote exists, ask the user: `Generate PR description? [Y/n]`.
   - If yes: produce `pr.md` (description, test plan, back-links to artifacts).
   - This phase **does not** run `gh pr create`. The user decides what to do with `pr.md`.

## Auto-Fix vs. Defer vs. Escalate

### Auto-Fix (Minor Issues)

Fix immediately, in place. Note in the review under "Minor Fixes Applied":

- Missing imports, typos in strings/comments.
- Trivial bugs (off-by-one, wrong comparison operator).
- Small lint/format issues.
- Missing comment for non-obvious code.

### Defer to review.md (Major Issues)

Write to the appropriate review file with assessment "Yellow" or "Red." Do **not** auto-fix:

- Logic changes that affect behavior.
- Missing features or incomplete implementations.
- Architecture mismatches with `design.md`.
- Missing tests for significant logic.
- Security vulnerabilities.

The user reads the review and decides next steps. **Review does not auto-iterate with Implement.** No automatic re-runs. (See [revision-rules.md](references/revision-rules.md).)

### Escalate (Systemic / Cross-Cutting Problems)

Stop and surface to the user when the issue is bigger than "Implement should fix this." See Triggers for Escalation below.

## Output Artifact

### `slices/slice-N/review.md` (per-slice)

```markdown
# Review: Slice <N>
**Assessment:** <Green | Yellow | Red>

## Minor Fixes Applied
- ...

## Issues Found
### [<Severity>] <title>
- **File:** `src/...`
- **What:** ...
- **Recommended Fix:** ...
```

Per-slice reviews stay tight — Correctness Tier only.

### `review.md` (final)

```markdown
# Review: <project name>
**Date:** <date>
**Assessment:** <Green | Yellow | Red>

## Summary
<Brief summary of overall findings>

## Minor Fixes Applied
- ...

## Correctness Findings
### [<Severity>] <title>
- ...

## Thoroughness Findings
### [<Severity>] <title>
- ...

## Cross-Slice Integration Notes
<Any issues that emerged from looking at slices in combination, not individually>
```

### `pr.md` (optional, only when user opts in)

```markdown
# <PR title>

## Summary
<1-3 bullet points: what changed and why>

## Test Plan
- [ ] <verification command from each slice>
- [ ] ...

## Background
- Questions: [questions.md](questions.md)
- Research: [research.md](research.md)
- Design: [design.md](design.md)
- Structure: [structure.md](structure.md)
- Slices:
  - [Slice 1: ...](slices/slice-1/plan.md)
  - [Slice 2: ...](slices/slice-2/plan.md)

## Notes for Reviewer
<Anything from the final review that's worth a reviewer's attention>
```

## Severity

Three levels — same in both modes:

- **High** — correctness, security, missing core functionality, broken integration.
- **Medium** — missing tests, edge cases, error-handling gaps.
- **Low** — style, minor improvements, documentation gaps.

## Transition

### Per-slice mode

- **Green** — auto-transition to next slice's Plan (autonomous mode) or to next slice's gate (gated mode).
- **Yellow** (minor issues found, all auto-fixed) — same auto-transition; the issues are documented but did not block.
- **Red** (high-severity issues remain unfixed) — escalate. Don't proceed to next slice.

### Final mode

- **Green** — final review complete. Surface to user (with optional `pr.md`).
- **Yellow** — review complete with notable findings. Surface to user.
- **Red** — surface to user with recommended action (likely re-run an upstream phase).

## Picking Up After a Previous Review

If a review file already exists from a prior run:

1. Read the previous review to see what was found.
2. Read the current state of the code to see if previous issues were addressed.
3. Update the review file with the current state — remove items that have been fixed, keep items that haven't, add anything new.
4. Note in the review what was checked and what changed since last time.

## Triggers for Escalation to the User

Stop the phase and surface to the user when:

- **Per-slice review finds high-severity issues that aren't auto-fixable** — likely upstream cause: Plan-slice or Implement. Suggested user action: re-run Implement-slice with the findings, or re-run Plan-slice if the plan was wrong.
- **Final review finds systemic problems** (the architecture as built doesn't match `design.md` in a fundamental way; security model is broken across slices; multiple slices duplicate logic that should have been factored) — likely upstream cause: Design or Structure. Suggested user action: re-run the affected upstream phase.
- **Cross-slice integration is broken** even though individual slice reviews were Green — likely upstream cause: Structure (slices were over-coupled or under-coupled). Suggested user action: re-run Structure with the integration findings.
- **The user opts into PR mode but `git remote` fails or returns ambiguous output** — likely upstream cause: external. Suggested user action: clarify the remote setup, or skip PR mode.
