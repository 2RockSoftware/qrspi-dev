# Design Patterns

Reference for creating effective architecture designs during the QRSPI flow.

## Architecture Patterns to Consider

### Monolith
- Everything in one deployable unit
- Best for: small teams, simple projects, rapid development
- Worst for: teams needing independent scaling, complex distributed systems

### Modular / Plugin Architecture
- Core system with pluggable modules
- Best for: extensible systems, plugins/extensions, evolving feature sets
- Worst for: when module boundaries create unnecessary complexity

### Layered Architecture
- Presentation → Business Logic → Data Access
- Best for: traditional applications, clear separation of concerns
- Worst for: event-driven systems, real-time applications

### Event-Driven
- Components communicate through events/message buses
- Best for: async processing, real-time systems, decoupled services
- Worst for: simple CRUD apps, systems needing strict transaction guarantees

### Client-Server
- Clients request services from a centralized server
- Best for: web apps, API services, centralized data management
- Worst for: peer-to-peer systems, offline-first applications

### Microservices
- Independent, deployable services communicating via network
- Best for: large teams, independent scaling needs, polyglot systems
- Worst for: small projects, network latency sensitivity, operational complexity

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

## Template for Technology Decisions

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
