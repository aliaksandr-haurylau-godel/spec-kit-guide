---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/README.md (lines 112-118)
---

# /speckit.plan

## Purpose

Creates a comprehensive technical implementation plan from your feature specification. Translates **what** you want to build into **how** it will be built, with technology choices, architecture, and detailed design documents.

## When to Use

**Fifth step** in the workflow, after refining your spec with `/speckit.clarify`. Use this command when you're ready to:
- Choose your tech stack (frameworks, languages, databases)
- Define architecture and system design
- Create API contracts and data models
- Plan implementation phases
- Validate constitutional compliance

## Prerequisites

- Spec created and refined (`/speckit.specify` + `/speckit.clarify`)
- Active feature branch (or `SPECIFY_FEATURE` set)
- `specs/[feature]/spec.md` exists
- No remaining `[NEEDS CLARIFICATION]` markers in spec (recommended)

## Input Format

```bash
/speckit.plan <tech-stack-and-architecture-description>
```

Provide specific technology choices, architecture decisions, and implementation preferences.

## Output

**Files Created:**

1. **`specs/[feature]/plan.md`** - High-level implementation plan with:
   - Technology decisions and rationale
   - System architecture
   - Implementation phases
   - Phase -1 Gates (constitutional compliance checks)
   - Testing strategy

2. **`specs/[feature]/data-model.md`** - Database schemas, entities, relationships

3. **`specs/[feature]/contracts/`** - API specifications:
   - REST API endpoints (`api-spec.json` or `.md`)
   - WebSocket events (`websocket-spec.md`)
   - GraphQL schemas (if applicable)
   - Service contracts

4. **`specs/[feature]/research.md`** - Technology research:
   - Library comparisons
   - Version-specific details
   - Best practices for chosen stack

5. **`specs/[feature]/quickstart.md`** - Key validation scenarios for manual testing

## Example

**Generic Example (Vite + SQLite):**

```bash
/speckit.plan The application uses Vite with minimal number of libraries. Use vanilla HTML, CSS, and JavaScript as much as possible. Images are not uploaded anywhere and metadata is stored in a local SQLite database.
```

**Taskify Example (.NET Aspire + Blazor):**

```bash
/speckit.plan We are going to generate this using .NET Aspire, using Postgres as the database. The frontend should use Blazor server with drag-and-drop task boards, real-time updates. There should be a REST API created with a projects API, tasks API, and a notifications API.
```

## What Happens

1. **Specification Analysis**: AI reads the spec to understand:
   - User stories and acceptance criteria
   - Non-functional requirements
   - Constraints and scope

2. **Constitutional Alignment**: Ensures plan respects:
   - Constitution principles (`.specify/memory/constitution.md`)
   - Phase -1 Gates (Simplicity, Anti-Abstraction, Integration-First)

3. **Technical Translation**: Converts business requirements into:
   - Data models (entities, fields, relationships)
   - API contracts (endpoints, request/response formats)
   - System architecture (components, communication patterns)

4. **Supporting Documents**: Creates:
   - Research on chosen technologies
   - Quickstart validation guide
   - Implementation phase breakdown

5. **File Generation**: Writes all documents to `specs/[feature]/` directory

## Plan Structure

The generated `plan.md` includes:

```markdown
# Implementation Plan: [Feature]

## Technology Stack
- Frontend: [framework]
- Backend: [framework]
- Database: [choice]
- Real-time: [WebSocket/SignalR/etc]

**Rationale:** [Why these choices]

## System Architecture
[Component diagram / description]

## Phase -1: Pre-Implementation Gates

### Simplicity Gate (Article VII)
- [ ] Using ≤3 projects?
- [ ] No future-proofing?

### Anti-Abstraction Gate (Article VIII)
- [ ] Using framework directly?
- [ ] Single model representation?

### Integration-First Gate (Article IX)
- [ ] Contracts defined?
- [ ] Contract tests written?

## Core Implementation
[Detailed phases and steps]

## Testing Strategy
- Unit tests
- Integration tests
- Contract tests
- E2E tests

## See Also
- Data model: `data-model.md`
- API contracts: `contracts/`
- Research: `research.md`
```

## Advanced Usage

### Deep Research for Rapidly Changing Tech

If using a rapidly evolving framework:

```text
I want you to go through the implementation plan and implementation details, looking for areas that could benefit from additional research as [framework name] is a rapidly changing library. For those areas that you identify that require further research, I want you to update the research document with additional details about the specific versions that we are going to be using in this application and spawn parallel research tasks to clarify any details using research from the web.
```

### Validate Against Constitution

After plan generation:

```text
Please review the implementation plan against the constitution in .specify/memory/constitution.md and ensure all Phase -1 Gates pass. Simplify any unnecessary complexity.
```

### Refine Plan Details

Ask AI to add cross-references:

```text
Now I want you to go and audit the implementation plan and the implementation detail files. Read through it with an eye on determining whether or not there is a sequence of tasks that you need to be doing that are obvious from reading this. For example, when I look at the core implementation, it would be useful to reference the appropriate places in the implementation details where it can find the information as it walks through each step.
```

## Related Commands

- `/speckit.tasks` - Next step: Generate actionable task breakdown
- `/speckit.analyze` - Validate plan consistency
- `/speckit.clarify` - Previous step: Refine spec

## See Also

- [Plan Example (Taskify)](../artifacts/plan-example-taskify.md) - Complete plan
- [Basic Workflow Step 5](../workflows/basic-workflow.md#step-5-create-a-technical-implementation-plan) - In context
- [Constitutional Gates](../tricks/constitutional-gates.md) - Phase -1 enforcement
- [Constitution Reference](../docs/constitution-reference.md) - Principles to follow
