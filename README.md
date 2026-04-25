# qrspi-dev

A phased agentic development framework for [pi](https://pi.dev). Pronounced **"crispy"**.

## The Phases

QRSPI stands for **Questions → Research → Design → Outline → Plan → Worktree → Implement → Review**.

| Phase | What it does | Output |
|-------|-------------|--------|
| **Questions** | Open dialogue to refine rough ideas into clarified requirements | `questions.md` |
| **Research** | Gather information from docs, code, web, best practices | `research.md` |
| **Design** | Create a broad architecture blueprint | `design.md` |
| **Outline** | Human-readable overview: project description, structure, broad plan | `outline.md` |
| **Plan** | Detailed implementation plan with checkbox task list | `plan.md` |
| **Worktree** | Scaffold directory structure and set up dev tooling | skeleton files + configs |
| **Implement** | Build the project by executing the plan's tasks | working code |
| **Review** | Review for bugs, quality, completeness. Fix minor issues, defer major ones | `review.md` |

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
- Ask you to approve before moving from each human-gated phase (Questions, Research, Design, Outline, Plan)
- Flow automatically through Worktree → Implement → Review
- Loop back to Implement if Review finds deferred issues

To resume an interrupted flow, just run `/skill:qrspi-dev` again — it detects where you left off.

## Artifact Guide

Each phase writes a document to the working directory. Later phases read earlier artifacts:

```
questions.md  →  research.md  →  design.md  →  outline.md  →  plan.md
                                                      ↓
                                            (skeleton files + tooling)
                                                      ↓
                                              implementing the plan
                                                      ↓
                                              review.md (feedback only)
```

## Revision Workflow

When the Review phase finds issues:
- **Minor issues** (typos, missing imports, trivial bugs) → auto-fixed
- **Major issues** → written to `review.md`, handed back to Implement
- **Systemic problems** → Review stops and asks the human for guidance

The Implement + Review loop repeats until `review.md` is clean or escalation is needed.

## Extending QRSPI

These skills are project-agnostic. Domain-specific skills (like `django-htmx-tailwind`) integrate naturally — for example, a Django skill could be loaded during the Questions or Design phase to inform technology choices and design decisions.
