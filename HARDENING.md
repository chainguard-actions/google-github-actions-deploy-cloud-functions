<!-- markdownlint-disable -->

# Hardening Report: google-github-actions--deploy-cloud-functions/v3.0.8

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **google-github-actions--deploy-cloud-functions/v3.0.8** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is interpolated directly inside a run: shell command string. The expression `${{ vars.PROJECT_ID }}` is substituted into the shell script before the shell ever sees it, allowing an attacker who can control the vars.PROJECT_ID repository variable to inject arbitrary shell commands. The offending line is: `gcloud config set core/project "${{ vars.PROJECT_ID }}"`

Locations:

- `.github/workflows/cleanup.yml:29`

### unpinned-uses (severity: high)

Multiple workflow files reference external actions and reusable workflows using mutable version tags (@v2, @v3) instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved. Failing references: cleanup.yml — `google-github-actions/auth@v2` and `google-github-actions/setup-gcloud@v2`; draft-release.yml — `google-github-actions/.github/.github/workflows/draft-release.yml@v3`; integration.yml — `google-github-actions/auth@v2` (two steps); release.yml — `google-github-actions/.github/.github/workflows/release.yml@v3`; unit.yml — `google-github-actions/auth@v2`.

Locations:

- `.github/workflows/cleanup.yml:20`
- `.github/workflows/cleanup.yml:24`
- `.github/workflows/draft-release.yml:17`
- `.github/workflows/integration.yml:28`
- `.github/workflows/integration.yml:57`
- `.github/workflows/release.yml:10`
- `.github/workflows/unit.yml:37`

### missing-permissions (severity: medium)

Two workflow files have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions. `draft-release.yml` has one job (`draft-release`) with no permissions block. `release.yml` has one job (`release`) with no permissions block.

Locations:

- `.github/workflows/draft-release.yml:1`
- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all three findings: (1) Script injection in cleanup.yml fixed by moving ${{ vars.PROJECT_ID }} into an env: block and referencing it as $PROJECT_ID in the shell. (2) All 7 unpinned action references pinned to full commit SHAs: google-github-actions/auth@v2 → c200f3691d83b41bf9bbd8638997a462592937ed, google-github-actions/setup-gcloud@v2 → e427ad8a34f8676edf47cf7d7925499adf3eb74f, google-github-actions/.github@v3 → 29c6d38eeb974133b4b66401985f7c70cf4a6681. (3) Added top-level 'permissions: {}' to draft-release.yml and release.yml which had no permissions blocks.

