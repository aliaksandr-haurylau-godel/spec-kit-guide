---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/README.md (lines 167-276)
---

# CLI Reference

The `specify` command-line interface provides tools for initializing and managing Spec-Kit projects.

## Commands

| Command | Description |
|---------|-------------|
| `init` | Initialize a new Specify project from the latest template |
| `check` | Check for installed tools (`git`, `claude`, `gemini`, `code`/`code-insiders`, `cursor-agent`, `windsurf`, `qwen`, `opencode`, `codex`, `shai`, `qoder`, and more) |

## specify init

Initialize a new Spec-Kit project or add Spec-Kit to an existing project.

### Arguments

| Argument | Type | Description |
|----------|------|-------------|
| `<project-name>` | Positional | Name for your new project directory (optional if using `--here` or `.` for current directory) |

### Options

| Option | Type | Description |
|--------|------|-------------|
| `--ai` | Choice | AI assistant to use: `claude`, `gemini`, `copilot`, `cursor-agent`, `qwen`, `opencode`, `codex`, `windsurf`, `kilocode`, `auggie`, `roo`, `codebuddy`, `amp`, `shai`, `q`, `agy`, `bob`, or `qoder` |
| `--script` | Choice | Script variant to use: `sh` (bash/zsh) or `ps` (PowerShell) |
| `--ignore-agent-tools` | Flag | Skip checks for AI agent tools like Claude Code |
| `--no-git` | Flag | Skip git repository initialization |
| `--here` | Flag | Initialize project in the current directory instead of creating a new one |
| `--force` | Flag | Force merge/overwrite when initializing in current directory (skip confirmation) |
| `--skip-tls` | Flag | Skip SSL/TLS verification (not recommended) |
| `--debug` | Flag | Enable detailed debug output for troubleshooting |
| `--github-token` | String | GitHub token for API requests (or set `GH_TOKEN` or `GITHUB_TOKEN` env variable) |

### Examples

**Basic project initialization:**

```bash
specify init my-project
```

**Initialize with specific AI assistant:**

```bash
specify init my-project --ai claude
specify init my-project --ai gemini
specify init my-project --ai copilot
```

**Initialize with IDE-based agents:**

```bash
specify init my-project --ai cursor-agent
specify init my-project --ai qoder
specify init my-project --ai windsurf
specify init my-project --ai amp
specify init my-project --ai opencode
```

**Initialize with CLI-based agents:**

```bash
specify init my-project --ai shai
specify init my-project --ai bob
specify init my-project --ai auggie
specify init my-project --ai roo
```

**Initialize with PowerShell scripts (Windows/cross-platform):**

```bash
specify init my-project --ai copilot --script ps
```

**Initialize in current directory:**

```bash
specify init . --ai copilot
# or use the --here flag
specify init --here --ai copilot
```

**Force merge into current (non-empty) directory without confirmation:**

```bash
specify init . --force --ai copilot
# or
specify init --here --force --ai copilot
```

**Skip git initialization:**

```bash
specify init my-project --ai gemini --no-git
```

**Enable debug output for troubleshooting:**

```bash
specify init my-project --ai claude --debug
```

**Use GitHub token for API requests (helpful for corporate environments):**

```bash
specify init my-project --ai claude --github-token ghp_your_token_here
```

---

## specify check

Check system requirements and installed tools.

### Usage

```bash
specify check
```

### What It Checks

- Git installation
- AI agent tools:
  - `claude` (Claude Code)
  - `gemini` (Gemini CLI)
  - `code` / `code-insiders` (VS Code with Copilot)
  - `cursor-agent` (Cursor)
  - `windsurf` (Windsurf)
  - `qwen` (Qwen Code)
  - `opencode` (OpenCode)
  - `codex` (Codex CLI)
  - `shai` (SHAI by OVHcloud)
  - `qoder` (Qoder CLI)
  - `agy` (Antigravity)

---

## Slash Commands

After running `specify init`, your AI coding agent will have access to these slash commands for structured development. See the [Commands](../commands/) directory for detailed documentation on each.

### Core Commands

Essential commands for the Spec-Driven Development workflow:

| Command | Description |
|---------|-------------|
| `/speckit.constitution` | Create or update project governing principles and development guidelines |
| `/speckit.specify` | Define what you want to build (requirements and user stories) |
| `/speckit.plan` | Create technical implementation plans with your chosen tech stack |
| `/speckit.tasks` | Generate actionable task lists for implementation |
| `/speckit.implement` | Execute all tasks to build the feature according to the plan |

### Optional Commands

Additional commands for enhanced quality and validation:

| Command | Description |
|---------|-------------|
| `/speckit.clarify` | Clarify underspecified areas (recommended before `/speckit.plan`; formerly `/quizme`) |
| `/speckit.analyze` | Cross-artifact consistency & coverage analysis (run after `/speckit.tasks`, before `/speckit.implement`) |
| `/speckit.checklist` | Generate custom quality checklists that validate requirements completeness, clarity, and consistency (like "unit tests for English") |

---

## Environment Variables

### SPECIFY_FEATURE

Override feature detection for non-Git repositories.

**Usage:** Set to the feature directory name (e.g., `001-photo-albums`) to work on a specific feature when not using Git branches.

**Important:** Must be set in the context of the agent you're working with prior to using `/speckit.plan` or follow-up commands.

**Example:**

```bash
export SPECIFY_FEATURE=001-photo-albums
```

### GH_TOKEN / GITHUB_TOKEN

GitHub token for API requests. Useful in corporate environments or when rate limits are encountered.

**Example:**

```bash
export GH_TOKEN=ghp_your_token_here
# or
export GITHUB_TOKEN=ghp_your_token_here
```

---

## Installation Modes

### Persistent Installation

Install once, use everywhere:

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

**Benefits:**
- Available in PATH system-wide
- Managed with `uv tool list`, `uv tool upgrade`, `uv tool uninstall`
- No shell aliases needed

### One-Time Usage

Run without installing:

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>
```

**Benefits:**
- Always uses latest version
- No local installation
- Good for CI/CD pipelines

---

## Upgrading

To upgrade an existing Spec-Kit installation:

```bash
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

See the [Upgrade Guide](../workflows/upgrade-project.md) for detailed instructions on upgrading both the CLI and project files.

---

## See Also

- [Installation Guide](installation.md) - Getting started
- [Supported AI Agents](supported-agents.md) - Full compatibility table
- [Commands Directory](../commands/) - Detailed slash command documentation
- [Basic Workflow](../workflows/basic-workflow.md) - Using the slash commands
