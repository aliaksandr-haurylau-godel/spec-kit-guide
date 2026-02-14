---
version: 1
source_date: 2026-02-12
derived_from: Synthesized from raw_data/2026-02-12-spec-kit-official-docs/spec-driven.md and Taskify examples
---

# Constitution Example: Taskify

This document shows how the nine standard articles of the Spec-Kit constitution apply to the Taskify project (a team productivity platform).

## Preamble

Taskify is a "Security-First" application focused on team productivity. This constitution governs all development decisions to ensure consistency, quality, and architectural integrity.

---

## Article I: Library-First Principle

**Standard Text:**
> Every feature in Specify MUST begin its existence as a standalone library. No feature shall be implemented directly within application code without first being abstracted into a reusable library component.

**Taskify Implementation:**

All Taskify features are implemented as libraries first:

```text
lib/
├── taskify-core/          # Core domain logic (projects, tasks, users)
│   ├── entities/
│   ├── services/
│   └── cli.js            # CLI interface for library
├── taskify-kanban/        # Kanban board functionality
│   ├── board-manager.js
│   └── cli.js
└── taskify-comments/      # Comment system
    ├── comment-service.js
    └── cli.js
```

**Example:**
- ❌ BAD: Embed task creation directly in Blazor component
- ✅ GOOD: Create `taskify-core` library, expose via CLI, consume in Blazor

---

## Article II: CLI Interface Mandate

**Standard Text:**
> All CLI interfaces MUST: Accept text as input (via stdin, arguments, or files), Produce text as output (via stdout), Support JSON format for structured data exchange.

**Taskify Implementation:**

Every Taskify library exposes CLI commands:

```bash
# taskify-core library CLI
$ taskify-core create-project --name "Website Redesign" --owner "PM-001"
{"id": "proj-001", "name": "Website Redesign", "owner": "PM-001", "created": "2026-02-13"}

# taskify-kanban library CLI
$ taskify-kanban move-task --task-id "task-042" --from "To Do" --to "In Progress"
{"success": true, "task": "task-042", "status": "In Progress"}

# taskify-comments library CLI
$ taskify-comments add --task-id "task-042" --user "ENG-002" --text "Working on API integration"
{"id": "cmt-123", "task": "task-042", "user": "ENG-002", "timestamp": "2026-02-13T10:30:00Z"}
```

**Why This Matters:**
- Testing without UI
- Automation and scripting
- Integration with other tools

---

## Article III: Test-First Imperative

**Standard Text:**
> This is NON-NEGOTIABLE: All implementation MUST follow strict Test-Driven Development. No implementation code shall be written before: 1. Unit tests are written, 2. Tests are validated and approved by the user, 3. Tests are confirmed to FAIL (Red phase).

**Taskify Implementation:**

**Process for adding "Drag and Drop" feature:**

```javascript
// STEP 1: Write test FIRST (Red phase)
describe('KanbanBoard', () => {
  test('moveTask updates task status', () => {
    const board = new KanbanBoard();
    board.addTask({id: 'task-1', status: 'To Do'});
    
    board.moveTask('task-1', 'In Progress');
    
    expect(board.getTask('task-1').status).toBe('In Progress');
  });
});

// STEP 2: Run test - VERIFY IT FAILS ✗
// Output: ReferenceError: KanbanBoard is not defined

// STEP 3: Write minimal implementation (Green phase)
class KanbanBoard {
  constructor() { this.tasks = {}; }
  addTask(task) { this.tasks[task.id] = task; }
  moveTask(id, newStatus) { this.tasks[id].status = newStatus; }
  getTask(id) { return this.tasks[id]; }
}

// STEP 4: Run test - VERIFY IT PASSES ✓
```

---

## Article IV: Documentation Principle

**Taskify Implementation:**

All public functions documented with JSDoc/XML Doc:

```csharp
/// <summary>
/// Moves a task from one Kanban column to another.
/// </summary>
/// <param name="taskId">Unique identifier of the task</param>
/// <param name="fromStatus">Current status column</param>
/// <param name="toStatus">Target status column</param>
/// <returns>Updated task object</returns>
/// <exception cref="TaskNotFoundException">Thrown when task doesn't exist</exception>
public Task MoveTask(string taskId, string fromStatus, string toStatus) { ... }
```

---

## Article V: Versioning Principle

**Taskify Implementation:**

Semantic versioning for all libraries:

```json
{
  "name": "taskify-core",
  "version": "1.2.0",
  "changelog": {
    "1.2.0": "Added multi-project support",
    "1.1.0": "Added comment editing",
    "1.0.0": "Initial release"
  }
}
```

Breaking changes require major version bump and migration guide.

---

## Article VI: Dependency Management

**Taskify Implementation:**

Minimal dependencies, framework-first:

```json
{
  "dependencies": {
    "postgres": "^3.4.0",        // Database driver (essential)
    "signalr": "^8.0.0"          // Real-time updates (essential)
  },
  "devDependencies": {
    "jest": "^29.0.0",           // Testing (essential)
    "typescript": "^5.0.0"       // Type safety (essential)
  }
}
```

**Rejected dependencies:**
- ❌ `lodash` - Use native ES6 methods instead
- ❌ `moment` - Use native `Date` and `Intl` instead
- ❌ Custom DB wrapper - Use Postgres driver directly

---

## Article VII: Simplicity Principle

**Standard Text:**
> Section 7.3: Minimal Project Structure - Maximum 3 projects for initial implementation. Additional projects require documented justification.

**Taskify Implementation:**

**Initial Structure (3 projects max):**

```text
1. taskify-core/        # Domain logic, entities, services
2. taskify-api/         # REST API + WebSocket server
3. taskify-web/         # Blazor frontend
```

**Rejected over-engineering:**
- ❌ Separate projects for: users, projects, tasks, comments (4 extra projects)
- ❌ Shared abstractions project
- ❌ Separate API gateway project

**Why 3 is enough:**
- Core logic isolated for reuse
- API provides interface
- Web provides UI
- All concerns addressed without unnecessary splitting

---

## Article VIII: Anti-Abstraction Principle

**Standard Text:**
> Section 8.1: Framework Trust - Use framework features directly rather than wrapping them.

**Taskify Implementation:**

**❌ BAD (Unnecessary abstraction):**
```csharp
// Custom wrapper around Entity Framework
public class TaskifyDbContext : ITaskifyDatabase {
  private readonly DbContext _context;
  public void Save<T>(T entity) { _context.Set<T>().Add(entity); }
  public T Get<T>(int id) { return _context.Set<T>().Find(id); }
}
```

**✅ GOOD (Use Entity Framework directly):**
```csharp
// Use DbContext directly
public class TaskService {
  private readonly TaskifyDbContext _db;
  
  public Task CreateTask(string name) {
    var task = new Task { Name = name };
    _db.Tasks.Add(task);        // Direct EF Core method
    _db.SaveChanges();           // Direct EF Core method
    return task;
  }
}
```

---

## Article IX: Integration-First Testing

**Standard Text:**
> Tests MUST use realistic environments: Prefer real databases over mocks, Use actual service instances over stubs, Contract tests mandatory before implementation.

**Taskify Implementation:**

**✅ GOOD (Real Postgres):**
```csharp
[Test]
public async Task CreateTask_SavesToDatabase() {
  // Use real test database
  var testDb = new TaskifyDbContext(TestDbConnectionString);
  var service = new TaskService(testDb);
  
  var task = await service.CreateTask("Implement login");
  
  // Query actual database
  var saved = await testDb.Tasks.FindAsync(task.Id);
  Assert.NotNull(saved);
  Assert.Equal("Implement login", saved.Name);
}
```

**❌ BAD (Over-mocked):**
```csharp
[Test]
public void CreateTask_CallsSaveChanges() {
  var mockDb = new Mock<IDatabase>();
  var service = new TaskService(mockDb.Object);
  
  service.CreateTask("Implement login");
  
  mockDb.Verify(db => db.SaveChanges(), Times.Once);
  // This tests the mock, not the real behavior!
}
```

**Contract Tests (Before implementation):**
```javascript
// Define API contract test BEFORE implementing endpoint
describe('POST /api/tasks', () => {
  test('returns 201 with task ID', async () => {
    const response = await fetch('/api/tasks', {
      method: 'POST',
      body: JSON.stringify({ name: 'Test task' })
    });
    
    expect(response.status).toBe(201);
    expect(response.json()).toHaveProperty('id');
  });
});
```

---

## Phase -1 Gates (Constitutional Compliance)

Before implementation begins, verify:

### Simplicity Gate (Article VII)
- [x] Using ≤3 projects (core, api, web)
- [x] No future-proofing (no "we might need" features)

### Anti-Abstraction Gate (Article VIII)
- [x] Using Entity Framework directly (no custom wrappers)
- [x] Using SignalR directly (no custom abstractions)
- [x] Single model representation (Task class, not TaskDTO + TaskEntity + TaskModel)

### Integration-First Gate (Article IX)
- [x] Contracts defined (`contracts/api-spec.json`, `contracts/websocket-spec.md`)
- [x] Contract tests written before implementation
- [x] Test database configured (Postgres in Docker)

---

## Complexity Tracking

If constitutional rules are violated, document here:

**None.** All implementation follows constitutional principles.

---

## Amendment History

**Version 1.0** (2026-02-12): Initial constitution for Taskify project.

---

## See Also

- [Constitution Reference](../docs/constitution-reference.md) - Detailed article explanations
- [Spec Example (Taskify)](spec-example-taskify.md) - Taskify specification
- [Plan Example (Taskify)](plan-example-taskify.md) - Taskify implementation plan
