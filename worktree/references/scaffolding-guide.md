# Scaffolding Guide

Reference for scaffolding projects across common ecosystems.

## Ecosystem Detection

Determine the ecosystem from the plan and design documents. Look for:

- Programming language mentions (Python, TypeScript, Rust, Go, etc.)
- Framework mentions (Django, FastAPI, Express, Next.js, etc.)
- Package manager mentions (pip, npm, cargo, go mod, etc.)
- Technology stack decisions in design.md

## Skeleton File Patterns

### Python

**Module skeleton:**
```python
"""Module: {description}.

TODO: Implement {purpose}.
"""

# TODO: Add imports
# TODO: Add module-level constants
# TODO: Add utility functions
```

**Class skeleton:**
```python
class {ClassName}:
    """{ClassName} handles {responsibility}.
    
    TODO: Implement {key behaviors}.
    """
    
    def __init__(self, ...):
        """Initialize {ClassName}.
        
        TODO: Set up {initialization needs}.
        """
        # TODO: Initialize instance attributes
        pass
    
    def {method_name}(self, ...):
        """{What the method does}.
        
        TODO: Implement {behavior}.
        
        Args:
            ...: {description}
        
        Returns:
            ...: {description}
        
        Raises:
            ...: {when and why}
        """
        # TODO: Implement
        pass
```

**Function skeleton:**
```python
def {function_name}(...):
    """{What the function does}.
    
    TODO: Implement {logic}.
    
    Args:
        ...: {description}
    
    Returns:
        ...: {description}
    
    Raises:
        ...: {when and why}
    """
    # TODO: Implement
    pass
```

**Data model skeleton (SQLAlchemy):**
```python
from sqlalchemy import Column, String
from sqlalchemy.orm import DeclarativeBase

class Base(DeclarativeBase):
    pass

class {ModelName}(Base):
    """TODO: Describe model purpose."""
    
    __tablename__ = "{table_name}"
    
    # TODO: Define columns
    id = Column(..., primary_key=True)
    # TODO: Add remaining fields
    
    # TODO: Add relationships
    # TODO: Add validation
    # TODO: Add helper methods
```

### TypeScript/JavaScript

**Module skeleton:**
```typescript
/**
 * {Module description}.
 *
 * TODO: Implement {purpose}.
 */

// TODO: Add imports
// TODO: Add types and interfaces
// TODO: Add exports
```

**Function skeleton:**
```typescript
/**
 * {Function description}.
 *
 * TODO: Implement {behavior}.
 *
 * @param {params} - {parameter descriptions}
 * @returns {return description}
 */
export function {functionName}({params}): {ReturnType} {
    // TODO: Implement
    throw new Error('Not implemented');
}
```

**Component skeleton (React):**
```typescript
/**
 * {Component description}.
 *
 * TODO: Implement {functionality}.
 */
export function {ComponentName}({props}): JSX.Element {
    // TODO: Implement component logic
    // TODO: Add state management
    // TODO: Add event handlers
    return (
        // TODO: Implement UI
        <div>{/* TODO: Render content */}</div>
    );
}
```

**Service/API client skeleton:**
```typescript
/**
 * {Service description}.
 *
 * TODO: Implement {capabilities}.
 */
export class {ServiceName} {
    // TODO: Add dependencies
    // TODO: Add state
    
    /**
     * TODO: Describe method purpose.
     */
    async {methodName}({params}): Promise<{ReturnType}> {
        // TODO: Implement
        throw new Error('Not implemented');
    }
}
```

### Rust

**Module skeleton:**
```rust
//! {Module description}
//!
//! TODO: {module purpose}

// TODO: Add imports
// TODO: Add types
// TODO: Add exports (pub mod, pub use)
```

**Struct skeleton:**
```rust
/// {Struct description}.
///
/// TODO: {purpose}
pub struct {StructName} {
    // TODO: Add fields
}

impl {StructName} {
    /// Creates a new {StructName}.
    ///
    /// TODO: Implement initialization.
    pub fn new(...) -> Self {
        // TODO: Implement
        todo!()
    }
    
    /// TODO: Describe method purpose.
    pub fn {method_name}(&self, ...) -> {ReturnType} {
        // TODO: Implement
        todo!()
    }
}
```

### Go

**Package skeleton:**
```go
// Package {package} provides {description}.
//
// TODO: {package purpose}

package {package}

// TODO: Add imports
// TODO: Add types
// TODO: Add exports
```

**Struct skeleton:**
```go
// {TypeName} {description}.
//
// TODO: {purpose}
type {TypeName} struct {
    // TODO: Add fields
}

// New{TypeName} creates a new instance.
// TODO: Implement initialization.
func New{TypeName}(...) *{TypeName} {
    // TODO: Implement
    return nil
}

// {MethodName} {description}.
// TODO: Implement.
func (s *{TypeName}) {MethodName}(...) {
    // TODO: Implement
}
```

## Tooling Setup Checklist

### Python
- [ ] `requirements.txt` or `pyproject.toml` with dependencies
- [ ] Virtual environment instructions
- [ ] Linter: ruff, pylint, or flake8
- [ ] Formatter: black or autopep8
- [ ] Type checker: mypy (optional)
- [ ] Test framework: pytest
- [ ] `.gitignore` for Python
- [ ] Pre-commit hooks (optional, recommended)

### Node.js/TypeScript
- [ ] `package.json` with dependencies and scripts
- [ ] Linter: ESLint
- [ ] Formatter: Prettier
- [ ] Test framework: Jest, Vitest, or similar
- [ ] TypeScript config: `tsconfig.json`
- [ ] `.gitignore` for Node.js
- [ ] Pre-commit hooks (optional)

### Rust
- [ ] `Cargo.toml` with dependencies
- [ ] Dev dependencies: `cargo-dev`, `clippy`
- [ ] Formatter: `rustfmt` (included with rustc)
- [ ] Test framework: built-in `cargo test`
- [ ] `.gitignore` for Rust
- [ ] Pre-commit hooks (optional)

### Go
- [ ] `go.mod` with dependencies
- [ ] Linter: `golangci-lint`
- [ ] Formatter: `gofmt` (built-in)
- [ ] Test framework: built-in `go test`
- [ ] `.gitignore` for Go
- [ ] Pre-commit hooks (optional)
