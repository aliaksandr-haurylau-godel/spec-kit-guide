---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/README.md (lines 104-110), quickstart.md
---

# /speckit.specify

## Purpose

Creates a comprehensive feature specification from a simple user prompt. Transforms your idea into a structured document with user stories, acceptance criteria, and requirements—without any tech stack decisions.

## When to Use

**Second step** in the workflow, after defining your constitution. Use this command when you want to:
- Start a new feature or project
- Define **what** you're building and **why**
- Create user stories and acceptance criteria
- Avoid jumping straight to implementation details

## Prerequisites

- Spec-Kit initialized (`specify init`)
- Constitution defined (`/speckit.constitution`)
- Clear idea of what you want to build (even if vague)

## Input Format

```bash
/speckit.specify <description-of-what-you-want-to-build>
```

Focus on the **what** and **why**, not the tech stack (no frameworks, libraries, or implementation details).

## Output

**What Gets Created:**

1. **New Git branch** (e.g., `001-feature-name`) - Auto-numbered based on existing specs
2. **Feature directory** - `specs/001-feature-name/`
3. **Specification file** - `specs/001-feature-name/spec.md` containing:
   - Feature overview
   - User stories with acceptance criteria
   - Non-functional requirements
   - Out of scope items
   - Review & acceptance checklist
   - `[NEEDS CLARIFICATION]` markers for ambiguities

## Example

**Generic Example:**

```markdown
/speckit.specify Build an application that can help me organize my photos in separate photo albums. Albums are grouped by date and can be re-organized by dragging and dropping on the main page. Albums are never in other nested albums. Within each album, photos are previewed in a tile-like interface.
```

**Taskify Example:**

```markdown
/speckit.specify Develop Taskify, a team productivity platform. It should allow users to create projects, add team members, assign tasks, comment and move tasks between boards in Kanban style. In this initial phase for this feature, let's call it "Create Taskify," let's have multiple users but the users will be declared ahead of time, predefined. I want five users in two different categories, one product manager and four engineers. Let's create three different sample projects. Let's have the standard Kanban columns for the status of each task, such as "To Do," "In Progress," "In Review," and "Done." There will be no login for this application as this is just the very first testing thing to ensure that our basic features are set up.
```

## What Happens

1. **Branch Creation**: AI automatically:
   - Scans existing specs to determine next feature number
   - Generates semantic branch name from your description
   - Creates and switches to new branch (e.g., `001-create-taskify`)

2. **Directory Setup**: Creates `specs/001-feature-name/` directory

3. **Spec Generation**: 
   - Copies and customizes feature specification template
   - Populates with your requirements as structured user stories
   - Marks ambiguous areas with `[NEEDS CLARIFICATION]`
   - Includes review checklist

4. **Context Awareness**: Your AI agent now tracks this feature via Git branch

## Spec Structure

The generated `spec.md` follows this template structure:

```markdown
# Feature: [Name]

## Overview
[High-level description]

## User Stories

### Story 1: [Title]
**As a** [persona]
**I want** [goal]
**So that** [benefit]

**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2

## Non-Functional Requirements
- Performance
- Security
- Accessibility

## Out of Scope
- Features explicitly excluded

## Review & Acceptance Checklist
- [ ] All user stories have acceptance criteria
- [ ] No [NEEDS CLARIFICATION] markers remain
- [ ] Requirements are testable
```

## Common Patterns

**Good prompts include:**
- User personas ("team members can...", "project managers need...")
- User goals and motivations
- Interaction patterns ("drag and drop", "click to open")
- Data structures ("5 users", "3 projects")
- Constraints ("no login", "predefined users")

**Avoid in prompts:**
- Tech stack mentions (React, Python, PostgreSQL)
- Implementation details (REST APIs, WebSockets)
- Architecture decisions (microservices, serverless)

Save those for `/speckit.plan`!

## Related Commands

- `/speckit.clarify` - Next step: Refine ambiguities in the spec
- `/speckit.plan` - Create technical implementation plans
- `/speckit.checklist` - Generate quality checklists for the spec

## See Also

- [Spec Example (Taskify)](../artifacts/spec-example-taskify.md) - Complete specification
- [Basic Workflow Step 3](../workflows/basic-workflow.md#step-3-create-the-spec) - In context
- [Template Constraints](../tricks/template-constraints.md) - How templates guide AI
