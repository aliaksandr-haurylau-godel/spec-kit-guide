---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/README.md, spec-driven.md
---

# Customization FAQ

Questions about customizing Spec-Kit's constitution, templates, and workflows for your project.

## Constitution Customization

### Q: What is the constitution and why would I customize it?

**Answer:** The constitution (`.specify/memory/constitution.md`) contains your project's governing principles and development guidelines. It influences how the AI agent makes decisions during specification, planning, and implementation.

**Why customize:**
- Enforce specific coding standards
- Define architectural patterns
- Set testing requirements
- Establish UI/UX guidelines
- Configure performance thresholds
- Define security policies

**Example use cases:**
- "Always use TypeScript strict mode"
- "Prefer composition over inheritance"
- "All API endpoints must have integration tests"
- "Follow Material Design principles"
- "Never use global state"

### Q: How do I create a custom constitution?

**Answer:** Use the `/speckit.constitution` command with specific guidelines:

```text
/speckit.constitution Create principles focused on:
- Code Quality: TypeScript strict mode, ESLint rules, maximum cyclomatic complexity of 10
- Testing Standards: 80% code coverage minimum, integration tests for all API endpoints, E2E tests for critical paths
- User Experience: Follow WCAG 2.1 AA accessibility standards, mobile-first responsive design, max 3-second page load
- Performance: API responses under 200ms, optimize images, lazy load components
- Security: No hardcoded secrets, input validation on all forms, HTTPS only
```

The AI will generate a structured constitution document with these principles organized into articles.

### Q: What are the standard constitution articles?

**Answer:** The Spec-Kit constitution typically includes 9 articles (see [Constitution Reference](../docs/constitution-reference.md) for details):

1. **Library-First** - Use existing tools before building custom
2. **Minimize Dependencies** - Prefer fewer, well-maintained dependencies
3. **Test-First** - Mandate test-first workflows
4. **Refactor-Later** - Limit premature optimization
5. **Build-for-One** - Solve concrete problems, not imagined use cases
6. **Keep-Options-Open** - Defer irreversible decisions
7. **Leverage-Standards** - Prefer standards over custom solutions
8. **Vertical-Slicing** - Deliver end-to-end slices over horizontal layers
9. **Single-Focus** - One feature branch, one deliverable

You can modify these or add your own articles.

### Q: Can I add custom articles to the constitution?

**Answer:** Yes. Edit `.specify/memory/constitution.md` directly:

```markdown
## Article X: Custom Principle Name

**Principle:** [Your principle statement]

**Rationale:** [Why this matters for your project]

**Application:**
- [Guideline 1]
- [Guideline 2]
- [Guideline 3]

**Example:**
```typescript
// Example code showing the principle in action
```

**Examples:**
- **Article X: Offline-First** - All features must work offline
- **Article XI: Privacy-By-Design** - Minimize data collection, encrypt at rest
- **Article XII: API-First** - Public APIs before internal implementations

### Q: How do I update an existing constitution?

**Answer:** Two approaches:

**Option 1: Use `/speckit.constitution` again**

```text
/speckit.constitution Update the Testing Standards article to require 90% code coverage and add mutation testing requirements
```

The AI will read the existing constitution and update it.

**Option 2: Edit directly**

```bash
# Open in your editor
code .specify/memory/constitution.md

# Make changes, save, commit
git add .specify/memory/constitution.md
git commit -m "Update testing requirements to 90% coverage"
```

### Q: When should I customize the constitution?

**Answer:** Customize the constitution:

- **At project start** - Before running `/speckit.specify` for the first time
- **Before major features** - If new requirements need codification
- **After retrospectives** - When team identifies new principles
- **When onboarding** - To document existing but unwritten standards

**Don't customize** every time you write a feature. The constitution should be relatively stable.

## Template Customization

### Q: What templates can I customize?

**Answer:** Spec-Kit uses three main templates in `.specify/templates/`:

1. **`spec-template.md`** - Structure for specifications
2. **`plan-template.md`** - Structure for implementation plans
3. **`tasks-template.md`** - Structure for task lists

### Q: How do I customize templates?

**Answer:** Edit the template files directly:

```bash
# Edit the spec template
code .specify/templates/spec-template.md

# Example: Add a new section
## Security Considerations

Describe security implications:
- Authentication/Authorization requirements
- Data privacy concerns
- Input validation needs
- Threat model
```

**Important:**
- Keep the basic structure intact
- Don't remove essential sections (like User Stories, Requirements)
- Test with a new feature after changing templates
- Commit template changes to version control

### Q: Can I add custom sections to the spec template?

**Answer:** Yes. Common additions:

```markdown
## Accessibility Requirements

WCAG compliance level: [A/AA/AAA]
Screen reader support: [Yes/No]
Keyboard navigation: [Required/Optional]

## Performance Targets

Page load time: [X seconds]
API response time: [X ms]
Bundle size: [X KB]

## Internationalization

Supported locales: [en, es, fr, ...]
RTL support: [Yes/No]
Date/time format: [ISO 8601 / locale-specific]

## Analytics & Monitoring

Events to track:
- [Event 1]
- [Event 2]

Metrics to monitor:
- [Metric 1]
- [Metric 2]
```

### Q: What if I want different templates for different feature types?

**Answer:** Spec-Kit doesn't support multiple template sets out-of-the-box, but you can:

**Workaround 1: Manual template switching**

```bash
# Backup original templates
cp .specify/templates/spec-template.md .specify/templates/spec-template-default.md

# Create variant templates
cp .specify/templates/spec-template-default.md .specify/templates/spec-template-api.md
cp .specify/templates/spec-template-default.md .specify/templates/spec-template-ui.md

# Before starting a feature, swap in the right template
cp .specify/templates/spec-template-api.md .specify/templates/spec-template.md
```

**Workaround 2: Include all sections in one template**

```markdown
## API-Specific Sections (if applicable)

[Only fill if this is an API feature]

## UI-Specific Sections (if applicable)

[Only fill if this is a UI feature]
```

## Backup Strategies

### Q: How do I back up my customizations before upgrading?

**Answer:** Before running `specify init --here --force`:

```bash
# Option 1: Git commit
git add .specify/memory/constitution.md .specify/templates/
git commit -m "Backup constitution and templates before upgrade"

# Option 2: Manual backup
mkdir /tmp/specify-backup
cp -r .specify/memory/ /tmp/specify-backup/
cp -r .specify/templates/ /tmp/specify-backup/

# After upgrade, restore if needed
cp /tmp/specify-backup/memory/constitution.md .specify/memory/
```

**Best practice:** Always commit your customizations before upgrading.

### Q: What gets overwritten during an upgrade?

**Answer:** The `specify init --here --force` command will overwrite:

- ✅ `.specify/scripts/` - Automation scripts (safe to overwrite)
- ✅ `.specify/templates/` - **Templates (YOUR CUSTOMIZATIONS WILL BE LOST)**
- ✅ `.specify/memory/constitution.md` - **Constitution (YOUR CUSTOMIZATIONS WILL BE LOST)**
- ✅ Agent command files (`.claude/`, `.github/prompts/`, etc.)

**Safe from overwrite:**
- ❌ `specs/` - All specifications, plans, tasks
- ❌ Your source code
- ❌ `.git/` directory

**Action required:** Back up `.specify/memory/constitution.md` and `.specify/templates/` before upgrading.

### Q: How do I restore customizations after an upgrade?

**Answer:**

**If you committed before upgrading:**

```bash
# Restore from git
git restore .specify/memory/constitution.md
git restore .specify/templates/spec-template.md
git restore .specify/templates/plan-template.md
git restore .specify/templates/tasks-template.md
```

**If you backed up manually:**

```bash
# Restore from backup directory
cp /tmp/specify-backup/memory/constitution.md .specify/memory/
cp /tmp/specify-backup/templates/* .specify/templates/
```

**If you lost customizations:**

- Review `git log` to find the previous version
- Use `git show <commit>:.specify/memory/constitution.md` to view old versions
- Manually re-apply customizations to the new templates

## Script Customization

### Q: Can I customize the automation scripts?

**Answer:** You *can*, but it's **not recommended**. The scripts in `.specify/scripts/` are:

- `check-prerequisites.sh` - Verifies environment setup
- `common.sh` - Shared utilities
- `create-new-feature.sh` - Creates feature branches and directories
- `setup-plan.sh` - Sets up plan artifacts
- `update-claude-md.sh` - Updates agent command files (Claude-specific)

**Why not customize:**
- Scripts get overwritten during upgrades
- Breaking changes can disrupt your workflow
- Official updates may conflict with your changes

**If you must customize:**
1. Make a backup copy: `cp create-new-feature.sh create-new-feature.custom.sh`
2. Edit the copy
3. Document what you changed and why
4. After upgrades, re-apply your changes manually

### Q: How do I extend Spec-Kit with my own scripts?

**Answer:** Create custom scripts outside `.specify/scripts/`:

```bash
# Create a custom scripts directory
mkdir -p scripts/speckit-extensions/

# Add your custom script
cat > scripts/speckit-extensions/deploy-feature.sh << 'EOF'
#!/usr/bin/env bash
# Custom deployment script for Spec-Kit features
FEATURE_DIR=$1
# ... your logic ...
EOF

chmod +x scripts/speckit-extensions/deploy-feature.sh
```

**Add to `.gitignore` if needed:**

```gitignore
# Don't overwrite custom extensions
!scripts/speckit-extensions/
```

## Agent-Specific Customization

### Q: Can I customize slash commands for my agent?

**Answer:** Yes, edit the command files in your agent's directory:

```bash
# For GitHub Copilot
code .github/prompts/speckit-specify.md

# For Claude Code
code .claude/commands/speckit-specify.md

# For Cursor
code .cursor/commands/speckit-specify.md
```

**Example customization:**

```markdown
<!-- Original -->
Create a new specification for the requested feature following the Spec-Kit workflow.

<!-- Customized -->
Create a new specification for the requested feature. Always include:
- Security considerations section
- Performance requirements section
- Accessibility requirements (WCAG 2.1 AA minimum)
```

**Warning:** These files get overwritten during upgrades. Back them up if you customize.

### Q: Can I add my own slash commands?

**Answer:** Yes! Add new command files to your agent's commands directory:

```bash
# For GitHub Copilot
cat > .github/prompts/my-custom-command.md << 'EOF'
Review the current feature spec and generate a comprehensive test plan.
Include unit tests, integration tests, and E2E test scenarios.
EOF

# For Claude Code
cat > .claude/commands/my-custom-command.md << 'EOF'
# ... same content ...
EOF
```

Then use `/my-custom-command` in your agent.

## Sharing Customizations

### Q: How do I share my constitution and templates with my team?

**Answer:** Commit them to version control:

```bash
# Add customizations
git add .specify/memory/constitution.md
git add .specify/templates/
git commit -m "Add team constitution and custom templates"
git push

# Team members pull
git pull
# They now have your customizations
```

**Best practice:** Document your customizations in your project README:

```markdown
## Spec-Kit Customizations

This project uses a custom Spec-Kit constitution with:
- Strict TypeScript requirements (Article I)
- 90% test coverage mandate (Article III)
- WCAG 2.1 AA accessibility (Article X)

See `.specify/memory/constitution.md` for details.
```

### Q: Can I create a starter template with my customizations?

**Answer:** Yes! Create a "blessed" repository:

```bash
# 1. Set up Spec-Kit with your customizations
specify init my-company-template --ai copilot
cd my-company-template

# 2. Customize constitution and templates
# ... edit .specify/memory/constitution.md ...
# ... edit .specify/templates/* ...

# 3. Commit and push to GitHub/GitLab
git add .
git commit -m "Company-wide Spec-Kit template"
git remote add origin git@github.com:mycompany/speckit-template.git
git push -u origin main

# 4. Team members clone and start new projects
git clone git@github.com:mycompany/speckit-template.git my-new-project
cd my-new-project
# Ready to use /speckit.specify!
```

---

**See Also:**
- [Constitution Reference](../docs/constitution-reference.md) - Detailed explanation of all articles
- [Troubleshooting](troubleshooting.md) - Backup and restore issues
- [Upgrade Guide](../workflows/upgrade-project.md) - How to upgrade without losing customizations
- [Spec-Driven Development](../docs/spec-driven-development.md) - Core principles
