# Design Patterns

Reference for creating effective architecture designs during the QRSPI flow.

## Approach Comparison Template

Use this template in dialogue when surfacing 2-3 viable approaches for any major decision. The point is to make tradeoffs visible *before* converging, not after.

```markdown
## Decision: <decision name>

### Approach A: <name>
- **Sketch:** <one-paragraph description of the approach>
- **Strengths:** <what this approach gives us>
- **Weaknesses:** <what we sacrifice>
- **Fits our constraints because:** <ties back to questions.md / research.md>
- **Doesn't fit because:** <where this approach pushes against our constraints>

### Approach B: <name>
- **Sketch:** ...
- **Strengths:** ...
- **Weaknesses:** ...
- **Fits because:** ...
- **Doesn't fit because:** ...

### Approach C: <name>  (optional — skip if there are only two real options)
- ...

### Recommendation
<Single approach recommended, with the one or two tradeoffs that decided it. The user converges or pushes back.>
```

The recommendation comes **after** the comparison, not in place of it. If you can't articulate two real alternatives, that itself is a finding — surface it ("only one viable approach because constraint X eliminates everything else") rather than padding with strawmen.

## Architecture Patterns to Consider

### Monolith
- Everything in one deployable unit.
- Best for: small teams, simple projects, rapid development.
- Worst for: teams needing independent scaling, complex distributed systems.

### Modular / Plugin Architecture
- Core system with pluggable modules.
- Best for: extensible systems, plugins/extensions, evolving feature sets.
- Worst for: when module boundaries create unnecessary complexity.

### Layered Architecture
- Presentation → Business Logic → Data Access.
- Best for: traditional applications, clear separation of concerns.
- Worst for: event-driven systems, real-time applications.

### Event-Driven
- Components communicate through events/message buses.
- Best for: async processing, real-time systems, decoupled services.
- Worst for: simple CRUD apps, systems needing strict transaction guarantees.

### Client-Server
- Clients request services from a centralized server.
- Best for: web apps, API services, centralized data management.
- Worst for: peer-to-peer systems, offline-first applications.

### Microservices
- Independent, deployable services communicating via network.
- Best for: large teams, independent scaling needs, polyglot systems.
- Worst for: small projects, network latency sensitivity, operational complexity.

## Describing Component Relationships

### Text-Based Architecture Diagrams

Use markdown with clear visual hierarchy:

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Client    │────▶│   Server    │────▶│   Database  │
│  (Browser)  │◀────│  (API)      │◀────│             │
└─────────────┘     └─────────────┘     └─────────────┘
                         │
                         ▼
                   ┌─────────────┐
                   │  Cache      │
                   │  (Redis)    │
                   └─────────────┘
```

### Component Description Format

For each component, describe:

```markdown
### Component Name
**Purpose:** One-sentence description of what this component does.

**Responsibilities:**
- What it handles
- What it delegates to other components

**Dependencies:**
- What it depends on
- What depends on it

**Interface:**
- How external components interact with it
- Key methods, endpoints, or events
```

## Common Cross-Cutting Concerns

### Authentication & Authorization
- How are users identified? (sessions, tokens, API keys)
- What are the authorization models? (RBAC, ABAC, permissions)
- Where is auth enforced? (gateway, each service, per-resource)

### Error Handling
- How are errors represented? (HTTP status codes, error objects, events)
- How are errors propagated? (logging, monitoring, user feedback)
- What are the retry strategies? (idempotency, circuit breakers)

### Logging & Observability
- What gets logged and at what level?
- How are traces correlated across components?
- What metrics matter for this system?

### Configuration
- Where does configuration live? (env vars, config files, secret managers)
- How are environment differences managed? (dev, staging, production)

### Data Validation
- Where does validation happen? (input, business logic, persistence)
- What validation libraries or patterns are used?

## Template for Single Technology Decisions

When the comparison is only between two specific tools (not architectural directions), this lighter template is fine:

```markdown
### Technology: [Name]
**Decision:** [What was chosen]

**Why:** [Justification grounded in research and requirements]

**Alternatives Considered:**
- [Alternative 1]: [Why it wasn't chosen]
- [Alternative 2]: [Why it wasn't chosen]

**Tradeoffs:**
- What we gain
- What we sacrifice
```

For larger, architecture-shaping decisions, use the Approach Comparison Template at the top of this file instead.
