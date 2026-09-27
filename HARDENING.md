<!-- markdownlint-disable -->

# Hardening Report: raven-actions--actionlint/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **raven-actions--actionlint/v2.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Install dependencies' step's run: block directly interpolates the expression `${{ runner.temp }}` inside the shell command string. Any `${{ ... }}` expression interpolated directly into a run: block is a script-injection risk because the value is substituted by the GitHub Actions template engine before the shell ever sees it, bypassing shell quoting. The offending line is: `run: npm install --prefix "${{ runner.temp }}/actionlint-action" ...`. The fix is to pass the value via an env: variable and reference it as `"$RUNNER_TEMP"` in the shell command instead.

Locations:

- `action.yml:200`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed the 'Install dependencies' step in action.yml (line 200): moved `${{ runner.temp }}` out of the run: block into an env: variable `RUNNER_TEMP: ${{ runner.temp }}`, and updated the npm install command to reference `"$RUNNER_TEMP/actionlint-action"` instead of `"${{ runner.temp }}/actionlint-action"`.

