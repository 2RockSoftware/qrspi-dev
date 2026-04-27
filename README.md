# qrspi-dev

A phased agentic development framework for [pi](https://pi.dev). Pronounced **"crispy"**.

## The Phases

QRSPI stands for **Questions → Research → Design → Structure → Plan → (Worktree) → Implement → Review**.

| Phase | What it does | Output |
|-------|-------------|--------|
| **Questions** | Surface decisions as options through dialogue, force negative-space coverage | `questions.md` |
| **Research** | Gather facts (no recommendations) from docs, code, and the web | `research.md` |
| **Design** | Compare 2-3 approaches with tradeoffs; converge on architecture | `design.md` |
| **Structure** | Decompose into vertical, end-to-end-testable slices; slice 1 is a thin PoC | `structure.md` |
| **Plan** *(per slice)* | Mechanical task list for one slice + verification commands | `slices/slice-N/plan.md` |
| **Implement** *(per slice)* | Build the slice; tests written and passing is a hard gate | code + checked tasks |
| **Review** *(per slice, then final)* | Light correctness pass per slice; heavy thoroughness pass at the end | `slices/slice-N/review.md`, `review.md`, optional `pr.md` |

Worktree is a small operational helper that runs once at slice 1 to set up `git worktree`. It's not a reasoning phase.

## Installation

```bash
pi install git:github.com/2RockSoftware/qrspi-dev
```

Or clone the repo and place the `qrspi-dev/` directory in `~/.pi/agent/skills/`.

## Usage

Start with the orchestrator. It detects which phase you're at and guides you through the flow:

```
/skill:qrspi-dev "I want to build a CLI tool that manages my reading list"
```

The orchestrator checks for existing artifacts and starts at the right phase. It will:

- **Stop and wait for approval** after each of the four design phases (Questions, Research, Design, Structure).
- **Run autonomously through the per-slice loop** (Plan → Implement → Review) by default — no checkpoints unless something escalates.
- **Optionally gate per-slice** if `structure.md` is set to `mode: gated`.
- **Surface an escalation** when a phase gets stuck. The phase names the most likely upstream cause; the user decides what to do.

To resume an interrupted flow, run `/skill:qrspi-dev` again — phase detection picks up where the last run stopped.

## Artifact Layout

```
questions.md
research.md
design.md
structure.md          ← contains the slice list and `mode:` setting
slices/
  slice-1/
    plan.md
    review.md
  slice-2/
    plan.md
    review.md
  ...
review.md             ← final review (after all slices)
pr.md                 ← optional, generated only when user opts in and a git remote exists
```

The four design-phase artifacts (questions, research, design, structure) are gated and reviewed by the user. The per-slice files run autonomously by default.

## Per-Slice Autonomy and the `mode:` Field

`structure.md` carries a `mode:` field at the top:

- `mode: autonomous` (default) — the Plan / Implement / Review per-slice loop runs without checkpoints. The orchestrator advances to the next slice on Green/Yellow assessments. Stops only on escalation.
- `mode: gated` — Plan-slice waits for human approval on every slice before Implement starts.

The mode is decided by the user during the Structure human checkpoint. It can be edited later; the orchestrator re-reads it before each slice.

## Vertical Slicing

Each slice ships an end-to-end-testable user-visible capability. Horizontal slicing — "all DB, then all API, then all UI" — is forbidden. Slice 1 is always a thin PoC that proves the full stack works before any further slices build on it.

The Structure phase enforces this. See `structure/SKILL.md` and `structure/references/structure-checklist.md` for examples and anti-patterns.

## Tests-Pass Is a Hard Gate

A slice is not complete until its verification commands pass cleanly. Writing tests is part of Implement; running them until they pass is part of Implement. The verification-attempt budget is 3 — after 3 failed verification attempts (where all tasks are marked done and all tests are written), the phase escalates.

Test runs *during* development don't count toward this budget — only attempts where the model expects the suite to pass.

## Escalation, Not Auto-Loop-Back

When a phase gets stuck, it stops and surfaces:

- What went wrong.
- The most likely upstream cause.
- A suggested user action.

Review does **not** auto-iterate with Implement. The user reads the review file and decides whether to re-run Implement-slice, edit a plan, or loop further upstream. This is intentionally simple in v1 — automation will follow once the right loop-back patterns are clear from real use.

## Per-Phase Model Tiers

Each phase declares its expected model tier in frontmatter and prose:

| Phase | Tier |
|---|---|
| Questions | frontier-top |
| Research | frontier-mid |
| Design | frontier-top |
| Structure | frontier-top |
| Plan | local |
| Worktree | local |
| Implement | local |
| Review (per-slice) | frontier-mid |
| Review (final) | frontier-top |

A harness that routes per phase can read `model_tier` from each `SKILL.md`. The `frontier-top` / `frontier-mid` / `local` labels are coarse on purpose; refine if useful.

## Extending QRSPI

These skills are project-agnostic. Domain-specific skills (like `django-htmx-tailwind`) integrate naturally — for example, a Django skill could be loaded during Questions or Design to inform technology choices and architectural decisions.

A future direction worth experimenting with: parallel slice development. The worktree phase is already parameterized on branch name, leaving room for per-slice branches and dependency declarations in `structure.md`. No machinery for this in v1.
