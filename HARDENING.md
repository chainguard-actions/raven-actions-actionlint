<!-- markdownlint-disable -->

# Hardening Report: raven-actions--actionlint/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **raven-actions--actionlint/v2.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Install dependencies' step's `run:` block directly interpolates `${{ runner.temp }}` inside the shell command string. Any `${{ ... }}` expression interpolated directly in a `run:` script is a script-injection risk because YAML template substitution occurs before the shell ever sees the value. The offending line is: `run: npm install --prefix "${{ runner.temp }}/actionlint-action" ...`. This should be replaced with the equivalent process environment variable `$RUNNER_TEMP` instead.

Locations:

- `action.yml:196`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed the 'Install dependencies' step in action.yml (line 196): moved `${{ runner.temp }}` out of the `run:` block into an `env:` block as `RUNNER_TEMP_PATH: ${{ runner.temp }}`, and replaced the inline expression with `$RUNNER_TEMP_PATH` in the shell command. Also simplified the shell from the dynamic `${{ (runner.os == 'Windows' && 'pwsh') || 'bash' }}` to `bash`, which is available on all GitHub Actions platforms including Windows.

