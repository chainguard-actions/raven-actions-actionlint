<!-- markdownlint-disable -->

# Hardening Report: raven-actions--actionlint/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **raven-actions--actionlint/v2.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a) violation: A `${{ ... }}` expression is directly interpolated inside a `run:` shell command string in the 'Install dependencies' step. The offending line is:

  run: npm install --prefix "${{ runner.temp }}/actionlint-action" --no-save ...

Even though `runner.temp` is a GitHub-controlled context (not directly attacker-supplied), any `${{ ... }}` expression inside a `run:` block undergoes YAML template substitution before the shell ever sees it, making it a script-injection risk. The value should be passed via an `env:` variable and referenced as `"$RUNNER_TEMP"` in the shell command instead.

Locations:

- `action.yml:215`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed the script injection in the 'Install dependencies' step of action.yml (line 215). Moved `${{ runner.temp }}` from the `run:` shell command into an `env:` block as `RUNNER_TEMP_PATH`, and updated the shell command to reference it as `"$RUNNER_TEMP_PATH"`. Changed the shell from the dynamic `${{ (runner.os == 'Windows' && 'pwsh') || 'bash' }}` expression to explicit `bash` (bash is available on all GitHub Actions runners including Windows via Git Bash) to ensure the bash variable syntax works correctly.

