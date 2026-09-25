<!-- markdownlint-disable -->

# Hardening Report: raven-actions--actionlint/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **raven-actions--actionlint/v2.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a run: shell command string. The 'Install dependencies' step uses `${{ runner.temp }}` directly in the npm install command: `run: npm install --prefix "${{ runner.temp }}/actionlint-action" ...`. Even though runner.temp is not attacker-controlled, any ${{ ... }} expression inside a run: block is a script-injection finding because the value flows through YAML template substitution before the shell ever sees it. The fix is to use the $RUNNER_TEMP environment variable instead: `run: npm install --prefix "$RUNNER_TEMP/actionlint-action" ...`

Locations:

- `action.yml:201`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Replaced `${{ runner.temp }}` with `$RUNNER_TEMP` in the 'Install dependencies' step's run: command in action.yml. The GitHub Actions runner automatically sets the RUNNER_TEMP environment variable, making it a safe and equivalent replacement that avoids YAML template substitution in the shell command string.

