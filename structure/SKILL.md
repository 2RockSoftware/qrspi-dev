---
name: qrspi-pi-structure
description: Decompose the design into vertical, end-to-end-testable slices. First slice is always a thin proof-of-concept of the full stack. Use in the Structure phase of QRSPI development. Produces structure.md.
model_tier: frontier-top
inputs: design.md, questions.md, research.md
outputs: structure.md
forbidden_inputs: none
---

# Structure Phase

Decompose the Design into a sequence of **vertical slices** — *how we get there.* Each slice is end-to-end testable on its own. Structure is the highest-leverage doc to review: catching a bad decomposition here saves regenerating thousands of lines downstream.

## Phase Contract

- **Reads:** `design.md` (primary), `questions.md` and `research.md` (for context as needed).
- **Writes:** `structure.md` at the project root.
- **Does not read:** there are no forbidden inputs at this phase.
- **Tier:** `frontier-top`. Vertical decomposition is harder than it looks; horizontal layering is the path of least resistance and must be actively resisted. Frontier reasoning matters here.

## Vertical Slicing — The Rule

Every slice delivers a **user-visible, end-to-end-testable capability** that spans the full relevant stack (DB + API + UI, or the equivalent layers for the project). Horizontal slicing — "all DB, then all API, then all UI" — is **forbidden**.

### Why

A horizontal slice ships nothing testable until the last layer lands. Bugs surface at integration time, when fixes are most expensive. A vertical slice is testable on day one and proves the integration works as you go.

### Slice 1 is always a thin end-to-end PoC

The first slice's job is to prove the full stack works. It picks the simplest possible user-visible capability and implements it across every layer — minimal in scope, complete in stack coverage. If the stack has a flaw, slice 1 finds it before slices 2-N build on top.

### Anti-pattern examples (do not do these)

```
✗ Slice 1: All database models
✗ Slice 2: All API endpoints
✗ Slice 3: All UI screens
✗ Slice 4: Wire it all together
```

```
✗ Slice 1: Foundation / scaffolding
✗ Slice 2: Core features
✗ Slice 3: Extended features
✗ Slice 4: Polish
```

These look like decompositions; they are layer enumerations. None of them ships a testable user capability until the final integration step.

### Vertical examples (do these)

```
✓ Slice 1: User can register, log in, and see an empty dashboard.
           DB (users table) + API (POST /register, POST /login, GET /dashboard)
           + UI (register form, login form, empty dashboard screen).

✓ Slice 2: User can create a reading-list entry and see it on the dashboard.
           DB (entries table) + API (POST /entries, GET /entries) + UI (form, list).

✓ Slice 3: User can mark an entry as read and filter by status.
           ...
```

Each slice ends with a user able to *do something*, with tests that exercise the capability end-to-end.

## How It Works

1. **Read `design.md`** — internalize the architecture, components, and tech choices.
2. **Identify the smallest user-visible capability that exercises the full stack.** That's slice 1.
3. **Sequence the rest.** Each subsequent slice adds one user-visible capability and may build on prior slices' code, but must be testable end-to-end on its own.
4. **For each slice, name:**
   - **Capability** — what the user can do after this slice ships.
   - **Stack coverage** — which layers/components are touched.
   - **Verification checkpoint** — what test(s) prove the slice works end-to-end.
   - **Coarse file footprint** — directories or component areas expected to be touched. Not file-level.
5. **Discuss the `mode:` setting with the user.** This decision is human-owned (see Human Checkpoint below).
6. **Write `structure.md`.**

## Output Artifact

`structure.md` contains:

```markdown
# Structure: <project name>

mode: <autonomous | gated>

## Design Anchor

<1-2 paragraphs summarizing the design.md decisions that shape the slicing. Brief — design.md is the source of truth.>

## Slice 1 — <name>

**Capability:** <what the user can do after this slice ships>
**Stack coverage:** <layers/components touched>
**Verification:** <end-to-end tests that confirm the slice works>
**Footprint:** <coarse directories / component areas>

## Slice 2 — <name>

...

## Branch Hint  (optional)

<Suggested branch name for the worktree phase. Defaults to a sensible project-level name if omitted.>
```

The `mode:` field at the top controls per-slice gating:

- `autonomous` — Plan / Implement / Review run per-slice without human checkpoints. The default. Phases stop only on escalation triggers.
- `gated` — Plan-slice waits for human approval before Implement-slice starts, on every slice. Useful for spot-checking the first few slices on a new project.

## Human Checkpoint

After writing `structure.md`:

> "Structure phase is complete. I've written `structure.md` with the slice list and verification checkpoints. Two things to confirm:
>
> 1. **Slicing.** Are the slices the right shape? Does slice 1 prove the full stack? Anything missing?
> 2. **Mode.** Should I run subsequent phases in `autonomous` mode (default — Plan / Implement / Review per slice without checkpoints, escalations only) or `gated` mode (Plan waits for your approval on every slice)?"

The `mode:` field is **human-decided**, not model-decided. Do not pre-fill it without surfacing the choice. Update `structure.md`'s `mode:` line based on the user's answer.

Do not proceed to Plan-slice-1 until the human approves the structure and confirms the mode.

## Triggers for Escalation to the User

Stop the phase and surface to the user when:

- **`design.md` does not decompose into vertical slices** (e.g., the architecture is essentially a single monolith with no user-visible capabilities, or capabilities can't be ordered without forward dependencies) — likely upstream cause: Design. Suggested user action: re-run Design with the slicing constraint visible.
- **No slice-1 candidate is a representative PoC of the full stack** (e.g., the smallest user capability still requires building most of the system) — likely upstream cause: Design (the architecture is too tightly coupled to admit thin slices). Suggested user action: re-run Design with thin-slice-feasibility as a constraint.
- **Slices have unavoidable circular dependencies** — likely upstream cause: Design. Suggested user action: re-run Design to break the cycle, or accept a coarser first slice.
