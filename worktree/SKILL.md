---
name: qrspi-dev-worktree
description: Scaffold directory structure and set up dev tooling. Creates skeleton files and configures linting, formatting, and testing. Use in the Worktree phase of QRSPI development.
---

# Worktree Phase

Materialize the directory structure from the Outline and set up dev tooling. Creates skeleton files and configures the development environment so the Implement phase has a concrete workspace.

## How It Works

1. **Read `outline.md`** — understand the intended structure
2. **Read `plan.md`** — understand the Worktree Guidance section and the overall plan
3. **Detect the language/ecosystem** from the tech stack in the plan
4. **Scaffold the structure** — create directories and skeleton files
5. **Set up dev tooling** — project initialization, linting, formatting, testing, git
6. **Output:** A real directory tree with skeleton files and configured tooling

## Scaffolding

### Create Directory Structure
- Create all directories from the outline
- Follow the module boundaries described in the outline
- Use the naming conventions from the plan

### Create Skeleton Files
For each module/component, create a skeleton file containing:

**Python modules:**
```python
"""Module description."""
# TODO: Implement {module purpose}
```

**Python classes:**
```python
class ClassName:
    """Class description.
    
    TODO: Implement {responsibilities}
    """
    
    def method(self):
        """Method description.
        
        TODO: Implement {behavior}
        """
        pass
```

**TypeScript/JavaScript modules:**
```typescript
/** Module description.
 * TODO: Implement {module purpose}
 */

export function functionName(): ReturnType {
    // TODO: Implement
    return undefined as any;
}
```

**Component shells (web):**
```typescript
export function ComponentName() {
    // TODO: Implement component
    return null;
}
```

Skeleton files contain:
- Module/class/function signatures
- TODO comments indicating what needs to be implemented
- Minimal, valid code that won't crash if imported
- **Not implementation** — just the shape of what's needed

### README Placeholder
Create a basic README.md if one doesn't exist:
```markdown
# Project Name

[Project description from outline.md]

## Setup
[Will be filled in by tooling setup]

## Usage
[To be determined]
```

## Dev Tooling Setup

### Project Initialization
Initialize the project based on the detected ecosystem:

| Ecosystem | Initialization Command |
|-----------|----------------------|
| Python | `pip` + `requirements.txt` or `pyproject.toml` |
| Node.js | `npm init` + `package.json` |
| Rust | `cargo init` or `cargo new` |
| Go | `go mod init` |
| Ruby | `Gemfile` + bundler setup |
| Java | `pom.xml` (Maven) or `build.gradle` (Gradle) |

### Essential Tooling (configure per ecosystem)

**Language:**
- Linter (ruff, eslint, clippy, go vet, etc.)
- Formatter (black, prettier, gofmt, etc.)

**Testing:**
- Test framework (pytest, jest, cargo test, etc.)
- Basic test configuration

**Git:**
- `.gitignore` appropriate for the ecosystem and tooling
- Initialize git repo if not already done

**Config Files:**
- Linter config (e.g., `pyproject.toml` for ruff, `.eslintrc`, etc.)
- Formatter config (e.g., `.prettierrc`, `pyproject.toml` for black, etc.)
- Test config (e.g., `pytest.ini`, `jest.config.js`, etc.)
- Build system config if applicable

### .gitignore Templates

**Python:**
```
__pycache__/
*.py[cod]
*.egg-info/
dist/
build/
.venv/
venv/
*.egg
.pytest_cache/
.mypy_cache/
```

**Node.js:**
```
node_modules/
dist/
build/
*.log
.env
.env.*
coverage/
.nyc_output/
```

**Rust:**
```
target/
**/*.rs.bk
Cargo.lock
```

**Go:**
```
vendor/
*.exe
*.exe~
*.test
*.out
```

## No Human Checkpoint

After scaffolding and tooling are complete, **automatically proceed to the next phase**. The skeleton files and configured tooling are the output — there's no human review gate here.

Say something like:

> "Worktree phase is complete. I've scaffolded the directory structure, created skeleton files, and set up dev tooling (linter, formatter, test framework, git). The workspace is ready for implementation. Moving on."

Then automatically transition to the Implement phase.
