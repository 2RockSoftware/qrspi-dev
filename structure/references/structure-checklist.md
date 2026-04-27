# Structure Checklist

Use this checklist to ensure `structure.md` is complete and slice-shaped before human review.

## Must-Have Sections

- [ ] **`mode:` field** at the top — `autonomous` or `gated`, set after the human checkpoint conversation. Not pre-filled.
- [ ] **Design Anchor** — 1-2 paragraphs summarizing the `design.md` decisions that shape the slicing.
- [ ] **Slice list** — at least slice 1; each slice has Capability, Stack coverage, Verification, Footprint.
- [ ] **Slice 1 PoC justification** — explicit text that names slice 1 as a thin end-to-end PoC of the full stack.
- [ ] **Branch hint** (optional) — suggested branch name for the worktree phase.

## Slice Quality Checks

For each slice:

- [ ] **User-visible capability** — a sentence describing what the user can do after the slice ships. Not "build X module."
- [ ] **End-to-end stack coverage** — touches the layers needed to deliver the capability. For full-stack apps: DB + API + UI (or equivalent).
- [ ] **Verification** — names the test(s) that prove the slice works. End-to-end, not unit-level.
- [ ] **No forward dependencies** — does not require code from later slices to be testable.

For slice 1 specifically:

- [ ] **Smallest representative capability** — the simplest possible thing that still exercises the full stack.
- [ ] **No layer skipped** — if the system has a UI, slice 1 has UI. If it has auth, slice 1 has auth (even if minimal).

## Vertical Slicing Anti-Patterns

If the slice list looks like any of these, it's horizontal — re-decompose.

| Anti-pattern (✗) | Vertical replacement (✓) |
|---|---|
| Slice 1: Database models, Slice 2: API, Slice 3: UI | Slice 1: User registers + lands on empty dashboard (DB+API+UI) |
| Slice 1: Foundation, Slice 2: Core, Slice 3: Polish | Slice 1: One end-to-end capability, Slice 2: Next capability, ... |
| Slice 1: Set up framework, Slice 2: Implement features | Slice 1: Smallest end-to-end capability that proves the framework works |
| Slice 1: All CRUD endpoints | Slice 1: One CRUD operation, end-to-end (just create + read of one entity, with UI) |

If you cannot articulate slice 1 as a vertical slice, escalate to the user — likely cause is Design.

## What to Include vs. Defer

| Include in `structure.md` | Defer to slice's `plan.md` |
|---|---|
| "Slice 1: user can register and log in" | "Task 1: create POST /register handler with email/password validation" |
| "Slice 1 verification: end-to-end test of register → login → dashboard" | "Task 5: write pytest fixture for authenticated test client" |
| Coarse footprint: "touches `auth/`, `api/users/`, `web/components/auth/`" | "Create `auth/models.py` with `User` class having `id`, `email`, `password_hash` fields" |

## Length Guidelines

- **Per slice:** ~5-10 lines. If a slice's description grows past that, the slice is probably too big — split it.
- **Whole document:** scales with project size. A small CLI tool might have 3 slices; a multi-feature app might have 8-12. If you find yourself writing more than ~12 slices, the slicing is probably too fine — coarsen.
- The Plan phase produces the per-task detail. `structure.md` only commits to the slice list.

## Picking the `mode:`

The `mode:` field is the user's call. The phase surfaces both options at the human checkpoint and writes the user's choice into the artifact.

- `autonomous` — fits the user's stated dream: phases run end-to-end without checkpoints unless they escalate. Default suggestion.
- `gated` — fits when the user wants to spot-check planning quality on a new project, an unfamiliar stack, or a high-stakes change. Plan-slice waits for approval on every slice.

The `mode:` value can be edited by the user at any point during the run. The orchestrator re-reads it before each slice.
