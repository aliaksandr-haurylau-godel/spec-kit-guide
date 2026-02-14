---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/README.md (lines 92-108)
---

# /speckit.constitution

## Purpose

Creates or updates your project's governing principles and development guidelines. The constitution acts as the "architectural DNA" of your project, ensuring consistency across all generated code.

## When to Use

**First step** in any new Spec-Kit project, before creating specifications. Use it to establish:
- Development principles (e.g., "Library-First", "Test-Driven Development")
- Quality standards (code coverage, documentation requirements)
- Architectural constraints (project count limits, abstraction rules)
- Security and performance requirements

## Prerequisites

- Spec-Kit initialized in your project (`specify init`)
- Clear understanding of your project's quality standards and principles

## Input Format

```bash
/speckit.constitution <your-principles-description>
```

Provide a natural language description of your project's principles and guidelines.

## Output

**File Created:**
- `.specify/memory/constitution.md` - Your project's constitutional document

**Content Structure:**
The generated constitution typically includes nine articles (see [Constitution Reference](../docs/constitution-reference.md)):
1. Library-First Principle
2. CLI Interface Mandate
3. Test-First Imperative
4. Documentation Requirements
5. Versioning Standards
6. Dependency Management
7. Simplicity Principle
8. Anti-Abstraction Principle
9. Integration-First Testing

## Example

```markdown
/speckit.constitution Create principles focused on code quality, testing standards, user experience consistency, and performance requirements. Include governance for how these principles should guide technical decisions and implementation choices.
```

**Taskify Example (Security-First):**

```markdown
/speckit.constitution Taskify is a "Security-First" application. All user inputs must be validated. We use a microservices architecture with a maximum of 3 projects for the initial implementation. Code must follow strict TDD with tests written before implementation. We prefer functional programming patterns. All APIs must be documented. Real-time updates should use WebSockets or similar push technology. No unnecessary abstractions—use frameworks directly. Prefer real databases in tests over mocks.
```

## What Happens

1. AI reads your principles description
2. Generates a comprehensive constitution document with:
   - Project-specific interpretations of the nine standard articles
   - Custom principles based on your requirements
   - Enforcement guidelines for each principle
3. Saves to `.specify/memory/constitution.md`
4. AI will reference this constitution during all subsequent `/speckit.*` commands

## Related Commands

- `/speckit.specify` - Next step: Create specifications (uses constitution for guidance)
- `/speckit.plan` - Creates plans that comply with constitutional principles
- `/speckit.analyze` - Validates implementations against constitution

## See Also

- [Constitution Reference](../docs/constitution-reference.md) - Detailed explanation of nine articles
- [Constitution Example (Taskify)](../artifacts/constitution-example.md) - Complete example
- [Basic Workflow Step 2](../workflows/basic-workflow.md#step-2-define-your-constitution) - Constitution in context
- [Constitutional Gates](../tricks/constitutional-gates.md) - How gates enforce principles
