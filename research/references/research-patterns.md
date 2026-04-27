# Research Patterns

Guidelines for effective research during the QRSPI flow. Research is a documentarian, not a designer — these patterns reinforce that boundary.

## Research Methodology

### Step 1: Identify Knowledge Gaps
- Read `questions.md` and list its Open Decisions and Remaining Uncertainties.
- Prioritize gaps that block Design decisions over nice-to-knows.
- Anything outside `questions.md`'s scope is out of scope.

### Step 2: Gather Information
- Start broad (overviews, official documentation), then drill into specifics.
- Prefer official documentation over third-party blogs.
- Read existing code when an existing codebase is in scope — it often reveals more than docs.
- Capture file paths, URLs, version numbers as you go.

### Step 3: Cross-Reference and Mark Contradictions
- Cross-reference findings from multiple sources.
- When sources disagree, surface the conflict — do not pick a winner.
- Be honest about uncertainty: distinguish "documented behavior" from "behavior I observed in practice" from "what the community generally claims."
- **Do not generate recommendations.** Recommendations belong to Design. If a finding wants to become a recommendation, refile it as an Open Question (for Design).

### Step 4: Organize by Decision Area
- Group findings by the *decisions they inform*, not by source.
- Each section answers "What does the evidence say about decision X?" — not "What did source Y say?"

## Citing Sources

Use a consistent format:

```
- [Source Name](URL or file path) — brief description of what it covers
```

For web sources: include the URL and what specifically you found there.
For code sources: include `file:line` and what you learned from it.
For docs: include doc name, version, and relevant section.

## Organizing by Decision Area

Good organization (mirrors `questions.md` decision areas):

```markdown
## Authentication Strategy

### Findings
- Django's built-in auth uses session cookies, with optional token middleware via DRF. [Django docs](https://docs.djangoproject.com/en/5.0/topics/auth/)
- JWT requires a separate package (`djangorestframework-simplejwt`); no first-party support. [DRF SimpleJWT](https://django-rest-framework-simplejwt.readthedocs.io/)
- The existing codebase at `auth/middleware.py:42` already configures session-based auth.

### Open Question (for Design)
Should this project keep the existing session-based auth or layer JWT on top for the new API surface? Both are documented as viable.

## Data Storage

### Findings
- ...

### Open Question (for Design)
- ...
```

Avoid organizing by source:

```markdown
## From the Django docs...
## From StackOverflow...
## From the existing codebase...
```

Source-organized notes are intermediate scratch work, not the artifact.

## Identifying Relevant Patterns

### Red Flags (low relevance)
- Patterns for a different problem domain
- Solutions to problems you don't have
- Trends that don't match your constraints
- Over-engineered patterns for simple problems

### Green Flags (high relevance)
- Solutions used by similar projects in your domain
- Patterns that address constraints named in `questions.md`
- Approaches mentioned in official documentation
- Patterns already in use in any existing codebase being extended

## Handling Conflicting Information

When sources disagree:

1. Note the conflict explicitly.
2. Document each side with its source.
3. **Do not pick a winner.** Surface the conflict as an Open Question (for Design).

Example:

```markdown
## Template Engine

### Findings
- Django's built-in templates: included with the framework, well-documented. [Django docs](https://docs.djangoproject.com/en/5.0/topics/templates/)
- Jinja2: faster on benchmarks, more flexible feature set, requires a small integration shim. [Jinja2 docs](https://jinja.palletsprojects.com/)
- Existing codebase uses Django templates throughout (`templates/` directory).

### Open Question (for Design)
Stick with Django templates for consistency with the existing codebase, or switch to Jinja2 for the new feature for the performance / flexibility delta?
```

A previous version of this template included a `Recommendation:` subsection. That was wrong; recommendations are Design's job.
