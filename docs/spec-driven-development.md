---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/spec-driven.md
---

# Spec-Driven Development (SDD)

## Overview

Spec-Driven Development (SDD) is a methodology that fundamentally rethinks the relationship between specifications and code. Instead of treating code as the source of truth with specifications serving as documentation, SDD inverts this: **specifications become executable**, directly generating working implementations.

## The Power Inversion

For decades, code has been king. Specifications served code—they were the scaffolding we built and then discarded once the "real work" of coding began. We wrote PRDs to guide development, created design docs to inform implementation, drew diagrams to visualize architecture. But these were always subordinate to the code itself. Code was truth. Everything else was, at best, good intentions.

**Spec-Driven Development inverts this power structure.** Specifications don't serve code—code serves specifications. The Product Requirements Document (PRD) isn't a guide for implementation; it's the source that generates implementation. Technical plans aren't documents that inform coding; they're precise definitions that produce code.

The gap between specification and implementation has plagued software development since its inception. We've tried to bridge it with better documentation, more detailed requirements, stricter processes. These approaches fail because they accept the gap as inevitable. **SDD eliminates the gap** by making specifications and their concrete implementation plans executable. When specifications generate code, there is no gap—only transformation.

This transformation is now possible because AI can understand and implement complex specifications. But raw AI generation without structure produces chaos. SDD provides that structure through specifications that are precise, complete, and unambiguous enough to generate working systems.

## Core Principles

### Specifications as the Lingua Franca

The specification becomes the primary artifact. Code becomes its expression in a particular language and framework. Maintaining software means evolving specifications. The intent of the development team is expressed in natural language (**"intent-driven development"**), design assets, core principles, and other guidelines.

### Executable Specifications

Specifications must be precise, complete, and unambiguous enough to generate working systems. This eliminates the gap between intent and implementation. Domain concepts become data models. User stories become API endpoints. Acceptance scenarios become tests.

### Continuous Refinement

Consistency validation happens continuously, not as a one-time gate. AI analyzes specifications for ambiguity, contradictions, and gaps as an ongoing process. The feedback loop extends beyond initial development—production metrics and incidents update specifications for the next regeneration.

### Research-Driven Context

Research agents gather critical context throughout the specification process, investigating library compatibility, performance benchmarks, and security implications. Organizational constraints are discovered and applied automatically—your company's database standards, authentication requirements, and deployment policies seamlessly integrate into every specification.

### Constitutional Foundation

At the heart of SDD lies a constitution—a set of immutable principles that govern how specifications become code. The constitution acts as the architectural DNA of the system, ensuring that every generated implementation maintains consistency, simplicity, and quality. See [Constitution Reference](constitution-reference.md) for the complete nine articles.

### Bidirectional Feedback

Production reality informs specification evolution. Metrics, incidents, and operational learnings become inputs for specification refinement. Performance bottlenecks become new non-functional requirements. Security vulnerabilities become constraints that affect all future generations.

### Branching for Exploration

Generate multiple implementation approaches from the same specification to explore different optimization targets—performance, maintainability, user experience, cost. SDD supports what-if/simulation experiments and parallel implementations.

## The SDD Workflow in Practice

The workflow begins with an idea—often vague and incomplete. Through iterative dialogue with AI, this idea becomes a comprehensive PRD. The AI asks clarifying questions, identifies edge cases, and helps define precise acceptance criteria. What might take days of meetings and documentation in traditional development happens in hours of focused specification work.

**Key workflow steps:**

1. **Constitution** - Establish project principles that guide all development
2. **Specification** - Define what you want to build (the "what" and "why")
3. **Clarification** - Resolve ambiguities through structured questioning
4. **Planning** - Create technical implementation plans with technology choices
5. **Tasking** - Break down plans into executable tasks
6. **Implementation** - Generate code from specifications and tasks

This isn't just about initial development—it's about maintaining engineering velocity through inevitable changes. When specifications drive implementation, pivots become systematic regenerations rather than manual rewrites.

## Why SDD Matters Now

Three trends make SDD not just possible but necessary:

1. **AI capabilities** have reached a threshold where natural language specifications can reliably generate working code. This amplifies developer effectiveness by automating mechanical translation from specification to implementation.

2. **Software complexity** continues to grow exponentially. Modern systems integrate dozens of services, frameworks, and dependencies. SDD provides systematic alignment through specification-driven generation.

3. **Pace of change** accelerates. Requirements change far more rapidly today than ever before. SDD transforms requirement changes from obstacles into normal workflow. Change a core requirement in the PRD, and affected implementation plans update automatically.

## Template-Driven Quality

The true power of SDD lies in how templates guide AI behavior toward higher-quality specifications. Templates act as sophisticated prompts that constrain the AI's output in productive ways:

- **Preventing premature implementation details** - Keep specifications focused on "what" not "how"
- **Forcing explicit uncertainty markers** - Use `[NEEDS CLARIFICATION]` instead of making assumptions
- **Structured thinking through checklists** - Self-review output systematically
- **Constitutional compliance through gates** - Prevent over-engineering
- **Hierarchical detail management** - Maintain appropriate abstraction levels
- **Test-first thinking** - Think about contracts and testability before implementation
- **Preventing speculative features** - Every feature must trace back to concrete user stories

See [Template Constraints](../tricks/template-constraints.md) for detailed techniques.

## Implementation Approaches

Today, practicing SDD requires assembling existing tools and maintaining discipline throughout the process:

- AI assistants for iterative specification development
- Research agents for gathering technical context
- Code generation tools for translating specifications to implementation
- Version control systems adapted for specification-first workflows
- Consistency checking through AI analysis of specification documents

The Spec-Kit framework provides a complete toolkit with slash commands (`/speckit.*`) that automate the specification → planning → tasking workflow. See [Installation Guide](installation.md) to get started.

## The Transformation

This isn't about replacing developers or automating creativity. It's about amplifying human capability by automating mechanical translation. It's about creating a tight feedback loop where specifications, research, and code evolve together, each iteration bringing deeper understanding and better alignment between intent and implementation.

The development team focuses on their creativity, experimentation, and critical thinking. The workflow reorganizes around specifications as the central source of truth, with implementation plans and code as continuously regenerated output.

## Production Usage Patterns

This section documents patterns observed in real-world Spec-Kit usage across multiple production projects. These patterns complement the theoretical framework with practical insights.

### Research Artifact Usage

Approximately **20% of feature branches** include a `research.md` artifact for technical investigations. This optional artifact documents complex design decisions and trade-offs during the specification phase.

#### When Teams Create research.md

##### 1. Non-Obvious Technology Choices

When multiple viable frameworks or libraries exist, teams create research.md to document benchmarks and comparisons.

- Multiple viable frameworks/libraries exist
- Performance characteristics need benchmarking
- Different approaches have unclear trade-offs

**Example**: IdeaVim K1 Parser Selection compared 3 different parsers with performance benchmarks, evaluating each against latency requirements and maintenance burden.

##### 2. Integration Unknowns

When compatibility with existing systems is unclear, research.md explores the integration landscape.

- Compatibility with existing systems unclear
- API surface design needs exploration
- Platform constraints affect implementation choices

**Example**: IdeaVim K2 API Surface Design investigated how to expose safe mode-change operations while preventing inconsistent state across multiple coordinated subsystems (selection state, caret shape, command builder mode).

##### 3. Architectural Decisions with Long-Term Impact

When choices are expensive to reverse later, teams document the reasoning explicitly.

- Choices expensive to reverse later
- Trade-offs need explicit documentation
- Multiple stakeholders need shared understanding

**Example**: IdeaVim K5 Extension Architecture evaluated different patterns for plugin extensibility, documenting the rationale for choosing combined operations over atomic setters to prevent API misuse.

#### K-Numbered Investigation Pattern

Teams use K-numbered sections (K1, K2, K3...) in research.md for easy cross-referencing from plan.md and tasks.md. This creates traceable decision chains throughout the development process.

**Example structure**:

```markdown
# research.md

## K1: State Update Complexity

### Problem Statement
Mode changes require multiple coordinated state changes...

### Decision
Use safe combined operations instead of atomic setters.

### Rationale
1. Prevents inconsistent state
2. Matches Vim semantics
3. Simpler API for consumers

---

## K2: Editor Context

### Problem Statement
API requires editor context but may be called during initialization...
```

**Cross-referencing in plan.md**:

```markdown
# plan.md

## Phase 1: Core API Design

### Technology Choices
- **State Management**: Combined operations pattern [K1: State Update Complexity]
- **Context Handling**: Explicit editor context with optional variants [K2: Editor Context]
```

This creates traceable decision chains: research → plan → tasks. When reviewing a task, developers can trace back to the original research that informed the technical approach.

#### When to Skip research.md

Most features don't need research.md. Skip it when:

- Feature uses well-established patterns
- Technology choices are obvious from requirements
- Implementation is straightforward with no ambiguity
- Similar features exist in the codebase with proven approaches

**Further Reading**: See [artifacts/research-example-ideavim-api.md](../artifacts/research-example-ideavim-api.md) for a complete 418-line research artifact with 5 K-numbered decisions from the IdeaVim project.

### Constitution Evolution in Practice

Real-world constitutions evolve through distinct lifecycle phases as projects mature from proof-of-concept to production systems.

#### Phase 1: POC/Prototype (v0.x.x or early v1.0.0)

Early-stage projects start with minimal constitutional requirements:

- Core 9 articles with minimal customization
- Permissive complexity gates to enable rapid experimentation
- "DEFERRED FOR POC" markers on testing, performance, security
- Focus on rapid iteration and learning over comprehensive quality gates

**Example**: Manifest Generator v1.0.0 (before v2.0.0 production bump) included basic SOLID principles but deferred testing standards, performance requirements, and security practices to enable fast prototyping.

#### Phase 2: Production Transition (MAJOR bump)

When moving from POC to production, teams add stricter requirements based on prototype learnings:

- Stricter quality gates based on POC learnings
- Addition of project-specific articles (X, XI, XII...)
- NON-NEGOTIABLE markers for critical requirements
- Enhanced testing requirements (coverage thresholds, test types)
- Security and performance standards become mandatory

**Example**: Manifest Generator v1.1.0 → v2.0.0 (POC to production transition)

- **MAJOR bump** (1.x → 2.0.0)
- Removed all "DEFERRED FOR POC" markers
- Testing standards became mandatory (was deferred)
- Performance requirements became mandatory (was deferred)
- Added Article VI: Security Standards (new principle)
- Added Article VII: Community & Contribution Standards (new principle)

This MAJOR version bump reflected a fundamental shift in project maturity level and development expectations.

#### Phase 3: Mature Product (incremental MINOR/PATCH bumps)

Established projects refine constitutions through smaller updates:

- Fine-tuned gates based on production experience
- Template improvements for clarity
- New articles for emerging requirements
- Process optimizations learned from production incidents

**Examples**:

- **IdeaVim v1.2.1 → v1.2.2** (MINOR bump)
  - Modified Article VIII: Trunk-Based Development → Feature branches with frequent rebasing
  - Change rationale: Team found trunk-based caused merge conflicts in production
  - Added Article X: Vim Compatibility standards after production compatibility issues

- **Backpack v1.0.1 → v1.0.2** (PATCH bump)
  - Template formatting improvements only
  - Updated component name placeholders for clarity
  - Corrected Storybook examples structure
  - No principle changes, just documentation refinements

#### Version Bump Decision Tree

```text
Change Type?
│
├─ Template formatting only? → PATCH (1.0.1 → 1.0.2)
│  └─ Example: Backpack clarified wording in Article VII
│     Changed: Component name placeholders, color token examples
│     Impact: Documentation clarity only, no behavior changes
│
├─ New articles, backward compatible? → MINOR (1.1.0 → 1.2.0)
│  └─ Example: IdeaVim added Article X (Vim Compatibility)
│     Changed: Added new principle without breaking existing workflows
│     Impact: New requirements for future work, existing code grandfathered
│
└─ Breaking process changes? → MAJOR (1.x → 2.0.0)
   └─ Example: Manifest POC → Production transition
      Changed: Deferred requirements became mandatory
      Impact: All code must now meet testing/performance standards
```

**Further Reading**: See [faq/constitution-evolution.md](../faq/constitution-evolution.md) for comprehensive version management guidance.

### Clarification Workflows

Production teams handle specification ambiguities through structured clarification processes. Two primary patterns emerge from real-world usage.

#### Pattern 1: Dated Q&A Sessions (Backpack style)

High-stakes projects use comprehensive dated Q&A sessions to create an audit trail of decisions.

**Structure**:

```markdown
# spec.md

## Clarifications

### 2024-03-10 - Initial Review Session
Q: Should we support legacy workspace structure?
A: No, nx-only for this iteration. Legacy support deferred to v2.

Q: Performance target for package discovery?
A: <500ms for monorepos with <100 packages.

### 2024-03-15 - Security Review
Q: Authentication required for workspace operations?
A: Yes, but only for write operations. Read operations public.
```

**Benefits**:

- Creates audit trail of decisions with timestamps
- Prevents re-litigating resolved questions months later
- Provides context for future maintainers on why decisions were made
- Multiple stakeholders can reference specific dates/sessions
- Easy to see how requirements evolved over time

**When to use**: Complex requirements, multiple stakeholders, compliance requirements, or when decisions have long-term implications.

#### Pattern 2: [NEEDS CLARIFICATION] Markers (IdeaVim style)

Fast-moving projects mark critical ambiguities inline and block progress until resolved.

**Structure**:

```markdown
# spec.md

## Authentication Flow
[NEEDS CLARIFICATION: OAuth2 or SAML? Check with security team]

## Performance Requirements
Response time: <100ms
[NEEDS CLARIFICATION: 95th or 99th percentile?]
```

**Resolution Process**:

1. Mark ambiguity with `[NEEDS CLARIFICATION]` during spec creation
2. Block plan.md generation until all markers resolved
3. Update spec.md with decision and remove marker
4. Proceed to planning phase once spec is unambiguous

**Benefits**:

- Lightweight and fast for agile workflows
- Clearly identifies blocking questions
- Easy to search for unresolved ambiguities (grep for "[NEEDS CLARIFICATION]")
- Minimal ceremony for small teams

**When to use**: Fast iteration cycles, small teams, clear escalation paths, or when most requirements are already well-understood.

#### Team Balance: Completeness vs. Velocity

Teams balance clarification completeness against delivery velocity based on project risk:

**High-Stakes Projects** (e.g., Backpack infrastructure migration):

- Comprehensive dated Q&A sessions
- Block on ALL clarifications before proceeding
- Multiple review sessions with cross-functional stakeholders
- Document strategic context alongside decisions

**Fast-Moving Projects** (e.g., IdeaVim feature additions):

- Mark only critical ambiguities with `[NEEDS CLARIFICATION]`
- Proceed with reasonable assumptions for non-critical questions
- Document assumptions in spec and validate during implementation
- Iterate based on feedback

**Further Reading**: See [artifacts/spec-example-backpack-nx.md](../artifacts/spec-example-backpack-nx.md) for a real-world infrastructure spec with extensive dated clarification sessions (355 lines).

### Metadata and Estimation Practices

Production teams use standardized effort markers and complexity ratings to improve planning accuracy.

#### Standard Effort Markers

| Marker         | Time Range | Typical Use                                                             |
|----------------|------------|-------------------------------------------------------------------------|
| **S (Small)**  | 2-4 hours  | Single file changes, config updates, documentation improvements         |
| **M (Medium)** | 1-2 days   | Feature implementation, API endpoint creation, component development    |
| **L (Large)**  | 3-5 days   | Complex integrations, architectural changes, multi-component features   |

**Example from IdeaVim tasks.md:**

```markdown
### Task 1.3: Implement Keybinding Engine (L)
**Effort**: 4 days
**Complexity**: High
**Dependencies**: Task 1.1 (API design)
```

#### Complexity Ratings

Teams distinguish **effort** (time required) from **complexity** (technical unknowns):

- **Low Complexity**: Well-understood patterns, established approaches, clear implementation path
- **Medium Complexity**: Some unknowns, may require research, limited architectural impact
- **High Complexity**: Significant unknowns, requires research.md, long-term architectural impact

#### Effort vs. Complexity Examples

**High Effort, Low Complexity**:

```markdown
### Task 2.1: Migrate 50 Components to New API (L)
**Effort**: 5 days (mechanical repetition across 50 files)
**Complexity**: Low (straightforward pattern repetition, well-understood)
```

The work takes a long time but follows a clear, repeatable pattern with no technical unknowns.

**Low Effort, High Complexity**:

```markdown
### Task 0.2: Design Extension Plugin Architecture (S)
**Effort**: 4 hours (design document creation)
**Complexity**: High (long-term impact, many trade-offs, requires research.md)
```

The actual implementation is quick, but the decision has significant long-term implications and requires careful architectural analysis.

#### Time Estimate Calibration

Real-world project results demonstrate the importance of complexity-based buffer factors.

**IdeaVim Project (23 tasks, estimated 15 days)**:

- **Actual**: 18 days (20% over estimate)
- **Primary variance**: Task 1.3 (Keybinding Engine) took 6 days vs. 4-day estimate
- **Root cause**: High complexity task with unexpected integration challenges
- **Adjustment**: High-complexity tasks now get 1.5x time buffer in future estimates

**Backpack Project (18 tasks, estimated 12 days)**:

- **Actual**: 11 days (on target, finished early)
- **Success factor**: Conservative "L" ratings for all cross-package work
- **Success factor**: Thorough research phase eliminated surprises
- **Learning**: Infrastructure work benefits from defensive estimation

**Calibration Lessons**:

1. **High-complexity tasks consistently overrun**: Add 1.5-2x buffer for high-complexity work
2. **Research.md pays off**: Projects with research artifacts hit estimates more accurately
3. **Integration complexity underestimated**: Cross-component work deserves "L" rating even if individual changes are small
4. **First-time patterns expensive**: New architectural patterns take 2-3x longer than repeated patterns

### Production Patterns Summary

| Pattern                   | Usage Rate           | When Applied                                                  | Example                                                                                                                   |
|---------------------------|----------------------|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| **research.md**           | ~20% of branches     | Non-obvious tech choices, integration unknowns, architectural | [IdeaVim API Design](../artifacts/research-example-ideavim-api.md) (418 lines, 5 K-decisions)                            |
| **Dated Q&As**            | High-stakes projects | Complex requirements, multiple stakeholders, compliance       | [Backpack Nx Migration](../artifacts/spec-example-backpack-nx.md) (355 lines, 2 sessions)                                |
| **[NEEDS CLARIFICATION]** | Fast-moving projects | Critical ambiguities only, small teams, clear escalation      | IdeaVim feature specs (inline markers)                                                                                    |
| **K-numbered decisions**  | When research.md     | Cross-referencing from plan/tasks to research rationale       | All IdeaVim research artifacts                                                                                            |
| **NON-NEGOTIABLE**        | Mature projects      | Critical requirements that cannot be compromised              | [Backpack Constitution](../artifacts/constitution-example-backpack.md) (license headers, accessibility)                   |
| **MAJOR version bumps**   | POC → Production     | Fundamental process overhauls, mandatory requirements         | [Manifest v2.0.0](../artifacts/constitution-example-manifest.md) (added security/testing)                                 |

#### Further Reading

For comprehensive artifact documentation covering all templates, structure patterns, and cross-referencing techniques, see [Artifact Structure Guide](artifact-structure.md).

## See Also

- [Constitution Reference](constitution-reference.md) - Complete nine articles
- [Basic Workflow](../workflows/basic-workflow.md) - Step-by-step process
- [Installation Guide](installation.md) - Getting started
- [CLI Reference](cli-reference.md) - Command-line tools
- [Artifact Structure Guide](artifact-structure.md) - Complete artifact documentation
- [IdeaVim Research Example](../artifacts/research-example-ideavim-api.md) - K-numbered investigations
- [Backpack Spec Example](../artifacts/spec-example-backpack-nx.md) - Dated clarifications
- [Artifacts FAQ](../faq/artifacts.md) - Q&As about artifact usage
