# Revision Rules

Decision tree for handling review findings and managing the revision loop.

## Decision Tree

```
Review finds issue
        │
        ▼
  Is the issue minor?
        │
   ┌────┴────┐
   Yes       No
   │          │
   ▼          ▼
 Auto-fix   Is the issue major?
 in place   │
        ┌────┴────┐
        Yes       No
        │          │
        ▼          ▼
  Write to   Could this be
  review.md  a systemic
  with       problem?
  deferred   │
  status    ┌────┴────┐
             Yes       No
             │          │
             ▼          ▼
       Stop and   Write to
       escalate   review.md
       to user    as low
                  severity
```

## Minor → Auto-Fix

**Fix immediately without writing to review.md.**

Examples:
- Missing import
- Typo in string
- Wrong comparison operator (`=` vs `==`)
- Formatting/linting issues
- Missing closing bracket or quote
- Small error handling addition
- Wrong default parameter value

After auto-fixing, add to review.md under "Minor Fixes Applied" for transparency.

## Major → Defer to review.md

**Write to review.md with deferred status. Do not fix.**

Major issues require decisions beyond simple corrections:
- Logic changes that affect behavior
- Missing features that need implementation
- Architecture mismatches
- Security fixes that require design changes
- Test additions that need new test structure

Write each deferred issue with:
- Clear description of the problem
- Which file(s) need to change
- Recommended fix (but don't implement it)
- Why it was deferred (requires design decision, significant rework, etc.)

## Systemic → Stop and Escalate

**Stop. Explain to the user. Don't keep iterating.**

Systemic problems:
- The implementation is fundamentally broken (not just buggy)
- The design was wrong and the entire approach needs rethinking
- Fixing one issue consistently breaks three others
- Multiple high-severity issues across many files
- The codebase doesn't match the planned architecture in a fundamental way

When escalating:
1. Summarize the systemic problem clearly
2. Explain the evidence (what you've found)
3. Provide a recommendation (what needs to happen)
4. Ask the user how to proceed

Example:

> "Review found a systemic issue: the authentication implementation doesn't match the design.md specification. The design calls for JWT-based session auth with refresh tokens, but the implementation uses session cookies. This affects 12 files across the auth module, middleware, and API layer. I recommend either: (a) redesign to use session cookies and update design.md, or (b) rewrite the auth module to use JWT as designed. Which approach would you prefer?"

## The Revision Loop

### How It Works
1. Review runs → finds major issues → writes `review.md`
2. Human reviews `review.md` → approves revision
3. Implement runs with `review.md` as context → fixes deferred items
4. Review runs again → checks if deferred items are fixed
5. Repeat until clean or escalation

### How Implement Handles review.md
When the Implement phase is given `review.md` as context:
- Read `review.md` to understand deferred issues
- Read `plan.md` to understand what was originally planned
- Implement the fixes described in deferred items
- For each deferred item that is fixed, note it: "Fixed review.md item: [description]"
- If a deferred item can't be fixed as described, explain why and suggest an alternative
- Don't modify `review.md` — the Review phase owns it

### How Review Handles Previous review.md
When the Review phase finds an existing `review.md`:
- Read the previous review to see what was deferred
- Check if the deferred items have been addressed
- Re-review the previously problematic areas
- Update `review.md` with current status
- Remove fixed items from the deferred list
- Note any new issues

### When to Stop Iterating
- **Clean review**: review.md has no deferred major issues
- **Escalation**: Review identifies systemic problems
- **Stalemate**: The same issue keeps coming back after multiple fixes — stop and ask the user
- **After 3 iterations**: If after 3 Implement↔Review cycles there are still major issues, escalate to the user

## Writing review.md Entries

Good deferred issue entries:

```markdown
#### [High] User validation doesn't handle empty email
- **File:** `src/auth/models.py`, `src/auth/validators.py`
- **Location:** `validate_email()` function
- **What:** Empty string passes validation, should be rejected
- **Recommended Fix:** Add check for empty/whitespace-only email in `validate_email()`. Also add a test case.
- **Why deferred:** Requires changes to both the validator and the test suite
```

Bad deferred issue entries:

```markdown
#### [High] Something's wrong with validation
- **File:** somewhere in auth
- **What:** doesn't work right
- **Recommended Fix:** fix it
```

## review.md Structure

```markdown
# Review: [Project Name]
**Date:** [date]
**Assessment:** [Green/Yellow/Red]

## Summary
[Brief summary of the review findings]

## Minor Fixes Applied
- Fixed missing import in `src/utils.py`
- Fixed typo in error message

## Items Found

#### [High] Issue Title
- **File:** `src/file.py`
- **Location:** function_name(), line ~42
- **What:** Description of the problem
- **Recommended Fix:** What should be done
- **Status:** Deferred

## Deferred Issues

#### [Medium] Issue Title
- **File:** `src/file.py`
- **What:** Description of the problem
- **Recommended Fix:** What should be done
- **Why deferred:** Why this requires Implement-phase attention

## Escalation (if applicable)
If the review identifies systemic problems, document them here with a clear recommendation.
```
