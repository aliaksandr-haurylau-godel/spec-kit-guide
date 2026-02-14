---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/docs/quickstart.md, README.md
---

# Basic Workflow

The complete 6-step process for building software with Spec-Driven Development. This workflow uses **Taskify** (a team productivity platform) as a running example to illustrate each step.

## Overview

Spec-Kit provides a structured workflow that emphasizes specification-first development. Instead of jumping straight to code, you define **what** you're building (specification), **why** it matters (user stories), and then let AI handle the **how** (implementation).

**The 6 Steps:**
1. Install Specify
2. Define Your Constitution
3. Create the Spec
4. Refine the Spec
5. Create a Technical Implementation Plan
6. Break Down and Implement

---

## Prerequisites

Before starting, ensure you have:

- [uv](https://docs.astral.sh/uv/) package manager
- Python 3.11+
- Git
- An AI coding agent ([see supported agents](../docs/supported-agents.md))

---

## Step 1: Install Specify

**In your terminal**, run the `specify` CLI command to initialize your project:

```bash
# Create a new project directory
uvx --from git+https://github.com/github/spec-kit.git specify init taskify

# OR initialize in the current directory
uvx --from git+https://github.com/github/spec-kit.git specify init .
```

**For persistent installation (recommended):**

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git

# Then use directly:
specify init taskify --ai claude
```

**Pick script type explicitly (optional):**

```bash
specify init taskify --script ps  # Force PowerShell
specify init taskify --script sh  # Force POSIX shell
```

**What This Creates:**
- `.specify/` directory with templates, scripts, and memory
- `.claude/commands/` (or equivalent for your agent) with slash commands
- Git repository (unless you use `--no-git`)

See [Installation Guide](../docs/installation.md) for all options.

---

## Step 2: Define Your Constitution

**In your AI Agent's chat interface**, use the `/speckit.constitution` slash command to establish the core rules and principles for your project.

### Generic Guidance

The constitution should include:
- Development principles (e.g., "Library-First", "Test-Driven Development")
- Quality standards (e.g., code coverage requirements)
- Architectural constraints (e.g., maximum project count, no unnecessary abstractions)
- Security requirements
- Performance requirements

### Taskify Example

```markdown
/speckit.constitution Taskify is a "Security-First" application. All user inputs must be validated. We use a microservices architecture with a maximum of 3 projects for the initial implementation. Code must follow strict TDD with tests written before implementation. We prefer functional programming patterns. All APIs must be documented. Real-time updates should use WebSockets or similar push technology.
```

**What This Creates:**
- `.specify/memory/constitution.md` file with your project's governing principles
- AI agent will reference these principles during all subsequent steps

**Why This Matters:**
The constitution acts as the "architectural DNA" of your project. It ensures consistency across all generated code and prevents over-engineering. See [Constitution Reference](../docs/constitution-reference.md) for the nine standard articles.

---

## Step 3: Create the Spec

**In the chat**, use the `/speckit.specify` slash command to describe what you want to build. Focus on the **what** and **why**, not the tech stack.

### Generic Guidance

Good specifications include:
- **What** the application does (core features)
- **Why** it exists (user problems it solves)
- **Who** will use it (user personas)
- **User Stories** with acceptance criteria
- **No tech stack details** at this stage

### Taskify Example

```markdown
/speckit.specify Develop Taskify, a team productivity platform. It should allow users to create projects, add team members, assign tasks, comment and move tasks between boards in Kanban style. In this initial phase for this feature, let's call it "Create Taskify," let's have multiple users but the users will be declared ahead of time, predefined. I want five users in two different categories, one product manager and four engineers. Let's create three different sample projects. Let's have the standard Kanban columns for the status of each task, such as "To Do," "In Progress," "In Review," and "Done." There will be no login for this application as this is just the very first testing thing to ensure that our basic features are set up.
```

**What This Creates:**
- New Git branch (e.g., `001-create-taskify`)
- `specs/001-create-taskify/spec.md` with:
  - Feature overview
  - User stories
  - Acceptance criteria
  - Non-functional requirements
  - Review checklist

**Context Awareness:** Spec-Kit automatically detects the active feature based on your Git branch. To work on different features, simply switch branches.

See the complete [Taskify specification](../artifacts/spec-example-taskify.md).

---

## Step 4: Refine the Spec

**In the chat**, use the `/speckit.clarify` slash command to identify and resolve ambiguities in your specification.

### Generic Guidance

The AI will ask structured questions to clarify:
- Edge cases not addressed
- Ambiguous requirements
- Missing acceptance criteria
- Security and performance details
- User interaction patterns

You can also provide specific focus areas as arguments.

### Taskify Example

**Initial clarification:**
```bash
/speckit.clarify Focus on task card details and user interactions.
```

**Additional refinement with more details:**
```markdown
/speckit.clarify When you first launch Taskify, it's going to give you a list of the five users to pick from. There will be no password required. When you click on a user, you go into the main view, which displays the list of projects. When you click on a project, you open the Kanban board for that project. You're going to see the columns. You'll be able to drag and drop cards back and forth between different columns. You will see any cards that are assigned to you, the currently logged in user, in a different color from all the other ones, so you can quickly see yours. You can edit any comments that you make, but you can't edit comments that other people made. You can delete any comments that you made, but you can't delete comments anybody else made.
```

**Optional - Validate the checklist:**
```text
Read the review and acceptance checklist, and check off each item in the checklist if the feature spec meets the criteria. Leave it empty if it does not.
```

**What This Updates:**
- `specs/001-create-taskify/spec.md` with:
  - Clarifications section
  - Updated user stories
  - More detailed acceptance criteria
  - Checked items in review checklist

**Why This Matters:**
Clarification before planning prevents rework. It's easier to fix ambiguities in the spec than to refactor code later.

---

## Step 5: Create a Technical Implementation Plan

**In the chat**, use the `/speckit.plan` slash command to provide your tech stack and architecture choices. **Now** you can be specific about technology.

### Generic Guidance

The plan should specify:
- Framework and language choices
- Database technology
- API architecture (REST, GraphQL, etc.)
- Frontend approach
- Third-party libraries to use (or avoid)
- Deployment target

### Taskify Example

```bash
/speckit.plan We are going to generate this using .NET Aspire, using Postgres as the database. The frontend should use Blazor server with drag-and-drop task boards, real-time updates. There should be a REST API created with a projects API, tasks API, and a notifications API.
```

**What This Creates:**
- `specs/001-create-taskify/plan.md` - High-level implementation plan
- `specs/001-create-taskify/data-model.md` - Database schemas and entities
- `specs/001-create-taskify/contracts/` - API specifications (REST, WebSocket, etc.)
- `specs/001-create-taskify/research.md` - Technology research and version details
- `specs/001-create-taskify/quickstart.md` - Key validation scenarios

**What's in the plan:**
- Technology decisions with rationale
- Data model (entities, relationships)
- API contracts (endpoints, events)
- Test strategy
- Implementation phases
- Constitutional compliance checks (Phase -1 Gates)

**Optional - Deep research:**
If using rapidly changing tech (e.g., .NET Aspire):
```text
I want you to go through the implementation plan and implementation details, looking for areas that could benefit from additional research as .NET Aspire is a rapidly changing library. For those areas that you identify that require further research, I want you to update the research document with additional details about the specific versions that we are going to be using in this Taskify application and spawn parallel research tasks to clarify any details using research from the web.
```

See the [Taskify plan example](../artifacts/plan-example-taskify.md) (if available).

---

## Step 6: Break Down and Implement

### 6a: Generate Task Breakdown

**In the chat**, use the `/speckit.tasks` slash command to create an actionable task list.

```markdown
/speckit.tasks
```

**What This Creates:**
- `specs/001-create-taskify/tasks.md` with:
  - Tasks organized by user story
  - Dependency management (correct order)
  - Parallel execution markers `[P]`
  - File path specifications
  - Test-driven development structure
  - Checkpoint validation per phase

**Task Structure Example:**
```markdown
### User Story 1: User Selection Screen

1. [P] Create User model - `models/user.cs`
2. [P] Create User database schema - `migrations/001_users.sql`
3. Create User repository - `repositories/user-repo.cs`
4. Write tests for User API - `tests/user-api.test.cs`
5. Create User API endpoint - `api/users.cs`
6. [Checkpoint] Verify users endpoint returns 5 predefined users
```

### 6b: Optional - Analyze Consistency

**In the chat**, validate the plan with `/speckit.analyze`:

```markdown
/speckit.analyze
```

This performs cross-artifact consistency checks:
- Spec vs Plan alignment
- Contract vs Implementation alignment
- Test coverage
- Missing edge cases

### 6c: Implement

**In the chat**, use the `/speckit.implement` slash command to execute the plan.

```markdown
/speckit.implement
```

**What This Does:**
- Validates prerequisites (constitution, spec, plan, tasks exist)
- Parses task breakdown from `tasks.md`
- Executes tasks in correct order
- Respects dependencies and parallel execution markers
- Follows TDD approach (tests first)
- Provides progress updates

**Important:** The AI agent will execute local CLI commands (such as `dotnet`, `npm`, etc.). Make sure you have the required tools installed on your machine.

**After Implementation:**
- Test the application
- Resolve any runtime errors (e.g., browser console errors)
- Copy and paste errors back to your AI agent for resolution

---

## What You Should Have

After completing all 6 steps, your project structure should look like:

```text
taskify/
├── .specify/
│   ├── memory/
│   │   └── constitution.md
│   ├── scripts/
│   │   ├── *.sh
│   │   └── *.ps1
│   └── templates/
│       ├── spec-template.md
│       ├── plan-template.md
│       └── tasks-template.md
├── .claude/commands/ (or equivalent for your agent)
├── specs/
│   └── 001-create-taskify/
│       ├── spec.md
│       ├── plan.md
│       ├── tasks.md
│       ├── data-model.md
│       ├── research.md
│       ├── quickstart.md
│       └── contracts/
│           ├── api-spec.json
│           └── websocket-spec.md
├── src/ (your implementation)
├── tests/ (your test suite)
└── ... (build artifacts, configs, etc.)
```

**On a Git branch** (e.g., `001-create-taskify`) ready for review or merge to `main`.

---

## Key Principles

- **Be explicit** about what you're building and why
- **Don't focus on tech stack** during specification phase (Steps 2-4)
- **Iterate and refine** your specifications before implementation
- **Validate** the plan before coding begins (Step 6b)
- **Let the AI agent handle** the implementation details
- **Trust the process** - resist the urge to jump straight to code

---

## Next Steps

After implementing your first feature:

- **Create additional features** - Repeat this workflow for new features
- **Experiment with parallel implementations** - Try different tech stacks from the same spec
- **Refine your constitution** - Update principles based on what you've learned
- **Explore advanced commands** - `/speckit.checklist` for custom quality gates

---

## Troubleshooting

### Git Branch Issues

If you're not using Git, set the `SPECIFY_FEATURE` environment variable:
```bash
export SPECIFY_FEATURE=001-create-taskify
```

### Missing Slash Commands

Run `specify check` to verify your AI agent is detected and slash commands are installed.

### Constitutional Violations

If the AI suggests over-engineering:
```text
Please review the implementation plan against the constitution in .specify/memory/constitution.md and ensure all Phase -1 Gates pass. Simplify any unnecessary complexity.
```

---

## See Also

- [Installation Guide](../docs/installation.md) - Detailed setup
- [All Slash Commands](../commands/) - Command reference
- [Constitution Reference](../docs/constitution-reference.md) - Nine articles explained
- [Spec-Driven Development](../docs/spec-driven-development.md) - Core methodology
- [Upgrade Workflow](upgrade-project.md) - Updating Spec-Kit
- [Taskify Artifacts](../artifacts/) - Complete examples
