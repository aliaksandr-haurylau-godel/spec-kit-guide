---
version: 1
created_date: 2026-02-15
derived_from: raw_data/2026-02-15-real-world-spec-kit-artifacts/ (IdeaVim, Backpack, Manifest examples)
---

# Spec-Kit Artifact Structure

This document provides comprehensive documentation of all Spec-Kit artifact types, their structures, relationships, and usage patterns. It combines theoretical guidance with real-world examples from production projects.

## Table of Contents

1. [Overview](#overview)
2. [The spec.md Artifact](#the-specmd-artifact)
3. [The plan.md Artifact](#the-planmd-artifact)
4. [The tasks.md Artifact](#the-tasksmd-artifact)
5. [The research.md Artifact (Optional)](#the-researchmd-artifact-optional)
6. [The constitution.md Artifact](#the-constitutionmd-artifact)
7. [Artifact Relationships](#artifact-relationships)
8. [Comparison: Tutorial vs. Real-World](#comparison-tutorial-vs-real-world)
9. [Best Practices](#best-practices)
10. [See Also](#see-also)

---

## Overview

Spec-Kit artifacts are structured documents that guide AI-assisted software development from idea to implementation. Each artifact serves a specific purpose in the Spec-Driven Development workflow, with clear boundaries between "what to build" (specifications) and "how to build it" (implementation plans).

### The Five Core Artifacts

| Artifact | Purpose | Required? | When Created |
|----------|---------|-----------|--------------|
| **constitution.md** | Immutable architectural principles | ✅ Always | Once per project (evolves with versions) |
| **spec.md** | Defines WHAT to build and WHY | ✅ Always | Start of each feature |
| **research.md** | Technical investigations and decisions | ❌ Optional (~20% of features) | During specification when unknowns exist |
| **plan.md** | Defines HOW to implement | ✅ Always | After spec approval |
| **tasks.md** | Executable task breakdown | ✅ Always | After plan approval |

### Generation Flow

The artifacts follow a strict sequential generation pattern, with each artifact building on previous ones:

```text
constitution.md (project principles)
    ↓
spec.md (WHAT to build)
    ↓ (if complex decisions needed)
research.md (technical investigations) [OPTIONAL]
    ↓
plan.md (HOW to build, references research K-decisions)
    ↓
tasks.md (execution breakdown)
```

**Key Principle:** Each artifact type serves a distinct purpose and must not mix concerns. Specifications define requirements without prescribing implementation; plans define technical approaches without creating tasks; tasks define actionable work items with acceptance criteria.

---

## The spec.md Artifact

### Purpose

The specification defines **WHAT** needs to be built and **WHY**, without prescribing **HOW**. It serves as the contract between product requirements and technical implementation.

**Core Function:**

- Captures user needs and acceptance criteria
- Documents functional and non-functional requirements
- Identifies constraints and boundaries (in scope / out of scope)
- Provides clarity for implementation planning

### Standard Structure

Every spec.md follows this structure:

```markdown
---
version: 1
feature: [Feature Name]
branch: [branch-name]
effort: [S/M/L]
complexity: [Low/Medium/High]
sync_impact: [constitution version]
---

# Feature Specification: [Name]

## Overview
Brief summary of the feature

## User Stories
As a [role], I want [goal] so that [benefit]
- Acceptance Criteria (testable conditions)

## Functional Requirements
FR-001: System MUST/SHOULD...

## Non-Functional Requirements
NFR-001: Performance/Security/Usability requirements

## Constraints
Technical and organizational limitations

## Out of Scope
Explicitly excluded features

## Success Criteria
Measurable outcomes (SC-001, SC-002...)

## Clarifications
Dated Q&A sessions resolving ambiguities
```

### Real-World Patterns

#### 1. Clarifications Sections

Production specs use dated clarification sessions to document requirement evolution:

**Example from Backpack Nx Migration:**

```markdown
## Clarifications

### Session 2026-01-27

- Q: What directory hierarchy structure should be adopted? 
- A: Flat structure - all packages remain under `packages/` without subdirectory categorization

### Session 2026-01-27 (Banana Merge Planning)

- Q: What is Backpack's target location in Banana repository? 
- A: `libs/shared/universal/backpack/` - as a sub-library under universal
```

**Why This Pattern?**

- Preserves context of decisions
- Shows evolution of understanding
- Prevents re-litigating resolved questions
- Provides audit trail for future reference

#### 2. [NEEDS CLARIFICATION] Markers

Use explicit markers for unknowns rather than making assumptions:

```markdown
## Functional Requirements

FR-001: System MUST support user authentication [NEEDS CLARIFICATION: OAuth2, SAML, or both?]
FR-002: Tasks MUST be assignable to users
FR-003: Dashboard MUST load within [NEEDS CLARIFICATION: 2s? 5s? Target performance?]
```

**Why This Pattern?**

- Forces explicit identification of gaps
- Prevents premature technical decisions
- Creates clear checklist for spec completion
- Enables systematic clarification dialogues

#### 3. Metadata Frontmatter

Production specs include metadata for tracking and planning:

```yaml
---
version: 1
source_date: 2026-02-15
feature: IdeaVim Extension API Layer
branch: 001-api-layer
effort: L  # S (small), M (medium), L (large)
complexity: High  # Low, Medium, High
sync_impact: 1.2.2  # Constitution version this spec follows
estimate: 8 weeks
priority: P1
type: API Design & Refactoring
---
```

**Key Metadata Fields:**

- **effort**: Time investment (S: <1 week, M: 1-3 weeks, L: >3 weeks)
- **complexity**: Technical difficulty independent of size
- **sync_impact**: Constitution version (alerts when constitution updates)
- **type**: Feature category (helps pattern matching for similar work)

### Examples with Links

#### Tutorial Example (Simple)

**File:** [`artifacts/spec-example-taskify.md`](../artifacts/spec-example-taskify.md)

**Stats:** ~305 lines  
**Complexity:** Low (web app feature)  
**Use Case:** Learning Spec-Kit fundamentals

**Structure Highlights:**

- 5 user stories with detailed acceptance criteria
- Clear persona definition (PM, 4 Engineers)
- Sample data tables for seeding
- Explicit out-of-scope section (auth, notifications)
- Review & acceptance checklist

**When to Reference:** Building straightforward user-facing features with well-understood requirements.

---

#### Complex Feature Example

**File:** [`artifacts/spec-example-ideavim-api.md`](../artifacts/spec-example-ideavim-api.md)

**Stats:** 249 lines  
**Complexity:** High (architectural refactoring)  
**Use Case:** Large-scale API design with multiple stakeholders

**Structure Highlights:**

- **Prioritized user stories** (P1, P2, P3) for phased delivery
- **Independent testing criteria** for each story
- **Given/When/Then acceptance scenarios** (BDD style)
- **Known Issues to Address** section documenting prior analysis
- **Edge cases explicitly listed** (extensions using both APIs, version mismatches)

**Unique Features:**

- Companion `research.md` artifact (418 lines) for complex design decisions
- User input preserved in frontmatter (shows original problem statement)
- External plugin migration considerations

**When to Reference:** Designing APIs, refactoring architectures, multi-month projects with external dependencies.

---

#### Infrastructure/Tooling Example

**File:** [`artifacts/spec-example-backpack-nx.md`](../artifacts/spec-example-backpack-nx.md)

**Stats:** 355 lines (longest spec example)  
**Complexity:** Medium  
**Use Case:** Infrastructure changes, build system migrations

**Structure Highlights:**

- **Strategic Context** section explaining business motivation (not just technical details)
- **Clarifications** with multiple dated sessions (shows iterative refinement)
- **Current → Future state** documentation (mapping existing to target structure)
- **Effort/Estimate metadata**: L effort, S complexity, 2-week estimate

**Unique Features:**

- Non-user-facing feature (no traditional user stories)
- Focus on organizational alignment (PE team approval, standards compliance)
- References internal documentation (sanitized for guide)
- Migration considerations and rollback plans

**When to Reference:** DevOps work, build system changes, organizational process improvements, monorepo migrations.

---

## The plan.md Artifact

### Purpose

The implementation plan defines **HOW** to implement the specification. It bridges the gap between requirements (spec.md) and executable tasks (tasks.md).

**Core Function:**

- Selects technology stack with rationale
- Defines system architecture and data models
- Breaks implementation into phases
- Documents constitutional compliance
- Tracks complexity and justifies exceptions

### Standard Structure

```markdown
---
version: 1
companion_spec: spec-example-name.md
---

# Implementation Plan: [Feature Name]

## Summary
High-level approach to implementing the spec

## Technology Stack
- Frontend: [framework + rationale]
- Backend: [framework + rationale]
- Database: [choice + rationale]

## System Architecture
Diagram and component descriptions

## Phase -1: Pre-Implementation Constitutional Gates

### Simplicity Gate (Article VII)
- [ ] Using ≤3 projects?
- [ ] No future-proofing?

### Anti-Abstraction Gate (Article VIII)
- [ ] Using framework directly?
- [ ] Single model representation?

### Integration-First Gate (Article IX)
- [ ] Contracts defined?
- [ ] Contract tests written?
- [ ] Real database in tests?

## Data Model
Entity definitions and relationships

## API Contracts
Endpoint specifications

## Core Implementation Phases

### Phase 0: Research (if needed)
Document in research.md

### Phase 1: Foundation
Database schema, core models

### Phase 2: Core Features
Main functionality implementation

### Phase 3+: Additional Features
Enhancements and polish

## Testing Strategy
Unit, integration, contract, E2E tests

## Complexity Tracking
Constitutional violations with justifications

## Research Notes
References to research.md K-decisions
```

### Real-World Patterns

#### 1. Phase -1 Constitutional Gates

**All production plans** include Phase -1 gates as constitutional compliance checkpoints:

**Example from IdeaVim API Plan:**

```markdown
## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

| Principle                          | Compliance            | Notes                                                   |
|------------------------------------|-----------------------|---------------------------------------------------------|
| I. Vim Compatibility (IDE-First)   | ✅ PASS                | API exposes Vim functionality, doesn't change IDE       |
| II. IntelliJ Platform Integration  | ✅ PASS                | Uses IntelliJ Platform SDK patterns, XML extension      |
| III. vim-engine Separation         | ⚠️ REQUIRES ATTENTION | API module should not depend on vim-engine internals    |
| IV. Code Quality Standards         | ✅ PASS                | Will include tests and documentation                    |
```

**Why This Pattern?**

- Prevents over-engineering before it starts
- Documents intentional complexity with justification
- Creates systematic review checkpoint
- Operationalizes constitutional principles

#### 2. Complexity Tracking with Article VII Exceptions

When projects exceed constitutional limits, document justification:

**Example Pattern:**

```markdown
## Complexity Tracking

### Article VII Exception: 4 Projects Used

**Constitutional Limit:** Maximum 3 projects  
**Actual Count:** 4 projects

**Justification:**
We require 4 projects due to security boundary requirements:
1. `auth-lib` - Authentication library (sensitive credential handling)
2. `api-gateway` - Public API surface
3. `core-services` - Business logic
4. `database-migrations` - Schema versioning (run separately in prod)

**Why Not 3?** Combining auth-lib with api-gateway would violate Article VIII 
(security wrapper anti-pattern). Combining migrations with core-services 
creates deployment coupling.

**Approval:** Reviewed with tech lead 2026-02-10, documented in ADR-042
```

#### 3. K-Numbered Research References

Plans reference research.md decisions with K-numbers:

**Example from IdeaVim Plan:**

```markdown
### Phase 1: API Finalization (P1 Priority)

Resolve critical issues and API gaps before migration:

1. **State Update Safety (K1)**
   - Review all mode-changing operations in VimApi
   - Ensure combined operations handle: selection, marks, state machine
   - Implementation: See research.md K1 decision for full analysis

2. **Editor Context Fix (K2)**
   - Add optional editor parameter (see research.md K2)
   - Provide explicit `withEditor(editor) { ... }` scope
```

**Why This Pattern?**

- Keeps plan focused on "how" without duplicating "why"
- Enables cross-referencing to detailed research rationale
- Separates strategic decisions from tactical implementation
- Creates reusable decision documentation

### Examples with Links

#### Tutorial Example

**File:** [`artifacts/plan-example-taskify.md`](../artifacts/plan-example-taskify.md)

**Stats:** ~242 lines  
**Structure:** Basic 3-phase plan (Database, API, Frontend)  
**Use Case:** Simple web application

**Key Sections:**

- Technology stack with rationale (Blazor, .NET Aspire, PostgreSQL)
- Phase -1 gates (all passing, no exceptions)
- Test-first execution order (contracts → integration → E2E → unit)
- File creation order enforcing TDD

**When to Reference:** Standard web application development with established technology stacks.

---

#### Complex Example

**File:** [`artifacts/plan-example-ideavim-api.md`](../artifacts/plan-example-ideavim-api.md)

**Stats:** 307 lines  
**Complexity:** High (multi-month architectural refactoring)

**Structure Highlights:**

- **Constitution Check table** with compliance status per principle
- **Known Issues to Resolve** section (K1-K4) with resolution paths
- **API Gaps tracking** (G1-G6) with priorities
- **Migration Status table** tracking 12+ built-in extensions
- **Phased delivery** (4 phases: Finalization, Internal Migration, External Migration, Stabilization)
- **Decision Log** with ADR references

**Unique Features:**

- **Phase -1 includes "REQUIRES ATTENTION"** flag (vim-engine separation concern)
- External plugin compatibility considerations (9 plugins listed)
- Prior art analysis (4 extensions fully migrated, serving as reference implementations)

**When to Reference:** Large refactorings, API design, multi-stakeholder projects requiring phased rollouts.

---

#### Infrastructure Example

**File:** [`artifacts/plan-example-backpack-nx.md`](../artifacts/plan-example-backpack-nx.md)

**Stats:** 480 lines (longest plan example)  
**Use Case:** Infrastructure migration with forward planning

**Structure Highlights:**

- **Banana Merge Context** section (planning for future repository integration)
- **Target location mapping** showing current → future paths
- **Requirement alignment table** comparing Banana patterns to Backpack structure
- **Scope boundaries clearly defined** (in scope / out of scope / future milestones)
- **Configuration files requiring updates** (explicit list of 8+ config files)

**Unique Features:**

- Non-code deliverable focus (structure documentation)
- Organizational alignment emphasis (PE team, web-enablement team reviews)
- Risk mitigation strategies (atomic commits, rollback plan)
- Forward compatibility (prepares for future Banana integration without premature changes)

**When to Reference:** Infrastructure projects, repository migrations, organizational coordination efforts.

---

## The tasks.md Artifact

### Purpose

The tasks artifact breaks down the implementation plan into **executable tasks** with effort estimates, dependencies, and acceptance criteria.

**Core Function:**

- Translate plan phases into actionable work items
- Provide effort estimates (S/M/L)
- Define task dependencies
- Specify verification criteria
- Enable progress tracking

### Standard Structure

```markdown
---
version: 1
companion_plan: plan-example-name.md
total_effort: [X days/weeks]
complexity: [Low/Medium/High]
team_size: [number]
---

# Implementation Tasks: [Feature Name]

## Metadata

**Total Effort:** [X story points / days]  
**Complexity:** [Low/Medium/High]  
**Team Size:** [number] developers  
**Estimated Duration:** [X weeks]

## Phase 0: Foundation

### Task 0.1: [Task Name]
**Effort:** S (2-4 hours)  
**Complexity:** Low  
**Dependencies:** None  
**Assignee:** [TBD]

**Description:**
What needs to be done

**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2

**Implementation Notes:**
Technical hints or constraints

---

## Phase 1: Core Features

### Task 1.1: [Task Name]
**Effort:** M (1-2 days)  
**Complexity:** Medium  
**Dependencies:** Task 0.1, Task 0.3  
**Assignee:** [TBD]

[Same structure as above]
```

### Real-World Patterns

#### 1. Effort Estimates with Definitions

Production tasks use consistent sizing:

| Size | Time Range | Description |
|------|------------|-------------|
| **S (Small)** | 2-4 hours | Single file change, simple logic, well-understood |
| **M (Medium)** | 1-2 days | Multiple file changes, moderate complexity, some unknowns |
| **L (Large)** | 3-5 days | Cross-cutting changes, high complexity, research needed |

**Example from IdeaVim Tasks:**

```markdown
### Task 1.1: Implement Editor Context Parameter
**Effort:** M (1-2 days)  
**Complexity:** Medium  
**Dependencies:** K2 research decision

**Why M not S?** Requires changes to 8 API methods, backward compatibility 
testing with existing extensions, and documentation updates.
```

#### 2. Dependency Notation

Tasks clearly specify blocking dependencies:

```markdown
### Task 2.3: Migrate surround extension
**Effort:** L (3-5 days)  
**Complexity:** High  
**Dependencies:** 
- Task 1.4 (findBlockTagRange API implemented)
- Task 1.6 (modal input support added)
- External: AceJump plugin compatibility verified

**Blocking:** Tasks 2.4, 2.5, 2.6 (other extensions waiting for pattern validation)
```

**Why This Pattern?**

- Enables critical path analysis
- Identifies parallelizable work
- Prevents starting blocked tasks
- Makes external dependencies explicit

#### 3. Acceptance Criteria with Verification Commands

Tasks include specific verification steps:

```markdown
### Task 1.2: Add Database Schema Migration

**Acceptance Criteria:**
- [ ] Migration file created: `migrations/001_create_users.sql`
- [ ] Migration applies successfully: `npm run migrate`
- [ ] Schema matches spec.md User entity definition
- [ ] Rollback script tested: `npm run migrate:rollback`
- [ ] Migration documented in migrations/README.md

**Verification Commands:**
```bash
# Apply migration
npm run migrate

# Verify schema
psql -d taskify -c "\d users"

# Expected output: table with id, name, email, role columns
```

### Examples with Links

#### Tutorial Example

**File:** [`artifacts/tasks-example-taskify.md`](../artifacts/tasks-example-taskify.md)

**Stats:** ~150 lines  
**Task Count:** 12 tasks  
**Structure:** Simple linear progression

**Highlights:**

- Clear phase checkpoints after each user story
- Test-first ordering (contract tests → implementation → UI)
- Straightforward dependencies (mostly sequential)

**When to Reference:** Simple features with clear requirements and established patterns.

---

#### Complex Example

**File:** [`artifacts/tasks-example-ideavim-api.md`](../artifacts/tasks-example-ideavim-api.md)

**Stats:** 271 lines  
**Task Count:** 23 tasks across 4 phases  
**Complexity:** High

**Structure Highlights:**

- **Phase grouping** aligns with plan.md phases
- **Migration tracking table** for 12+ extensions
- **Parallel task opportunities** explicitly noted
- **External dependencies** documented (AceJump plugin, TeamCity CI)

**Unique Features:**

- Tasks reference research.md K-decisions (K1, K2, K3, K4)
- Migration tasks ordered by complexity (easiest to hardest)
- Explicit rollback/verification tasks for API stability

**When to Reference:** Multi-phase projects with external dependencies and migration requirements.

---

#### Infrastructure Example

**File:** [`artifacts/tasks-example-backpack-nx.md`](../artifacts/tasks-example-backpack-nx.md)

**Stats:** 261 lines  
**Task Count:** 18 tasks  
**Focus:** Configuration and validation

**Structure Highlights:**

- Tasks focus on documentation and verification (not code generation)
- Configuration update tasks grouped by subsystem (CI, Jest, Storybook)
- Validation checkpoints after each configuration change

**Unique Features:**

- "Structure mapping document" as primary deliverable (not code)
- PE team review tasks (organizational approval gates)
- Cross-repository compatibility tasks (Banana alignment verification)

**When to Reference:** Infrastructure work, repository reorganizations, multi-repo coordination.

---

## The research.md Artifact (Optional)

### Purpose

The research artifact documents **technical investigations** and **decision rationales** for complex features. It answers "why" questions that would clutter the spec or plan.

**When to Create research.md:**

- Technology choices are non-obvious or contested
- Performance benchmarks are needed to inform decisions
- Compatibility/integration unknowns exist
- Multiple architectural approaches need evaluation
- Team needs shared understanding before implementation

**When to Skip research.md:**

- Feature uses established patterns from previous work
- Technology choices are obvious (company standards)
- Implementation approach is straightforward
- No significant unknowns or trade-offs exist

**Estimated Usage:** ~20% of feature branches in production projects.

### Standard Structure

```markdown
---
version: 1
companion_spec: spec-example-name.md
artifact_type: research
---

# Research: [Feature Name] Design Decisions

## Overview
Scope of investigation and questions to answer

## K1: [Question/Decision Title]

### Problem Statement
What needs to be decided?

### Context
Current state and constraints

### Options Considered

#### Option A: [Name]
**Pros:**
- Advantage 1
- Advantage 2

**Cons:**
- Drawback 1
- Drawback 2

**Benchmarks/Data:**
- Metric 1: [value]
- Metric 2: [value]

#### Option B: [Name]
[Same structure]

### Decision
**CHOSEN:** Option A

### Rationale
Why this option was selected

### Confidence Level
**HIGH** / MEDIUM / LOW

### Implementation Notes
Technical hints for plan.md and tasks.md

### Status
**Decision:** Approved  
**Implementation:** [Pending/In Progress/Complete]

---

## K2: [Next Decision]
[Same structure]
```

### Real-World Patterns

#### 1. K-Numbered Format

The "K" numbering system (K1, K2, K3...) enables easy cross-referencing:

**In research.md:**

```markdown
## K1: State Update Complexity

### Decision
Use safe combined operations instead of atomic setters.
```

**In plan.md:**

```markdown
### Phase 1: API Finalization

1. **State Update Safety (K1)**
   - Implement combined mode-change operations
   - See research.md K1 for full decision rationale
```

**In tasks.md:**

```markdown
### Task 1.1: Implement enterInsertMode()
**Dependencies:** K1 research decision approved
**Implementation:** Follow K1 combined operation pattern
```

**Why K-Numbers?**

- Unique identifiers persist across document edits
- Easy to search (`grep "K3"` finds all references)
- More stable than section numbers (don't change when adding new decisions)
- Feels like an authoritative knowledge base

#### 2. Benchmarking Tables

Include performance data when relevant:

**Example Pattern:**

```markdown
## K3: Parser Selection

### Options Considered

#### Option A: Tree-sitter
**Benchmarks:**

| File Size | Parse Time | Memory Usage |
|-----------|-----------|--------------|
| 1 KB | 0.8ms | 45 KB |
| 10 KB | 3.2ms | 180 KB |
| 100 KB | 28ms | 1.2 MB |

#### Option B: Regex-based
**Benchmarks:**

| File Size | Parse Time | Memory Usage |
|-----------|-----------|--------------|
| 1 KB | 0.3ms | 12 KB |
| 10 KB | 12ms | 85 KB |
| 100 KB | 980ms | 450 KB |

### Decision
**CHOSEN:** Tree-sitter

**Rationale:** O(n) complexity vs O(n²) for regex. Performance gap widens 
with file size. Memory overhead acceptable for editor use case.
```

#### 3. Confidence Levels

Document uncertainty explicitly:

```markdown
## K4: Coroutine Usage in Read/Write Locks

### Decision
Remove suspend functions from lock-sensitive paths

### Confidence Level
**MEDIUM** - IntelliJ Platform docs don't explicitly forbid this, but 
production logs show deadlocks. Removing suspend resolves issues, but 
root cause unclear.

### Follow-Up Investigation
- [ ] File IntelliJ Platform support ticket
- [ ] Test on IntelliJ 2024.2+ (may have fixed underlying issue)
```

**Confidence Definitions:**

- **HIGH:** Decision backed by data, team consensus, proven patterns
- **MEDIUM:** Decision addresses problem but unknowns remain
- **LOW:** Best guess; may need revision after initial implementation

### Example with Link

#### IdeaVim API Research (Only Example)

**File:** [`artifacts/research-example-ideavim-api.md`](../artifacts/research-example-ideavim-api.md)

**Stats:** 418 lines (longest artifact in guide)  
**Decisions Documented:** 5 major K-decisions (K1-K5)  
**Complexity:** High (architectural API design)

**Structure Highlights:**

- **K1: State Update Complexity** - Safe combined operations vs atomic setters
- **K2: Editor Context** - Explicit editor parameter vs always-focused
- **K3: Coroutine Usage** - Suspend function audit and removal strategy
- **K4: API Gaps** - Missing functionality analysis with priority assignments
- **K5: Extension Discovery** - XML registration vs KSP annotation comparison

**Unique Features:**

- Alternatives considered with pros/cons tables
- References to prior analysis (logseq notes, team discussions)
- Implementation notes for plan.md/tasks.md consumption
- Status tracking (Approved, Pending, In Progress)

**Why This Is the Only research.md Example:**
Research artifacts are **rare** (~20% of features). Most features don't need them. This example demonstrates when research is valuable: complex architectural decisions with multiple viable approaches and significant trade-offs.

**When to Reference:** API design, technology selection with multiple contenders, performance-critical decisions requiring benchmarks.

---

## The constitution.md Artifact

### Purpose

The constitution defines **immutable architectural principles** that govern all specifications and implementations. It acts as the "architectural DNA" of the project.

**Core Function:**

- Establishes non-negotiable development standards
- Prevents over-engineering through complexity gates
- Ensures consistency across features and time
- Enables constitutional compliance checks in plan.md

### Standard Structure

```markdown
---
version: [MAJOR.MINOR.PATCH]
constitution_version: [same as version]
previous_version: [previous MAJOR.MINOR.PATCH]
sync_impact: [summary of changes]
---

# [Project Name] Constitution

## Core Principles

### Article I: Library-First Principle
[Standard text from Spec-Kit framework]
[Project-specific adaptations if needed]

### Article II: CLI Interface Mandate
[Standard text]

### Article III: Test-First Imperative
[Standard text - NON-NEGOTIABLE]

### Article IV: Documentation Principle
[Standard text]

### Article V: Versioning Principle
[Standard text]

### Article VI: Dependency Management
[Standard text]

### Article VII: Simplicity Principle
[Standard text with complexity limits]

### Article VIII: Anti-Abstraction Principle
[Standard text]

### Article IX: Integration-First Testing
[Standard text]

---

## Project-Specific Articles

### Article X: [Domain-Specific Principle]
[Custom article for your project domain]

### Article XI: [Additional Principle]
[Additional custom articles as needed]
```

### Real-World Patterns

#### 1. SYNC IMPACT Version Tracking

Production constitutions use **SYNC IMPACT** declarations to track version changes:

**Example from IdeaVim v1.2.2:**

```yaml
---
version: 1.2.2
constitution_version: 1.2.2
previous_version: 1.2.1
---
```

**SYNC IMPACT Report (embedded as comment):**

```markdown
<!--
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
-->
```

**Why This Pattern?**

- Alerts teams when constitution changes affect active branches
- Documents template compatibility after changes
- Creates audit trail for constitutional evolution
- Enables "regenerate plan if constitution changed" workflows

#### 2. Version Semantics (MAJOR.MINOR.PATCH)

Constitution versions follow semantic versioning with specific meanings:

##### PATCH (x.y.Z): Editorial Changes Only

**Example:** Backpack 1.0.1 → 1.0.2

**Changes:**

- Updated component name placeholders in templates (`BpkComponentName` → `Bpk[ComponentName]`)
- Changed color token examples for better contrast
- Removed package.json references (monorepo doesn't use them)
- Corrected Storybook examples directory structure

**SYNC IMPACT:**

- Existing branches: Don't need regeneration
- New branches: Use improved templates
- Follow-up TODOs: None

**Characteristics:**

- Typo fixes, reformatting, clarifications
- No process or principle changes
- Existing work remains compliant

---

##### MINOR (x.Y.0): Backward-Compatible Additions

**Example:** IdeaVim 1.1.0 → 1.2.0

**Changes:**

- **Added** Article X: Vim Compatibility (new constitutional principle)
- **Enhanced** Section 3.1: Commit Clarity (expanded existing guidance)
- **Added** Architecture Decision Records requirement (Article VII)

**SYNC IMPACT:**

- New features: Must follow Article X (Vim compatibility)
- Existing features: Grandfathered; no changes required
- Templates: Compatible (Constitution Check verified)

**Characteristics:**

- New articles or sections added
- Enhanced guidance that doesn't contradict existing principles
- Existing code remains compliant

---

##### MAJOR (X.0.0): Breaking Changes

**Example:** Manifest 1.1.0 → 2.0.0 (POC → Production transition)

**Changes:**

- **REMOVED:** All "DEFERRED FOR POC" markers throughout
- **ACTIVATED:** Testing standards now mandatory (was deferred)
- **ACTIVATED:** Performance requirements now mandatory (was deferred)
- **ADDED:** Article VI: Security Standards (entirely new principle)
- **ADDED:** Article VII: Community & Contribution Standards

**SYNC IMPACT:**

- ALL active branches: Must regenerate plans to include testing and security
- Templates: Compatible (already had testing sections, now required)
- Follow-up: Update all in-progress features to meet new standards

**Characteristics:**

- Removed or fundamentally rewritten articles
- Incompatible principle changes
- Requires re-validation of all active work

---

#### 3. Project-Specific Articles (Beyond Article IX)

While Spec-Kit defines 9 standard articles, production projects extend with domain-specific requirements:

##### Article X Example: IdeaVim (Vim Compatibility)

```markdown
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

**Why This Article?** IDE plugins have unique compatibility requirements—balancing emulation fidelity with host application integrity.

---

##### Article X Example: Backpack (Design-Anchored Development)

```markdown
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

**Why This Article?** Design systems require design governance to maintain visual consistency across products.

---

#### 4. NON-NEGOTIABLE Markers

Production teams use **NON-NEGOTIABLE** markers for inflexible requirements:

**Example from Backpack (License Headers):**

```markdown
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

1. **Legal Requirements** - License headers, copyright notices
2. **Security Policies** - Input validation, authentication standards
3. **Critical Conventions** - Naming patterns that enable tooling
4. **Compliance Mandates** - WCAG accessibility, OWASP Top 10

**Enforcement Mechanisms:**

- Pre-commit hooks (block commits that violate requirements)
- CI checks (fail builds that don't meet standards)
- Phase -1 gates (cannot be marked "justified exception")
- Automated linting (error severity, not warnings)

### Examples with Links

#### 1. Taskify Constitution (Tutorial)

**File:** [`artifacts/constitution-example.md`](../artifacts/constitution-example.md)

**Version:** 1.0.0 (stable)  
**Lines:** ~200  
**Articles:** 9 (core articles only)  
**Purpose:** Teaching tool

**Use Case:** Learning Spec-Kit for the first time, understanding core 9 articles, building your first constitution.

---

#### 2. IdeaVim Constitution (IDE Plugin)

**File:** [`artifacts/constitution-example-ideavim.md`](../artifacts/constitution-example-ideavim.md)

**Version:** 1.2.2 (evolved)  
**Lines:** 279  
**Articles:** 10 (9 core + Article X: Vim Compatibility)  
**Domain:** IDE Plugin Development

**Key Features:**

- Multi-product architecture (vim-engine separation)
- Real-world evolution (v1.2.2 changed trunk-based → feature branches after production merge conflicts)
- ADR integration (Article VII requires Architecture Decision Records)

**Use Case:** Building IDE plugins or editor extensions, multi-product architectures with shared engines, balancing compatibility requirements with product goals.

---

#### 3. Backpack Constitution (Design System)

**File:** [`artifacts/constitution-example-backpack.md`](../artifacts/constitution-example-backpack.md)

**Version:** 1.0.2 (evolved)  
**Lines:** 604 (most comprehensive example)  
**Articles:** 13 (9 core + 4 custom)  
**Domain:** Design System & Component Library

**Key Features:**

- NON-NEGOTIABLE markers (license headers, accessibility)
- Design-first governance (Article X: Design approval before code)
- Monorepo conventions (95+ packages with strict naming)
- Modern Sass API requirements (mandates `@use` syntax, bans `@import`)

**Use Case:** Building design systems or component libraries, monorepo projects with many packages, projects with legal/compliance requirements, accessibility-first development.

---

#### 4. Manifest Constitution (CLI Tool)

**File:** [`artifacts/constitution-example-manifest.md`](../artifacts/constitution-example-manifest.md)

**Version:** 2.0.0 (MAJOR bump - POC → Production)  
**Lines:** 300  
**Articles:** 7 (customized from core 9)  
**Domain:** CLI Tools & Code Generation

**Key Features:**

- POC → Production evolution (best example of maturity progression)
- MAJOR bump rationale (removed "DEFERRED FOR POC" markers)
- Security standards added (Article VI in v2.0.0)
- Community standards (Article VII for open-source projects)

**Use Case:** Starting a new project (see v1.0.0 for POC approach), transitioning from POC to production (see v2.0.0 MAJOR bump), adding security/community requirements, CLI tool development patterns.

---

### Constitution Selection Table

| Constitution | Version | Lines | Articles | Domain | Use Case |
|--------------|---------|-------|----------|--------|----------|
| **Taskify** | 1.0.0 | ~200 | 9 | Tutorial | Learning Spec-Kit |
| **IdeaVim** | 1.2.2 | 279 | 10 | IDE Plugin | Multi-product, compatibility |
| **Manifest** | 2.0.0 | 300 | 7 | CLI Tool | POC → Production evolution |
| **Backpack** | 1.0.2 | 604 | 13 | Design System | Large monorepo, compliance |

**Recommendation:** Start with Taskify to learn the fundamentals, then study the production example that best matches your project's domain and maturity stage.

---

## Artifact Relationships

### Generation Flow

Artifacts are generated sequentially, with each building on previous ones:

```text
constitution.md (immutable principles)
    ↓
spec.md (WHAT to build)
    ↓ [optional investigation if complex decisions needed]
research.md (technical decisions: K1, K2, K3...)
    ↓
plan.md (HOW to build, references research K-decisions)
    ↓
tasks.md (execution breakdown)
```

**Key Relationships:**

1. **constitution.md → spec.md**
   - Constitution version tracked in spec.md frontmatter (`sync_impact: 1.2.2`)
   - Specs must not violate constitutional principles
   - Constitution changes trigger spec review for active branches

2. **spec.md → research.md** (optional)
   - Research investigates unknowns identified in spec
   - Spec may include `[NEEDS CLARIFICATION]` that research resolves
   - Research produces K-numbered decisions referenced later

3. **research.md → plan.md**
   - Plan references research with K-numbers: "See K1 for state update decision"
   - Technology choices in plan cite research benchmarks
   - Architecture decisions link to research rationale

4. **plan.md → tasks.md**
   - Tasks correspond to plan phases
   - Each task includes phase reference for context
   - Dependencies follow plan structure

### Cross-Referencing Patterns

#### From plan.md to research.md

```markdown
## Phase 1: API Finalization

### 1.1 State Update Safety (K1)
Implement combined mode-change operations instead of atomic setters.
See research.md K1 for full decision rationale and alternatives considered.
```

#### From tasks.md to plan.md

```markdown
### Task 1.3: Implement enterVisualMode() 
**Phase:** Phase 1 (API Finalization)  
**Plan Reference:** Section 1.1 "State Update Safety"  
**Dependencies:** K1 research decision approved
```

#### From spec.md to constitution.md

```yaml
---
version: 1
sync_impact: 1.2.2  # Constitution version this spec follows
---

## Constitution Check

- [x] Article VII: Using ≤3 projects (adheres to simplicity principle)
- [x] Article IX: Contract tests written before implementation
```

### Metadata Standards

Every artifact includes frontmatter metadata:

```yaml
---
version: 1  # Artifact version (usually 1)
source_date: 2026-02-15  # When created
derived_from: [source description]  # Provenance
project: [Project Name]  # Optional
domain: [Domain/Category]  # Optional
companion_spec: spec-example-name.md  # For plan.md/tasks.md
companion_plan: plan-example-name.md  # For tasks.md
constitution_version: 1.2.2  # For constitution.md
sync_impact: 1.2.2  # For spec.md/plan.md
effort: L  # For spec.md: S/M/L
complexity: High  # For spec.md: Low/Medium/High
note: "Description of this artifact"  # Optional
---
```

**Standard Fields:**

- **version:** Artifact version (typically `1`)
- **source_date:** Creation date (YYYY-MM-DD)
- **companion_spec/plan:** Links related artifacts
- **sync_impact:** Constitution version to detect staleness

---

## Comparison: Tutorial vs. Real-World

This table compares tutorial examples (Taskify) against production examples (IdeaVim, Backpack) to illustrate the scale difference between learning examples and real-world usage.

| Aspect | Taskify (Tutorial) | IdeaVim (Real-World) | Backpack (Real-World) |
|--------|-------------------|---------------------|---------------------|
| **Project Type** | Web app (tutorial) | IDE plugin (OSS) | Design system (enterprise) |
| **Domain** | Team productivity | Editor extension | Component library |
| **Spec Length** | ~305 lines | 249 lines | 355 lines |
| **Plan Length** | ~242 lines | 307 lines | 480 lines |
| **Tasks Count** | 12 tasks | 23 tasks | 18 tasks |
| **research.md** | ❌ Not included | ✅ 418 lines (5 K-decisions) | ❌ Not needed (established patterns) |
| **Constitution** | Core 9 articles | 10 articles (+Vim compatibility) | 13 articles (+Design governance) |
| **Constitution Version** | 1.0.0 (stable) | 1.2.2 (evolved 3x) | 1.0.2 (evolved 2x) |
| **Complexity** | Low (learning) | High (architectural API) | Medium (infrastructure) |
| **User Stories** | 5 stories | 4 prioritized stories (P1-P3) | 4 infrastructure stories |
| **Clarifications** | Simple Q&A | Dated sessions | Multiple dated sessions |
| **Phases** | 3 phases | 4 phases (phased rollout) | 4-phase strategy |
| **Estimated Duration** | 2-3 weeks | 8 weeks | 2 weeks |

### Key Takeaways

1. **Real-world artifacts are 50-150% longer** than tutorial examples due to:
   - Multiple stakeholder considerations
   - Migration/compatibility planning
   - Organizational alignment requirements
   - Risk mitigation strategies

2. **research.md is rare** (~20% of features):
   - Taskify: Skipped (straightforward web app)
   - IdeaVim: Included (complex API design with multiple architectural approaches)
   - Backpack: Skipped (established Nx migration pattern)

3. **Constitutions evolve with project maturity:**
   - Taskify: Stable 1.0.0 (tutorial, doesn't evolve)
   - IdeaVim: 1.2.2 after 3 updates (refined over years of production use)
   - Backpack: 1.0.2 after 2 updates (template improvements discovered during use)

4. **Infrastructure specs differ from feature specs:**
   - Backpack Nx: Longest spec (355 lines) despite medium complexity
   - Focus on organizational alignment, not user-facing functionality
   - Success criteria emphasize process/compliance, not features

---

## Best Practices

This section provides actionable guidelines for creating high-quality artifacts based on real-world patterns.

### For spec.md

#### ✅ DO

- **Use [NEEDS CLARIFICATION] for unknowns** rather than making assumptions

  ```markdown
  FR-003: Authentication MUST support [NEEDS CLARIFICATION: OAuth2, SAML, or both?]
  ```

- **Include dated clarification Q&A sessions** to preserve decision context

  ```markdown
  ## Clarifications
  
  ### Session 2026-02-10
  - Q: Should drag-and-drop work on mobile?
  - A: Yes, use library with touch event support
  ```

- **Define success criteria measurably** with specific numbers

  ```markdown
  SC-001: All 12 built-in extensions migrated and function identically
  SC-002: Migration documentation enables completion within single work session
  ```

- **Explicitly list out-of-scope items** to prevent scope creep

  ```markdown
  ## Out of Scope
  - User authentication (future phase)
  - Real-time collaboration (not in MVP)
  ```

#### ❌ DON'T

- **Prescribe technology choices** (that's for plan.md)

  ```markdown
  ❌ BAD: "Use React with Redux for state management"
  ✅ GOOD: "UI must update within 100ms of status change"
  ```

- **Include implementation details** (that's for plan.md)

  ```markdown
  ❌ BAD: "Create UserController.cs with GET /api/users endpoint"
  ✅ GOOD: "System MUST provide API to retrieve list of all users"
  ```

- **Mix user stories with technical requirements**

  ```markdown
  ❌ BAD: "As a user, I want PostgreSQL to store my tasks..."
  ✅ GOOD: "As a user, I want my tasks saved between sessions..."
  ```

---

### For plan.md

#### ✅ DO

- **Always include Phase -1 constitutional gates** before implementation planning

  ```markdown
  ## Phase -1: Pre-Implementation Gates
  
  ### Simplicity Gate (Article VII)
  - [ ] Using ≤3 projects?
  - [ ] No future-proofing?
  
  ### Anti-Abstraction Gate (Article VIII)
  - [ ] Using framework directly?
  ```

- **Reference research.md decisions with [K#] notation**

  ```markdown
  ### Phase 1: API Finalization
  1. State Update Safety (K1) - Implement combined operations
  2. Editor Context Fix (K2) - Add explicit editor parameter
  ```

- **Document complexity justifications** for constitutional violations

  ```markdown
  ## Complexity Tracking
  
  ### Article VII Exception: 4 Projects Used
  **Justification:** Security boundary requires isolated auth-lib...
  **Approval:** Tech lead review 2026-02-10, ADR-042
  ```

- **Provide technology choice rationale** (not just "what" but "why")

  ```markdown
  **Database:** PostgreSQL 15+
  **Rationale:** JSONB support for flexible task metadata, 
  proven reliability for relational data, team expertise
  ```

#### ❌ DON'T

- **Skip technology choice rationale** (future maintainers need context)

  ```markdown
  ❌ BAD: "Backend: Node.js with Express"
  ✅ GOOD: "Backend: Node.js with Express (team expertise, npm ecosystem, 
           matches frontend JavaScript, async I/O for real-time features)"
  ```

- **Create tasks in plan.md** (that's for tasks.md)

  ```markdown
  ❌ BAD: "Phase 1: Create UserController (2 hours), Create User entity (1 hour)..."
  ✅ GOOD: "Phase 1: Database Setup - Create schema, seed data, verify"
  ```

- **Make assumptions about unclear requirements** (clarify in spec.md first)

  ```markdown
  ❌ BAD: "Assume OAuth2 for authentication since it's industry standard"
  ✅ GOOD: [Add [NEEDS CLARIFICATION] to spec.md, get answer, then proceed]
  ```

---

### For tasks.md

#### ✅ DO

- **Estimate effort realistically** with standard sizing (S/M/L)

  ```markdown
  ### Task 2.3: Migrate surround extension
  **Effort:** L (3-5 days)
  **Why not M?** Complex input handling, tag parsing, multiple operators
  ```

- **Mark dependencies clearly** (enables parallelization and critical path)

  ```markdown
  **Dependencies:** 
  - Task 1.4 (findBlockTagRange API implemented)
  - External: AceJump plugin compatibility verified
  **Blocks:** Tasks 2.4, 2.5 (waiting for pattern validation)
  ```

- **Include acceptance criteria** that are testable

  ```markdown
  **Acceptance Criteria:**
  - [ ] Migration file created: `migrations/001_create_users.sql`
  - [ ] Schema verified: `psql -c "\d users"` shows correct columns
  - [ ] Rollback tested: `npm run migrate:rollback` succeeds
  ```

- **Provide verification commands** for unambiguous checking

  ```markdown
  **Verification:**
  ```bash
  npm run test:contract
  # Expected: All contract tests pass (12/12)
  ```markdown
  PLAN-STEP 1.2.1: Implementation details...
  ```

  ```markdown
  PLAN-STEP 1.2.2: Implementation details...
  ```

#### ❌ DON'T

- **Create tasks smaller than 2 hours** (too granular, overhead outweighs value)

  ```markdown
  ❌ BAD: "Task 3.1: Add import statement (5 min)"
  ✅ GOOD: "Task 3.1: Create UserController with all CRUD endpoints (M, 1-2 days)"
  ```

- **Use vague acceptance criteria**

  ```markdown
  ❌ BAD: "Code works and looks good"
  ✅ GOOD: "All 15 unit tests pass, ESLint passes with zero warnings"
  ```

- **Forget to update as you work** (tasks.md is living document)

  ```markdown
  ✅ GOOD: Mark tasks complete as you finish, add discovered sub-tasks,
           update estimates when reality differs from plan
  ```

---

### For research.md

#### ✅ DO

- **Use K-numbered format** for easy cross-referencing

  ```markdown
  ## K1: Parser Selection
  ## K2: Caching Strategy
  ## K3: API Authentication Method
  ```

- **Include benchmarks/data** to support decisions

  ```markdown
  ### Benchmarks
  
  | Library | Parse Time (100KB) | Memory | Bundle Size |
  |---------|-------------------|--------|-------------|
  | Option A | 28ms | 1.2 MB | 85 KB |
  | Option B | 980ms | 450 KB | 12 KB |
  ```

- **State confidence level** to signal uncertainty

  ```markdown
  ### Confidence Level
  **MEDIUM** - Decision resolves observed deadlocks, but root cause unclear.
  Consider retesting on IntelliJ 2024.2+ release.
  ```

- **List alternatives considered** (shows thorough analysis)

  ```markdown
  ### Options Considered
  
  #### Option A: Tree-sitter
  Pros: ... Cons: ...
  
  #### Option B: Regex-based
  Pros: ... Cons: ...
  
  #### Option C: Custom parser
  Rejected: Maintenance burden outweighs benefits
  ```

#### ❌ DON'T

- **Create for obvious choices** (research is for non-trivial decisions)

  ```markdown
  ❌ BAD: Research document for "Should we use Git for version control?"
  ✅ GOOD: Research document for "Git LFS vs S3 for large binary assets?"
  ```

- **Skip rationale** (future teams need to understand "why")

  ```markdown
  ❌ BAD: "Decision: Use PostgreSQL" [end of section]
  ✅ GOOD: "Decision: PostgreSQL. Rationale: JSONB support critical for 
           flexible metadata, team expertise, proven at scale..."
  ```

- **Assume decisions are permanent** (document when to revisit)

  ```markdown
  ### Follow-Up Investigation
  - [ ] Retest on IntelliJ 2024.2+ (may have fixed underlying issue)
  - [ ] File support ticket for coroutine-in-lock documentation
  ```

---

### For constitution.md

#### ✅ DO

- **Version every change** with semantic versioning (MAJOR.MINOR.PATCH)

  ```yaml
  ---
  version: 1.2.2
  constitution_version: 1.2.2
  previous_version: 1.2.1
  ---
  ```

- **Track SYNC IMPACT** in artifact frontmatter

  ```yaml
  # In spec.md:
  sync_impact: 1.2.2  # Constitution version this spec follows
  ```

- **Use NON-NEGOTIABLE for critical requirements**

  ```markdown
  ### II. License Headers (NON-NEGOTIABLE)
  ALL source files MUST include Apache 2.0 license header.
  Enforced via pre-commit hook.
  ```

- **Document version bump rationale** in SYNC IMPACT report

  ```markdown
  <!--
    SYNC IMPACT REPORT
    Version: 1.1.0 → 2.0.0 (MAJOR)
    
    Removed: "DEFERRED FOR POC" markers (breaking change)
    Rationale: POC → Production transition, testing now mandatory
    Impact: ALL active branches must regenerate plans
  -->
  ```

#### ❌ DON'T

- **Modify core articles lightly** (principles should be stable)

  ```markdown
  ❌ BAD: Change Article III (Test-First) to optional because deadline pressure
  ✅ GOOD: Keep Article III firm, document justified exception in plan.md
  ```

- **Skip template compatibility check** after changes

  ```markdown
  ✅ GOOD: After constitution change, verify:
           - Templates still work with new principles?
           - Need to regenerate existing specs/plans?
           - Follow-up TODOs documented?
  ```

- **Create project-specific articles prematurely**

  ```markdown
  ❌ BAD: Add Article X "Future AI Integration Requirements" before AI is real need
  ✅ GOOD: Add Article X only when pattern recurs (2-3 features hit same constraint)
  ```

---

## See Also

### Core Documentation

- [Constitution Reference](constitution-reference.md) - Complete explanation of the nine core articles
- [Spec-Driven Development](spec-driven-development.md) - Core methodology and philosophy
- [Artifact Examples Index](../artifacts/README.md) - Navigation hub for all artifact examples

### Guides and FAQs

- [Constitution Evolution FAQ](../faq/constitution-evolution.md) - Version management and SYNC IMPACT guidance
- [Artifacts FAQ](../faq/artifacts.md) - Common questions about artifact usage and structure

### Workflows

- [Basic Workflow](../workflows/basic-workflow.md) - Step-by-step process from idea to implementation

### Tricks and Patterns

- [Template Constraints](../tricks/template-constraints.md) - How templates guide AI toward better specifications
- [Constitutional Gates](../tricks/constitutional-gates.md) - Using Phase -1 gates to prevent over-engineering
- [Avoiding Over-Engineering](../tricks/avoiding-over-engineering.md) - Articles VII & VIII in practice

---

**Document Version:** 1.0  
**Last Updated:** 2026-02-15  
**Maintainer:** Spec-Kit Guide Research Project
