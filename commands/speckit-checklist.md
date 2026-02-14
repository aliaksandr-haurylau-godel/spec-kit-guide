---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/README.md (line 270 table)
---

# /speckit.checklist

## Purpose

Generates custom quality checklists that validate requirements completeness, clarity, and consistency. Acts like "unit tests for English"—systematic validation of specification quality before implementation.

## When to Use

**Optional step** after creating or refining your spec with `/speckit.specify` or `/speckit.clarify`. Use this command to:
- Validate specification completeness
- Check for ambiguous language
- Ensure testable acceptance criteria
- Verify consistent terminology
- Catch missing edge cases

## Prerequisites

- Spec created with `/speckit.specify`
- Active feature branch (or `SPECIFY_FEATURE` set)
- `specs/[feature]/spec.md` exists

## Input Format

```bash
/speckit.checklist [optional-focus-areas]
```

**Without arguments:** Generates comprehensive quality checklist covering all spec aspects.

**With arguments:** Generates checklist focused on specific areas (e.g., "security requirements", "API design", "user experience").

## Output

**Checklist Document** with systematic checks for:

1. **Requirement Completeness**
   - [ ] All user stories have acceptance criteria
   - [ ] No `[NEEDS CLARIFICATION]` markers remain
   - [ ] Non-functional requirements defined
   - [ ] Success metrics specified

2. **Clarity & Precision**
   - [ ] Requirements use unambiguous language
   - [ ] Acceptance criteria are testable
   - [ ] Edge cases identified
   - [ ] Error handling described

3. **Consistency**
   - [ ] Terminology used consistently
   - [ ] No contradictory requirements
   - [ ] Data structures match across stories
   - [ ] User roles clearly defined

4. **Constitutional Compliance**
   - [ ] Spec doesn't prescribe implementation details
   - [ ] Focus on "what" and "why", not "how"
   - [ ] Library-first thinking present
   - [ ] Test scenarios included

## Example

**Generic checklist:**

```bash
/speckit.checklist
```

**Focused checklist:**

```bash
/speckit.checklist Focus on security and data validation requirements
```

**Taskify Example Output:**

```markdown
## Quality Checklist: Create Taskify

### Requirement Completeness
- [x] All 3 user stories have acceptance criteria
- [x] No [NEEDS CLARIFICATION] markers remain
- [x] Non-functional requirements defined (real-time updates)
- [ ] Success metrics specified
  ⚠ MISSING: Define what "successful" means (e.g., response time < 200ms)

### Clarity & Precision
- [x] Requirements use unambiguous language
- [x] Acceptance criteria are testable
- [ ] Edge cases identified
  ⚠ MISSING: What happens if all projects are deleted?
  ⚠ MISSING: What if two users edit same comment simultaneously?
- [x] Error handling described (read-only comment edit attempts)

### Consistency
- [x] Terminology used consistently (tasks, projects, boards)
- [x] No contradictory requirements
- [x] Data structures match (5 users, 3 projects)
- [x] User roles clearly defined (PM + 4 engineers)

### Constitutional Compliance
- [x] Spec doesn't prescribe implementation details
- [x] Focus on "what" and "why", not "how"
- [ ] Library-first thinking present
  ⚠ NOTE: Spec doesn't mention libraries (OK for spec phase)
- [x] Test scenarios included (checkpoint validations)

### Recommendations
1. Add success metrics (response times, concurrent user count)
2. Define edge case: Behavior when all projects deleted
3. Clarify conflict resolution for simultaneous edits
```

## How to Use the Checklist

### Step 1: Generate

```bash
/speckit.checklist
```

### Step 2: Review Unchecked Items

The AI will mark items `[ ]` (unchecked) if they're missing or unclear.

### Step 3: Fix Issues

For each unchecked item, either:
- **Update the spec** with missing information
- **Use `/speckit.clarify`** to add details
- **Document as intentional** if it's out of scope

### Step 4: Regenerate (Optional)

After fixes, regenerate to verify all checks pass:

```bash
/speckit.checklist
```

## Checklist as "Unit Tests for English"

Just like unit tests catch bugs in code, checklists catch bugs in specifications:

**Code Unit Test:**
```javascript
test('createUser returns user with ID', () => {
  const user = createUser('Alice');
  expect(user.id).toBeDefined();
});
```

**Spec Checklist:**
```markdown
- [ ] User Story 1 has testable acceptance criteria
  ⚠ FAIL: "Users can create projects" is not testable
  ✓ FIX: "When user clicks 'New Project', a project with generated ID appears in list"
```

Both catch issues **before they become expensive to fix**.

## Common Checklist Failures

### ❌ Ambiguous Requirements

```text
- [ ] Requirements use unambiguous language
  ⚠ FAIL: "Users can edit comments" - Any comment or only their own?
```

**Fix:** Use `/speckit.clarify` to specify "Users can edit only their own comments".

---

### ❌ Untestable Acceptance Criteria

```text
- [ ] Acceptance criteria are testable
  ⚠ FAIL: "UI should be intuitive" - How do you test intuition?
```

**Fix:** Change to "New users can create a project in ≤3 clicks".

---

### ❌ Missing Edge Cases

```text
- [ ] Edge cases identified
  ⚠ MISSING: What happens when database is down?
  ⚠ MISSING: What if user deletes project while viewing it?
```

**Fix:** Add error handling requirements to spec.

---

### ❌ Inconsistent Terminology

```text
- [ ] Terminology used consistently
  ⚠ FAIL: Story 1 uses "tasks", Story 2 uses "work items"
```

**Fix:** Pick one term and update spec.

## Related Commands

- `/speckit.specify` - Create spec (then validate with checklist)
- `/speckit.clarify` - Refine spec based on checklist failures
- `/speckit.plan` - Move to planning after checklist passes

## See Also

- [Template Constraints](../tricks/template-constraints.md) - How checklists guide AI
- [Basic Workflow](../workflows/basic-workflow.md) - Using checklist in workflow
