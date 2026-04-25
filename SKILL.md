---
name: qrspi-dev
description: Orchestrates phased agentic software development (Questions → Research → Design → Outline → Plan → Worktree → Implement → Review). Use as the entry point when starting a new project. Pronounced "crispy".
---

# QRSPI-Dev Orchestrator

Welcome. This is the QRSPI (Questions → Research → Design → Outline → Plan → Worktree → Implement → Review) orchestrator. Pronounced "crispy."

## The Flow

```
Questions → Research → Design → Outline → Plan → Worktree → Implement → Review
```

## How to Use

1. **Provide a project description** — what you want to build, even if rough
2. **I detect where you are** by checking for existing artifacts
3. **I load the appropriate phase skill** and guide you through it
4. **After human-gated phases** (Questions, Research, Design, Outline, Plan), I'll stop and wait for your approval
5. **Worktree → Implement → Review flow automatically** — no checkpoints
6. **If Review finds major issues**, I'll loop back to Implement with the review feedback
7. **If things spiral out of control**, I'll stop and ask for your guidance

## Phase Detection

Check which artifacts already exist in the working directory. Start at the **first phase whose artifact does NOT exist**:

| If this exists | AND this does NOT exist | Start at |
|----------------|-------------------------|----------|
| *(none)*       | `questions.md`          | Questions |
| `questions.md` | `research.md`           | Research |
| `research.md`  | `design.md`             | Design |
| `design.md`    | `outline.md`            | Outline |
| `outline.md`   | `plan.md`               | Plan |
| `plan.md`      | (skeleton files exist)  | Worktree |
| *(skeletons)*  | `review.md`             | Implement → Review |

**Rules:**
- If no artifacts exist → start at **Questions**
- If only some exist → start at the **first missing** one in the sequence above
- If `plan.md` exists but no skeleton files → start at **Worktree**
- If `review.md` exists from a prior review → continue at **Review** and re-check deferred items

## Artifact Reference

| Artifact | Produced By | Read By |
|----------|-------------|----------|
| `questions.md` | Questions | Research |
| `research.md` | Research | Design |
| `design.md` | Design | Outline + Plan |
| `outline.md` | Outline | Plan |
| `plan.md` | Plan | Worktree + Implement |
| `review.md` | Review | Implement (revisions) |

## Phase Skills

Each phase is a detailed instruction set. I'll `read` the appropriate skill when that phase is active:

- `questions/SKILL.md` — Requirements dialogue
- `research/SKILL.md` — Information gathering
- `design/SKILL.md` — Architecture design
- `outline/SKILL.md` — Project overview + structure
- `plan/SKILL.md` — Detailed implementation plan
- `worktree/SKILL.md` — Scaffolding + dev tooling
- `implement/SKILL.md` — Code implementation
- `review/SKILL.md` — Code review + quality assurance

## Revision Loop

When Review finds major issues:
1. Review writes `review.md` with deferred issues
2. Implement works on deferred items with `review.md` as context
3. Review runs again to check if issues are fixed
4. Repeat until `review.md` is clean or escalation is needed

---

**Tell me what you want to build, and we'll get started.**
