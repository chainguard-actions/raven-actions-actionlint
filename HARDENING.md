<!-- markdownlint-disable -->

# Hardening Report: raven-actions--actionlint/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **raven-actions--actionlint/v2.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Install dependencies' step's run: block directly interpolates the GitHub Actions expression `${{ runner.temp }}` into the shell command string. Any `${{ ... }}` expression inside a run: block is subject to YAML template substitution before the shell processes it, making it a script-injection risk. The offending line is: `run: npm install --prefix "${{ runner.temp }}/actionlint-action" ...`. This should be replaced with the environment variable `$RUNNER_TEMP` (which is already available as a process env var) to avoid direct expression interpolation in the shell command.

Locations:

- `action.yml:208`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed the script-injection finding in action.yml at the 'Install dependencies' step (line 208). Replaced `${{ runner.temp }}` with `$RUNNER_TEMP` (the pre-existing GitHub Actions environment variable) in the npm install --prefix argument. Also changed `shell: ${{ (runner.os == 'Windows' && 'pwsh') || 'bash' }}` to `shell: bash` to ensure consistent variable syntax across all platforms (bash is available on all GitHub Actions runners including Windows via Git Bash).

