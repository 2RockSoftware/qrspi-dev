# Outline Checklist

Use this checklist to ensure the outline is complete before human review.

## Must-Have Sections

- [ ] **Project Overview** — what is this, what does it do, what problem does it solve?
- [ ] **Key Features** — bulleted list of main features at a high level
- [ ] **Structure** — file/directory organization, module boundaries, what goes where
- [ ] **Broad-Stroke Plan** — implementation sequence that a developer can visualize
- [ ] **Key Decisions** — important architectural/technical choices that shape the work

## Quality Checks

- [ ] **Human-readable** — written for a developer to read, not an agent to execute
- [ ] **1-2 pages** — concise but comprehensive, not exhaustive
- [ ] **No implementation details** — no code, no function signatures, no file paths
- [ ] **No task-level detail** — that belongs in the Plan
- [ ] **Picture-painting** — a good developer reading this should know roughly how to start
- [ ] **Consistent with design.md** — doesn't contradict the architecture document

## What to Include vs. Defer

| Include in Outline | Defer to Plan |
|-------------------|---------------|
| "We'll have a `models/` directory with SQLAlchemy models" | "Create `models/user.py` with `User` class having `id`, `email`, `password_hash` fields" |
| "Start with data layer, then API, then integration" | "Task 1: Create database connection module with connection string handling" |
| "REST API with authentication" | "Task 3: Implement `/auth/login` endpoint with JWT token generation" |
| "Error handling at the API boundary" | "Task 5: Create `handlers/errors.py` with `handle_404()`, `handle_500()` functions" |

## Length Guidelines

- **Too short:** Missing the structure section, vague broad-stroke plan, reads like a one-liner
- **Just right:** Covers all 4 sections, specific enough to visualize, 1-2 pages
- **Too long:** Includes implementation details, task-level specifics, code examples
