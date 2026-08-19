<!-- markdownlint-disable -->

# Hardening Report: google-github-actions--deploy-cloud-functions/v3.0.7

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **google-github-actions--deploy-cloud-functions/v3.0.7** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tags or branch names instead of full 40-character SHA digests, making them vulnerable to supply-chain attacks.

- cleanup.yml: `actions/checkout@v4` (line 17), `google-github-actions/auth@v2` (line 19), `google-github-actions/setup-gcloud@v2` (line 24)
- draft-release.yml: `google-github-actions/.github/.github/workflows/draft-release.yml@v0` (line 19)
- integration.yml: `actions/checkout@v4` (lines 25, 50), `actions/setup-node@v4` (lines 27, 52), `google-github-actions/auth@v2` (lines 33, 58)
- release.yml: `google-github-actions/.github/.github/workflows/release.yml@v1` (line 13)
- unit.yml: `actions/checkout@v4` (line 33), `actions/setup-node@v4` (line 35), `google-github-actions/auth@v2` (line 47)

Locations:

- `.github/workflows/cleanup.yml:17`
- `.github/workflows/cleanup.yml:19`
- `.github/workflows/cleanup.yml:24`
- `.github/workflows/draft-release.yml:19`
- `.github/workflows/integration.yml:25`
- `.github/workflows/integration.yml:27`
- `.github/workflows/integration.yml:33`
- `.github/workflows/integration.yml:50`
- `.github/workflows/integration.yml:52`
- `.github/workflows/integration.yml:58`
- `.github/workflows/release.yml:13`
- `.github/workflows/unit.yml:33`
- `.github/workflows/unit.yml:35`
- `.github/workflows/unit.yml:47`

### script-injection (severity: high)

Sub-rule (a): A `${{ }}` expression is interpolated directly inside a `run:` shell command string. In cleanup.yml, the `Delete services` step contains: `gcloud config set core/project "${{ vars.PROJECT_ID }}"`. The `vars.PROJECT_ID` value is substituted by the GitHub Actions template engine before the shell ever sees it, allowing an attacker who can control repository variables to inject arbitrary shell commands.

Locations:

- `.github/workflows/cleanup.yml:28`

### missing-permissions (severity: medium)

The following workflow files have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs, meaning the GITHUB_TOKEN is granted its default (broad) permissions:

- draft-release.yml: the `draft-release` job calls a reusable workflow with no permissions block.
- release.yml: the `release` job calls a reusable workflow with no permissions block.

Locations:

- `.github/workflows/draft-release.yml:1`
- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all findings across 5 workflow files:

1. **unpinned-uses**: Pinned all action references to full SHA digests:
   - `actions/checkout@v4` → `@11d5960a326750d5838078e36cf38b85af677262` (cleanup.yml, integration.yml x2, unit.yml)
   - `google-github-actions/auth@v2` → `@c200f3691d83b41bf9bbd8638997a462592937ed` (cleanup.yml, integration.yml x2, unit.yml)
   - `google-github-actions/setup-gcloud@v2` → `@e427ad8a34f8676edf47cf7d7925499adf3eb74f` (cleanup.yml)
   - `actions/setup-node@v4` → `@49933ea5288caeca8642d1e84afbd3f7d6820020` (integration.yml x2, unit.yml)
   - `google-github-actions/.github@v0` → `@7db31c4dda4d67c9f66fc070137881f1cb4c7c37` (draft-release.yml)
   - `google-github-actions/.github@v1` → `@6900f1ed495961bca1d6c2e6cb679e7ce7e23a88` (release.yml)

2. **script-injection**: In cleanup.yml, moved `${{ vars.PROJECT_ID }}` from the `run:` shell string into an `env:` block as `PROJECT_ID`, then referenced it as `$PROJECT_ID` in the shell script.

3. **missing-permissions**: Added `permissions: {}` top-level blocks to draft-release.yml and release.yml (both call reusable workflows and need no direct GITHUB_TOKEN permissions).

