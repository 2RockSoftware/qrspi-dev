# Review Checklist

Comprehensive checklist organized by category. Use this to ensure systematic review coverage.

## Correctness

- [ ] Code matches the plan's task descriptions
- [ ] Code matches the design document's architecture
- [ ] Functions/methods implement the described behavior
- [ ] Data flow is correct (inputs → processing → outputs)
- [ ] No logical errors in algorithms or business logic
- [ ] No incorrect assumptions about external dependencies
- [ ] Error cases are handled (not just the happy path)
- [ ] The pieces fit together (integration points work)

## Completeness

- [ ] All `[x]` tasks in plan.md have corresponding code
- [ ] No TODOs remain in non-skeleton files that should be implemented
- [ ] All files referenced in the plan exist
- [ ] No placeholder functions left unimplemented
- [ ] README is present (even if basic)
- [ ] Configuration files are present and correct
- [ ] Any features mentioned in outline.md or design.md are implemented

## Testing

- [ ] Tests exist for core logic (not just infrastructure code)
- [ ] Tests cover the happy path
- [ ] Tests cover key edge cases
- [ ] Tests cover error/exception paths
- [ ] Tests are meaningful (assert something real, not just "no crash")
- [ ] Tests pass when run
- [ ] Test setup/teardown is correct (fixtures, mocks, temp data)
- [ ] No tests are skipped without justification

## Security

- [ ] No hardcoded secrets, API keys, or credentials
- [ ] No hardcoded passwords or tokens
- [ ] Input is validated before use
- [ ] SQL queries use parameterized queries (no injection)
- [ ] User input is sanitized (no XSS in web contexts)
- [ ] Authentication is implemented where required by plan
- [ ] Authorization is checked at appropriate boundaries
- [ ] Error messages don't expose stack traces or internals
- [ ] Sensitive data is not logged
- [ ] Dependencies are up to date (no known critical CVEs)

## Code Quality

### Readability
- [ ] Variable and function names are meaningful
- [ ] Code is organized logically
- [ ] No excessively long functions (> 50 lines)
- [ ] No excessively long files (> 500 lines without clear justification)
- [ ] Consistent style throughout

### Error Handling
- [ ] Errors are caught at appropriate boundaries
- [ ] Errors are logged or reported (not swallowed)
- [ ] User-facing errors have helpful messages
- [ ] No bare `except:` / `catch (e)` without handling

### Edge Cases
- [ ] Empty inputs are handled
- [ ] Null/None values are handled
- [ ] Large inputs don't cause issues
- [ ] Concurrent access is handled (if applicable)
- [ ] Timeouts and retries are handled (if applicable)

## Tooling

- [ ] Linter runs without errors
- [ ] Formatter applied consistently
- [ ] Build compiles without errors
- [ ] No unused imports or dead code
- [ ] No obvious console.log / print statements left in production code
- [ ] .gitignore is appropriate for the project
- [ ] Dependencies are listed in the package manifest

## Integration

- [ ] Module imports resolve correctly
- [ ] Cross-module references are correct
- [ ] API endpoints match the design specification
- [ ] Database schema matches the model definitions
- [ ] Configuration values are used correctly
- [ ] External service calls have proper error handling

## Severity Classification

Use this to classify found issues:

### High Severity
- Logic errors that produce wrong results
- Security vulnerabilities
- Missing core functionality from the plan
- Crashes or panics in normal operation
- Data corruption or loss risk

### Medium Severity
- Missing tests for significant logic
- Edge cases not handled
- Poor error handling (crashes on expected failures)
- Missing documentation that would help future developers
- Performance issues (noticesable slowness)

### Low Severity
- Style inconsistencies
- Minor improvements to readability
- Missing comments for non-obvious code
- Trivial documentation improvements
- Suggested refactoring (nice-to-have, not required)
