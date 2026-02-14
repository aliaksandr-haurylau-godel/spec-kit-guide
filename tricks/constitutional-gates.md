---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/spec-driven.md
---

# Constitutional Gates: Pre-Implementation Quality Checks

Constitutional gates are checkpoints in the implementation plan template that enforce architectural principles **before** any code is written. They act as "compile-time checks" for design decisions, preventing common over-engineering patterns.

## What Are Phase -1 Gates?

Phase -1 refers to pre-implementation validation. Before Phase 0 (setup) or Phase 1 (implementation), the AI must verify that the planned architecture passes constitutional requirements.

**Template Structure:**

```markdown
### Phase -1: Pre-Implementation Gates

#### Simplicity Gate (Article VII)
- [ ] Using ≤3 projects?
- [ ] No future-proofing?

#### Anti-Abstraction Gate (Article VIII)
- [ ] Using framework directly?
- [ ] Single model representation?

#### Integration-First Gate (Article IX)
- [ ] Contracts defined?
- [ ] Contract tests written?

**If ANY gate fails:** Document justification in Complexity Tracking section
```

## The Three Core Gates

### Gate 1: Simplicity Gate (Article VII)

**Purpose:** Prevent unnecessary project proliferation and future-proofing.

**Checks:**

1. **Using ≤3 projects for initial implementation?**
   - Count: API + Frontend + Shared = 3 ✅
   - Count: API + Frontend + Shared + Admin + Workers = 5 ❌

2. **No future-proofing?**
   - "Support current requirements" ✅
   - "Designed for future microservice extraction" ❌

3. **Every component has a concrete use case?**
   - "TaskRepository for current task CRUD" ✅
   - "GenericRepository for future entities" ❌

**Example Failure:**

```markdown
❌ FAILED: Simplicity Gate

Planned architecture:
- api-gateway (routing)
- auth-service (authentication)
- task-service (tasks)
- notification-service (emails)
- analytics-service (tracking)
- shared-types (TypeScript defs)

**Violation:** 6 projects exceeds maximum of 3
```

**How to Pass:**

```markdown
✅ PASSED: Simplicity Gate

Simplified architecture:
- api (monolith with auth, tasks, notifications)
- frontend (React app)
- shared-types (TypeScript defs)

Total: 3 projects
```

---

### Gate 2: Anti-Abstraction Gate (Article VIII)

**Purpose:** Prevent premature abstraction and wrapper proliferation.

**Checks:**

1. **Using framework features directly?**
   - "Use Express middleware" ✅
   - "Create custom MiddlewareManager class" ❌

2. **Single model representation?**
   - "Task entity = Task model = Task DTO" ✅
   - "TaskEntity → TaskModel → TaskDTO → TaskViewModel" ❌

3. **No unnecessary interfaces for single implementations?**
   - "class TaskService { ... }" ✅
   - "interface ITaskService + class TaskService implements ITaskService" ❌

**Example Failure:**

```markdown
❌ FAILED: Anti-Abstraction Gate

Planned layers:
1. ITaskRepository interface
2. BaseRepository abstract class
3. TaskRepositoryImpl extends BaseRepository
4. TaskRepositoryFactory to instantiate
5. TaskRepositoryProxy for caching

**Violation:** 5 abstraction layers for simple CRUD
```

**How to Pass:**

```markdown
✅ PASSED: Anti-Abstraction Gate

Direct implementation:
- Use Prisma ORM directly
- Single Task model
- No repository pattern (framework provides it)

Abstraction count: 0
```

---

### Gate 3: Integration-First Gate (Article IX)

**Purpose:** Ensure contract-first development and realistic testing.

**Checks:**

1. **API contracts defined before implementation?**
   - OpenAPI/GraphQL schema exists ✅
   - "Will define contracts during coding" ❌

2. **Contract tests written first?**
   - Pact/OpenAPI validation tests ✅
   - "Will test after building" ❌

3. **Using realistic test environments?**
   - Real database (Postgres in Docker) ✅
   - In-memory mocks everywhere ❌

**Example Failure:**

```markdown
❌ FAILED: Integration-First Gate

Planned approach:
1. Build TaskService
2. Mock all dependencies
3. Write unit tests with mocks
4. Later add integration tests

**Violation:** No contracts defined, mock-heavy testing
```

**How to Pass:**

```markdown
✅ PASSED: Integration-First Gate

Contract-first approach:
1. Define OpenAPI spec for Task API
2. Write contract tests (validate API against spec)
3. Run tests against real Postgres (Docker)
4. Implement service to pass tests

Tests use real database with test fixtures
```

---

## Handling Gate Failures

### When a Gate Fails Legitimately

Sometimes you have valid reasons to violate a gate. **Document thoroughly:**

```markdown
### Complexity Tracking

#### Gate Failures & Justifications

**Simplicity Gate: 5 projects (exceeds 3)**

**Justification:**
We need to violate Article VII due to concrete technical constraints:

1. **api-gateway** - Required for rate limiting and auth (security boundary)
2. **task-service** - Core business logic
3. **notification-service** - Async email/SMS (separate failure domain)
4. **frontend** - React SPA
5. **shared-types** - TypeScript contracts (DRY between services)

**Alternatives considered:**
- Monolith: Rejected - notification delays would block API requests
- Combined gateway+task: Rejected - mixing concerns violates security posture

**Mitigation:**
- Services communicate via contracts only
- Shared-types is tiny (<100 LOC)
- Plan to consolidate gateway+task in 6 months if load is low
```

### When to Reconsider Your Design

If you find yourself writing long justifications, it might indicate over-engineering:

**Red flags:**
- "We might need this later"
- "Industry best practices"
- "More scalable"
- "Professional architecture"

**Green lights:**
- "Current requirements demand it"
- "Concrete performance issue measured"
- "Security boundary required"
- "Independent deployment schedule"

## Gate Checklist Workflow

### Step 1: Design Your Architecture

Sketch out your planned implementation:
- Number of projects/services
- Abstraction layers
- Testing approach

### Step 2: Run Through Gates

Check each gate systematically:

```markdown
[ ] Simplicity Gate
    [ ] ≤3 projects?
    [ ] No future-proofing?
    
[ ] Anti-Abstraction Gate
    [ ] Framework used directly?
    [ ] Single model representation?
    
[ ] Integration-First Gate
    [ ] Contracts defined?
    [ ] Realistic tests planned?
```

### Step 3: Justify or Simplify

For each failed check:
1. **First:** Try to simplify to pass
2. **If impossible:** Document detailed justification
3. **Review:** Can the justification convince a skeptic?

### Step 4: Update Complexity Tracking

Maintain a running log:

```markdown
### Complexity Tracking

**Total Projects:** 3 ✅
**Abstraction Layers:** 1 (Prisma ORM) ✅
**Justified Violations:** 0 ✅

**Design Principles Upheld:**
- Direct framework usage (no wrappers)
- Single Task model (no DTO layers)
- Contract-first (OpenAPI spec written)
- Real database tests (Postgres in Docker)
```

## Real-World Examples

### Example 1: E-Commerce Checkout (Passes All Gates)

```markdown
### Phase -1: Pre-Implementation Gates

✅ Simplicity Gate
- 3 projects: api, frontend, shared-types
- No speculative features

✅ Anti-Abstraction Gate
- Express + Prisma used directly
- Single Product model
- No repository pattern

✅ Integration-First Gate
- OpenAPI spec defined
- Contract tests ready
- Uses real Stripe test API + real Postgres

### Complexity Tracking
Total Projects: 3
Abstraction Layers: 0 (framework features only)
Justified Violations: 0
```

### Example 2: Analytics Dashboard (Justified Violation)

```markdown
### Phase -1: Pre-Implementation Gates

❌ Simplicity Gate - FAILED
- 4 projects (exceeds 3)

✅ Anti-Abstraction Gate
- Direct framework usage

✅ Integration-First Gate
- Contracts defined

### Complexity Tracking

**Simplicity Gate Violation: 4 projects**

Projects:
1. api - REST API for dashboard
2. data-pipeline - ETL jobs (separate runtime)
3. frontend - Dashboard UI
4. shared-types - TypeScript defs

**Justification:**
- data-pipeline runs on CRON schedule (not web server)
- Different runtime = separate project (Node cron vs Express server)
- Tried combining: cron jobs blocked API requests
- Measured: 200ms latency added when combined

**Mitigation:**
- Pipeline is <300 LOC
- Shares DB with API (no new infrastructure)
- Single deployment script for both
```

### Example 3: Social Network (Over-Engineered)

```markdown
### Phase -1: Pre-Implementation Gates

❌ Simplicity Gate - FAILED (7 projects)
❌ Anti-Abstraction Gate - FAILED (4 abstraction layers)
✅ Integration-First Gate

### Complexity Tracking

**Simplicity Gate Violation: 7 projects**
- auth-service
- user-service
- post-service
- feed-service
- notification-service
- frontend
- shared-types

**Justification:**
"Microservices are best practice for scalability"

**RECOMMENDATION: SIMPLIFY**
Current requirements support 100 users. Premature microservices.

**Simplified architecture:**
- api (monolith: auth, users, posts, feed, notifications)
- frontend
- shared-types

Defer microservices until proven bottleneck (>10k users).
```

## Common Pitfalls

### Pitfall 1: "But It's Best Practice"

**Trap:** "Industry best practices say use microservices"

**Reality:** Best practices are context-dependent. Gates force you to justify based on *current* requirements, not future hypotheticals.

### Pitfall 2: Overcomplicating Tests

**Trap:** Creating elaborate mock frameworks

**Reality:** Integration-First gate pushes toward real dependencies (databases, APIs) in isolated test environments.

### Pitfall 3: Interface Everything

**Trap:** "What if we need to swap implementations?"

**Reality:** YAGNI (You Ain't Gonna Need It). Anti-Abstraction gate says: wait until you have a concrete second implementation.

## Benefits of Constitutional Gates

1. **Prevents Over-Engineering**: Catches complexity before code is written
2. **Forces Justification**: Makes you articulate *why* you need complexity
3. **Provides Audit Trail**: Complexity Tracking documents all design decisions
4. **Enables Refactoring**: Clear baseline to compare against when simplifying
5. **Team Alignment**: Shared vocabulary for discussing architecture

---

**See Also:**
- [Template Constraints](template-constraints.md) - How templates guide AI behavior
- [Avoiding Over-Engineering](avoiding-over-engineering.md) - Simplicity in practice
- [Constitution Reference](../docs/constitution-reference.md) - All constitutional articles
- [Plan Command](../commands/speckit-plan.md) - Where gates are enforced
