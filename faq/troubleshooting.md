---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/README.md, docs/upgrade.md
---

# Troubleshooting

Common issues and solutions when working with Spec-Kit.

## Installation & Setup Issues

### Q: Slash commands not showing up after installation

**Cause:** Agent didn't load the command files, or they don't exist.

**Fix:**

1. **Restart your IDE/editor completely** (not just reload window)
2. **Verify command files exist:**

   ```bash
   # For GitHub Copilot
   ls -la .github/prompts/

   # For Claude Code
   ls -la .claude/commands/

   # For Gemini
   ls -la .gemini/commands/

   # For Cursor
   ls -la .cursor/commands/
   ```

3. **Check you're in the correct directory** where you ran `specify init`
4. **For some agents**, you may need to reload the workspace or clear cache
5. **Check agent-specific setup:**
   - Codex requires `CODEX_HOME` environment variable
   - Some agents need workspace restart or cache clearing

### Q: CLI installation doesn't seem to work

**Cause:** Installation path issues or tool not found.

**Fix:** Verify the installation:

```bash
# Check installed tools
uv tool list
# Should show specify-cli

# Verify path
which specify
# Should point to the uv tool installation directory
```

If not found, reinstall:

```bash
uv tool uninstall specify-cli
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

### Q: "Warning: Current directory is not empty"

**Full warning message:**

```text
Warning: Current directory is not empty (25 items)
Template files will be merged with existing content and may overwrite existing files
Do you want to continue? [y/N]
```

**What this means:**

This warning appears when you run `specify init --here` (or `specify init .`) in a directory that already has files. It's telling you:

1. **The directory has existing content** - In the example, 25 files/folders
2. **Files will be merged** - New template files will be added alongside your existing files
3. **Some files may be overwritten** - If you already have Spec Kit files (`.claude/`, `.specify/`, etc.), they'll be replaced with the new versions

**What gets overwritten:**

Only Spec Kit infrastructure files:

- Agent command files (`.claude/commands/`, `.github/prompts/`, etc.)
- Scripts in `.specify/scripts/`
- Templates in `.specify/templates/`
- Memory files in `.specify/memory/` (including constitution)

**What stays untouched:**

- Your `specs/` directory (specifications, plans, tasks)
- Your source code files
- Your `.git/` directory and git history
- Any other files not part of Spec Kit templates

**How to respond:**

- **Type `y` and press Enter** - Proceed with the merge (recommended if upgrading)
- **Type `n` and press Enter** - Cancel the operation
- **Use `--force` flag** - Skip this confirmation entirely:

  ```bash
  specify init --here --force --ai copilot
  ```

**When you see this warning:**

- ✅ **Expected** when upgrading an existing Spec Kit project
- ✅ **Expected** when adding Spec Kit to an existing codebase
- ⚠️ **Unexpected** if you thought you were creating a new project in an empty directory

**Prevention tip:** Before upgrading, commit or back up your `.specify/memory/constitution.md` if you customized it.

## Authentication & Credentials

### Q: Git authentication issues on Linux

**Cause:** Missing or misconfigured Git credential manager.

**Fix:** Install Git Credential Manager:

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

## Upgrade Issues

### Q: Slash commands not showing up after upgrade

**Cause:** Agent didn't reload the command files.

**Fix:**

1. **Restart your IDE/editor completely** (not just reload window)
2. **For CLI-based agents**, verify files exist:

   ```bash
   ls -la .claude/commands/      # Claude Code
   ls -la .gemini/commands/       # Gemini
   ls -la .cursor/commands/       # Cursor
   ```

3. **Check agent-specific setup:**
   - Codex requires `CODEX_HOME` environment variable
   - Some agents need workspace restart or cache clearing

### Q: I lost my constitution customizations during upgrade

**Fix:** Restore from git or backup:

```bash
# If you committed before upgrading
git restore .specify/memory/constitution.md

# If you backed up manually
cp /tmp/constitution-backup.md .specify/memory/constitution.md
```

**Prevention:** Always commit or back up `constitution.md` before upgrading.

## Usage & Workflow Issues

### Q: Do I need to run specify every time I open my project?

**Short answer:** No, you only run `specify init` once per project (or when upgrading).

**Explanation:**

The `specify` CLI tool is used for:

- **Initial setup:** `specify init` to bootstrap Spec Kit in your project
- **Upgrades:** `specify init --here --force` to update templates and commands
- **Diagnostics:** `specify check` to verify tool installation

Once you've run `specify init`, the slash commands (like `/speckit.specify`, `/speckit.plan`, etc.) are **permanently installed** in your project's agent folder (`.claude/`, `.github/prompts/`, etc.). Your AI assistant reads these command files directly—no need to run `specify` again.

**If your agent isn't recognizing slash commands:**

1. **Verify command files exist** (see installation troubleshooting above)
2. **Restart your IDE/editor completely** (not just reload window)
3. **Check you're in the correct directory** where you ran `specify init`
4. **For some agents**, you may need to reload the workspace or clear cache

### Q: Copilot can't open local files or uses PowerShell commands unexpectedly

**Cause:** This is typically an IDE context issue, not related to `specify`.

**Fix:**

- Restart VS Code
- Check file permissions
- Ensure the workspace folder is properly opened

## Agent-Specific Issues

### Q: Agent tools check is preventing installation

**Cause:** The CLI checks if your selected agent is installed before proceeding.

**Fix:** Use `--ignore-agent-tools` flag:

```bash
specify init <project_name> --ai claude --ignore-agent-tools
```

This allows you to get the templates without checking for the right tools.

## Debug & Diagnostics

### Q: How do I enable debug output for troubleshooting?

**Solution:** Use the `--debug` flag:

```bash
specify init my-project --ai claude --debug
```

### Q: How do I check if my system meets the requirements?

**Solution:** Run the check command:

```bash
specify check
```

This will verify:
- Python version
- Git installation
- uv tool installation
- Agent tool availability

## Corporate/Enterprise Issues

### Q: GitHub API rate limits in corporate environments

**Cause:** Corporate networks or rate limiting.

**Fix:** Use a GitHub token:

```bash
specify init my-project --ai claude --github-token ghp_your_token_here
```

---

**See Also:**
- [Git & Branches FAQ](git-and-branches.md) - Feature branch workflows
- [Customization FAQ](customization.md) - Customizing constitution and templates
- [Installation Guide](../docs/installation.md) - Initial setup instructions
- [Upgrade Guide](../workflows/upgrade-project.md) - How to upgrade Spec-Kit
