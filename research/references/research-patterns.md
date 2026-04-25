# Research Patterns

Guidelines for effective research during the QRSPI flow.

## Research Methodology

### Step 1: Identify Knowledge Gaps
- Read the upstream artifacts (questions.md, etc.) and list things you don't know
- Prioritize gaps that block design decisions
- Separate "need to know now" from "nice to know later"

### Step 2: Gather Information
- Start broad (overview, best practices) then drill into specifics
- Prefer official documentation over third-party blogs when possible
- Search for real-world examples and case studies, not just theory
- Read existing code when available — it often reveals more than docs

### Step 3: Evaluate and Synthesize
- Cross-reference findings from multiple sources
- Note contradictions or conflicting recommendations
- Ground all recommendations in evidence, not opinion
- Be honest about uncertainty — mark things as "recommended" vs. "unconfirmed"

### Step 4: Organize by Decision Area
- Group findings by the *decisions they inform*, not by source
- Each section should answer: "What should we do about X, and why?"

## Citing Sources

Use a consistent format:

```
- [Source Name](URL or file path) — brief description of what it covers
```

For web sources: include the URL and what specifically you found there.
For code sources: include the file path and what you learned from it.
For docs: include the doc name, version, and relevant sections.

## Organizing by Topic

Good organization:

```markdown
## Authentication Strategy
### Findings
- ...
### Sources
- ...
### Recommendation
- ...

## Data Storage
### Findings
- ...
### Sources
- ...
### Recommendation
- ...
```

Avoid organizing by source:

```markdown
## From the Django docs...
## From StackOverflow...
## From the existing codebase...
```

## Identifying Relevant Patterns

### Red Flags (low relevance)
- Patterns for a different problem domain
- Solutions to problems you don't have
- Trends that don't match your constraints
- Over-engineered solutions for simple problems

### Green Flags (high relevance)
- Solutions used by similar projects in your domain
- Patterns that address your specific constraints
- Approaches mentioned in official documentation
- Patterns already used in an existing codebase

## Handling Conflicting Information

When sources disagree:
1. Note the conflict explicitly
2. Explain both sides
3. Recommend based on your assessment of the project's priorities
4. Mark as "needs user decision" if you can't determine the right choice

Example:

```markdown
## Template Engine
- Django's template engine: simple, built-in, well-documented
- Jinja2: more flexible, faster, but requires additional setup
- **Recommendation**: Django templates for simplicity, unless the team has strong Jinja2 experience
```
