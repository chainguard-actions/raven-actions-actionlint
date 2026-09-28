<!-- markdownlint-disable -->

# Hardening Report: raven-actions--actionlint/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **raven-actions--actionlint/v2.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Install dependencies' step's run: block directly interpolates GitHub Actions expressions into the shell command string. Specifically, `${{ runner.temp }}` is embedded in the npm install --prefix path, and `${{ inputs.working-directory }}` (a caller-controlled input) is passed as the working-directory field. Any `${{ ... }}` expression inside a run: block is a script-injection risk because YAML template substitution occurs before the shell ever sees the value. The offending lines are:
  run: npm install --prefix "${{ runner.temp }}/actionlint-action" ...
  working-directory: ${{ inputs.working-directory }}
Fix: use the $RUNNER_TEMP environment variable instead of ${{ runner.temp }}, and pass working-directory via an env: variable (e.g. WORKING_DIR: ${{ inputs.working-directory }}) then reference "$WORKING_DIR" in the script.

Locations:

- `action.yml:200`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed the 'Install dependencies' step in action.yml: (1) replaced `${{ runner.temp }}` in the run: block with `$RUNNER_TEMP` (the built-in GitHub Actions env var, already available at shell runtime), eliminating YAML template substitution before the shell executes; (2) removed `working-directory: ${{ inputs.working-directory }}` and moved the caller-controlled input into an `env:` block as `WORKING_DIR: ${{ inputs.working-directory }}` so it is passed as an environment variable rather than being directly interpolated; (3) changed the shell from the dynamic expression `${{ (runner.os == 'Windows' && 'pwsh') || 'bash' }}` to `bash` for consistent behavior with the bash-style `$RUNNER_TEMP` variable reference.

