<!-- markdownlint-disable -->

# Hardening Report: raven-actions--actionlint/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **raven-actions--actionlint/v2.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Install dependencies' step's run: block directly interpolates the expression `${{ runner.temp }}` into the shell command string: `run: npm install --prefix "${{ runner.temp }}/actionlint-action" ...`. Any `${{ ... }}` expression interpolated directly inside a run: shell command is a script-injection risk because the value is substituted into the shell command before the shell ever parses it, allowing metacharacters to be interpreted. The value should be passed via an env: variable and referenced as `"$RUNNER_TEMP"` instead.

Locations:

- `action.yml:222`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in the 'Install dependencies' step of action.yml (line 222). Moved `${{ runner.temp }}` out of the run: shell command and into an env: block as `RUNNER_TEMP: ${{ runner.temp }}`. The shell command now references `$RUNNER_TEMP` instead of the inline expression. Changed shell from the conditional pwsh/bash expression to `bash` (available on all GitHub Actions runners including Windows) to ensure the bash variable syntax works correctly.

