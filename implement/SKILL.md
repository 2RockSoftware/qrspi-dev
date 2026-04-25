---
name: qrspi-dev-implement
description: Build the project by following the Plan. Fill in skeleton files with real, working code. Use in the Implement phase of QRSPI development.
---

# Implement Phase

Build the project by following the Plan. Fill in skeleton files with real, working code. Execute tasks sequentially, marking each as complete before moving to the next.

## How It Works

1. **Read `plan.md`** — this is your primary guide. Understand the context, tasks, and Worktree guidance.
2. **Find the first unchecked task** (`[ ]`) — if none exist, implementation is complete.
3. **Execute the task** following the instructions, file targets, and design decisions referenced in the task.
4. **Mark it complete** — change `[ ]` to `[x]` in `plan.md`.
5. **Move to the next task** — repeat until all tasks are complete.

## Task Execution

For each task:

1. **Read the task description** carefully — note what files to create/modify, dependencies, and decisions to apply
2. **Read relevant existing files** — understand what already exists before modifying it
3. **Read skeleton files** — understand the shape of what needs to be filled in
4. **Read referenced artifacts** — if a task references a design.md decision, read that section
5. **Implement the code** — write real, working code
6. **Verify the work** — ensure the code compiles/runs, tests pass (if applicable), and matches the task description
7. **Mark the task complete** — update `[ ]` to `[x]` in plan.md

### Coding Standards

- **Clean, readable code** — meaningful names, logical structure, no clever tricks
- **Follow language/ecosystem conventions** — idiomatic code that other developers in the ecosystem would recognize
- **Handle errors appropriately** — don't swallow errors, don't let unhandled errors crash the program
- **Write tests where the plan calls for them** — don't skip tests if they're in the plan
- **Don't over-engineer** — follow the plan, not your imagination. Implement what's specified, not what you think might be useful later.
- **Comments for counter-intuitive code** — if a solution isn't obvious, add a comment explaining why

### Task Execution Rules

- **One task at a time** — complete it fully before starting the next
- **If a task needs a new file** the plan didn't mention: create it and add a note
- **If a task is blocked or unclear**: stop and ask the user
- **If you discover the plan is wrong or incomplete**: note it in the task output but continue with best effort — don't modify plan.md
- **If you discover a fundamental design issue**: stop, explain it to the user, and ask for guidance

### Revisitation Rules

- **Never edit upstream artifacts** — `questions.md`, `research.md`, `design.md`, `outline.md`, and `plan.md` are frozen. They capture decisions that were made and approved. If you find something wrong, note it but don't change the documents.
- **If the Plan seems wrong**: continue with best effort, noting your concerns in the task output
- **If a fundamental design issue is discovered**: stop, explain the issue, and ask the user for guidance
- **If things spiral out of control**: stop and ask for guidance — don't keep pushing forward blindly

## No Human Checkpoint

After implementing all tasks, **automatically transition to the Review phase**. Say:

> "Implementation is complete. All tasks in the plan have been executed. Moving on to the Review phase for code review and quality assurance."

## Picking Up Mid-Implementation

If you're starting the Implement phase and some tasks are already checked off:

1. Read `plan.md` to see what's been completed
2. Read the implemented files to understand the current state
3. Continue from the first unchecked task
4. If you find that the completed work doesn't match the task descriptions, note it but don't redo completed work — flag it in the Review phase
