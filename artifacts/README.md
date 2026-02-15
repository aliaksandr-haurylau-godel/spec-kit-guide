# Spec-Kit Artifacts Examples

This directory contains real-world examples of Spec-Kit artifacts from production projects. These examples demonstrate how teams use the framework across different domains and complexity levels.

## Quick Reference Table

| Example | Project | Domain | Complexity | Lines | Has Research? | Constitution Version |
|---------|---------|---------|-----------|-------|---------------|---------------------|
| **Taskify** (Tutorial) | Tutorial App | Web Application | Simple | 150-200 | ❌ No | v1.0 (stable) |
| **IdeaVim API** | Open-Source IDE Plugin | API Design & Refactoring | High | 249-418 | ✅ **Yes** | v1.2.2 (evolved) |
| **Backpack Nx** | Enterprise Design System | Infrastructure/Tooling | Medium | 261-480 | ❌ No | v1.0.2 (evolved) |

## By Artifact Type

### Specifications (spec.md)

Defines **WHAT** needs to be built and **WHY**.

- **[Taskify](./spec-example-taskify.md)** - Tutorial example, simple web app feature
- **[IdeaVim API Layer](./spec-example-ideavim-api.md)** - Complex architectural refactoring, prioritized user stories (P1/P2/P3), 249 lines
- **[Backpack Nx Migration](./spec-example-backpack-nx.md)** - Infrastructure change, tooling migration, "Clarifications" section pattern, 355 lines

**When to reference:**
- **Simple feature:** Use Taskify
- **Large refactoring/API design:** Use IdeaVim
- **Infrastructure/tooling changes:** Use Backpack

### Plans (plan.md)

Defines **HOW** to implement (architecture, approach, steps).

- **[Taskify Plan](./plan-example-taskify.md)** - Basic implementation approach
- **[IdeaVim API Plan](./plan-example-ideavim-api.md)** - Phased delivery, dependency management, 307 lines
- **[Backpack Nx Plan](./plan-example-backpack-nx.md)** - Infrastructure migration strategy, 480 lines

### Tasks (tasks.md)

Breaks plan into **actionable work items** with verification criteria.

- **[Taskify Tasks](./tasks-example-taskify.md)** - Simple task breakdown
- **[IdeaVim API Tasks](./tasks-example-ideavim-api.md)** - Complex task dependencies, 271 lines
- **[Backpack Nx Tasks](./tasks-example-backpack-nx.md)** - Infrastructure tasks, 261 lines

### Research (research.md) ⭐

**OPTIONAL artifact** - Documents complex design decisions and trade-offs.

- **[IdeaVim API Research](./research-example-ideavim-api.md)** - K-numbered decision logs (K1-K6), architectural choices, 418 lines

**When to create research.md:**
- Multiple architectural approaches need evaluation
- Complex API design requires documented trade-offs
- Team needs shared understanding of "why" decisions were made
- Estimated in ~20% of feature branches

**Pattern:** K-numbered sections (K1, K2...) with Problem/Decision/Rationale structure

### Constitutions (constitution.md)

Defines **project principles** and **non-negotiable standards**.

- **[Taskify Constitution](./constitution-example.md)** - Tutorial, 9 standard articles
- **[IdeaVim Constitution v1.2.2](./constitution-example-ideavim.md)** - IDE plugin principles, evolved over time, 225 lines
- **[Backpack Constitution v1.0.2](./constitution-example-backpack.md)** - Design system rules, NON-NEGOTIABLE markers, 561 lines
- **[Manifest Constitution v2.0.0](./constitution-example-manifest.md)** - POC → Production transition (MAJOR bump), 249 lines

**See also:** [Constitution Evolution FAQ](../faq/constitution-evolution.md) for version management guidance

## Choosing the Right Example

### By Project Phase
- **Starting new project:** Taskify (tutorial)
- **Established project with standards:** IdeaVim or Backpack constitutions
- **POC → Production transition:** Manifest constitution (v1→v2)

### By Change Type
- **New feature:** Taskify spec
- **API design/refactoring:** IdeaVim spec + research
- **Infrastructure/tooling:** Backpack spec

### By Team Size
- **Solo/small team:** Taskify examples (lighter weight)
- **Large team/open-source:** IdeaVim examples (detailed, multi-stakeholder)
- **Enterprise:** Backpack examples (design system scale)

### By Decision Complexity
- **Straightforward implementation:** Skip research.md, use spec + plan + tasks
- **Complex architectural choices:** Add research.md (IdeaVim pattern)

## Source Attribution

All real-world examples are derived from production open-source projects:
- **IdeaVim**: https://github.com/JetBrains/ideavim (MIT License)
- **Backpack**: https://github.com/Skyscanner/backpack (Apache 2.0 License)
- **Manifest**: Internal project (sanitized for public documentation)

Collected: 2026-02-15 | Guide Version: 1.0

---

**Next Steps:**
- Read [Artifact Structure Guide](../docs/artifact-structure.md) for deep dive into each artifact type
- See [Constitution Evolution FAQ](../faq/constitution-evolution.md) for version management
- Start with [Basic Workflow](../workflows/basic-workflow.md) to use these patterns
