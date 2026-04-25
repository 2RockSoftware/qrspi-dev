# Task Execution Reference

How to execute tasks from plan.md effectively.

## Execution Flow

```
1. Read plan.md → find first [ ] task
2. Read task description, files, dependencies, decisions
3. Read existing files (for context)
4. Read referenced artifacts (design.md decisions, etc.)
5. Implement the code
6. Verify (compile, run basic check, check tests)
7. Mark task [x] in plan.md
8. Repeat
```

## Checkbox Management

- **Always use the exact checkbox format**: `[ ]` and `[x]`
- **Update in plan.md** — the source of truth
- **One task at a time** — mark complete before moving on
- **If you can't complete a task** — leave it as `[ ]` and explain why

## Handling Unexpected Files

### Task Requires a New File Not in Plan
- Create the file as needed
- Note it in the task output: "Created `src/extra_util.py` — not in original plan but needed for task completion"
- Don't modify plan.md

### Skeleton File Has Different Structure Than Expected
- Follow the existing skeleton structure
- Adapt your implementation to match
- Note any significant deviations in the task output
- Don't modify plan.md or skeleton files' signatures

## Handling Blocked Tasks

### Task Is Blocked By Missing Information
- Identify what information is missing
- Explain the block to the user
- Suggest how to proceed (if you have ideas)
- Stop and ask for guidance

### Task Depends On An Incomplete Task
- Verify the dependency is actually complete
- If the dependency was marked complete but isn't working, note it
- Try to work around the issue if possible
- Otherwise, stop and ask for guidance

## When to Stop and Ask vs. Proceed

### Stop and Ask
- The task description is ambiguous and you can't determine intent
- A required file or dependency is missing and you don't know what to do
- The task requires a technology or tool not available
- You discover a contradiction between plan.md and design.md
- You're uncertain about a critical decision and it would change the implementation

### Proceed with Best Effort
- Minor implementation details aren't specified (e.g., exact variable names)
- You can reasonably infer the intent from context
- The task has multiple valid implementations and you choose one
- You find a small bug in an upstream artifact (note it, but don't block on it)

## Verifying Work

Before marking a task complete:

1. **Code compiles / syntax is valid** — run a basic syntax check
2. **Imports resolve** — verify all imports are correct
3. **Tests pass** — if the task includes or enables tests, run them
4. **Matches task description** — re-read the task and verify you've addressed everything
5. **No obvious bugs** — quick manual review for common mistakes

## Logging Your Progress

When you complete a task, briefly note:
- What was done
- Any files created or significantly modified
- Any decisions made that aren't in the plan
- Any concerns or observations

Example:

> "Task 1.1 complete: Created database connection module with async SQLAlchemy engine, connection pooling, and retry logic. Created `src/database.py` and `src/database_pool.py`. Used connection pooling from design.md recommendation. Note: used SQLite in-memory for dev config as plan suggested."
