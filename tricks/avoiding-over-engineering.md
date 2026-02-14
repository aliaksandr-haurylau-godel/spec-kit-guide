---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/spec-driven.md
---

# Avoiding Over-Engineering: Articles VII & VIII in Practice

Two constitutional articles—**Article VII (Simplicity)** and **Article VIII (Anti-Abstraction)**—work together to prevent over-engineering. This guide shows how to apply them in practice.

## The Over-Engineering Problem

AI coding assistants (and human developers) naturally tend toward:
- **Premature abstraction**: Interfaces, factories, and patterns "just in case"
- **Future-proofing**: Building for hypothetical scale or requirements
- **Complexity creep**: "Professional" architectures that exceed current needs

Spec-Kit's constitution fights this by making simplicity **mandatory** and complexity **justified**.

## Article VII: Simplicity

> **Principle:** Simplicity is not a suggestion—it's a requirement. Complexity must be justified against concrete current needs.

### Section 7.3: Minimal Project Structure

**Rule:** Maximum 3 projects for initial implementation. Additional projects require documented justification.

**Why 3?**

Three projects handle 90% of typical applications:
1. **Backend** (API, services, database)
2. **Frontend** (UI, presentation)
3. **Shared** (types, contracts, utilities)

**Examples:**

✅ **Simple E-Commerce (3 projects)**
```text
- api/          (Express + Prisma: products, cart, checkout)
- frontend/     (React: shop UI)
- shared-types/ (TypeScript interfaces)
```

✅ **Mobile + Web App (3 projects)**
```text
- api/          (Backend for both clients)
- web-app/      (React web UI)
- mobile-app/   (React Native iOS/Android)
- shared-types/ (API contracts)
```
*Note: 4 projects, but justified - separate mobile/web apps are concrete requirement*

❌ **Over-Engineered E-Commerce (7 projects)**
```text
- api-gateway/       (routing)
- auth-service/      (authentication)
- product-service/   (product CRUD)
- cart-service/      (cart management)
- payment-service/   (checkout)
- frontend/          (UI)
- shared-types/      (contracts)
```
*Violation: Microservices for <1000 users is premature*

### How to Apply Article VII

**Step 1: Start with monolith assumption**

Default to single backend project until proven otherwise.

**Step 2: Challenge each additional project**

For each project beyond 3, ask:
- **Is there a concrete current requirement?** (not "might need")
- **Can it be a module in an existing project instead?**
- **What's the measurable cost of combining them?**

**Step 3: Document justifications**

When you exceed 3 projects, document in `plan.md`:

```markdown
### Complexity Tracking

**Article VII Violation: 4 projects**

**Justification:**
- notification-service separated due to:
  1. **Different runtime**: CRON-based (not HTTP server)
  2. **Failure isolation**: Email provider outage shouldn't affect API
  3. **Measured impact**: Combined version had 200ms added latency

**Mitigation:**
- Notification service is <300 LOC
- Shares database (no new infrastructure)
- Single deployment pipeline
```

### Common "Justifications" That Don't Count

❌ "It's more scalable"
- **Why not:** Scalability is a future concern. Scale when you have data.

❌ "It's best practice"
- **Why not:** Best practices are context-dependent. What's the concrete benefit *now*?

❌ "Separation of concerns"
- **Why not:** Modules within a project also separate concerns.

❌ "Easier to test"
- **Why not:** Testing difficulty usually indicates poor design, not need for more projects.

✅ "Service A must deploy independently of Service B due to different release schedules"
- **Why:** Concrete operational requirement.

✅ "Background job runs for 30 minutes, would block API requests if combined"
- **Why:** Measurable performance impact.

---

## Article VIII: Anti-Abstraction

> **Principle:** Trust the framework. Use its features directly. Avoid wrapping, proxying, or abstracting what already works.

### Section 8.1: Framework Trust

**Rule:** Use framework features directly rather than wrapping them.

**Examples:**

✅ **Direct Framework Usage (Express)**
```typescript
// app.ts
import express from 'express';
const app = express();

app.get('/tasks', async (req, res) => {
  const tasks = await prisma.task.findMany();
  res.json(tasks);
});
```

❌ **Over-Abstracted (Express)**
```typescript
// Abstract HTTP framework
interface IHttpRequest { params: any; body: any; }
interface IHttpResponse { send(data: any): void; }
interface IRouter { get(path: string, handler: Handler): void; }

class ExpressRequestAdapter implements IHttpRequest { ... }
class ExpressResponseAdapter implements IHttpResponse { ... }
class RouterFactory { 
  static create(): IRouter { return new ExpressRouterAdapter(); }
}

// Now write handlers against abstract interfaces
class TaskController {
  constructor(private router: IRouter) {}
  
  registerRoutes() {
    this.router.get('/tasks', this.getTasks.bind(this));
  }
  
  getTasks(req: IHttpRequest, res: IHttpResponse) { ... }
}
```

**Violation:** 5 abstraction layers to avoid importing `express`. No concrete benefit.

### Section 8.2: Single Model Representation

**Rule:** One concept = one model. Avoid DTO/Entity/ViewModel proliferation.

**Examples:**

✅ **Single Model**
```typescript
// task.ts
export interface Task {
  id: string;
  title: string;
  status: 'todo' | 'done';
  createdAt: Date;
}

// Used everywhere:
// - Prisma schema
// - API responses
// - Frontend state
// - Database queries
```

❌ **DTO Proliferation**
```typescript
// domain/task-entity.ts
class TaskEntity {
  constructor(
    private id: string,
    private title: string,
    private status: TaskStatus
  ) {}
}

// dto/task-dto.ts
class TaskDTO {
  id: string;
  title: string;
  status: string;
}

// view-models/task-view-model.ts
class TaskViewModel {
  id: string;
  displayTitle: string;
  statusLabel: string;
  statusColor: string;
}

// mappers/task-mapper.ts
class TaskMapper {
  static toDTO(entity: TaskEntity): TaskDTO { ... }
  static toViewModel(dto: TaskDTO): TaskViewModel { ... }
  static toEntity(dto: TaskDTO): TaskEntity { ... }
}
```

**Violation:** 4 representations of the same concept. Requires 3 mapper functions.

**When multiple models are justified:**

✅ **Public API vs Internal Model**
```typescript
// internal-model.ts (includes sensitive data)
interface UserInternal {
  id: string;
  email: string;
  passwordHash: string;  // Never expose
  role: 'admin' | 'user';
}

// public-api.ts (external clients)
interface UserPublic {
  id: string;
  email: string;
  role: string;
}

// Justified: Security boundary requires separation
```

### How to Apply Article VIII

**Step 1: Import the framework directly**

```typescript
// ✅ Do this
import { Router } from 'express';
const router = Router();

// ❌ Not this
import { IRouter, RouterFactory } from './abstractions/router';
const router = RouterFactory.create();
```

**Step 2: Use one model until you hit a real problem**

Start with a single interface/class. Split only when:
- **Security boundary**: Public API needs fewer fields than internal model
- **Different serialization**: gRPC vs JSON vs GraphQL requires different shapes
- **Performance**: Lean query model vs rich domain model

**Step 3: Challenge every interface**

When you see:
```typescript
interface ITaskService {
  getTasks(): Promise<Task[]>;
}

class TaskService implements ITaskService {
  getTasks(): Promise<Task[]> { ... }
}
```

Ask: **Do I have a second implementation?**
- **No?** Delete the interface. Use the class directly.
- **Yes?** Keep it—interfaces are for polymorphism.

**Common "justifications" that don't count:**

❌ "What if we need to mock it for tests?"
- **Why not:** Modern test frameworks can mock classes directly.

❌ "What if we switch frameworks later?"
- **Why not:** YAGNI. Framework switches are rare. Refactor when it happens.

❌ "It's more professional"
- **Why not:** Professionalism = solving real problems simply.

✅ "We have MySQLTaskService and PostgresTaskService"
- **Why:** Two concrete implementations justify an interface.

---

## Practical Decision Framework

Use this checklist when designing:

### Adding a New Project?

1. **Can this be a folder in an existing project?**
   - Yes → Make it a folder
   - No → Continue

2. **Does it have a different runtime/deployment schedule?**
   - Yes → Justified (document in Complexity Tracking)
   - No → Make it a folder

3. **Is there a measured performance/security issue with combining?**
   - Yes → Justified (document measurements)
   - No → Make it a folder

### Adding an Abstraction Layer?

1. **Do I have 2+ concrete implementations right now?**
   - Yes → Abstraction justified
   - No → Use concrete implementation directly

2. **Is this a security/compliance boundary?**
   - Yes → Abstraction justified (e.g., public API vs internal model)
   - No → Use concrete implementation directly

3. **Am I wrapping a stable framework feature?**
   - Yes → **DON'T.** Use framework directly.
   - No → Reconsider if you need the feature at all

### Adding a Model/DTO?

1. **Is this the same concept with different fields?**
   - Yes → Consider if you can use a single model
   - No → Separate models justified

2. **Is the only difference formatting (e.g., Date vs string)?**
   - Yes → Use single model, format at boundary
   - No → Separate models may be justified

3. **Do I have a security reason to separate?**
   - Yes → Separate models justified
   - No → Use single model

---

## Real-World Examples

### Example 1: Todo App (Simple)

**Requirements:** Users can create, complete, and delete tasks.

**Initial impulse (over-engineered):**
```text
Architecture:
- task-service/
  - domain/
    - task-entity.ts
    - task-repository.interface.ts
  - application/
    - task-service.interface.ts
    - task-service.ts
    - create-task.use-case.ts
    - complete-task.use-case.ts
    - delete-task.use-case.ts
  - infrastructure/
    - prisma-task-repository.ts
- api-gateway/
- frontend/
- shared-types/
```

**After Articles VII & VIII (simplified):**
```text
Architecture:
- api/
  - task.service.ts  (Prisma direct access)
  - task.routes.ts   (Express routes)
- frontend/
- shared-types/
  - task.ts          (Single Task interface)

✅ 3 projects
✅ 0 abstraction layers
✅ 1 model representation
```

**Result:** 80% less code, same functionality.

---

### Example 2: Multi-Tenant SaaS (Justified Complexity)

**Requirements:** 
- B2B app with 50 tenants
- Each tenant: independent database
- Tenant isolation is regulatory requirement

**Justified architecture:**
```text
Architecture:
- tenant-router/      (routes requests to tenant-specific DB)
- api/                (shared business logic)
- frontend/
- shared-types/

Total: 4 projects
```

**Complexity Tracking:**
```markdown
### Article VII Violation: 4 projects

**Justification:**
- tenant-router required for:
  1. Regulatory requirement: tenant data isolation (GDPR, SOC2)
  2. Different deployment: router updates shouldn't restart app servers
  3. Performance: routing layer caches tenant→DB mappings

**Alternative considered:**
- Middleware in single API: rejected due to shared memory space (security risk)

**Mitigation:**
- Router is <200 LOC (simple lookup table)
- Single source of truth for tenant mappings
```

---

### Example 3: Payment Processing (Justified Abstraction)

**Requirements:** Support Stripe and PayPal payment providers.

**Justified abstraction:**
```typescript
// payment.service.ts
interface PaymentProvider {
  charge(amount: number, token: string): Promise<PaymentResult>;
}

class StripeProvider implements PaymentProvider {
  charge(amount: number, token: string) {
    return stripe.charges.create({ amount, source: token });
  }
}

class PayPalProvider implements PaymentProvider {
  charge(amount: number, token: string) {
    return paypal.payment.create({ ... });
  }
}

// Usage
const provider: PaymentProvider = getProvider(tenant);
await provider.charge(1000, token);
```

**Why justified:**
- 2 concrete implementations (Stripe, PayPal)
- Real polymorphism needed (tenant-specific provider selection)
- Not wrapping framework—coordinating multiple external APIs

---

## Checklist: Before You Commit

Run through these checks before finalizing your plan:

```markdown
### Simplicity Check (Article VII)

- [ ] ≤3 projects OR documented justification?
- [ ] No "might need" features?
- [ ] Every component has concrete current use case?

### Anti-Abstraction Check (Article VIII)

- [ ] Framework features used directly (no wrappers)?
- [ ] Single model representation (no DTO layers)?
- [ ] Interfaces only where 2+ implementations exist?

### Justified Complexity (if any)

- [ ] Documented in Complexity Tracking section?
- [ ] Justification based on current requirements (not future)?
- [ ] Alternative approaches considered and rejected with reasons?
```

If all checks pass → **Proceed with implementation**

If any check fails → **Simplify or document thoroughly**

---

**See Also:**
- [Constitutional Gates](constitutional-gates.md) - Automated enforcement of these principles
- [Template Constraints](template-constraints.md) - How templates prevent over-engineering
- [Constitution Reference](../docs/constitution-reference.md) - Full text of Articles VII & VIII
- [Plan Command](../commands/speckit-plan.md) - Where these principles are enforced
