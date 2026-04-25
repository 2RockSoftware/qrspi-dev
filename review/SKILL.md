---
name: qrspi-dev-review
description: Review completed work for correctness, completeness, quality, and security. Fix minor issues automatically; defer major issues. Use in the Review phase of QRSPI development. Produces review.md.
---

# Review Phase

Review the completed work for correctness, completeness, quality, and security. Produce a review document. Fix minor issues automatically; defer major issues and explain.

## How It Works

1. **Read `plan.md`** — understand what was supposed to be built
2. **Read `design.md`** — understand the intended architecture
3. **Read the implemented code** — understand what was actually built
4. **Review systematically** — check correctness, completeness, testing, security, quality
5. **Fix minor issues automatically** — typos, missing imports, trivial bugs
6. **Defer major issues** — write them to `review.md`
7. **Write `review.md`** — structured review with assessment and findings

## What to Review

### Correctness
- Does the code match the plan and design?
- Are the behaviors specified in tasks implemented correctly?
- Are there logical errors or incorrect implementations?
- Do the pieces fit together as designed?

### Completeness
- Are all plan tasks fulfilled?
- Are there TODOs left in skeleton files that should be implemented?
- Are there missing pieces that the plan implied?
- Is anything that should have been built left undone?

### Testing
- Are tests present for the implemented features?
- Are tests meaningful (testing real logic, not just coverage)?
- Do tests cover key edge cases?
- Do tests pass?

### Security
- Any obvious vulnerabilities (SQL injection, XSS, exposed secrets, etc.)?
- Proper input validation?
- Safe error handling (no stack traces exposed)?
- No hardcoded secrets or credentials?

### Code Quality
- Readability and naming
- Error handling completeness
- Edge case handling
- Consistent style and patterns
- No obvious code smells

### Tooling
- Does linting pass? Run the linter
- Does formatting pass? Run the formatter
- Are there obvious build/compile errors?
- Is the project setup correctly?

## Fixing vs. Deferring

### Auto-Fix (Minor Issues)
- Missing imports or typos
- Trivial bugs (off-by-one, wrong comparison operator)
- Missing error handling on obvious paths
- Small formatting/linting issues
- Missing comments for counter-intuitive code
- Small corrections that don't change behavior

### Defer to review.md (Major Issues)
- Logic errors that change behavior
- Missing features or incomplete implementations
- Security vulnerabilities
- Architecture mismatches with the design
- Missing tests for significant logic
- Performance problems
- Any issue that requires design-level decisions

### Stop and Escalate (Systemic Problems)
- The work is fundamentally broken and can't be fixed incrementally
- The design was wrong and the entire implementation needs rethinking
- Multiple interconnected failures suggest a cascade of issues
- The codebase is in a state where fixing one issue breaks three others

For systemic problems, stop and explain to the user with a clear recommendation.

## Output Artifact

Write `review.md` with:

### Overall Assessment
- **Green**: No major issues, minor fixes applied, ready for human review
- **Yellow**: Minor issues found and fixed, a few items to note, generally good
- **Red**: Major issues found, significant work needed before ready

### Minor Fixes Applied
List of issues you auto-fixed (for transparency):
```markdown
- Fixed missing `import os` in `src/utils.py`
- Fixed typo in error message: "User not found" → "User not found."
- Fixed linting issue: removed unused import in `src/models.py`
```

### Items Found
Each item with:
```markdown
#### [Severity] Issue Title
- **File:** `src/file.py`
- **Location:** Line numbers or function name
- **What:** Description of the problem
- **Recommended Fix:** What should be done
```

Severity levels:
- **High** — correctness, security, missing core functionality
- **Medium** — missing tests, edge cases, error handling
- **Low** — style, minor improvements, documentation

### Deferred Issues
Issues that were deferred to Implement (for the revision loop):
- Clear description of what needs to change
- Which file(s) need modification
- Why it was deferred (requires design decision, significant rework, etc.)

## Revision Loop

### If review.md Has Only Minor Issues (all auto-fixed)
> "Review complete. Minor issues were auto-fixed. `review.md` shows a green assessment — the work is ready for human review."

### If review.md Has Major Issues (deferred)
> "Review found {N} major issues. I've written `review.md` with detailed findings. The deferred issues need to be addressed before the work is ready. Let me know if you'd like me to hand these off to the Implement phase for revision."

The revision flow:
1. Review finds major issues → writes `review.md`
2. Human approves → Implement works on deferred items with `review.md` as context
3. Implement completes → back to Review
4. Repeat until review.md is clean or escalation is needed

### If Review Escalates (systemic problems)
> "Review has identified systemic issues that suggest the implementation needs significant rework. Here's what I found: [summary]. I recommend [recommendation]. Should I proceed with [option] or would you like to take a different approach?"

## Picking Up After a Previous Review

If `review.md` already exists from a prior review:

1. Read the existing `review.md` to see what was found before
2. Read the current state of the code to see if previous issues were fixed
3. Re-review the previously deferred items
4. Check for any new issues
5. Update `review.md` with the current assessment
