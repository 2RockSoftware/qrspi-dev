---
name: qrspi-dev-worktree
description: Set up a git worktree for the project branch. Minimal git operations; no scaffolding or tooling install. Use in the Worktree phase of QRSPI development.
model_tier: local
inputs: structure.md (branch hint only)
outputs: git worktree state
forbidden_inputs: design.md, plan.md, slice contents
---

# Worktree Phase

Set up a `git worktree` for the project branch. That's it. Worktree does not scaffold files, configure tooling, or install dependencies — those concerns belong to slice 1 of Implement (where they can be vertical-slice-aware).

## Phase Contract

- **Reads:** `structure.md` only, and only the branch hint (if present). Project name optional, derived from the directory if needed.
- **Writes:** git worktree state. No artifact file.
- **Does not read:** `design.md`, slice plans, or any code. Worktree has no architectural opinion.
- **Tier:** `local`. Mechanical git operations.

## How It Works

1. **Detect repo presence.** Run `git rev-parse --is-inside-work-tree`. If non-zero (no repo), log "vibe mode — no repo, skipping worktree" and transition. The orchestrator may also skip entry to this phase entirely in vibe mode; the skill handles either path gracefully.
2. **Determine branch name.** Use the `Branch Hint` from `structure.md` if present. Otherwise default to a sensible project-level name (e.g., `qrspi/<project-slug>`). The skill is parameterized on branch name — see Future note below.
3. **Ensure clean working dir.** `git status --porcelain`. If non-empty, escalate.
4. **Create the worktree.** `git worktree add <path> <branch>`. If the branch already exists, escalate (don't reuse silently).
5. **Transition to Plan-slice-1.**

## Future: Parallel Slices

The current implementation runs slices serially in one project-level worktree. A future extension can run independent slices in parallel by creating a worktree per slice. The seam is small: the skill is already parameterized on branch name, so per-slice mode adds a per-slice branch (`<project>/slice-<N>`) plus a dependency declaration in `structure.md` indicating which slices can run in parallel and which must be serial.

No machinery for this in v1 — only the parameterization. When you want to experiment with parallel slices, add a `parallelism:` field to `structure.md` and a per-slice branch derivation here.

## No Human Checkpoint

Worktree transitions automatically:

> "Worktree set up on branch `<name>` at `<path>`. Moving to Plan for slice 1."

In vibe mode (no repo):

> "No git repo detected — vibe mode. Skipping worktree. Moving to Plan for slice 1."

## Triggers for Escalation to the User

Stop the phase and surface to the user when:

- **Working directory is dirty** — `git status --porcelain` returned uncommitted changes. Suggested user action: stash or commit changes, then re-run.
- **Target branch already exists** — re-using a branch is destructive. Suggested user action: pick a different branch name (edit `Branch Hint` in `structure.md`) or delete the existing branch deliberately.
- **`git worktree add` fails** — git error output is the user's signal. Suggested user action: read the git error, resolve manually.
- **Repo is in an unusual state** (detached HEAD, mid-rebase, mid-merge) — likely upstream cause: external. Suggested user action: resolve the git state first.
