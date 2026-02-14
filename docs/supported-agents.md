---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/README.md (lines 143-165)
---

# Supported AI Agents

Spec-Kit supports a wide range of AI coding agents through custom slash commands. Below is the complete compatibility table.

## Compatibility Table

| Agent | Support | Notes |
|-------|---------|-------|
| [Qoder CLI](https://qoder.com/cli) | ✅ Full | |
| [Amazon Q Developer CLI](https://aws.amazon.com/developer/learning/q-developer-cli/) | ⚠️ Limited | Amazon Q Developer CLI [does not support](https://github.com/aws/amazon-q-developer-cli/issues/3064) custom arguments for slash commands |
| [Amp](https://ampcode.com/) | ✅ Full | |
| [Auggie CLI](https://docs.augmentcode.com/cli/overview) | ✅ Full | |
| [Claude Code](https://www.anthropic.com/claude-code) | ✅ Full | |
| [CodeBuddy CLI](https://www.codebuddy.ai/cli) | ✅ Full | |
| [Codex CLI](https://github.com/openai/codex) | ✅ Full | |
| [Cursor](https://cursor.sh/) | ✅ Full | |
| [Gemini CLI](https://github.com/google-gemini/gemini-cli) | ✅ Full | |
| [GitHub Copilot](https://code.visualstudio.com/) | ✅ Full | |
| [IBM Bob](https://www.ibm.com/products/bob) | ✅ Full | IDE-based agent with slash command support |
| [Jules](https://jules.google.com/) | ✅ Full | |
| [Kilo Code](https://github.com/Kilo-Org/kilocode) | ✅ Full | |
| [opencode](https://opencode.ai/) | ✅ Full | |
| [Qwen Code](https://github.com/QwenLM/qwen-code) | ✅ Full | |
| [Roo Code](https://roocode.com/) | ✅ Full | |
| [SHAI (OVHcloud)](https://github.com/ovh/shai) | ✅ Full | |
| [Windsurf](https://windsurf.com/) | ✅ Full | |
| [Antigravity (agy)](https://agy.ai/) | ✅ Full | |

## Agent Categories

### CLI-Based Agents

These agents run in your terminal and support custom commands through configuration files:

- **Claude Code** - Anthropic's CLI agent
- **Gemini CLI** - Google's CLI agent
- **Qoder CLI** - Qoder's terminal agent
- **CodeBuddy CLI** - CodeBuddy's command-line interface
- **Auggie CLI** - Augment Code's CLI tool
- **Roo Code** - Roo's CLI agent
- **Codex CLI** - OpenAI's Codex CLI
- **Qwen Code** - Qwen's coding CLI
- **SHAI** - OVHcloud's CLI agent
- **Amazon Q Developer CLI** - AWS's Q Developer CLI (limited support)
- **Antigravity (agy)** - Antigravity's CLI

### IDE-Based Agents

These agents integrate with code editors and support slash commands through their IDE extensions:

- **GitHub Copilot** - VS Code extension
- **Cursor** - Standalone editor with AI
- **opencode** - OpenCode IDE integration
- **Windsurf** - Windsurf IDE agent
- **Amp** - Amp editor integration
- **Kilo Code** - Kilo IDE agent
- **IBM Bob** - IBM's IDE-based agent
- **Jules** - Google's IDE agent

## Using Spec-Kit with Your Agent

### Installation

When initializing a project, specify your agent with the `--ai` flag:

```bash
specify init my-project --ai <agent-name>
```

**Agent names:**
- `claude` - Claude Code
- `gemini` - Gemini CLI
- `copilot` - GitHub Copilot
- `cursor-agent` - Cursor
- `qoder` - Qoder CLI
- `opencode` - opencode
- `windsurf` - Windsurf
- `kilocode` - Kilo Code
- `auggie` - Auggie CLI
- `roo` - Roo Code
- `codebuddy` - CodeBuddy CLI
- `amp` - Amp
- `shai` - SHAI
- `q` - Amazon Q Developer CLI
- `agy` - Antigravity
- `bob` - IBM Bob
- `qwen` - Qwen Code
- `codex` - Codex CLI

### Available Commands

All supported agents (except Amazon Q Developer CLI with limited support) provide these slash commands:

**Core Commands:**
- `/speckit.constitution` - Create project principles
- `/speckit.specify` - Create specifications
- `/speckit.plan` - Generate implementation plans
- `/speckit.tasks` - Generate task lists
- `/speckit.implement` - Execute implementation

**Optional Commands:**
- `/speckit.clarify` - Clarify requirements
- `/speckit.analyze` - Analyze consistency
- `/speckit.checklist` - Generate quality checklists

See the [Commands](../commands/) directory for detailed documentation.

## Agent-Specific Notes

### Amazon Q Developer CLI

**Limitation:** Does not support custom arguments for slash commands ([issue #3064](https://github.com/aws/amazon-q-developer-cli/issues/3064)).

**Impact:** You can use Spec-Kit commands, but cannot pass inline arguments. You'll need to provide details in follow-up messages.

**Workaround:**
```bash
# Instead of:
/speckit.specify Build a todo app with user authentication

# Use:
/speckit.specify
# Then provide details when prompted
```

### IDE-Based Agents (Cursor, Kilo Code, Windsurf)

**Note:** After upgrading Spec-Kit, you may see duplicate slash commands (old and new versions).

**Solution:** Manually delete old command files from your agent's commands folder.

**Example for Kilo Code:**
```bash
cd .kilocode/rules/
ls -la  # Identify duplicates
rm old-command-files.md
```

See the [Upgrade Guide](../workflows/upgrade-project.md#3-duplicate-slash-commands-ide-based-agents) for details.

## Checking Agent Installation

Use the `specify check` command to verify your agent is installed:

```bash
specify check
```

This checks for:
- Git
- Claude (`claude`)
- Gemini (`gemini`)
- VS Code (`code` / `code-insiders`)
- Cursor (`cursor-agent`)
- Windsurf (`windsurf`)
- And all other supported agents

## Adding a New Agent

If your AI agent supports custom slash commands but isn't listed here:

1. Check if it follows the MCP (Model Context Protocol) or similar standards
2. Ensure it can read custom command files from a directory (e.g., `.claude/commands/`, `.github/prompts/`)
3. Open an issue in the [Spec-Kit repository](https://github.com/github/spec-kit/issues) to request support

## See Also

- [Installation Guide](installation.md) - Getting started with your agent
- [CLI Reference](cli-reference.md) - Command options including `--ai`
- [Commands Directory](../commands/) - Slash command documentation
- [Basic Workflow](../workflows/basic-workflow.md) - Using Spec-Kit with any agent
