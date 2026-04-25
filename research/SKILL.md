---
name: qrspi-dev-research
description: Gather information from docs, code, web, and best practices to inform project design. Use in the Research phase of QRSPI development. Produces research.md.
---

# Research Phase

Gather the information needed to inform the project design. Research bridges the gap between clarified requirements and a concrete architecture.

## How It Works

1. **Read `questions.md`** — identify knowledge gaps, technology decisions that need justification, and areas requiring research
2. **Research the following sources as needed:**
   - **Project documentation** — if an existing codebase or project is present, read its docs, README, architecture docs, and AGENTS.md
   - **Existing source code** — if extending or merging with existing work, understand the current codebase structure, conventions, and patterns
   - **Outside documentation** — web search for best practices, framework docs, API references, design patterns, and relevant libraries
   - **User-provided documents** — any docs the user has shared
3. **Synthesize findings** into organized, actionable knowledge
4. **Write `research.md`** — findings by topic, with sources cited, patterns identified, and recommendations

## What to Research

Prioritize based on what `questions.md` indicates is uncertain:

- **Technology choices** — evaluate frameworks, libraries, tools against the requirements
- **Integration patterns** — how does your project connect to existing systems?
- **Best practices** — patterns specific to the problem domain
- **API documentation** — details about external APIs or internal interfaces
- **Existing conventions** — coding standards, architecture patterns, testing approaches already in use in the codebase
- **Constraints** — confirm any technical or environmental constraints

## Output Artifact

Write `research.md` with:

- **Findings organized by topic** — not by source, but by decision area
- **Cited sources** — URLs, file paths, document names with consistent citation format
- **Patterns identified** — architectural patterns, code patterns, integration patterns
- **Recommendations** — what you recommend and why (grounded in findings)
- **Remaining open questions** — things you couldn't resolve and need user input on

## Human Checkpoint

After writing `research.md`, **stop and wait for approval**. Say:

> "Research phase is complete. I've written `research.md` with findings from our investigation. Review it and let me know if we're ready to move on to Design, or if anything needs adjustment."

Do not proceed to Design until the human explicitly approves or requests changes.
