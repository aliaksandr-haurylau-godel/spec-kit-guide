---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/README.md
---

# Git & Branches FAQ

Questions about how Spec-Kit manages Git repositories, feature branches, and version control workflows.

## Feature Branch Management

### Q: Does Spec-Kit automatically create feature branches?

**Answer:** Yes, by default. When you run `/speckit.specify`, Spec-Kit automatically:

1. Creates a new feature branch with a sequential name (e.g., `001-create-taskify`, `002-add-authentication`)
2. Checks out that branch
3. Creates a corresponding `specs/` directory with the same number prefix

**Example workflow:**

```bash
# Before running /speckit.specify
git branch
* main

# After running /speckit.specify with prompt "Create Taskify"
git branch
  main
* 001-create-taskify
```

The feature directory will be: `specs/001-create-taskify/`

### Q: How does Spec-Kit determine the feature number?

**Answer:** Spec-Kit scans existing branches and `specs/` directories to find the highest numbered feature, then increments by 1.

For example:
- If you have `001-auth`, `002-dashboard`, next will be `003-*`
- If you have branches but no `specs/`, it checks both
- First feature is always `001-*`

### Q: Can I work on a feature without Git?

**Answer:** Yes, use the `--no-git` flag during initialization:

```bash
specify init my-project --ai copilot --no-git
```

When `--no-git` is used:
- No feature branches are created
- No automatic git commits
- You must manually manage the `specs/` directory structure
- Use the `SPECIFY_FEATURE` environment variable to tell Spec-Kit which feature you're working on

### Q: How do I work on a feature without Git branches?

**Answer:** Set the `SPECIFY_FEATURE` environment variable to the feature directory name:

```bash
# Bash/Linux/macOS
export SPECIFY_FEATURE=001-photo-albums

# PowerShell (Windows)
$env:SPECIFY_FEATURE = "001-photo-albums"
```

**Important:** This variable must be set in the context of your AI agent **before** running `/speckit.plan` or follow-up commands.

**When to use this:**

- Non-Git repositories
- Monorepos where feature branches aren't practical
- Testing/experimentation without creating branches
- Working in environments where you can't create branches

### Q: What if I want to use custom branch names?

**Answer:** Spec-Kit automatically generates branch names based on your specification title, but you can:

1. **Let Spec-Kit create the branch**, then rename it:

   ```bash
   git branch -m 001-create-taskify 001-taskify-mvp
   ```

2. **Create your own branch first**, then use `SPECIFY_FEATURE`:

   ```bash
   git checkout -b feature/my-custom-name
   export SPECIFY_FEATURE=001-my-feature
   # Then run /speckit.specify
   ```

### Q: Can I work on multiple features simultaneously?

**Answer:** Yes, using Git branches:

```bash
# Start feature 1
# Run /speckit.specify "Feature 1 description"
# This creates branch 001-feature-1 and specs/001-feature-1/

# Switch to main and start feature 2
git checkout main
# Run /speckit.specify "Feature 2 description"
# This creates branch 002-feature-2 and specs/002-feature-2/

# Switch between features
git checkout 001-feature-1  # Work on feature 1
git checkout 002-feature-2  # Work on feature 2
```

Each branch maintains its own:
- Specification files (`specs/XXX-*/spec.md`)
- Plan files (`specs/XXX-*/plan.md`)
- Task files (`specs/XXX-*/tasks.md`)
- Implementation code

## Branch Lifecycle

### Q: What happens to the feature branch after implementation?

**Answer:** After running `/speckit.implement` and completing the feature:

1. **Review your changes:**

   ```bash
   git status
   git diff main
   ```

2. **Commit your work:**

   ```bash
   git add .
   git commit -m "Implement feature: Create Taskify"
   ```

3. **Merge back to main:**

   ```bash
   git checkout main
   git merge 001-create-taskify
   ```

4. **Optionally delete the feature branch:**

   ```bash
   git branch -d 001-create-taskify
   ```

**Note:** The `specs/001-create-taskify/` directory remains in your repository as documentation.

### Q: Should I keep or delete the specs/ directory after merging?

**Answer:** **Keep it.** The `specs/` directories serve as:

- Historical documentation of feature requirements
- Reference for future modifications
- Audit trail for project decisions
- Context for onboarding new team members

**Best practice:**
- Commit `specs/` directories to your repository
- Include them in your main branch
- Update them if the feature evolves significantly

## Repository Structure

### Q: What's the relationship between branches and specs/ directories?

**Answer:**

```text
Repository Structure:

main branch
├── .specify/
│   ├── memory/constitution.md
│   ├── scripts/
│   └── templates/
├── specs/
│   ├── 001-create-taskify/     ← Created by first /speckit.specify
│   │   ├── spec.md
│   │   ├── plan.md
│   │   └── tasks.md
│   └── 002-add-auth/           ← Created by second /speckit.specify
│       ├── spec.md
│       ├── plan.md
│       └── tasks.md
└── src/                        ← Your actual code

Branch structure:
- main
- 001-create-taskify            ← Feature branch for first feature
- 002-add-auth                  ← Feature branch for second feature
```

**Key points:**
- Branch names match `specs/` directory names
- All `specs/` directories live in the main branch after merging
- Code implementations may live in `src/`, `app/`, or your project structure

### Q: Can I reorganize the specs/ directory structure?

**Answer:** It's not recommended. Spec-Kit expects:

```text
specs/
└── NNN-feature-name/
    ├── spec.md
    ├── plan.md
    └── tasks.md
```

Where `NNN` is a zero-padded 3-digit number (e.g., `001`, `002`, `015`).

**If you must reorganize:**
- Keep the numbering scheme consistent
- Update any scripts in `.specify/scripts/` that reference the structure
- Use `SPECIFY_FEATURE` to manually specify the directory
- Document your custom structure for your team

## Working with Existing Repositories

### Q: How do I add Spec-Kit to an existing repository?

**Answer:**

```bash
# Navigate to your existing repo
cd my-existing-project

# Initialize Spec-Kit in the current directory
specify init --here --ai copilot

# The CLI will warn you about existing files and merge Spec-Kit files
```

See [Troubleshooting: "Warning: Current directory is not empty"](troubleshooting.md#q-warning-current-directory-is-not-empty) for details.

### Q: What if my repo already has a specs/ directory?

**Answer:** Spec-Kit will:
- Not overwrite your existing `specs/` directory
- Create new feature directories alongside existing ones
- Use the next available number (e.g., if you have `specs/research/`, next feature will be `specs/001-feature-name/`)

**Recommendation:** If you have an existing `specs/` structure, consider:
1. Renaming it (e.g., `specs/` → `old-specs/` or `docs/specs/`)
2. Migrating important specs into the Spec-Kit format
3. Keeping both structures if they serve different purposes

### Q: Can I use Spec-Kit in a monorepo?

**Answer:** Yes, but with considerations:

**Option 1: One Spec-Kit per service/package**

```text
monorepo/
├── services/
│   ├── api/
│   │   ├── .specify/
│   │   └── specs/
│   └── frontend/
│       ├── .specify/
│       └── specs/
└── packages/
    └── shared/
        ├── .specify/
        └── specs/
```

**Option 2: Shared Spec-Kit at root**

```text
monorepo/
├── .specify/          ← Shared constitution & templates
├── specs/
│   ├── 001-api-auth/
│   ├── 002-frontend-dashboard/
│   └── 003-shared-utils/
├── services/
│   ├── api/
│   └── frontend/
└── packages/
    └── shared/
```

**Recommendation:** Use separate Spec-Kit instances per service for isolation, or use `SPECIFY_FEATURE` with a shared root installation.

## Git Workflow Tips

### Q: Should I commit the .specify/ directory?

**Answer:** **Yes**, commit everything in `.specify/` except:

```gitignore
# .gitignore
.specify/memory/tmp/
.specify/cache/
```

**What to commit:**
- `.specify/memory/constitution.md` - Project principles
- `.specify/scripts/` - Automation scripts
- `.specify/templates/` - Spec/plan/task templates
- `specs/` - All feature specifications, plans, and tasks

### Q: What's the recommended git workflow with Spec-Kit?

**Answer:**

```bash
# 1. Start on main with latest code
git checkout main
git pull

# 2. Create specification (creates feature branch automatically)
# Run: /speckit.specify "My new feature description"

# 3. Clarify and plan
# Run: /speckit.clarify
# Run: /speckit.plan

# 4. Review and commit the planning artifacts
git add specs/NNN-feature-name/
git commit -m "Add specification and plan for feature: My new feature"

# 5. Implement
# Run: /speckit.implement

# 6. Commit implementation
git add .
git commit -m "Implement feature: My new feature"

# 7. Merge back to main
git checkout main
git merge NNN-feature-name

# 8. Push to remote
git push origin main

# 9. Optionally delete feature branch
git branch -d NNN-feature-name
```

---

**See Also:**
- [Basic Workflow](../workflows/basic-workflow.md) - Complete feature development process
- [Troubleshooting](troubleshooting.md) - Common Git-related issues
- [Installation Guide](../docs/installation.md) - Setting up Spec-Kit with Git options
