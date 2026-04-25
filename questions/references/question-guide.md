# Question Guide

Use this guide to ensure you're asking comprehensive questions across all relevant dimensions.

## Question Categories

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

## Asking Techniques

- **Avoid yes/no questions** — ask "what" and "how" instead of "do you want"
  - ❌ "Do you need authentication?"
  - ✅ "How should users authenticate and what access levels do they need?"
- **Explore tradeoffs** — help the user think through options
  - "If we use a monolithic architecture vs. modular, what would that mean for your timeline and maintenance?"
- **Ask about constraints before solutions** — constraints drive good design decisions
  - "What deployment environment do you need to support?" before "Should we use Docker?"
- **Surface implicit assumptions** — people often forget to mention the obvious
  - "You mentioned a web interface — should this be responsive for mobile browsers?"
- **Check for consistency** — make sure different parts of the requirements align
  - "You mentioned real-time updates but also offline support — how should those interact?"

## Transition Signal

The Questions phase is over when questions shift from the *what* and *why* to the *how*. Specifically:

- Questions about "where does this connect to existing code?" → that's Research
- Questions about "what technology should we use?" → that's Research
- Questions about "how should we implement X?" → that's Design
- Questions about file structure, module boundaries → that's Outline
