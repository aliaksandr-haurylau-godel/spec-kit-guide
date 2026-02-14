---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/spec-driven.md
---

# Template Constraints: How Structure Guides AI Behavior

The true power of Spec-Kit's slash commands lies not just in automation, but in how the templates guide AI behavior toward higher-quality specifications. The templates act as sophisticated prompts that constrain the AI's output in productive ways.

## Key Constraint Mechanisms

### 1. Preventing Premature Implementation Details

**The Problem:** AI naturally jumps to implementation details when asked to design features.

**The Solution:** Feature specification templates explicitly instruct:

```text
- ✅ Focus on WHAT users need and WHY
- ❌ Avoid HOW to implement (no tech stack, APIs, code structure)
```

**Example:**

**Without constraint:**
> "Users need real-time updates → implement using React with Redux and WebSockets"

**With constraint:**
> "Users need to see task status changes immediately without manual refresh"

**Benefit:** Specifications remain stable even as implementation technologies change. The "what" endures while the "how" evolves.

---

### 2. Forcing Explicit Uncertainty Markers

**The Problem:** AI makes plausible but potentially incorrect assumptions to provide complete answers.

**The Solution:** Both spec and plan templates mandate `[NEEDS CLARIFICATION]` markers:

```text
When creating this spec from a user prompt:
1. **Mark all ambiguities**: Use [NEEDS CLARIFICATION: specific question]
2. **Don't guess**: If the prompt doesn't specify something, mark it
```

**Example:**

**Without constraint:**
> "Login system will use email/password authentication with bcrypt hashing"

**With constraint:**
> "Login system [NEEDS CLARIFICATION: auth method not specified - email/password, SSO, OAuth, magic link?]"

**Benefit:** Forces explicit conversations about requirements rather than hidden assumptions that cause rework later.

---

### 3. Structured Thinking Through Checklists

**The Problem:** AI can miss important considerations when generating free-form specifications.

**The Solution:** Templates include comprehensive checklists that act as "unit tests" for the specification:

```markdown
### Requirement Completeness

- [ ] No [NEEDS CLARIFICATION] markers remain
- [ ] Requirements are testable and unambiguous
- [ ] Success criteria are measurable
- [ ] All user stories have acceptance criteria
- [ ] Edge cases and error scenarios documented
```

**Example Application:**

The AI must systematically review its output against each criterion, catching gaps like:
- Missing error handling scenarios
- Vague acceptance criteria ("should be fast" → "responds in <200ms")
- Untestable requirements ("should look professional" → specific design criteria)

**Benefit:** Self-review framework baked into the generation process.

---

### 4. Constitutional Compliance Through Gates

**The Problem:** AI tends toward over-engineering and complex abstractions.

**The Solution:** Implementation plan template enforces architectural principles through phase gates:

```markdown
### Phase -1: Pre-Implementation Gates

#### Simplicity Gate (Article VII)

- [ ] Using ≤3 projects?
- [ ] No future-proofing?
- [ ] Every component has a concrete use case?

#### Anti-Abstraction Gate (Article VIII)

- [ ] Using framework directly?
- [ ] Single model representation?
- [ ] No premature abstraction layers?
```

**Example:**

**Without gates:**
> "Create TaskRepository interface, InMemoryTaskRepository, SqlTaskRepository, TaskRepositoryFactory, and TaskRepositoryProxy for future flexibility"

**With gates:**
> "Use framework's ORM directly. Single Task model. [COMPLEXITY JUSTIFIED: none - using framework as-is]"

**Benefit:** Prevents over-engineering by making AI explicitly justify any complexity.

---

### 5. Hierarchical Detail Management

**The Problem:** Specifications become unreadable code dumps with too much detail.

**The Solution:** Templates enforce proper information architecture:

```text
**IMPORTANT**: This implementation plan should remain high-level and readable.
Any code samples, detailed algorithms, or extensive technical specifications
must be placed in the appropriate `implementation-details/` file
```

**Structure:**

```text
specs/001-feature/
├── spec.md                          ← High-level, readable
├── plan.md                          ← Implementation overview
├── tasks.md                         ← Actionable steps
└── implementation-details/
    ├── api-contracts.md             ← API specs
    ├── database-schema.md           ← Schema details
    └── algorithm-pseudocode.md      ← Complex algorithms
```

**Benefit:** Main documents stay navigable while technical details remain accessible.

---

### 6. Test-First Thinking

**The Problem:** Implementation-first code lacks testability and contracts.

**The Solution:** Implementation template enforces test-first development:

```text
### File Creation Order
1. Create `contracts/` with API specifications
2. Create test files in order: contract → integration → e2e → unit
3. Create source files to make tests pass

**DO NOT** create implementation files before tests.
```

**Example Workflow:**

```bash
# ❌ Wrong order
src/task-service.ts
tests/task-service.test.ts

# ✅ Correct order
tests/task-service.test.ts      # Define behavior first
src/task-service.ts             # Implement to pass tests
```

**Benefit:** AI thinks about testability and contracts before implementation, leading to more robust code.

---

### 7. Preventing Speculative Features

**The Problem:** AI adds "nice to have" features that complicate MVP delivery.

**The Solution:** Templates explicitly discourage speculation:

```text
- [ ] No speculative or "might need" features
- [ ] All phases have clear prerequisites and deliverables
- [ ] Every requirement traces to a concrete user story
```

**Example:**

**Without constraint:**
> "Include user preferences system for future customization, export to PDF/CSV/Excel, and admin dashboard for analytics"

**With constraint:**
> "Core requirement: Users can create and complete tasks. Future enhancements tracked separately in backlog."

**Benefit:** Focused, deliverable MVPs instead of scope-creep.

---

## The Compound Effect

These constraints work together to produce specifications that are:

- **Complete**: Checklists ensure nothing is forgotten
- **Unambiguous**: Forced clarification markers highlight uncertainties
- **Testable**: Test-first thinking baked into the process
- **Maintainable**: Proper abstraction levels and information hierarchy
- **Implementable**: Clear phases with concrete deliverables
- **Simple**: Constitutional gates prevent over-engineering

The templates transform the AI from a creative writer into a disciplined specification engineer, channeling its capabilities toward producing consistently high-quality, executable specifications that truly drive development.

## Practical Tips

### Tip 1: Resist the urge to remove constraints

When templates feel "too restrictive," that's often when they're doing their job. The friction is intentional—it forces better thinking.

### Tip 2: Use template violations as learning signals

If you find yourself fighting a template constraint, ask:
- "Why does this constraint exist?"
- "What problem is it preventing?"
- "Is there a better way to express my requirement within the constraint?"

### Tip 3: Customize templates for your domain

While keeping core constraints, add domain-specific sections:

```markdown
## Healthcare Compliance (HIPAA)

- [ ] PHI handling documented
- [ ] Audit logging specified
- [ ] Access controls defined

## Financial Domain (PCI-DSS)

- [ ] Payment data isolation
- [ ] Encryption at rest/transit
- [ ] No card data in logs
```

### Tip 4: Review generated specs against checklists manually

Even with constraints, periodically review generated specs:
1. Read each requirement aloud - does it make sense?
2. Check for hidden assumptions
3. Verify all user stories have clear acceptance criteria
4. Ensure technical plans pass constitutional gates

### Tip 5: Document justified complexity

When you *must* violate a gate (e.g., need >3 projects for valid reasons), document thoroughly:

```markdown
### Complexity Tracking

**Article VII Gate Violation: 4 projects used**

**Justification**: 
- `api-gateway` - External-facing service (security boundary)
- `task-service` - Core business logic
- `notification-service` - Async processing (performance requirement)
- `shared-types` - TypeScript definitions (DRY for contracts)

**Alternative considered**: Monorepo with shared code - rejected due to independent deployment requirements
```

---

**See Also:**
- [Constitutional Gates](constitutional-gates.md) - Deep dive on Phase -1 gates
- [Constitution Reference](../docs/constitution-reference.md) - All constitutional articles
- [Avoiding Over-Engineering](avoiding-over-engineering.md) - Simplicity in practice
- [Spec-Driven Development](../docs/spec-driven-development.md) - Core philosophy
