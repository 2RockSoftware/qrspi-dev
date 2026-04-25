---
name: qrspi-dev-outline
description: Create a human-readable project overview with structure and broad-stroke plan. Use in the Outline phase of QRSPI development. Produces outline.md.
---

# Outline Phase

Create a one-to-two page document that gives a strong overview of the work ahead. This is the **human-facing checkpoint** — a good developer should be able to read the outline and picture the plan.

## How It Works

1. **Read `design.md`** — understand the architecture and component decisions
2. **Create `outline.md`** — a broad-stroke overview covering project description, structure, and implementation sequence

## What to Include

### Project Overview
- What this project is and what it will accomplish (written for a human reader)
- The problem it solves and the value it delivers
- Key features at a high level
- Write clearly and conversationally — this is for humans, not agents

### Structure
- File and directory organization
- Module boundaries and what goes where
- How the code is organized into logical units
- Any naming conventions or structural patterns

### Broad-Stroke Plan
- Not a detailed task list — that belongs in the Plan
- But enough that a developer can picture the implementation sequence
- Example: "We'll start with the data layer, then build the API, then the UI, then integrate them"
- Mention any interesting sequences or dependencies between major pieces

### Key Decisions
- The important architectural and technical choices that shape the work
- Briefly reference the design.md decisions without repeating them verbatim
- Any decisions that need special attention during implementation

## Writing Style

- **Write for a human developer** — clear, concise, picture-painting
- **Keep it to 1-2 pages** — not exhaustive, not shallow
- **Avoid nitty-gritty details** — implementation specifics belong in the Plan
- **Use headings and bullet points** for readability
- **Be specific enough to be useful** — "create a database models module" not "set up data things"

## Output Artifact

Write `outline.md` with:

- **Project Overview** — what, why, key features
- **Structure** — directory organization, module boundaries
- **Broad-Stroke Plan** — implementation sequence overview
- **Key Decisions** — important choices that shape the work

## Human Checkpoint

After writing `outline.md`, **stop and wait for approval**. This is the most heavily human-reviewed artifact in the flow. Say:

> "Outline phase is complete. I've written `outline.md` as a human-readable overview of the project. This is the document you'll review most carefully — let me know if we're ready to move on to the detailed Plan, or if any changes are needed."

Incorporate any feedback and re-prompt for approval before proceeding. Do not move to the Plan phase until the outline is approved.
