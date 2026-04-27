---
name: qrspi-dev-research
description: Gather objective facts from docs, code, and the web to inform Design. Use in the Research phase of QRSPI development. Produces research.md.
model_tier: frontier-mid
inputs: questions.md, codebase, web sources
outputs: research.md
forbidden_inputs: original user prompt / ticket
---

# Research Phase

Gather the facts needed to inform Design. **Research is a documentarian, not a designer.** It collects evidence; Design makes the choices.

## Phase Contract

- **Reads:** `questions.md` and the sources it points to (existing codebase, library docs, web search results, user-shared documents).
- **Writes:** `research.md` at the project root.
- **Does not read:** the user's original prompt or any framing of the project that pre-dates `questions.md`. If the researcher knows what the human is hoping to build, findings turn into opinions. Read only what `questions.md` says is open and the sources needed to address those open items.
- **Tier:** `frontier-mid`. Synthesis of facts is real work, but the load is bounded by the questions list rather than open-ended reasoning.

## How It Works

1. **Read `questions.md`** — identify the Open Decisions and Remaining Uncertainties. Those define the research scope.
2. **Gather facts from these source types as needed:**
   - **Project documentation** — if an existing codebase or project is present, read its docs, README, architecture notes, AGENTS.md, etc.
   - **Existing source code** — when extending or merging with existing work, learn the current structure, conventions, and patterns by reading files (not by guessing).
   - **Outside documentation** — official docs and API references for any libraries / frameworks / services in scope. Prefer official sources to blogs.
   - **User-provided documents** — anything the user has shared in or alongside `questions.md`.
3. **Cross-reference** — note where sources agree, disagree, or leave a question unanswered.
4. **Write `research.md`** — facts organized by decision area, with citations.

## What Research Is And Isn't

| Research is | Research isn't |
|---|---|
| Findings with citations | Recommendations |
| Open questions surfaced for Design | Choices made for Design |
| Cross-references between sources | Synthesis into an architecture |
| "X behaves as Y under condition Z" | "X is the right choice because..." |

If a finding wants to become a recommendation, surface it as an **Open Question (for Design)** instead. Design owns the verdict.

## What to Research

Prioritize by what `questions.md` says is open. Likely areas:

- **Technology choices** — capabilities, constraints, tradeoffs of candidate frameworks/libraries/tools — *as facts, not endorsements*.
- **Integration patterns** — how candidate systems actually connect to the targets named in `questions.md`.
- **Best practices** — patterns documented for the specific problem domain.
- **API contracts** — the literal interface details (auth model, rate limits, request/response shapes).
- **Existing conventions** — coding standards, architecture patterns, testing approaches already in use in any existing codebase being extended.
- **Constraint validation** — confirm or refute claimed technical/environmental constraints.

## Output Artifact

`research.md` contains:

- **Findings, organized by decision area** — not by source. Each section answers "What does the evidence say about decision X?"
- **Citations** — URLs, file:line references, doc names with versions. Every non-trivial claim is cited.
- **Open Questions (for Design)** — items the evidence surfaces but does not resolve. These are the handoff to Design.

Each section uses this shape:

```markdown
## <Decision area, mirroring questions.md when possible>

### Findings
- <Fact, cited>. [<Source>](<url-or-path>)
- <Fact, cited>. [<Source>](<url-or-path>)

### Open Question (for Design)
<What Design needs to decide, framed as a question. No verdict.>
```

Not a `Recommendation:` subsection. Not "Research suggests…" framing. Just facts and the surfaced question.

## Human Checkpoint

After writing `research.md`:

> "Research phase is complete. I've written `research.md` with findings organized by decision area, plus open questions for Design. Review it and let me know if we're ready to move on to Design, or if anything needs adjustment."

Do not proceed to Design until the human explicitly approves or requests changes.

## Triggers for Escalation to the User

Stop the phase and surface to the user when:

- **`questions.md` contradicts itself on a load-bearing requirement** — likely upstream cause: Questions. Suggested user action: re-run Questions to resolve the contradiction.
- **A required source is inaccessible** (paywalled doc, missing private repo, dead link to a needed reference) — likely upstream cause: external constraint. Suggested user action: provide access, supply equivalent material, or accept the gap and let Design handle it.
- **Findings reveal that two stated requirements are mutually exclusive** — likely upstream cause: Questions (the conflict was not surfaced). Suggested user action: re-run Questions with the conflict named.
