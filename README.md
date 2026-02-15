# GitHub Spec Kit Guide

A comprehensive, community-driven guide to the [GitHub Spec-Kit Framework](https://github.com/github/spec-kit) for Spec-Driven Development (SDD).

## Quick Navigation

### 📖 Core Documentation

Start here to understand Spec-Kit's philosophy and setup:

- **[Spec-Driven Development](docs/spec-driven-development.md)** - Core methodology and principles
- **[Installation Guide](docs/installation.md)** - Complete setup instructions
- **[Constitution Reference](docs/constitution-reference.md)** - All 9 constitutional articles explained
- **[Artifact Structure Guide](docs/artifact-structure.md)** - Complete guide to specs, plans, tasks, and research artifacts
- **[CLI Reference](docs/cli-reference.md)** - Complete command-line tool documentation
- **[Supported AI Agents](docs/supported-agents.md)** - Compatibility table for 20+ agents
- **[Video Resources](docs/video-resources.md)** - Tutorial videos and demos

### 🔄 Workflows

Step-by-step processes for common tasks:

- **[Basic Workflow](workflows/basic-workflow.md)** - Complete feature development cycle (start here!)
- **[Upgrade Project](workflows/upgrade-project.md)** - How to upgrade existing Spec-Kit projects
- **[Local Development](workflows/local-development.md)** - Contributing to Spec-Kit itself

### ⚡ Commands

Detailed guides for each slash command:

**Core Commands:**
- **[/speckit.constitution](commands/speckit-constitution.md)** - Define project principles
- **[/speckit.specify](commands/speckit-specify.md)** - Create feature specifications
- **[/speckit.clarify](commands/speckit-clarify.md)** - Clarify requirements interactively
- **[/speckit.plan](commands/speckit-plan.md)** - Generate implementation plans
- **[/speckit.tasks](commands/speckit-tasks.md)** - Break down into actionable tasks
- **[/speckit.implement](commands/speckit-implement.md)** - Execute the implementation

**Quality Commands:**
- **[/speckit.analyze](commands/speckit-analyze.md)** - Verify specification consistency
- **[/speckit.checklist](commands/speckit-checklist.md)** - Generate quality checklists

### 📦 Artifacts

Real examples of generated documents:

**Tutorial Examples (Taskify):**
- **[Constitution Example](artifacts/constitution-example.md)** - Taskify project constitution
- **[Specification Example](artifacts/spec-example-taskify.md)** - Complete feature spec
- **[Plan Example](artifacts/plan-example-taskify.md)** - Implementation plan structure
- **[Tasks Example](artifacts/tasks-example-taskify.md)** - Task breakdown structure

**Real-World Examples from Production Projects:**
- **[Artifact Examples Index](artifacts/README.md)** - Complete navigation for 11 production examples
- **[IdeaVim API Layer](artifacts/spec-example-ideavim-api.md)** - Complex feature with research.md (IDE plugin)
- **[Backpack Nx Migration](artifacts/spec-example-backpack-nx.md)** - Monorepo feature (design system)
- **[Constitution Examples](artifacts/README.md#constitution-examples)** - 3 real constitutions showing version evolution

### ❓ FAQ

Common questions and troubleshooting:

- **[Troubleshooting](faq/troubleshooting.md)** - Installation, upgrade, and usage issues
- **[Git & Branches](faq/git-and-branches.md)** - Feature branches and version control
- **[Customization](faq/customization.md)** - Customize constitution, templates, and workflows
- **[Constitution Evolution](faq/constitution-evolution.md)** - Version management and MAJOR/MINOR/PATCH bumps
- **[Artifacts FAQ](faq/artifacts.md)** - Questions about specs, plans, tasks, and research.md

### 💡 Tricks & Tips

Advanced techniques and best practices:

- **[Template Constraints](tricks/template-constraints.md)** - How templates guide AI behavior
- **[Constitutional Gates](tricks/constitutional-gates.md)** - Pre-implementation quality checks
- **[Avoiding Over-Engineering](tricks/avoiding-over-engineering.md)** - Simplicity in practice

---

## Quick Start

### Installation

```bash
# Install the Spec-Kit CLI using uv
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git

# Verify installation
specify check
```

### Initialize a New Project

```bash
# Create a new project with GitHub Copilot
specify init my-project --ai copilot

# Or initialize in an existing directory
cd my-existing-project
specify init --here --ai claude
```

### Your First Feature

1. **Define principles:** `/speckit.constitution Create principles for code quality and testing`
2. **Specify feature:** `/speckit.specify Build a todo list with add, complete, and delete functionality`
3. **Clarify requirements:** `/speckit.clarify`
4. **Plan implementation:** `/speckit.plan`
5. **Generate tasks:** `/speckit.tasks`
6. **Implement:** `/speckit.implement`

See the [Basic Workflow](workflows/basic-workflow.md) for detailed steps.

---

## Repository Structure

- **`raw_data/`** - Unprocessed research data (not for general use - see [AGENTS.md](AGENTS.md))
- **`docs/`** - Core documentation and theory
- **`workflows/`** - Proven step-by-step processes
- **`commands/`** - Slash command reference library
- **`artifacts/`** - Example documents (specs, plans, tasks)
- **`faq/`** - Troubleshooting and common questions
- **`tricks/`** - Advanced tips and best practices
- **`media/`** - Images and visual resources

---

## Contributing

This is a community research project. To contribute:

1. **Research sessions** go in `raw_data/YYYY-MM-DD-topic-name/`
2. **Update the index** in `raw_data/README.md` after adding research
3. **Follow the structure** defined in [AGENTS.md](AGENTS.md)
4. **Use kebab-case** for all filenames and directories

For detailed guidelines, see [AGENTS.md](AGENTS.md).

---

## About Spec-Kit

Spec-Kit is a framework developed by GitHub that brings **Spec-Driven Development (SDD)** to AI-assisted coding. It helps you:

- Define **what** you want before diving into **how** to build it
- Generate structured specifications, plans, and tasks
- Enforce architectural principles through a project "constitution"
- Maintain consistency across AI-assisted development sessions

**Learn more:**
- [Official Spec-Kit Repository](https://github.com/github/spec-kit)
- [Spec-Driven Development Philosophy](docs/spec-driven-development.md)
- [Video Introduction](docs/video-resources.md)

---

## License

This guide follows the same license as the official Spec-Kit repository. See LICENSE for details.
