# Plan Format

Specification for writing effective `plan.md` documents.

## Overall Structure

```markdown
# Plan: [Project Name]

## Context
[Project summary, tech choices, constraints, cross-cutting concerns]

## Phase 1: [Name]
[1-3 sentence description of what this phase accomplishes]

- [ ] **1.1 Task Title** — One-line summary
  - **Files:** `src/file1.py`, `src/file2.py`
  - **Description:** Detailed description of what to do
  - **Dependencies:** Task 1.0
  - **Decisions:** [design.md](#technology-choices) — use FastAPI for the API layer

- [ ] **1.2 Task Title** — One-line summary
  - **Files:** `src/database.py`
  - **Description:** ...
  - **Dependencies:** Task 1.1

## Phase 2: [Name]
...

## Worktree Guidance
Expected directory structure:
```
project/
├── src/
│   ├── api/
│   ├── models/
│   └── utils/
├── tests/
└── config/
```

Tooling: Python 3.11+, pip, pytest, ruff, black
```
```

## Task Naming Conventions

- Use **numbered phases**: Phase 1, Phase 2, etc.
- Use **numbered tasks**: 1.1, 1.2, 1.3, 2.1, 2.2, etc.
- Use **descriptive titles**: "Implement user authentication" not "Task 1"
- Use **action verbs**: Create, Implement, Configure, Set up, Write, Add

## Writing Task Descriptions

Good task descriptions include:

### What
- What should be created or modified
- The expected outcome of the task
- Any specific behaviors the code should have

### Where
- Which files to create or modify
- Which files to read for context
- Where new code should be placed

### How (high-level)
- Key decisions that apply to this task
- Patterns or conventions to follow
- Any specific algorithms or approaches to use

### Dependencies
- Which tasks must be completed before this one
- Which outputs from previous tasks this task consumes

### Decisions to Apply
- Which design.md decisions inform this task
- Which research.md findings should be applied

## Example Task

```markdown
- [ ] **1.1 Create database models**
  - **Files:** `src/models/user.py`, `src/models/project.py`, `src/models/__init__.py`
  - **Description:** Create SQLAlchemy ORM models for User and Project entities.
    - User model: id (UUID primary key), email (unique string), password_hash (string),
      created_at (datetime), projects (relationship to Project)
    - Project model: id (UUID primary key), name (string), description (text, nullable),
      owner_id (FK to User), created_at (datetime)
    - Use UUIDs for all primary keys
    - Use UTC timestamps with timezone awareness
    - Add __init__.py that exports both models
  - **Dependencies:** Task 1.0 (database connection setup)
  - **Decisions:** [design.md](#data-layer) — use SQLAlchemy 2.0 async API, UUID primary keys
```

## Phase Grouping

Group tasks into logical phases. Common phase groupings:

- **Data layer → API layer → Integration → Polish**
- **Core functionality → Extended features → Edge cases → Polish**
- **Foundation → Features → Integration → Testing**

Each phase should have a one-sentence description explaining its scope.

## Checkbox Management

- Start all tasks with `[ ]` (empty checkbox)
- The Implement phase will mark them as `[x]` (completed) as work progresses
- Do not pre-mark any tasks as complete
- Order tasks so checkboxes are naturally filled left-to-right, top-to-bottom

## Task Granularity

**Too granular:**
- [ ] "Create file `src/utils.py`"
- [ ] "Add import for `requests` in `src/utils.py`"
- [ ] "Define function `fetch_data()` in `src/utils.py`"

**Just right:**
- [ ] "Create utility module with `fetch_data()` function that handles HTTP requests with retry logic"

**Too broad:**
- [ ] "Build the entire API layer"
