<!-- markdownlint-disable -->

# Hardening Report: raven-actions--actionlint/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **raven-actions--actionlint/v2.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ ... }} expression is directly interpolated inside a run: shell command string. In the 'Install dependencies' step, ${{ runner.temp }} is embedded directly in the npm install command: `run: npm install --prefix "${{ runner.temp }}/actionlint-action" ...`. Even though runner.temp is a GitHub-controlled context, any ${{ ... }} expression inside a run: block is a script-injection finding because the value flows through YAML template substitution before the shell ever sees it. The safe alternative is to use the $RUNNER_TEMP environment variable instead.

Locations:

- `action.yml:200`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed the script-injection finding in the 'Install dependencies' step of action.yml. Moved `${{ runner.temp }}` from the inline `run:` command into the step's `env:` block as `RUNNER_TEMP_DIR: ${{ runner.temp }}`, and replaced the inline expression with `$RUNNER_TEMP_DIR` in the shell command. Changed `shell:` from the dynamic expression `${{ (runner.os == 'Windows' && 'pwsh') || 'bash' }}` to `shell: bash`, which works on all GitHub Actions runner platforms (Windows has Git Bash available). This prevents the runner.temp value from flowing through YAML template substitution before the shell processes it.

