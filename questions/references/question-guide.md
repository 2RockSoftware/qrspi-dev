# Question Guide

Use this guide to ensure you're asking comprehensive questions across all relevant dimensions.

## Two Registers

The Questions phase runs in two registers simultaneously:

- **Dialogue register** — open, conversational questions. Avoid yes/no framing; explore tradeoffs; surface implicit assumptions.
- **Artifact register** — option-framed decisions in `questions.md`. Each decision is rendered with options, default, and consequences.

The dialogue is for the user's benefit; the artifact is the structured record. The same topic almost always appears in both — open in conversation, structured in the artifact.

## Worked Example

A user says: *"I want to build a CLI tool that manages my reading list."*

In dialogue you might ask: *"How do you want this stored — local file you can sync across machines yourself, or a cloud-backed store that syncs automatically?"*

In `questions.md` the same topic renders as:

```markdown
**Q3: Reading-list storage backend.**
- Options: A) local SQLite, B) local JSON file, C) cloud-synced (e.g. SQLite + Litestream, or a managed service)
- Default: A — simplest for a single-user CLI.
- Consequence of A: no automatic cross-device sync; user copies the file or uses their own sync (Dropbox, etc).
- Consequence of B: human-readable; lossy on concurrent writes; no schema enforcement.
- Consequence of C: works across devices out of the box; adds a network dependency and an account to manage.
- Resolved: A
```

Note: `Resolved:` only appears once the user has converged on an option. Open decisions ship to Research without that line.

## Question Categories

For each category, surface at least one option-framed decision in the artifact. The dialogue may cover much more.

### Functional Requirements

- What should the system do? (core features, capabilities)
- What are the user stories or use cases?
- What inputs does it accept? What outputs does it produce?
- What are the user journeys?

### Non-Functional Requirements

- Performance expectations (speed, throughput, latency)?
- Scale requirements (users, data volume)?
- Reliability / availability expectations?
- Security requirements (authentication, authorization, data protection)?
- Accessibility requirements?
- Compatibility requirements (browsers, OS, devices)?

### Integrations

- Does it connect to existing codebases?
- External APIs, databases, services?
- Data import/export formats?
- Third-party services or dependencies?

### Constraints

- Technology stack preferences or requirements?
- Programming language, framework, library preferences?
- Deployment environment (cloud, on-prem, edge, mobile)?
- Timeline constraints?
- Budget constraints?
- Compliance or regulatory requirements?
- Team skill level and preferences?

### Edge Cases

- What happens when input is invalid?
- What happens when external services are unavailable?
- What are the unusual or uncommon scenarios?
- How should failures be handled and communicated?

### Success Criteria

- How will we know this project is successful?
- What does "done" look like?
- What metrics matter (user adoption, performance benchmarks, test coverage)?
- What are the must-have vs. nice-to-have features?

### Scope Boundaries

- What is explicitly out of scope for this project?
- What will be addressed in future iterations?
- Are there any parts that should be reused from existing work?

## Negative-Space Reasoning

A common failure mode: the model only addresses what the user explicitly raised. Counter it deliberately. For each architectural area above, ask **what's *not* been said yet** and surface at least three undecided things with their consequences. The artifact's Open Decisions section is where this coverage shows up.

Concrete example: if the user said "build a web app" but never said anything about auth, the artifact should still carry an Open Decision for auth (`Q?: Authentication approach. Options: ...`). Negative-space reasoning is what catches that.

## Asking Techniques (Dialogue Register)

- **Avoid yes/no questions** — ask "what" and "how" instead of "do you want."
  - "Do you need authentication?" → "How should users authenticate and what access levels do they need?"
- **Explore tradeoffs** — help the user think through options.
  - "Monolith vs. modular — what would each mean for your timeline and maintenance?"
- **Ask about constraints before solutions.**
  - "What deployment environment do you need to support?" before "Should we use Docker?"
- **Surface implicit assumptions.**
  - "You mentioned a web interface — should it be responsive for mobile browsers?"
- **Check for consistency.**
  - "You mentioned real-time updates but also offline support — how should those interact?"

The artifact register stays structured even when dialogue is open. Don't water down the artifact to match the conversational tone.

## Transition Signal

The Questions phase is over when questions shift from the *what* and *why* to the *how*:

- "Where does this connect to existing code?" → Research.
- "What technology should we use?" → Research.
- "How should we implement X?" → Design.
- File structure, slice boundaries → Structure.
