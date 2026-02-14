---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/spec-driven.md (lines 274-406)
---

# Constitution Reference

The constitution (`memory/constitution.md`) acts as the architectural DNA of Spec-Kit projects, ensuring that every generated implementation maintains consistency, simplicity, and quality. These nine articles govern how specifications become code.

## The Nine Articles

### Article I: Library-First Principle

**Full Text:**
```text
Every feature in Specify MUST begin its existence as a standalone library.
No feature shall be implemented directly within application code without
first being abstracted into a reusable library component.
```

**Purpose:** Forces modular design from the start. Ensures specifications generate modular, reusable code rather than monolithic applications.

**Example:**

```javascript
// ❌ BAD: Feature embedded in application
app.post('/users', (req, res) => {
  // User creation logic here...
});

// ✅ GOOD: Feature as standalone library
import { UserLibrary } from './lib/users';
app.post('/users', UserLibrary.createUser);
```

---

### Article II: CLI Interface Mandate

**Full Text:**
```text
All CLI interfaces MUST:
- Accept text as input (via stdin, arguments, or files)
- Produce text as output (via stdout)
- Support JSON format for structured data exchange
```

**Purpose:** Enforces observability and testability. Everything must be accessible and verifiable through text-based interfaces.

**Example:**

```bash
# ❌ BAD: Hidden functionality
# (no way to test/verify)

# ✅ GOOD: Text-based CLI interface
$ ./lib/users create --name "Alice" --email "alice@example.com"
{"id": 1, "name": "Alice", "email": "alice@example.com"}
```

---

### Article III: Test-First Imperative

**Full Text:**
```text
This is NON-NEGOTIABLE: All implementation MUST follow strict Test-Driven Development.
No implementation code shall be written before:
1. Unit tests are written
2. Tests are validated and approved by the user
3. Tests are confirmed to FAIL (Red phase)
```

**Purpose:** Completely inverts traditional AI code generation. Tests define behavior before implementation.

**Example:**

```javascript
// ✅ STEP 1: Write test FIRST
test('createUser returns user with ID', () => {
  const user = createUser('Alice');
  expect(user.id).toBeDefined();
});

// STEP 2: Run test - VERIFY IT FAILS

// ✅ STEP 3: Write implementation to pass test
function createUser(name) {
  return { id: generateId(), name };
}
```

---

### Article IV: [Documentation Principle]

**Note:** Full text not provided in official docs. Typically covers documentation requirements for libraries and APIs.

**Example:**

```javascript
// ✅ GOOD: Documented function
/**
 * Creates a new user in the system.
 * @param {string} name - User's full name
 * @returns {User} Created user object with ID
 */
function createUser(name) { ... }
```

---

### Article V: [Versioning Principle]

**Note:** Full text not provided in official docs. Typically covers semantic versioning and compatibility requirements.

**Example:**

```json
{
  "version": "1.2.0",
  "breaking_changes": false
}
```

---

### Article VI: [Dependency Management]

**Note:** Full text not provided in official docs. Typically covers how to manage external dependencies.

**Example:**

```javascript
// ✅ GOOD: Minimal dependencies
import { db } from 'postgres'; // Direct use

// ❌ BAD: Unnecessary wrapper
import { MyCustomDbWrapper } from './wrappers';
```

---

### Article VII: Simplicity Principle

**Full Text:**
```text
Section 7.3: Minimal Project Structure
- Maximum 3 projects for initial implementation
- Additional projects require documented justification
```

**Purpose:** Combats over-engineering. Forces justification for every layer of complexity.

**Example:**

```text
// ❌ BAD: Over-engineered structure
projects/
  user-api/
  user-service/
  user-repository/
  user-models/
  user-validators/

// ✅ GOOD: Simple structure (≤3 projects)
projects/
  user-lib/
  user-api/
  user-tests/
```

---

### Article VIII: Anti-Abstraction Principle

**Full Text:**
```text
Section 8.1: Framework Trust
- Use framework features directly rather than wrapping them
```

**Purpose:** Prevents unnecessary abstraction layers. Trust the framework.

**Example:**

```javascript
// ❌ BAD: Unnecessary wrapper
class MyExpressWrapper {
  route(path, handler) {
    this.app.get(path, handler);
  }
}

// ✅ GOOD: Use framework directly
app.get('/users', usersHandler);
```

---

### Article IX: Integration-First Testing

**Full Text:**
```text
Tests MUST use realistic environments:
- Prefer real databases over mocks
- Use actual service instances over stubs
- Contract tests mandatory before implementation
```

**Purpose:** Ensures generated code works in practice, not just in theory.

**Example:**

```javascript
// ❌ BAD: Over-mocked test
test('createUser', () => {
  const mockDb = { insert: jest.fn() };
  createUser('Alice', mockDb);
});

// ✅ GOOD: Real database test
test('createUser', async () => {
  const testDb = await setupTestDatabase();
  const user = await createUser('Alice', testDb);
  expect(await testDb.findById(user.id)).toBeDefined();
});
```

---

## Constitutional Enforcement Through Templates

The implementation plan template operationalizes these articles through concrete checkpoints called **Phase -1: Pre-Implementation Gates**:

### Simplicity Gate (Article VII)
- [ ] Using ≤3 projects?
- [ ] No future-proofing?

### Anti-Abstraction Gate (Article VIII)
- [ ] Using framework directly?
- [ ] Single model representation?

### Integration-First Gate (Article IX)
- [ ] Contracts defined?
- [ ] Contract tests written?

These gates act as compile-time checks for architectural principles. The AI cannot proceed without either passing the gates or documenting justified exceptions in the "Complexity Tracking" section.

## The Power of Immutable Principles

The constitution's power lies in its immutability. While implementation details can evolve, the core principles remain constant. This provides:

1. **Consistency Across Time** - Code generated today follows the same principles as code generated next year
2. **Consistency Across LLMs** - Different AI models produce architecturally compatible code
3. **Architectural Integrity** - Every feature reinforces rather than undermines the system design
4. **Quality Guarantees** - Test-first, library-first, and simplicity principles ensure maintainable code

## Amendment Process

While principles are immutable, their application can evolve:

```text
Section 4.2: Amendment Process
Modifications to this constitution require:
- Explicit documentation of the rationale for change
- Review and approval by project maintainers
- Backwards compatibility assessment
```

This allows the methodology to learn and improve while maintaining stability. The constitution can show its own evolution with dated amendments, demonstrating how principles are refined based on real-world experience.

## Beyond Rules: A Development Philosophy

The constitution isn't just a rulebook—it's a philosophy that shapes how AI thinks about code generation:

- **Observability Over Opacity** - Everything must be inspectable through CLI interfaces
- **Simplicity Over Cleverness** - Start simple, add complexity only when proven necessary
- **Integration Over Isolation** - Test in real environments, not artificial ones
- **Modularity Over Monoliths** - Every feature is a library with clear boundaries

By embedding these principles into the specification and planning process, SDD ensures that generated code isn't just functional—it's maintainable, testable, and architecturally sound.

## See Also

- [Spec-Driven Development](spec-driven-development.md) - Core methodology
- [Constitutional Gates](../tricks/constitutional-gates.md) - Phase -1 enforcement
- [Avoiding Over-Engineering](../tricks/avoiding-over-engineering.md) - Articles VII & VIII in practice
- [Constitution Example (Taskify)](../artifacts/constitution-example.md) - All 9 articles applied
