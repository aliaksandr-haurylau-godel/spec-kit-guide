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

## See Also

- [Constitution Reference](constitution-reference.md) - Complete nine articles
- [Basic Workflow](../workflows/basic-workflow.md) - Step-by-step process
- [Installation Guide](installation.md) - Getting started
- [CLI Reference](cli-reference.md) - Command-line tools
