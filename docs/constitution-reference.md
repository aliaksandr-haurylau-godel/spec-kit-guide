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

## Real-World Constitution Patterns

This section documents patterns observed in production Spec-Kit projects, based on three real-world constitutions from JetBrains IdeaVim, Skyscanner Backpack, and Manifest Generator projects.

### SYNC IMPACT: Version Tracking

Production teams use **SYNC IMPACT** declarations in artifact frontmatter to track constitution versions. This pattern allows teams to identify when specs, plans, or tasks were generated against outdated constitutional principles.

**Example from IdeaVim (v1.2.2):**

```yaml
---
version: 1.2.2
constitution_version: 1.2.2
previous_version: 1.2.1
---
```

The SYNC IMPACT report documents:

```text
SYNC IMPACT REPORT
==================
Version change: 1.2.1 → 1.2.2

Modified sections:
- VIII. Trunk-Based Development → prefer feature branches with frequent rebasing

Templates requiring updates:
- .specify/templates/plan-template.md ✅ (no changes needed)
- .specify/templates/spec-template.md ✅ (compatible)
- .specify/templates/tasks-template.md ✅ (compatible)

Follow-up TODOs: None
```

**Versioning Semantics:**

Constitution versions follow semantic versioning with these meanings:

- **PATCH (x.y.Z)**: Editorial changes only
  - Typo fixes, reformatting, clarifications
  - No process or principle changes
  - SYNC IMPACT: Existing branches don't need regeneration
  - Example: Backpack 1.0.1 → 1.0.2 (clarified license headers)

- **MINOR (x.Y.0)**: Backward-compatible additions
  - New articles or sections added
  - Enhanced guidance that doesn't contradict existing principles
  - SYNC IMPACT: New features must follow new articles; existing features unaffected
  - Example: IdeaVim 1.1.0 → 1.2.0 (added Article X: Vim Compatibility)

- **MAJOR (X.0.0)**: Breaking changes
  - Removed or fundamentally rewritten articles
  - Incompatible principle changes
  - SYNC IMPACT: ALL active branches must regenerate plans
  - Example: Manifest 1.1.0 → 2.0.0 (removed POC markers, added security requirements)

**Cross-Reference:** See [Constitution Evolution FAQ](../faq/constitution-evolution.md) for detailed version management guidance.

---

### Project-Specific Articles (Beyond Article IX)

While the core Spec-Kit framework defines 9 standard articles, production projects frequently extend the constitution with domain-specific requirements as additional articles (X, XI, XII, etc.).

#### Article X Examples from Production

##### IdeaVim: Vim Compatibility (Article X)

The IdeaVim constitution adds Article X to govern IDE-specific requirements:

```text
### X. Vim Compatibility (IDE-First)

IdeaVim MUST match Vim functionality where feasible for JetBrains IDEs, 
while preserving IDE integrity.

Priority Order (when behaviors conflict):
1. JetBrains IDE behavior takes precedence over Vim behavior
2. Vim behavior is implemented where it does not conflict with the IDE

Section 10.1: Vim Semantics Preservation
- Commands MUST behave like Vim unless it conflicts with IDE behavior
- Motions MUST follow Vim's inclusive/exclusive/linewise semantics
- Key mappings MUST support standard Vim syntax
```

##### Backpack: Monorepo Standards (Article X)

The Backpack constitution adds Article X to govern design system-specific requirements:

```text
### X. Design-Anchored Development (NON-NEGOTIABLE)

Backpack is an encoding of Skyscanner's visual design language. All component 
changes MUST be design-approved before implementation:

Section 10.1: Design Approval Requirements
- Changes MUST go through design review with wider design community, OR
- Be reviewed and approved by a Backpack designer
- For non-trivial changes, reach out to Backpack squad on Slack (#backpack) 
  BEFORE starting work

Section 10.2: Design Deliverables Required
- Figma designs with ALL states documented (default, hover, focus, active, 
  disabled, loading, error)
- Visual examples for each state
- Responsive behavior specifications (mobile, tablet, desktop)
- Accessibility annotations (using Figma accessibility annotation kit)
```

#### When to Add Custom Articles

Add project-specific articles when you have:

1. **Domain-Specific Constraints**
   - IDE plugin compatibility requirements (IdeaVim)
   - Design system governance (Backpack)
   - CLI tool conventions (Manifest)

2. **Organizational Requirements**
   - Legal compliance (license headers)
   - Security policies (OWASP compliance)
   - Community standards (code of conduct)

3. **Technology-Specific Conventions**
   - Modern Sass API requirements (Backpack)
   - Vim engine separation (IdeaVim)
   - Performance budgets (all three examples)

**Best Practice:** Start with the core 9 articles and add custom articles only when you have recurring patterns that need constitutional enforcement. Don't add speculative articles "just in case."

---

### NON-NEGOTIABLE Markers

Production teams use **NON-NEGOTIABLE** markers to designate critical, inflexible requirements that cannot be waived or justified away through "Complexity Tracking" exceptions.

**Example from Backpack (Article II, Section 10.3: License Headers):**

```text
### II. Naming & File Conventions (NON-NEGOTIABLE)

**License Headers (NON-NEGOTIABLE)**:

ALL source files MUST include the Apache 2.0 license header at the top. 
This applies to:
- TypeScript files (.ts, .tsx)
- JavaScript files (.js, .jsx)
- Sass/CSS files (.scss, .css)
- Bash scripts (.sh)
```

**Common NON-NEGOTIABLE Use Cases:**

1. **Legal Requirements**
   - License headers (Backpack)
   - Copyright notices
   - Terms of service compliance

2. **Security Policies**
   - Input validation requirements
   - Authentication standards
   - Secrets management (Manifest v2.0.0)

3. **Critical Conventions**
   - Naming patterns that enable tooling
   - API restrictions that prevent breaking changes
   - Accessibility requirements (WCAG compliance)

4. **Compliance Mandates**
   - OWASP Top 10 review requirements
   - Data privacy regulations
   - Industry standards (PCI-DSS, HIPAA, etc.)

**Enforcement Mechanisms:**

NON-NEGOTIABLE requirements are typically enforced through:

- **Pre-commit hooks**: Block commits that violate requirements (e.g., missing license headers)
- **CI checks**: Fail builds that don't meet standards (e.g., accessibility tests)
- **Phase -1 gates**: Constitutional gates in plan templates that cannot be marked "justified exception"
- **Automated linting**: ESLint rules with `error` (not `warn`) severity

**Example from Backpack:** The Backpack repository uses Husky pre-commit hooks + lint-staged to enforce license headers and ESLint rules before code can even be committed.

---

### Version Bump Scenarios from Production

Here are real examples of each version bump type from production constitutions.

#### PATCH Bump Example: Backpack 1.0.1 → 1.0.2

**Changes Made:**

- Updated component name placeholders in templates (`BpkComponentName` → `Bpk[ComponentName]`)
- Changed color token examples (`$bpk-color-primary` → `$bpk-color-white` for better contrast)
- Removed package.json references (monorepo doesn't use individual package.json)
- Corrected Storybook examples directory structure

**Rationale:**

- Purely editorial improvements
- No changes to principles or development process
- Templates updated for consistency and accuracy

**SYNC IMPACT:**

- Existing branches: Don't need regeneration
- New branches: Use improved templates
- Follow-up TODOs: None

**From SYNC IMPACT Report:**

```text
Version: 1.0.1 → 1.0.2 (PATCH - Template improvements and example corrections)
Modified principles: None (content clarifications only)
Changes made:
- Updated component name placeholders across all templates for consistency
- Changed color token examples for better visual contrast demonstration
Templates status:
- ✅ .specify/templates/spec-template.md - Updated with Bpk[ComponentName] format
- ✅ .specify/templates/plan-template.md - Updated structure
- ✅ .specify/templates/tasks-template.md - Updated with correct task sequence
Follow-up TODOs: None - all templates aligned and corrected
```

---

#### MINOR Bump Example: IdeaVim 1.1.0 → 1.2.0

**Changes Made:**

- Added Article X: Vim Compatibility (new constitutional principle)
- Enhanced Section 3.1: Commit Clarity (expanded existing guidance)
- Added Architecture Decision Records requirement (Article VII)

**Rationale:**

- New requirements added for IDE plugin development
- Existing principles unchanged
- Backward compatible—old features don't need to be rewritten

**SYNC IMPACT:**

- New features: Must follow Article X (Vim compatibility)
- Existing features: Grandfathered; no changes required
- Templates: Compatible (Constitution Check verified)

**From Constitution Comment:**

```text
Constitution Version History:

v1.2.0 (Minor update)
- Added Article VII: Architecture Decision Records
- Added Article VI: Documentation Goals

Key Impact: New features must document ADRs and match Vim semantics.
Existing code remains compliant.
```

---

#### MAJOR Bump Example: Manifest 1.1.0 → 2.0.0

**Changes Made:**

- **REMOVED:** All "DEFERRED FOR POC" markers throughout
- **ACTIVATED:** Testing standards now mandatory (was deferred)
- **ACTIVATED:** Performance requirements now mandatory (was deferred)
- **ADDED:** Article VI: Security Standards (entirely new principle)
- **ADDED:** Article VII: Community & Contribution Standards (entirely new principle)
- Changed project phase: POC → MVP/Production

**Rationale:**

- Fundamental shift from proof-of-concept to production-ready
- Requirements that were previously optional are now mandatory
- Process changes from "move fast" to "build it right"

**SYNC IMPACT:**

- ALL active branches: Must regenerate plans to include testing and security
- Templates: Compatible (already had testing sections, now required)
- Follow-up: Update all in-progress features to meet new standards

**From SYNC IMPACT Report:**

```text
SYNC IMPACT REPORT
==================
Version Change: 1.1.0 → 2.0.0 (MVP transition with production-grade requirements)

Removed Sections:
  - POC Scope Declaration (no longer applicable)
  - "DEFERRED FOR POC" markers throughout

Modified Principles:
  - II. Testing Standards: Now mandatory (was deferred)
  - IV. Performance Requirements: Now mandatory (was deferred)

Added Sections:
  - VI. Security Standards (new principle)
  - VII. Community & Contribution Standards (new principle)

Templates Reviewed:
  - .specify/templates/plan-template.md: ✅ Compatible (already has testing sections)
  - .specify/templates/spec-template.md: ✅ Compatible (user stories support MVP)
  - .specify/templates/tasks-template.md: ✅ Compatible (has test task structure)

Follow-up TODOs: None
```

**Key Lesson:** MAJOR bumps signal a fundamental change in project maturity or approach. All stakeholders must be aware that active work needs to be brought into compliance.

---

### Constitution Maturity Patterns

Production projects typically evolve through three maturity phases, reflected in their constitution versions:

#### Phase 1: POC/Prototype (v0.x.x or v1.0.0)

**Characteristics:**

- Core 9 articles with minimal customization
- Focus on rapid iteration and learning
- Permissive complexity limits (loose Article VII enforcement)
- "DEFERRED FOR POC" markers on time-consuming requirements
- Testing standards defined but not strictly enforced

##### Example: Manifest v1.0.0

```text
### II. Testing Standards

All features SHOULD include appropriate test coverage:
- Unit Tests: Public functions SHOULD have unit tests
- [DEFERRED FOR POC] Integration Tests: Full integration testing deferred
- [DEFERRED FOR POC] Coverage Threshold: Target 80%, enforced post-POC
```

**Purpose:** Validate the core idea quickly without over-engineering. The constitution establishes ideals but acknowledges pragmatic shortcuts.

---

#### Phase 2: Production-Ready (v1.x.x or v2.0.0)

**Characteristics:**

- Added project-specific articles (X, XI, XII...)
- Stricter quality gates enforced
- NON-NEGOTIABLE requirements for critical standards
- "DEFERRED" markers removed
- Testing, performance, and security requirements activated

**Example: Manifest v2.0.0** (MAJOR bump marking this transition)

```text
### II. Testing Standards

All features MUST include appropriate test coverage:
- Unit Tests: Every public function MUST have unit tests
- Integration Tests: Features with external dependencies MUST have integration tests
- Coverage Threshold: Minimum 80% line coverage for new features (enforced)
```

**MAJOR Version Bump Rationale:** Removing deferrals and making requirements mandatory is a breaking change to the development workflow. This signals the shift from "build to learn" (POC) to "build to last" (production).

**Typical Additions in Phase 2:**

- Security standards (Article VI in Manifest)
- Community guidelines (Article VII in Manifest)
- Design governance (Article X in Backpack)
- Domain-specific constraints (Article X in IdeaVim)

---

#### Phase 3: Mature Product (v2.x.x+)

**Characteristics:**

- Fine-tuned gates based on production learnings
- Detailed project-specific conventions (12+ articles)
- MINOR/PATCH bumps as process continuously improves
- Constitution reflects battle-tested best practices

**Example: IdeaVim v1.2.2** (mature product with continuous improvement)

- Version 1.2.2 (PATCH): Refined development workflow (trunk-based → feature branches)
- 8 core articles refined over years of production use
- ADR documentation requirement added after realizing knowledge loss

**Example: Backpack v1.0.2** (mature design system)

- 13 articles covering design system, monorepo, accessibility, experimentation
- NON-NEGOTIABLE markers on license, naming, accessibility
- PATCH bumps for template improvements discovered during use

**Characteristics of Mature Constitutions:**

- Specific, not generic (e.g., exact coverage thresholds, exact performance targets)
- Reflect lessons learned from production incidents
- Include enforcement mechanisms (hooks, CI checks)
- Updated quarterly or semi-annually based on team retrospectives

---

### Real-World Constitution Examples

The `artifacts/` directory contains four complete constitution examples from production projects. These demonstrate the patterns described above in full context.

#### 1. [IdeaVim Constitution](../artifacts/constitution-example-ideavim.md) (v1.2.2)

**Project:** JetBrains IdeaVim (10,000+ stars on GitHub)  
**Domain:** IDE Plugin Development  
**Constitution Size:** 279 lines  
**Version History:** v1.0.0 → v1.2.2 (MINOR bumps)  

**Key Features:**

- **Article X: Vim Compatibility** - Domain-specific requirement for IDE plugins
- **Multi-product architecture** - Separates vim-engine from IdeaVim plugin
- **Real-world evolution** - Shows how constitution evolves with team learnings
  - v1.2.2: Changed from trunk-based to feature branches after production merge conflicts

**When to Study This:**

- Building IDE plugins or editor extensions
- Multi-product architectures with shared engines
- Balancing compatibility requirements with product goals

---

#### 2. [Backpack Constitution](../artifacts/constitution-example-backpack.md) (v1.0.2)

**Project:** Skyscanner Backpack Design System  
**Domain:** Design System & Component Library  
**Constitution Size:** 604 lines  
**Version History:** v1.0.0 → v1.0.2 (PATCH bumps)  

**Key Features:**

- **13 Articles** - Most comprehensive example
- **NON-NEGOTIABLE markers** - Enforces critical standards (license headers, accessibility)
- **Design-first governance** - Article X requires design approval before code
- **Monorepo conventions** - 95+ packages with strict naming and structure
- **Modern Sass API** - Mandates `@use` syntax, bans `@import`

**When to Study This:**

- Building design systems or component libraries
- Monorepo projects with many packages
- Projects with legal/compliance requirements (license headers)
- Accessibility-first development

---

#### 3. [Manifest Constitution](../artifacts/constitution-example-manifest.md) (v2.0.0)

**Project:** Manifest Generator (CLI Tool)  
**Domain:** CLI Tools & Code Generation  
**Constitution Size:** 300 lines  
**Version History:** v1.0.0 (POC) → v1.1.0 → v2.0.0 (MAJOR - Production transition)  

**Key Features:**

- **POC → Production evolution** - Best example of maturity progression
- **MAJOR bump rationale** - Shows what qualifies as breaking change
- **Security standards added** - Article VI added in v2.0.0
- **Community standards** - Article VII for open-source projects
- **"DEFERRED FOR POC" pattern** - Shows how to defer requirements during rapid prototyping

**When to Study This:**

- Starting a new project (see v1.0.0 for POC approach)
- Transitioning from POC to production (see v2.0.0 MAJOR bump)
- Adding security/community requirements
- CLI tool development patterns

---

#### 4. [Taskify Constitution](../artifacts/constitution-example.md) (Tutorial)

**Project:** Taskify (Educational Example)  
**Domain:** Web Application (Tutorial)  
**Constitution Size:** ~200 lines  
**Purpose:** Teaching tool  

**Key Features:**

- Demonstrates all 9 core articles
- Simpler than production examples (easier to learn from)
- Companion to Spec-Driven Development tutorials

**When to Study This:**

- Learning Spec-Kit for the first time
- Understanding the core 9 articles
- Building your first constitution

---

**Comparison Table:**

| Constitution | Version | Lines | Articles | Domain | Use Case |
|--------------|---------|-------|----------|--------|----------|
| **Taskify** | 1.0.0 | ~200 | 9 | Tutorial | Learning Spec-Kit |
| **IdeaVim** | 1.2.2 | 279 | 10 | IDE Plugin | Multi-product, compatibility |
| **Manifest** | 2.0.0 | 300 | 7 | CLI Tool | POC → Production evolution |
| **Backpack** | 1.0.2 | 604 | 13 | Design System | Large monorepo, compliance |

**Recommendation:** Start with Taskify to learn the fundamentals, then study the production example that best matches your project's domain and maturity stage.

---

## See Also

- [Spec-Driven Development](spec-driven-development.md) - Core methodology
- [Constitutional Gates](../tricks/constitutional-gates.md) - Phase -1 enforcement
- [Avoiding Over-Engineering](../tricks/avoiding-over-engineering.md) - Articles VII & VIII in practice
- [Constitution Example (Taskify)](../artifacts/constitution-example.md) - All 9 articles applied
- [Constitution Evolution FAQ](../faq/constitution-evolution.md) - Version management guide
- [IdeaVim Constitution Example](../artifacts/constitution-example-ideavim.md) - MINOR bump (v1.2.2)
- [Backpack Constitution Example](../artifacts/constitution-example-backpack.md) - PATCH bump (v1.0.2)
- [Manifest Constitution Example](../artifacts/constitution-example-manifest.md) - MAJOR bump (v2.0.0)
