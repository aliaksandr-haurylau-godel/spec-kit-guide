---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/README.md (lines 127-134, 600-616)
---

# /speckit.implement

## Purpose

Executes all tasks from your task breakdown to build the feature according to the plan. Follows test-driven development, respects dependencies, and provides progress updates.

## When to Use

**Final step** in the workflow, after generating tasks with `/speckit.tasks`. Use this command when you're ready to:
- Generate working code from specifications
- Execute tasks in correct order
- Follow TDD (tests before implementation)
- Build the feature incrementally by user story

## Prerequisites

- Tasks generated with `/speckit.tasks`
- Active feature branch (or `SPECIFY_FEATURE` set)
- `specs/[feature]/tasks.md` exists
- All required development tools installed (compilers, runtimes, CLI tools)
- Constitution, spec, and plan in place

## Input Format

```bash
/speckit.implement
```

No arguments needed. The command reads from `tasks.md` automatically.

## Output

**What Gets Created:**
- Source code files (as specified in `tasks.md`)
- Test files (TDD structure)
- Database migrations
- Configuration files
- Build artifacts

**Progress Updates:**
- Task completion notifications
- Test execution results
- Error messages (if any)
- Checkpoint validation results

## Example

```markdown
/speckit.implement
```

## What Happens

1. **Prerequisites Validation**: Checks that all required files exist:
   - `.specify/memory/constitution.md`
   - `specs/[feature]/spec.md`
   - `specs/[feature]/plan.md`
   - `specs/[feature]/tasks.md`

2. **Task Parsing**: Reads `tasks.md` and extracts:
   - Task list in order
   - Parallel execution groups (tasks marked `[P]`)
   - File paths for each task
   - Checkpoint validations

3. **Execution Loop**: For each user story phase:
   - **Tests First** (if TDD): Creates test files, verifies they FAIL (Red phase)
   - **Implementation**: Creates source files to make tests pass
   - **Parallel Tasks**: Executes independent tasks simultaneously
   - **Sequential Tasks**: Respects dependencies (models → services → endpoints)
   - **Checkpoints**: Validates functionality after each phase

4. **Progress Reporting**: Provides updates:
   ```text
   ✓ Task 1: Create User model [COMPLETE]
   ✓ Task 2: Create Project model [COMPLETE]
   ⧗ Task 3: Create database migrations [IN PROGRESS]
   ```

5. **Error Handling**: If errors occur:
   - Pauses execution
   - Reports error details
   - Awaits user input to fix and continue

## Execution Flow Example

For a Taskify feature:

```text
Phase 1: Data Models (Parallel)
  ✓ Create User model - models/user.cs
  ✓ Create Project model - models/project.cs
  ✓ Create Task model - models/task.cs

Phase 2: Database Setup
  ✓ Create migrations - migrations/001_initial.sql
  ✓ Seed data - migrations/002_seed.sql
  ⧗ Running migrations...
  ✓ Database ready

Phase 3: Tests (TDD)
  ✓ Write User API tests - tests/user-api.test.cs
  ✓ Verify tests FAIL (Red phase) ✗ [Expected]

Phase 4: API Implementation
  ✓ Create User API - api/users.cs
  ✓ Run tests - ✓ ALL PASS (Green phase)

[Checkpoint] Verify: API returns 5 users
  ✓ Checkpoint passed

Phase 5: Frontend
  ✓ Create UserSelector component
  ✓ Create ProjectList component

[Checkpoint] Verify: Can select user and see projects
  ✓ Checkpoint passed
```

## Important Notes

### Development Tools Required

The AI agent will execute **local CLI commands** such as:
- `dotnet` (for .NET projects)
- `npm` / `yarn` (for Node.js projects)
- `python` / `pip` (for Python projects)
- `cargo` (for Rust projects)
- Database CLIs (`psql`, `mysql`, `sqlite3`)

**Ensure these are installed** before running `/speckit.implement`.

### Test-Driven Development

The implementation follows strict TDD:
1. Write test (task says "Write X tests")
2. Verify test FAILS (task says "Verify tests FAIL")
3. Write implementation (task says "Create X")
4. Verify test PASSES (task says "Run tests")

This enforces Article III of the constitution.

### Parallel Execution

Tasks marked `[P]` in `tasks.md` may execute simultaneously:
- Multiple models created at once
- Independent components built in parallel
- Tests run in parallel groups

This speeds up implementation but requires proper task independence analysis.

### After Implementation

Once complete:
1. **Test the application manually** - Use `quickstart.md` as a guide
2. **Check for runtime errors** - Browser console, server logs, etc.
3. **Report errors to AI agent** - Copy/paste errors for resolution
4. **Iterate if needed** - Fix issues and re-run affected tasks

## Troubleshooting

### "Prerequisites not found"

**Error:**
```text
Error: Could not find specs/001-feature/tasks.md
```

**Fix:** Run `/speckit.tasks` first to generate the task file.

---

### "Command not found: dotnet"

**Error:**
```text
Error: dotnet: command not found
```

**Fix:** Install the required development tools for your tech stack.

---

### Tests fail unexpectedly

**During Green phase:**
```text
✗ Tests FAILED (expected to pass)
```

**Fix:** Check test output, fix implementation, ask AI agent for help:
```text
The tests are failing with error: [paste error]. Can you fix the implementation?
```

---

### Runtime errors (browser console)

**After implementation:**
```text
Uncaught TypeError: Cannot read property 'id' of undefined
```

**Fix:** Copy error to AI agent:
```text
I'm seeing this error in the browser console: [paste error]. The app doesn't work as expected. Can you diagnose and fix?
```

## Related Commands

- `/speckit.tasks` - Previous step: Generate task breakdown
- `/speckit.analyze` - Optional: Validate before implementing
- `/speckit.plan` - Revise plan if major issues found

## See Also

- [Basic Workflow Step 6c](../workflows/basic-workflow.md#6c-implement) - Implementation in context
- [Constitution Article III](../docs/constitution-reference.md#article-iii-test-first-imperative) - TDD enforcement
- [Taskify Artifacts](../artifacts/) - Examples of generated code
