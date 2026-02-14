---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/README.md (lines 120-134), spec-driven.md
---

# /speckit.tasks

## Purpose

Generates an actionable, executable task list from your implementation plan. Converts high-level plans into specific tasks with correct ordering, dependency management, and parallel execution markers.

## When to Use

**Sixth step (part A)** in the workflow, after creating your plan with `/speckit.plan` and before implementing with `/speckit.implement`. Use this command to:
- Break down plans into concrete tasks
- Establish correct task execution order
- Identify tasks that can run in parallel
- Prepare for test-driven development
- Create checkpoint validations per user story

## Prerequisites

- Plan created with `/speckit.plan`
- Active feature branch (or `SPECIFY_FEATURE` set)
- `specs/[feature]/plan.md` exists
- Optional: `data-model.md`, `contracts/`, `research.md` in feature directory

## Input Format

```bash
/speckit.tasks
```

No arguments needed. The command analyzes existing plan documents automatically.

## Output

**File Created:**
- `specs/[feature]/tasks.md` - Executable task breakdown with:
  - Tasks organized by user story
  - Dependency management (correct order)
  - Parallel execution markers `[P]`
  - File path specifications for each task
  - Test-driven development structure
  - Checkpoint validation per phase

## Example

```markdown
/speckit.tasks
```

## What Happens

1. **Input Analysis**: Reads from feature directory:
   - `plan.md` (required) - High-level implementation plan
   - `data-model.md` (if present) - Entity schemas
   - `contracts/` (if present) - API specifications
   - `research.md` (if present) - Technology details

2. **Task Derivation**: Converts high-level plans into tasks:
   - Maps entities to model creation tasks
   - Maps contracts to endpoint implementation tasks
   - Maps scenarios to test creation tasks
   - Orders tasks by dependencies (models before services, services before endpoints)

3. **Parallelization Analysis**: Marks independent tasks with `[P]`:
   - Multiple models can be created in parallel
   - Tests that don't depend on each other can run in parallel
   - Frontend and backend tasks can proceed simultaneously (if decoupled)

4. **File Path Assignment**: Specifies exact file paths:
   - `models/user.cs` - Where to create User model
   - `tests/user-api.test.cs` - Where to write tests
   - `api/users.cs` - Where to implement API

5. **Checkpoint Creation**: Adds validation steps after each user story phase

## Task Structure

The generated `tasks.md` follows this structure:

```markdown
# Implementation Tasks: [Feature]

## User Story 1: [Title]

### Phase 1: Data Models

1. [P] Create User model - `models/user.cs`
2. [P] Create Project model - `models/project.cs`
3. [P] Create Task model - `models/task.cs`

### Phase 2: Database Setup

4. Create database migrations - `migrations/001_initial.sql`
5. Seed predefined data - `migrations/002_seed.sql`

### Phase 3: Tests (TDD)

6. [P] Write User API tests - `tests/user-api.test.cs`
7. [P] Write Project API tests - `tests/project-api.test.cs`
8. Verify tests FAIL (Red phase)

### Phase 4: API Implementation

9. Create User API endpoint - `api/users.cs`
10. Create Project API endpoint - `api/projects.cs`

### Phase 5: Frontend

11. [P] Create user selection component - `components/UserSelector.tsx`
12. [P] Create project list component - `components/ProjectList.tsx`

**[Checkpoint]** Verify: Users can select a user and see project list

## User Story 2: [Title]
[... repeat structure ...]
```

## Task Markers

### `[P]` - Parallel Execution

Tasks marked with `[P]` can run simultaneously:

```markdown
1. [P] Create User model - `models/user.cs`
2. [P] Create Project model - `models/project.cs`
3. [P] Create Task model - `models/task.cs`
```

All three models can be created in parallel—they have no dependencies on each other.

### `[Checkpoint]` - Validation Points

Checkpoints define what should work after completing a phase:

```markdown
**[Checkpoint]** Verify: API returns 5 predefined users with correct data
```

Use these for incremental testing during implementation.

## Test-Driven Development Structure

Tasks follow strict TDD order:

```markdown
### Phase 1: Write Tests
6. [P] Write User API tests - `tests/user-api.test.cs`
7. Verify tests FAIL (Red phase)

### Phase 2: Implement
8. Create User API endpoint - `api/users.cs`
9. Verify tests PASS (Green phase)

### Phase 3: Refactor
10. Optimize User API if needed
11. Verify tests still PASS
```

This enforces Article III of the constitution (Test-First Imperative).

## Related Commands

- `/speckit.implement` - Next step: Execute all tasks
- `/speckit.analyze` - Optional: Validate task consistency before implementing
- `/speckit.plan` - Previous step: Create implementation plan

## See Also

- [Tasks Example (Taskify)](../artifacts/tasks-example-taskify.md) - Complete task breakdown
- [Basic Workflow Step 6](../workflows/basic-workflow.md#6a-generate-task-breakdown) - In context
- [Constitution Article III](../docs/constitution-reference.md#article-iii-test-first-imperative) - Test-first principle
