---
version: 1
created_date: 2026-02-15
derived_from: raw_data/2026-02-15-real-world-spec-kit-artifacts/ (IdeaVim, Backpack)
---

# Artifacts FAQ

Practical questions about Spec-Kit artifacts (spec.md, plan.md, tasks.md, and the optional research.md), grounded in our real-world examples.

## Artifact Basics

### Q: What are the core Spec-Kit artifacts?

**Answer:** In day-to-day feature work, teams typically produce three core artifacts plus a project-level constitution:

- `constitution.md` (project-level): The durable principles and constraints that govern all work.
- `spec.md` (per feature): What to build and why (requirements, constraints, acceptance).
- `plan.md` (per feature): How to implement (approach, phases, gates, risks).
- `tasks.md` (per feature): Executable work breakdown (dependencies, acceptance checks).

`research.md` is a common optional companion when the feature has real unknowns or competing approaches.

Examples:

- `../docs/artifact-structure.md`
- `../artifacts/spec-example-taskify.md`
- `../artifacts/plan-example-taskify.md`
- `../artifacts/tasks-example-taskify.md`

### Q: Are all artifacts required?

**Answer:** For feature work, teams generally treat `spec.md`, `plan.md`, and `tasks.md` as required, and `research.md` as optional.

- Required in practice: `spec.md` -> `plan.md` -> `tasks.md`.
- Optional: `research.md` when you need decision logs, benchmarks, or platform constraints captured in one place.

The constitution is required at the project level, but it is not regenerated per feature.

Examples:

- `../docs/artifact-structure.md`
- `../artifacts/research-example-ideavim-api.md`

### Q: In what order are artifacts created?

**Answer:** The typical creation order is sequential, because each artifact depends on the previous one:

1. `spec.md` defines the contract (scope, requirements, success criteria).
2. `research.md` (optional) resolves unknowns and records K-numbered decisions.
3. `plan.md` selects an approach, references any research decisions, and defines phases.
4. `tasks.md` translates the plan into actionable tasks with dependencies and verification.

If you find yourself creating tasks before the plan can cite stable decisions, it is usually a signal that you need either clarifications in the spec or a research artifact.

Examples:

- `../artifacts/spec-example-ideavim-api.md`
- `../artifacts/research-example-ideavim-api.md`
- `../artifacts/plan-example-ideavim-api.md`
- `../artifacts/tasks-example-ideavim-api.md`

### Q: How should artifacts reference each other?

**Answer:** Use explicit, stable links and identifiers so a reviewer can trace a requirement to a decision to an implementation task:

- `plan.md` should link to its spec (often via `companion_spec` in frontmatter, and inline references).
- `tasks.md` should link to its plan (often via `companion_plan` in frontmatter, and phase references per task).
- If you have `research.md`, reference decisions by K-number (K1, K2, ...) rather than by section number.

Practical cross-reference patterns:

```markdown
---
version: 1
companion_spec: spec-example-ideavim-api.md
---

See research decisions: K1 (state update safety), K2 (editor context).
```

```markdown
### Task 1.1: Implement editor context parameter
**Dependencies:** K2 research decision approved
**Plan reference:** Phase 1 (API Finalization)
```

Examples:

- `../docs/artifact-structure.md`
- `../artifacts/plan-example-ideavim-api.md`
- `../artifacts/tasks-example-ideavim-api.md`

### Q: Where should these artifacts live in a repo?

**Answer:** In a typical Spec-Kit project, artifacts live per feature under a `specs/NNN-feature-name/` directory (often created by `/speckit.specify`).

In this guide repository, the real-world examples are mirrored under `artifacts/` so you can study them without needing the original repos.

Examples:

- `../artifacts/README.md`
- `../docs/artifact-structure.md`

### Q: How long should an artifact be?

**Answer:** Length is a proxy for risk and novelty, not virtue.

- Short specs are fine when scope and constraints are obvious.
- Longer specs are common for infrastructure work or multi-stakeholder changes.
- A long `research.md` usually indicates non-trivial decisions that should not be re-litigated during implementation.

If you add length, make sure it adds clarity: testable requirements, crisp boundaries, traceable decisions.

Examples:

- `../artifacts/spec-example-taskify.md`
- `../artifacts/spec-example-backpack-nx.md`
- `../artifacts/research-example-ideavim-api.md`

## research.md

### Q: When should I create a research.md?

**Answer:** Create `research.md` when the feature has meaningful unknowns or trade-offs that would otherwise leak into the plan and tasks as repeated debate.

Good triggers:

- Multiple viable architectural approaches exist.
- Platform constraints are subtle (threading, locks, lifecycle ordering).
- You need benchmarks or data to pick an approach.
- The team needs a durable rationale for future maintainers.

Skip it when the work is mostly applying established patterns.

Examples:

- `../artifacts/research-example-ideavim-api.md`
- `../artifacts/spec-example-ideavim-api.md`

### Q: What is the K-numbered decision format?

**Answer:** K-numbered decisions are a simple decision-log convention: each major decision gets an identifier like K1, K2, K3.

The point is traceability and stability:

- K-numbers are stable even if you reorder sections.
- They are easy to reference from other artifacts.
- They allow fast searching across artifacts.

A practical structure:

```markdown
## K1: State Update Safety

### Problem Statement
...

### Options Considered
...

### Decision
**CHOSEN:** ...

### Rationale
...

### Implementation Notes
...
```

Examples:

- `../artifacts/research-example-ideavim-api.md`

### Q: How do I reference K-decisions from plan.md and tasks.md?

**Answer:** Reference K-decisions as stable dependencies and cite them where they affect approach.

- In `plan.md`, cite K-decisions at the point the plan commits to an approach.
- In `tasks.md`, cite K-decisions as dependencies (so a task is not started before the decision is accepted).

Example references:

```markdown
## Phase 1: API Finalization

1. State Update Safety (K1)
2. Editor Context Fix (K2)
```

```markdown
### Task 1.1: Implement editorOrNull()
**Dependencies:** K2
```

Examples:

- `../artifacts/plan-example-ideavim-api.md`
- `../artifacts/tasks-example-ideavim-api.md`
- `../artifacts/research-example-ideavim-api.md`

### Q: What should go in research.md vs plan.md?

**Answer:** Put the reasoning in `research.md`, and keep `plan.md` focused on execution approach.

- `research.md`: trade-offs, alternatives, data, platform quirks, decision rationale.
- `plan.md`: chosen approach, phases, gates, and how you will sequence work.

If your plan starts looking like an argument, that content usually belongs in research.

Examples:

- `../artifacts/research-example-ideavim-api.md`
- `../artifacts/plan-example-ideavim-api.md`

### Q: Can I keep research in the spec instead?

**Answer:** You can, but it is usually a trade-off:

- Keeping it in `spec.md` can be fine for small unknowns resolved quickly.
- For larger decisions, research content tends to distract from the requirements contract, and it becomes harder to reference cleanly from plan and tasks.

A pragmatic rule:

- If a decision changes how you will implement, and you expect it to be referenced more than once, put it in `research.md` and reference it by K-number.

Examples:

- `../artifacts/spec-example-ideavim-api.md`
- `../artifacts/research-example-ideavim-api.md`

### Q: How "finished" does research.md need to be before planning?

**Answer:** It needs to be complete enough that `plan.md` can commit to an approach without leaving major unknowns unresolved.

- Resolve K-decisions that affect architecture, boundaries, or sequencing.
- Leave follow-ups explicitly labeled (for example, "needs investigation") only when they do not block a coherent plan.

Examples:

- `../artifacts/research-example-ideavim-api.md`
- `../artifacts/plan-example-ideavim-api.md`

## Metadata and Estimation

### Q: What frontmatter should I include?

**Answer:** Start with minimal metadata that supports traceability and planning, then add fields only when they drive real behavior.

Common fields seen in production artifacts:

- `version`: artifact version (often `1`).
- `feature` and `branch`: to keep repo structure and Git aligned.
- `effort` (S/M/L): a time-size bucket.
- `complexity` (Low/Medium/High): novelty and unknowns.
- `sync_impact`: the constitution version this artifact assumes.
- `companion_spec` / `companion_plan`: links between artifacts.

Example frontmatter (spec-style):

```yaml
---
version: 1
feature: IdeaVim Extension API Layer
branch: 001-api-layer
effort: L
complexity: High
sync_impact: 1.2.2
---
```

Examples:

- `../docs/artifact-structure.md`
- `../artifacts/spec-example-ideavim-api.md`
- `../artifacts/spec-example-backpack-nx.md`

### Q: How do teams use SYNC IMPACT fields?

**Answer:** `sync_impact` is a "staleness detector": it records which constitution version the artifact was written against.

Teams use it to:

- Notice when a constitution changed mid-feature (and decide whether to regenerate plan/tasks).
- Reduce "silent drift" where a plan violates newly added principles.
- Make constitutional changes safer by making the impact explicit.

Practical workflow:

1. Constitution changes from 1.2.1 -> 1.2.2.
2. Active features with `sync_impact: 1.2.1` get reviewed.
3. If the change affects planning gates, regenerate `plan.md` and `tasks.md` (or explicitly justify exceptions).

Examples:

- `../docs/artifact-structure.md`
- `../docs/spec-driven-development.md`

### Q: How should I estimate effort (S/M/L) and complexity?

**Answer:** Treat effort and complexity as separate dimensions:

- Effort (S/M/L): time and surface area.
- Complexity (Low/Medium/High): uncertainty and integration risk.

Practical sizing guidance grounded in the examples:

- S: 2-4 hours (single subsystem, obvious path).
- M: 1-2 days (multiple files, moderate integration).
- L: 3-5 days (cross-cutting changes, migrations, or research required).

Practical complexity guidance:

- Low: repeatable pattern, constraints known.
- Medium: some unknowns, but bounded.
- High: platform constraints or architectural decisions; likely benefits from `research.md`.

Examples:

- `../artifacts/tasks-example-taskify.md`
- `../artifacts/tasks-example-ideavim-api.md`
- `../artifacts/tasks-example-backpack-nx.md`

### Q: How do I keep estimates honest as work evolves?

**Answer:** Update `tasks.md` as you learn:

- If a task grows, split it and adjust estimates.
- If a task depends on an unresolved decision, add the dependency explicitly (often a K-decision or a clarification).
- If the plan changes due to new constraints, update `plan.md` and let tasks follow.

In the real-world examples, you can see that explicit dependencies and phased breakdowns are what make updates survivable.

Examples:

- `../artifacts/plan-example-ideavim-api.md`
- `../artifacts/tasks-example-ideavim-api.md`

### Q: Should I include calendar estimates (like "2 weeks") in addition to S/M/L?

**Answer:** Only if the number will be used. Many teams keep S/M/L for portability, and add calendar estimates when the plan needs to align with external timelines.

If you include both:

- Put S/M/L and complexity in frontmatter.
- Put the calendar estimate near the top of `plan.md` or `tasks.md` so it is visible.
- Call out the assumptions (team size, review latency, external dependencies).

Examples:

- `../artifacts/spec-example-backpack-nx.md`
- `../artifacts/plan-example-backpack-nx.md`

## Clarifications

### Q: What does [NEEDS CLARIFICATION] mean?

**Answer:** `[NEEDS CLARIFICATION]` is a deliberate marker that the spec is not yet unambiguous.

Use it to:

- Flag missing inputs that materially change implementation.
- Avoid premature technical decisions.
- Create an explicit checklist of open questions.

Good `[NEEDS CLARIFICATION]` markers are specific:

```markdown
FR-003: Dashboard MUST load within [NEEDS CLARIFICATION: 95th percentile under 2s, or 99th?]
```

Examples:

- `../docs/spec-driven-development.md`
- `../docs/artifact-structure.md`

### Q: Should I track dated Q&A clarifications in the spec?

**Answer:** Yes when decisions benefit from an audit trail.

Dated sessions help when:

- Multiple stakeholders are involved.
- Requirements evolve over several reviews.
- You need to preserve decision context (not just the final answer).

A lightweight format:

```markdown
## Clarifications

### Session 2026-01-27
- Q: What directory structure should we adopt?
- A: Keep packages under `packages/` with a flat structure.
```

Examples:

- `../artifacts/spec-example-backpack-nx.md`
- `../artifacts/spec-example-ideavim-api.md`

### Q: What should block plan generation vs be deferred?

**Answer:** Block plan generation when the unknown changes architecture, sequencing, or acceptance criteria.

Generally blocking:

- Unclear scope boundaries (what is in/out).
- Missing non-functional targets that drive design (performance, security constraints).
- Unknown platform constraints that affect feasibility.
- External dependency commitments (APIs you must integrate with).

Generally deferrable:

- Cosmetic naming decisions.
- Nice-to-have stretch goals.
- Minor UX polish details that do not change core data flows.

If you defer something, document it explicitly as out of scope or as a future milestone, so it does not quietly become "required" during implementation.

Examples:

- `../artifacts/plan-example-taskify.md`
- `../artifacts/plan-example-backpack-nx.md`

### Q: Where should clarifications live if they affect the plan?

**Answer:** Put clarification outcomes in `spec.md` (as the contract), then reference them in `plan.md` when they constrain approach.

Avoid duplicating full Q&A in the plan. The plan should cite the requirement, not re-argue it.

Examples:

- `../artifacts/spec-example-backpack-nx.md`
- `../artifacts/plan-example-backpack-nx.md`

### Q: What if I discover a missing requirement during implementation?

**Answer:** Treat the spec as the source of truth:

1. Update `spec.md` with the new requirement or clarification (and date it if appropriate).
2. Update `plan.md` if the approach changes.
3. Update `tasks.md` to reflect the new work.

This keeps artifacts consistent and prevents the codebase from drifting away from the stated contract.

Examples:

- `../docs/spec-driven-development.md`
- `../docs/artifact-structure.md`

## Real-World Examples

### Q: Which example set should I copy for my project?

**Answer:** Pick the example closest to your change type:

- Product feature / learning: Taskify.
- API design / refactoring with migrations: IdeaVim API.
- Infrastructure / tooling migration with stakeholder alignment: Backpack Nx.

Examples:

- `../artifacts/spec-example-taskify.md`
- `../artifacts/plan-example-taskify.md`
- `../artifacts/tasks-example-taskify.md`
- `../artifacts/spec-example-ideavim-api.md`
- `../artifacts/plan-example-ideavim-api.md`
- `../artifacts/tasks-example-ideavim-api.md`
- `../artifacts/spec-example-backpack-nx.md`
- `../artifacts/plan-example-backpack-nx.md`
- `../artifacts/tasks-example-backpack-nx.md`

### Q: What does a "research-heavy" feature look like?

**Answer:** It usually has:

- K-numbered decisions (K1, K2, ...) that are referenced by the plan and tasks.
- Platform or integration constraints that require careful sequencing.
- Tasks that explicitly list K-decisions as dependencies.

If you are writing an API for external consumers, or changing a behavior with large compatibility surface area, the IdeaVim set is a good model.

Examples:

- `../artifacts/research-example-ideavim-api.md`
- `../artifacts/plan-example-ideavim-api.md`
- `../artifacts/tasks-example-ideavim-api.md`

### Q: What does an infrastructure spec look like compared to a product spec?

**Answer:** Infrastructure specs often emphasize:

- Strategic context (why the change matters to the organization).
- Current-state vs future-state mapping.
- Clarifications and alignment with external standards.
- Verification-heavy tasks (config changes, CI checks) rather than user-visible acceptance scenarios.

Examples:

- `../artifacts/spec-example-backpack-nx.md`
- `../artifacts/plan-example-backpack-nx.md`
- `../artifacts/tasks-example-backpack-nx.md`

### Q: How do real-world plans keep "how" from turning into a task list?

**Answer:** A useful plan describes phases, gates, and approach decisions without turning into a checklist of micro-steps.

Practical techniques:

- Use phase-level steps, not file-level instructions.
- Put acceptance checks and verification commands in `tasks.md`.
- Reference K-decisions instead of duplicating rationale.

Examples:

- `../artifacts/plan-example-taskify.md`
- `../artifacts/plan-example-ideavim-api.md`

### Q: How do tasks stay actionable in large migrations?

**Answer:** The migration-style examples use:

- Phase grouping aligned with the plan.
- Explicit dependencies (including external dependencies).
- Task acceptance criteria that are testable.
- Ordering that reduces risk (easy migrations first, stabilization last).

Examples:

- `../artifacts/tasks-example-ideavim-api.md`
- `../artifacts/tasks-example-backpack-nx.md`

---

**See Also:**

- `../docs/artifact-structure.md`
- `../docs/spec-driven-development.md`
- `constitution-evolution.md`
- `../artifacts/README.md`
