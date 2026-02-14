---
version: 1
source_date: 2026-02-12
derived_from: Synthesized from README.md and quickstart.md examples
status: placeholder
---

# Implementation Plan: Create Taskify

**Note:** This is a placeholder document. A complete implementation plan would be generated using `/speckit.plan`. This document shows the expected structure and key sections.

---

## Technology Stack

**Frontend:**
- Blazor Server (for real-time updates)
- HTML5 drag-and-drop API or SortableJS library
- SignalR for WebSocket communication

**Backend:**
- .NET Aspire (orchestration)
- ASP.NET Core Web API
- Entity Framework Core

**Database:**
- PostgreSQL 15+

**Real-time:**
- SignalR (WebSocket fallback to long polling)

**Rationale:**
- Blazor Server chosen for built-in real-time capabilities via SignalR
- PostgreSQL for robust relational data integrity
- .NET Aspire for simplified microservices orchestration and observability
- Minimal external dependencies (adheres to Article VII: Simplicity)

---

## System Architecture

```text
[Browser] <--> [Blazor Server] <--> [Web API] <--> [PostgreSQL]
               (SignalR)            (REST)
```

**Components:**
1. **Blazor Server App** - UI rendering, drag-and-drop interaction
2. **Web API** - REST endpoints for projects, tasks, comments
3. **PostgreSQL Database** - Persistent storage

**Communication:**
- Blazor ↔ API: REST calls for CRUD operations
- Blazor ↔ Browser: SignalR for real-time updates
- API ↔ Database: Entity Framework Core

---

## Phase -1: Pre-Implementation Gates

### Simplicity Gate (Article VII)

- [x] Using ≤3 projects? **YES** (taskify-core, taskify-api, taskify-web)
- [x] No future-proofing? **YES** (no "might need" features included)
- [x] Justified complexity? **N/A** (no additional projects needed)

### Anti-Abstraction Gate (Article VIII)

- [x] Using framework directly? **YES** (EF Core and SignalR used directly)
- [x] Single model representation? **YES** (Task class used everywhere, no DTOs initially)
- [x] Avoiding unnecessary wrappers? **YES** (no custom DB abstraction layer)

### Integration-First Gate (Article IX)

- [x] Contracts defined? **YES** (`contracts/api-spec.json`, `contracts/signalr-spec.md`)
- [x] Contract tests written? **YES** (in `tests/contract/`)
- [x] Real database in tests? **YES** (Postgres test container)

---

## Data Model

See `data-model.md` for complete entity definitions.

**Key Entities:**
- **User** (id, name, role)
- **Project** (id, name, user_id)
- **Task** (id, title, status, project_id, assigned_user_id)
- **Comment** (id, text, task_id, author_id, created_at)

**Relationships:**
- Project → User (many-to-one)
- Task → Project (many-to-one)
- Task → User (many-to-one, assigned_user)
- Comment → Task (many-to-one)
- Comment → User (many-to-one, author)

---

## API Contracts

See `contracts/api-spec.json` for complete endpoint definitions.

**Key Endpoints:**
- `GET /api/users` - List all users
- `GET /api/projects` - List all projects
- `GET /api/projects/{id}/tasks` - Get tasks for a project
- `PATCH /api/tasks/{id}` - Update task (status or assigned_user)
- `POST /api/tasks/{id}/comments` - Add comment
- `PUT /api/comments/{id}` - Edit comment (owner only)
- `DELETE /api/comments/{id}` - Delete comment (owner only)

---

## Core Implementation Phases

### Phase 1: Database Setup

1. Create database schema (migrations)
2. Seed predefined users
3. Seed 3 sample projects
4. Seed 5-15 tasks per project (random distribution)
5. Verify: Database contains all sample data

### Phase 2: API Development (TDD)

1. Write contract tests (REST endpoints)
2. Verify tests FAIL
3. Implement API endpoints
4. Verify tests PASS
5. Verify: All endpoints return expected JSON

### Phase 3: Blazor Components

1. Create UserSelector component
2. Create ProjectList component
3. Create KanbanBoard component (with drag-and-drop)
4. Create TaskCard component
5. Create TaskDetail modal
6. Create CommentList component
7. Verify: All UI components render correctly

### Phase 4: Real-Time Integration

1. Configure SignalR hub
2. Implement task status change broadcasting
3. Add SignalR client to Blazor components
4. Verify: Changes appear in real-time (or near-real-time with polling)

### Phase 5: Authorization & Permissions

1. Implement comment ownership checks
2. Add edit/delete buttons conditionally (own comments only)
3. Add API validation for comment modifications
4. Verify: Users cannot edit/delete others' comments

---

## Testing Strategy

### Unit Tests
- Entity validation logic
- Service layer business rules
- Utility functions

### Contract Tests (Integration-First, Article IX)
- All REST endpoints
- Request/response format validation
- Status code verification

### Integration Tests
- API → Database interaction (real Postgres in Docker)
- Blazor → API communication
- SignalR message propagation

### E2E Tests
- User selection flow
- Project browsing
- Task drag-and-drop
- Comment CRUD operations

**Test Execution Order (TDD, Article III):**
1. Write test
2. Verify test FAILS (Red)
3. Write implementation
4. Verify test PASSES (Green)
5. Refactor if needed
6. Verify test still PASSES

---

## File Creation Order (Article III: Test-First)

1. Create `contracts/` with API specifications
2. Create test files in order:
   - Contract tests
   - Integration tests
   - E2E tests
   - Unit tests (last, only if needed)
3. Create source files to make tests pass

---

## Research Notes

See `research.md` for detailed technology research.

**Key Research Areas:**
- .NET Aspire setup and configuration
- Blazor Server real-time capabilities
- SignalR WebSocket configuration
- PostgreSQL with Entity Framework Core
- Drag-and-drop libraries for Blazor

---

## Quickstart Validation

See `quickstart.md` for manual testing scenarios.

**Key Validation Scenarios:**
1. Launch app → See 5 users → Click user → See 3 projects
2. Click project → See Kanban board with 4 columns and tasks
3. Drag task between columns → Task moves → Status updates
4. Click task → Open detail view → Add comment → Comment appears
5. Try to edit another user's comment → Edit button not visible

---

## Complexity Tracking

**No constitutional violations.** All design decisions comply with Taskify's constitution.

---

## See Also

- [Spec (Taskify)](spec-example-taskify.md) - Feature specification
- [Constitution (Taskify)](constitution-example.md) - Governing principles
- [Tasks (Taskify)](tasks-example-taskify.md) - Actionable task breakdown
- [Basic Workflow](../workflows/basic-workflow.md#step-5-create-a-technical-implementation-plan) - Planning step
