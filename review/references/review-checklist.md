# Review Checklist

The checklist is split into two tiers. Per-slice reviews run **Correctness Tier** only. The final review runs **Correctness + Thoroughness Tiers**.

## Correctness Tier (per-slice and final)

Focused on "does this work as specified."

### Plan & Design Adherence

- [ ] All `[x]` tasks in the slice's `plan.md` have corresponding code.
- [ ] Code matches the task descriptions.
- [ ] Code matches the architecture decisions in `design.md` (no silent re-architecting).
- [ ] No tasks left as `[ ]` without a documented reason.

### Tests Pass

- [ ] All verification commands from the plan pass cleanly.
- [ ] At least one end-to-end test exercises the slice's user-visible capability.
- [ ] No tests skipped without justification (e.g., `pytest.skip` with a TODO).
- [ ] No tests exist that pass without asserting anything real.

### Logic & Behavior

- [ ] Functions/methods implement the described behavior.
- [ ] Data flow is correct (inputs → processing → outputs).
- [ ] No obvious logic errors in algorithms or business logic.
- [ ] No incorrect assumptions about external dependencies.
- [ ] Error cases are handled (not just the happy path).
- [ ] Integration points work (the pieces fit together).

### Security (correctness-grade)

- [ ] No hardcoded secrets, API keys, credentials, or passwords.
- [ ] User input validated at boundaries.
- [ ] SQL queries parameterized (no injection).
- [ ] No XSS vulnerabilities in web contexts.
- [ ] Auth checks present where the design specified them.
- [ ] Error messages don't expose stack traces or internals to end users.
- [ ] Sensitive data not logged.

### Build & Tooling

- [ ] Linter passes.
- [ ] Formatter applied.
- [ ] Build/compile succeeds.
- [ ] Module imports resolve.
- [ ] No unused imports or obvious dead code introduced by this slice.

## Thoroughness Tier (final review only)

Focused on "is this code good." Not gated; findings are recommendations the user weighs.

### Test Coverage Quality

- [ ] Tests cover key edge cases, not just happy paths.
- [ ] Tests cover error/exception paths.
- [ ] Test setup/teardown is correct (fixtures, mocks, temp data).
- [ ] No "smoke test only" pattern — tests assert behavior, not just absence of crash.
- [ ] Coverage gaps in business logic are noted (even if not gating).

### Type Design & API Surface

- [ ] Type signatures express intent (no gratuitous `any` / `interface{}` / `Object`).
- [ ] Public APIs are minimal — internal helpers stay internal.
- [ ] Naming is consistent across modules.
- [ ] Exception types are meaningful, not just `Exception` / `Error`.

### Simplification Opportunities

- [ ] No dead code or unreachable branches.
- [ ] No premature abstractions for one-off cases.
- [ ] No over-engineered patterns where straightforward code would do.
- [ ] No duplicated logic that should be factored.

### Performance Considerations

- [ ] No N+1 queries in DB-touching code.
- [ ] No obvious quadratic loops over large inputs.
- [ ] Resource cleanup (file handles, connections, locks) is correct.
- [ ] No unbounded memory growth in long-running paths.

### Documentation & Clarity

- [ ] Public APIs have docstrings/jsdoc/etc. as appropriate to the language.
- [ ] Counter-intuitive code has a comment explaining *why*.
- [ ] README is present and reflects the current project state.
- [ ] Configuration is documented (env vars, config files, defaults).

### Cross-Cutting

- [ ] Error handling is consistent across modules.
- [ ] Logging is consistent (level, format, what's logged).
- [ ] Configuration values aren't duplicated across files.
- [ ] Module boundaries match the structure decided in `design.md`.

## Severity Classification

Severities apply across both tiers. Tag each tier item with the severity it tends to produce.

### High Severity
- Logic errors that produce wrong results.
- Security vulnerabilities.
- Missing core functionality from the plan.
- Crashes or panics in normal operation.
- Data corruption / loss risk.

### Medium Severity
- Missing tests for significant logic.
- Edge cases not handled.
- Poor error handling (crashes on expected failures).
- Performance issues that affect normal use.
- Architecture mismatches that aren't immediately broken but accumulate debt.

### Low Severity
- Style inconsistencies.
- Minor readability improvements.
- Missing comments for non-obvious code.
- Trivial documentation gaps.
- Suggested refactoring (nice-to-have).

## Tier × Mode Application

| Tier | Per-slice review | Final review |
|---|---|---|
| Correctness | Required | Required |
| Thoroughness | Skipped | Required |

The split keeps per-slice review fast (correctness only — does this slice work?) so autonomous flow stays moving, and reserves thoroughness for the end-of-flow review where it can see the whole project at once.
