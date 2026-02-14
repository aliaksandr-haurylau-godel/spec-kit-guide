---
version: 1
source_date: 2026-02-12
derived_from: raw_data/2026-02-12-spec-kit-official-docs/docs/local-development.md
---

# Local Development Workflow

This guide is for contributors to the Spec-Kit project itself. It shows how to iterate on the `specify` CLI locally without publishing a release or committing to `main` first.

> **Note:** Scripts now have both Bash (`.sh`) and PowerShell (`.ps1`) variants. The CLI auto-selects based on OS unless you pass `--script sh|ps`.

---

## Prerequisites

- Python 3.11+
- [uv](https://docs.astral.sh/uv/) package manager
- Git

---

## 1. Clone and Switch Branches

```bash
git clone https://github.com/github/spec-kit.git
cd spec-kit

# Work on a feature branch
git checkout -b your-feature-branch
```

---

## 2. Run the CLI Directly (Fastest Feedback)

You can execute the CLI via the module entrypoint without installing anything:

```bash
# From repo root
python -m src.specify_cli --help
python -m src.specify_cli init demo-project --ai claude --ignore-agent-tools --script sh
```

If you prefer invoking the script file style (uses shebang):

```bash
python src/specify_cli/__init__.py init demo-project --script ps
```

**Use this method for:** Quick iteration, debugging, testing new flags.

---

## 3. Use Editable Install (Isolated Environment)

Create an isolated environment using `uv` so dependencies resolve exactly like end users get them:

```bash
# Create & activate virtual env (uv auto-manages .venv)
uv venv
source .venv/bin/activate  # or on Windows PowerShell: .venv\Scripts\Activate.ps1

# Install project in editable mode
uv pip install -e .

# Now 'specify' entrypoint is available
specify --help
specify init demo-project --ai claude
```

Re-running after code edits requires no reinstall because of editable mode.

**Use this method for:** Testing in a clean, isolated environment that matches user installs.

---

## 4. Invoke with uvx Directly From Git (Current Branch)

`uvx` can run from a local path (or a Git ref) to simulate user flows:

### Run from local directory:

```bash
uvx --from . specify init demo-uvx --ai copilot --ignore-agent-tools --script sh
```

### Run from a specific branch:

You can also point uvx at a specific branch without merging:

```bash
# Push your working branch first
git push origin your-feature-branch
uvx --from git+https://github.com/github/spec-kit.git@your-feature-branch specify init demo-branch-test --script ps
```

### Absolute Path uvx (Run From Anywhere)

If you're in another directory, use an absolute path instead of `.`:

```bash
uvx --from /mnt/c/GitHub/spec-kit specify --help
uvx --from /mnt/c/GitHub/spec-kit specify init demo-anywhere --ai copilot --ignore-agent-tools --script sh
```

**Set an environment variable for convenience:**

```bash
export SPEC_KIT_SRC=/mnt/c/GitHub/spec-kit
uvx --from "$SPEC_KIT_SRC" specify init demo-env --ai copilot --ignore-agent-tools --script ps
```

**Define a shell function (optional):**

```bash
specify-dev() { uvx --from /mnt/c/GitHub/spec-kit specify "$@"; }
# Then
specify-dev --help
specify-dev init my-test-project --ai claude
```

**Use this method for:** Testing the complete user flow, CI/CD simulation, branch-specific testing.

---

## 5. Testing Script Permission Logic

After running an `init`, check that shell scripts are executable on POSIX systems:

```bash
ls -l scripts | grep .sh
# Expect owner execute bit (e.g. -rwxr-xr-x)
```

On Windows you will instead use the `.ps1` scripts (no chmod needed).

**Test both script types:**

```bash
# Test Bash scripts
specify init test-sh --script sh --ai claude --ignore-agent-tools
ls -l test-sh/.specify/scripts/*.sh

# Test PowerShell scripts
specify init test-ps --script ps --ai copilot --ignore-agent-tools
ls test-ps/.specify/scripts/*.ps1
```

---

## 6. Run Lint / Basic Checks

Currently no enforced lint config is bundled, but you can quickly sanity check importability:

```bash
python -c "import specify_cli; print('Import OK')"
```

**Add your own linting:**

```bash
# Example: ruff or black
ruff check src/
black --check src/
```

---

## 7. Build a Wheel Locally (Optional)

Validate packaging before publishing:

```bash
uv build
ls dist/
```

Install the built artifact into a fresh throwaway environment if needed:

```bash
uv venv test-env
source test-env/bin/activate
uv pip install dist/*.whl
specify --help
```

---

## 8. Using a Temporary Workspace

When testing `init --here` in a dirty directory, create a temp workspace:

```bash
mkdir /tmp/spec-test && cd /tmp/spec-test
python -m src.specify_cli init --here --ai claude --ignore-agent-tools --script sh
```

Or copy only the modified CLI portion if you want a lighter sandbox.

**Clean up after testing:**

```bash
rm -rf /tmp/spec-test
```

---

## 9. Debug Network / TLS Skips

If you need to bypass TLS validation while experimenting:

```bash
specify check --skip-tls
specify init demo --skip-tls --ai gemini --ignore-agent-tools --script ps
```

**⚠️ Use only for local experimentation.** Never recommend `--skip-tls` to users in production.

---

## 10. Rapid Edit Loop Summary

| Action | Command |
|--------|---------|
| Run CLI directly | `python -m src.specify_cli --help` |
| Editable install | `uv pip install -e .` then `specify ...` |
| Local uvx run (repo root) | `uvx --from . specify ...` |
| Local uvx run (abs path) | `uvx --from /mnt/c/GitHub/spec-kit specify ...` |
| Git branch uvx | `uvx --from git+URL@branch specify ...` |
| Build wheel | `uv build` |

---

## 11. Cleaning Up

Remove build artifacts / virtual env quickly:

```bash
rm -rf .venv dist build *.egg-info
```

**Clean up test projects:**

```bash
rm -rf demo-* test-* /tmp/spec-test
```

---

## 12. Common Issues

| Symptom | Fix |
|---------|-----|
| `ModuleNotFoundError: typer` | Run `uv pip install -e .` |
| Scripts not executable (Linux) | Re-run init or `chmod +x scripts/*.sh` |
| Git step skipped | You passed `--no-git` or Git not installed |
| Wrong script type downloaded | Pass `--script sh` or `--script ps` explicitly |
| TLS errors on corporate network | Try `--skip-tls` (not for production) |

---

## 13. Testing Your Changes

### Test Matrix

Test your changes against multiple scenarios:

**Agents:**
- Claude (`--ai claude`)
- Copilot (`--ai copilot`)
- Cursor (`--ai cursor-agent`)
- Gemini (`--ai gemini`)

**Script Types:**
- Bash (`--script sh`)
- PowerShell (`--script ps`)

**Installation Types:**
- New project (`specify init my-project`)
- Existing project (`specify init --here`)
- With force flag (`specify init --here --force`)
- Without Git (`--no-git`)

**Example test script:**

```bash
#!/bin/bash
set -e

echo "Testing new project..."
specify init test-new --ai claude --ignore-agent-tools --script sh

echo "Testing existing project..."
cd test-new
specify init --here --force --ai claude --script sh

echo "Testing without Git..."
cd ..
specify init test-no-git --ai copilot --no-git --ignore-agent-tools --script ps

echo "All tests passed!"
```

---

## 14. Contributing Changes

Once you're satisfied with your changes:

1. **Run through the Quick Start** using your modified CLI
2. **Update documentation** if you changed behavior or added flags
3. **Test against multiple agents and script types**
4. **Open a Pull Request** with:
   - Clear description of changes
   - Test results
   - Breaking changes (if any)

**PR Checklist:**
- [ ] Code runs via `python -m src.specify_cli`
- [ ] Editable install works (`uv pip install -e .`)
- [ ] Scripts are executable on Linux (`.sh` files)
- [ ] Both script types work (`--script sh` and `--script ps`)
- [ ] Documentation updated
- [ ] No regressions in existing functionality

---

## 15. Release Process (Maintainers)

**Note:** This section is for project maintainers only.

After changes land in `main`:

1. **Update version** in `pyproject.toml`
2. **Tag release** with semantic version
3. **Publish to PyPI** (if applicable)
4. **Announce** in README and release notes

---

## See Also

- [Basic Workflow](basic-workflow.md) - User-facing workflow
- [Upgrade Guide](upgrade-project.md) - Upgrading process
- [CLI Reference](../docs/cli-reference.md) - All command options
- [GitHub Repository](https://github.com/github/spec-kit) - Source code
