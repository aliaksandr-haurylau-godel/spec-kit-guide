---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/docs/installation.md
---

# Installation Guide

Get started with Spec-Kit in minutes. This guide covers prerequisites, installation methods, and verification steps.

## Prerequisites

Before installing Spec-Kit, ensure you have:

- **Operating System**: Linux, macOS, or Windows (PowerShell scripts supported without WSL)
- **AI Coding Agent**: One of the [supported agents](supported-agents.md)
  - [Claude Code](https://www.anthropic.com/claude-code)
  - [GitHub Copilot](https://code.visualstudio.com/)
  - [Cursor](https://cursor.sh/)
  - [Gemini CLI](https://github.com/google-gemini/gemini-cli)
  - [And 16 more...](supported-agents.md)
- **Package Manager**: [uv](https://docs.astral.sh/uv/) for Python package management
- **Python**: [Python 3.11+](https://www.python.org/downloads/)
- **Version Control**: [Git](https://git-scm.com/downloads)

## Installation Methods

### Option 1: Persistent Installation (Recommended)

Install once and use the `specify` command everywhere:

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

**Benefits:**
- Tool stays installed and available in PATH
- No need to create shell aliases
- Better tool management with `uv tool list`, `uv tool upgrade`, `uv tool uninstall`
- Cleaner shell configuration

**Usage after installation:**

```bash
# Create new project
specify init my-project

# Or initialize in existing project
specify init . --ai claude
# or
specify init --here --ai claude

# Check installed tools
specify check
```

**To upgrade later:**

```bash
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

See the [Upgrade Guide](../workflows/upgrade-project.md) for detailed upgrade instructions.

---

### Option 2: One-Time Usage

Run directly without installing:

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init my-project
```

This fetches and runs the latest version each time. No local installation needed.

## Configuration Options

### Specify Your AI Agent

Proactively specify your AI agent during initialization:

```bash
specify init my-project --ai claude
specify init my-project --ai gemini
specify init my-project --ai copilot
specify init my-project --ai cursor-agent
specify init my-project --ai opencode
```

See [Supported AI Agents](supported-agents.md) for the complete list.

---

### Choose Script Type (Shell vs PowerShell)

All automation scripts have both Bash (`.sh`) and PowerShell (`.ps1`) variants.

**Auto behavior:**
- Windows default: `ps`
- Other OS default: `sh`
- Interactive mode: you'll be prompted unless you pass `--script`

**Force a specific script type:**

```bash
specify init my-project --script sh   # Force Bash
specify init my-project --script ps   # Force PowerShell
```

---

### Initialize in Current Directory

Instead of creating a new directory:

```bash
specify init .
# or use the --here flag
specify init --here --ai copilot
```

**Force merge without confirmation:**

```bash
specify init . --force --ai copilot
# or
specify init --here --force --ai copilot
```

---

### Skip Agent Tools Check

If you prefer to get the templates without checking for required tools:

```bash
specify init my-project --ai claude --ignore-agent-tools
```

---

### Skip Git Initialization

For projects that don't need Git:

```bash
specify init my-project --ai gemini --no-git
```

---

### Debug Mode

Enable detailed debug output for troubleshooting:

```bash
specify init my-project --ai claude --debug
```

---

### GitHub Token (Corporate Environments)

Provide a GitHub token for API requests:

```bash
specify init my-project --ai claude --github-token ghp_your_token_here
```

Or set the environment variable:

```bash
export GH_TOKEN=ghp_your_token_here
# or
export GITHUB_TOKEN=ghp_your_token_here
```

## Verification

After initialization, verify the installation:

```bash
specify check
```

This command checks for:
- Git installation
- AI agent tools (claude, gemini, code/code-insiders, cursor-agent, windsurf, etc.)
- Python environment

**Expected output structure:**

```text
.
├── .specify/
│   ├── memory/
│   │   └── constitution.md
│   ├── scripts/
│   │   ├── *.sh (Bash variants)
│   │   └── *.ps1 (PowerShell variants)
│   └── templates/
│       ├── spec-template.md
│       ├── plan-template.md
│       └── tasks-template.md
├── .claude/commands/ (or equivalent for your agent)
└── specs/ (created after first /speckit.specify)
```

**Available slash commands in your AI agent:**

- `/speckit.constitution` - Create project governing principles
- `/speckit.specify` - Create specifications
- `/speckit.clarify` - Clarify underspecified areas
- `/speckit.plan` - Generate implementation plans
- `/speckit.tasks` - Break down into actionable tasks
- `/speckit.implement` - Execute all tasks
- `/speckit.analyze` - Cross-artifact consistency analysis
- `/speckit.checklist` - Generate custom quality checklists

## Troubleshooting

### Git Credential Manager on Linux

If you're having issues with Git authentication on Linux, install Git Credential Manager:

```bash
#!/usr/bin/env bash
set -e
echo "Downloading Git Credential Manager v2.6.1..."
wget https://github.com/git-ecosystem/git-credential-manager/releases/download/v2.6.1/gcm-linux_amd64.2.6.1.deb
echo "Installing Git Credential Manager..."
sudo dpkg -i gcm-linux_amd64.2.6.1.deb
echo "Configuring Git to use GCM..."
git config --global credential.helper manager
echo "Cleaning up..."
rm gcm-linux_amd64.2.6.1.deb
```

### SSL/TLS Issues

If you encounter SSL/TLS verification issues (not recommended for production):

```bash
specify init my-project --skip-tls
```

### AI Agent Not Detected

If `specify check` doesn't detect your AI agent:

1. Ensure the agent is installed and in your PATH
2. Try using `--ignore-agent-tools` to skip the check
3. Manually verify your agent supports custom slash commands

## Next Steps

After installation:

1. **Define Your Constitution** - Use `/speckit.constitution` to establish project principles
2. **Create Your First Spec** - Use `/speckit.specify` to describe what you want to build
3. **Follow the Basic Workflow** - See [Basic Workflow](../workflows/basic-workflow.md) for step-by-step guidance

## See Also

- [Basic Workflow](../workflows/basic-workflow.md) - Complete 6-step process
- [Supported AI Agents](supported-agents.md) - Full compatibility table
- [CLI Reference](cli-reference.md) - All commands and options
- [Upgrade Guide](../workflows/upgrade-project.md) - Upgrading existing projects
