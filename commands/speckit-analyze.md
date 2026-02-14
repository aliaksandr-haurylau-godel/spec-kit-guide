---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/docs/quickstart.md
---

# /speckit.analyze

## Purpose

Performs cross-artifact consistency and coverage analysis across your spec, plan, and tasks. Identifies gaps, inconsistencies, and missing elements before implementation begins.

## When to Use

**Optional step** between `/speckit.tasks` and `/speckit.implement`. Use this command to:
- Validate consistency across spec, plan, and tasks
- Identify missing test coverage
- Find requirements not addressed in the plan
- Catch contradictions between documents
- Ensure all user stories have corresponding tasks

## Prerequisites

- Plan and tasks created (`/speckit.plan` + `/speckit.tasks`)
- Active feature branch (or `SPECIFY_FEATURE` set)
- `specs/[feature]/spec.md`, `plan.md`, and `tasks.md` exist

## Input Format

```bash
/speckit.analyze
```

No arguments needed. Analyzes all documents in the feature directory automatically.

## Output

**Analysis Report** covering:

1. **Spec → Plan Alignment**
   - User stories mapped to plan sections
   - Non-functional requirements addressed
   - Missing requirements in plan

2. **Plan → Tasks Alignment**
   - Plan components mapped to tasks
   - Unimplemented plan elements
   - Tasks without plan justification

3. **Test Coverage**
   - Acceptance criteria with tests
   - Acceptance criteria without tests
   - Edge cases covered/uncovered

4. **Contract Consistency**
   - API endpoints defined in contracts
   - API endpoints implemented in tasks
   - Missing contract definitions

5. **Recommendations**
   - Suggested additions to plan
   - Suggested test scenarios
   - Potential risks

## Example

```markdown
/speckit.analyze
```

**Example Output:**

```text
## Analysis Report: 001-create-taskify

### ✓ Spec → Plan Alignment
- User Story 1 (User Selection): Addressed in plan Section 2.1
- User Story 2 (Project List): Addressed in plan Section 2.2
- User Story 3 (Kanban Board): Addressed in plan Section 2.3
- Non-functional Req (Real-time updates): Addressed via WebSocket plan

### ⚠ Plan → Tasks Alignment
- FOUND: plan.md Section 3.2 (Notification service) not in tasks.md
- RECOMMENDATION: Add tasks for notification service implementation

### ✓ Test Coverage
- User Story 1: 3/3 acceptance criteria have tests
- User Story 2: 4/4 acceptance criteria have tests
- User Story 3: 5/6 acceptance criteria have tests
  ⚠ MISSING: Test for "Comments by other users are read-only"

### ✓ Contract Consistency
- All REST endpoints from api-spec.json have tasks
- All WebSocket events from websocket-spec.md have tasks

### Recommendations
1. Add test for read-only comments (User Story 3)
2. Include notification service tasks or remove from plan
3. Consider edge case: What happens when all 3 projects are deleted?
```

## What Gets Analyzed

### Spec Analysis
- All user stories present
- Acceptance criteria defined
- Non-functional requirements clear
- `[NEEDS CLARIFICATION]` markers resolved

### Plan Analysis
- Every user story addressed
- Technology choices justified
- Phase -1 Gates completed
- Architecture diagram present

### Tasks Analysis
- Every plan section has tasks
- Tasks have file paths
- Test tasks present (TDD)
- Checkpoints defined

### Cross-Document Analysis
- Spec requirements → Plan sections → Tasks
- Plan architecture → Task file structure
- Acceptance criteria → Test tasks
- Contracts → Implementation tasks

## Why Use This

**Catch issues before coding:**

Without `/speckit.analyze`:
1. Implement all tasks
2. Realize a user story isn't addressed
3. Go back to planning
4. Refactor code

With `/speckit.analyze`:
1. Run analysis
2. Fix gaps in plan/tasks
3. Implement with confidence
4. No refactoring needed

**Example caught issue:**

```text
⚠ INCONSISTENCY FOUND:
  - spec.md User Story 2 requires "Real-time project updates"
  - plan.md uses REST API (polling) instead of WebSocket
  - RECOMMENDATION: Update plan to use WebSocket for real-time updates,
    or clarify spec that polling is acceptable
```

## When to Skip

You can skip `/speckit.analyze` if:
- Your spec and plan are simple and straightforward
- You're doing a spike or prototype (exploratory work)
- Time constraints require fast implementation
- You're confident in the consistency (experienced with Spec-Kit)

## Related Commands

- `/speckit.tasks` - Previous step: Generate tasks
- `/speckit.implement` - Next step: Execute tasks
- `/speckit.plan` - Revise plan if issues found
- `/speckit.clarify` - Clarify spec if ambiguities found

## See Also

- [Basic Workflow Step 6b](../workflows/basic-workflow.md#6b-optional---analyze-consistency) - Analysis in context
- [Template Constraints](../tricks/template-constraints.md) - How templates ensure consistency
