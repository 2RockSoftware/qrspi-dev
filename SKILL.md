---
name: qrspi-dev
description: Orchestrates phased agentic software development (Questions → Research → Design → Structure → Plan → Worktree → Implement → Review). Use as the entry point when starting a new project. Pronounced "crispy".
model_tier: frontier-mid
inputs: user prompt, project artifact state
outputs: phase routing decision
forbidden_inputs: none
---

# QRSPI-Dev Orchestrator

This is the QRSPI orchestrator. The phases are **Questions → Research → Design → Structure → Plan → Worktree → Implement → Review**. Pronounced "crispy."

## Phase Contract

- **Reads:** the user's prompt and the project's artifact state (which files exist in the working directory and what `mode:` is set in `structure.md`).
- **Writes:** routing decisions (which phase skill to load next). Does not write artifacts directly.
- **Does not read:** the contents of phase artifacts in detail. The orchestrator picks the next phase; the phase skill reads its own inputs.
- **Tier:** `frontier-mid`. Routing logic is bounded but the model needs to handle escalation messages and resume from arbitrary states.

## The Flow

```
[gated]   Questions → Research → Design → Structure
[mode]    └──────────── per-slice loop ────────────┐
[loop]    │  Plan → Worktree* → Implement → Review │
          │  (* worktree runs once at slice 1)     │
          └────────────────────────────────────────┘
[final]   Review (heavy, optional pr.md)
```

The first four phases are human-gated. The per-slice loop runs **autonomously** by default, with opt-in gating via `mode: gated` in `structure.md`.

## How to Use

1. **Provide a project description.**
2. **The orchestrator detects where you are** by checking which artifacts exist.
3. **It loads the appropriate phase skill** and runs it.
4. **For human-gated phases (Q, R, D, S):** stop after each and wait for approval before moving to the next.
5. **For the per-slice loop:** behavior depends on `mode:` in `structure.md`:
   - `mode: autonomous` (default) — Plan-slice → Implement-slice → Review-slice runs without checkpoints, advancing to the next slice on Green/Yellow assessments.
   - `mode: gated` — Plan-slice waits for human approval before Implement-slice on every slice.
6. **After all slices reviewed Green/Yellow:** optional final Review (heavy, with optional `pr.md` if a git remote is detected).
7. **If anything escalates:** the phase stops, names the most likely upstream cause, and waits for the user.

## Phase Detection

Check the working directory and pick the next action by walking this sequence:

1. **No `questions.md`?** → run **Questions** (gated).
2. **`questions.md` exists, no `research.md`?** → run **Research** (gated).
3. **`research.md` exists, no `design.md`?** → run **Design** (gated).
4. **`design.md` exists, no `structure.md`?** → run **Structure** (gated).
5. **`structure.md` exists?** Read it. For each slice in order:
   - **No `slices/slice-N/plan.md`?** → run **Plan-slice-N**. (Run **Worktree** first if it's slice 1 and worktree state isn't set up.)
   - **`plan.md` exists, slice tasks not all `[x]` or verification not run?** → run **Implement-slice-N**.
   - **Slice complete, no `slices/slice-N/review.md`?** → run **Review** (per-slice, light).
   - **Per-slice review is Red?** → escalate.
   - **Per-slice review is Green/Yellow?** → continue to next slice.
6. **All slices reviewed Green/Yellow, no final `review.md`?** → run **Review** (final, heavy). Ask user about PR mode if a git remote exists.
7. **Final `review.md` exists?** → flow complete.

## Artifact Reference

| Artifact | Produced by | Read by |
|---|---|---|
| `questions.md` | Questions | Research |
| `research.md` | Research | Design |
| `design.md` | Design | Structure, Plan |
| `structure.md` | Structure | Plan, Worktree, Implement, Review (final) |
| `slices/slice-N/plan.md` | Plan | Implement, Review (per-slice) |
| `slices/slice-N/review.md` | Review (per-slice) | user; final Review aggregates |
| `review.md` | Review (final) | user |
| `pr.md` | Review (final, optional) | user |

## Phase Skills

The orchestrator loads each phase's `SKILL.md` when entering that phase:

- `questions/SKILL.md` — `frontier-top`
- `research/SKILL.md` — `frontier-mid`
- `design/SKILL.md` — `frontier-top`
- `structure/SKILL.md` — `frontier-top`
- `plan/SKILL.md` — `local`
- `worktree/SKILL.md` — `local`
- `implement/SKILL.md` — `local`
- `review/SKILL.md` — `frontier-mid` (per-slice) / `frontier-top` (final)

A harness that supports per-phase model routing should consult each phase's `model_tier` frontmatter; otherwise the prose tier note in each Phase Contract section guides the user / executing model.

## Mode

The `mode:` field at the top of `structure.md` controls the per-slice loop:

- `autonomous` (default) — Plan / Implement / Review-per-slice run without human checkpoints. Phases stop only on escalation.
- `gated` — Plan-slice waits for human approval on every slice before Implement starts.

The `mode:` field is **human-decided**. Structure phase asks the user explicitly at its checkpoint; the orchestrator reads `mode:` before each slice's Plan to decide whether to gate.

## Escalation

When a phase escalates, it stops and surfaces:

- What it was trying to do.
- What went wrong (with evidence).
- The most likely upstream cause (named).
- A suggested user action.

Review does **not** auto-iterate with Implement. The user reads the relevant `review.md` and decides whether to re-run Implement, edit a plan, or loop further upstream. There is no automatic loop-back machinery.

Each phase's `## Triggers for Escalation to the User` section lists its own escalation triggers and likely causes.

## Resuming an Interrupted Flow

Re-running the orchestrator with no arguments picks up where the last run stopped — the phase detection sequence above is idempotent. To re-run a specific phase deliberately (e.g., after the user wants to revise Design after seeing slice 3 surface a problem), the user invokes that phase's skill directly; the orchestrator's auto-detection will then see stale downstream artifacts and the user decides what to keep vs. regenerate.

---

**Tell me what you want to build, and we'll get started.**
