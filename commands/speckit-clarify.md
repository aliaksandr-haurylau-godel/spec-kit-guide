---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/docs/quickstart.md (lines 50-54), README.md (lines 464-482)
---

# /speckit.clarify

## Purpose

Identifies and resolves ambiguities in your specification through structured, sequential questioning. Helps ensure your spec is complete, clear, and unambiguous before moving to technical planning.

## When to Use

**Fourth step** in the workflow, after creating your initial spec with `/speckit.specify` and before planning with `/speckit.plan`. Use this command to:
- Remove `[NEEDS CLARIFICATION]` markers from your spec
- Address edge cases not captured in the initial prompt
- Define detailed interaction patterns
- Clarify security and performance requirements
- Ensure acceptance criteria are testable

## Prerequisites

- Spec created with `/speckit.specify`
- Active feature branch (or `SPECIFY_FEATURE` set)
- `specs/[feature]/spec.md` exists

## Input Format

```bash
/speckit.clarify [optional-focus-areas]
```

**Without arguments:** AI performs structured, coverage-based questioning across all spec areas.

**With arguments:** Provide specific details or focus areas you want to clarify.

## Output

**File Updated:**
- `specs/[feature]/spec.md` - Updated with:
  - New "Clarifications" section documenting Q&A
  - Removed or resolved `[NEEDS CLARIFICATION]` markers
  - Enhanced user stories and acceptance criteria
  - More detailed interaction patterns

## Example

**Structured clarification (no arguments):**

```bash
/speckit.clarify
```

The AI will ask sequential questions like:
- "How should the system handle invalid input in [User Story 2]?"
- "What permissions do different user roles have?"
- "What's the expected performance for [feature]?"

**Focused clarification (with details):**

```bash
/speckit.clarify Focus on security and performance requirements.
```

**Taskify Example (Initial):**

```bash
/speckit.clarify Focus on task card details and user interactions.
```

**Taskify Example (Additional refinement):**

```markdown
/speckit.clarify When you first launch Taskify, it's going to give you a list of the five users to pick from. There will be no password required. When you click on a user, you go into the main view, which displays the list of projects. When you click on a project, you open the Kanban board for that project. You're going to see the columns. You'll be able to drag and drop cards back and forth between different columns. You will see any cards that are assigned to you, the currently logged in user, in a different color from all the other ones, so you can quickly see yours. You can edit any comments that you make, but you can't edit comments that other people made. You can delete any comments that you made, but you can't delete comments anybody else made.
```

## What Happens

### Structured Mode (No Arguments)

1. AI analyzes the spec for:
   - `[NEEDS CLARIFICATION]` markers
   - Ambiguous requirements
   - Missing edge cases
   - Undefined behavior patterns

2. AI asks targeted questions sequentially:
   - Waits for your answer before proceeding
   - Records answers in "Clarifications" section
   - Updates relevant user stories

3. Repeats until all critical ambiguities are resolved

### Direct Clarification Mode (With Arguments)

1. AI interprets your clarification as answers to implicit questions
2. Updates spec immediately with new details
3. Enhances user stories and acceptance criteria
4. Removes resolved `[NEEDS CLARIFICATION]` markers

## Best Practices

**Run structured clarification first:**
```bash
/speckit.clarify
```
Let the AI guide you through systematic questions.

**Then use focused clarification for remaining details:**
```bash
/speckit.clarify [specific details about edge cases]
```

**Validate the checklist after:**
```text
Read the review and acceptance checklist, and check off each item in the checklist if the feature spec meets the criteria. Leave it empty if it does not.
```

## Why This Matters

**Clarification before planning prevents rework.** It's much easier to fix ambiguities in natural language than to refactor code later.

**Example of prevented rework:**

Without clarification:
```text
1. Write spec with ambiguity: "users can edit comments"
2. Plan assumes anyone can edit any comment
3. Implement full edit permissions
4. Realize spec meant "users can edit THEIR OWN comments"
5. Refactor code to add ownership checks
```

With clarification:
```text
1. Write spec with ambiguity: "users can edit comments"
2. /speckit.clarify asks: "Can users edit any comment, or only their own?"
3. You answer: "Only their own"
4. Spec updated: "users can edit their own comments but not others'"
5. Plan correctly implements ownership checks from the start
```

## Common Clarification Areas

The AI typically asks about:
- **Permissions & Authorization** - Who can do what?
- **Edge Cases** - What happens when things go wrong?
- **Data Validation** - What's valid/invalid input?
- **User Interactions** - Exact click/tap flows
- **Performance Expectations** - How fast should it be?
- **Error Handling** - What error messages appear?
- **Data Relationships** - How entities connect

## Related Commands

- `/speckit.specify` - Previous step: Create initial spec
- `/speckit.checklist` - Validate spec completeness
- `/speckit.plan` - Next step: Create technical plan

## See Also

- [Basic Workflow Step 4](../workflows/basic-workflow.md#step-4-refine-the-spec) - Clarification in context
- [Template Constraints](../tricks/template-constraints.md) - How templates force explicit clarification
