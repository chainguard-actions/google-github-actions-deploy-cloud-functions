<!-- markdownlint-disable -->

# Hardening Report: google-github-actions--deploy-cloud-functions/v4.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **google-github-actions--deploy-cloud-functions/v4.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions and reusable workflows using mutable version tags instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit.

Failing references:
- cleanup.yml: `google-github-actions/auth@v3` (line 21)
- cleanup.yml: `google-github-actions/setup-gcloud@v2` (line 26)
- draft-release.yml: `google-github-actions/.github/.github/workflows/draft-release.yml@v3` (line 17)
- integration.yml: `google-github-actions/auth@v3` (line 31, line 55)
- release.yml: `google-github-actions/.github/.github/workflows/release.yml@v3` (line 10)
- unit.yml: `google-github-actions/auth@v3` (line 44)

Locations:

- `.github/workflows/cleanup.yml:21`
- `.github/workflows/cleanup.yml:26`
- `.github/workflows/draft-release.yml:17`
- `.github/workflows/integration.yml:31`
- `.github/workflows/integration.yml:55`
- `.github/workflows/release.yml:10`
- `.github/workflows/unit.yml:44`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all mutable tag references to full commit SHAs across 5 workflow files:
- cleanup.yml: `google-github-actions/auth@v3` → `@7c6bc770dae815cd3e89ee6cdf493a5fab2cc093 # v3`; `google-github-actions/setup-gcloud@v2` → `@e427ad8a34f8676edf47cf7d7925499adf3eb74f # v2`
- draft-release.yml: `google-github-actions/.github/.github/workflows/draft-release.yml@v3` → `@29c6d38eeb974133b4b66401985f7c70cf4a6681 # v3`
- integration.yml: both occurrences of `google-github-actions/auth@v3` → `@7c6bc770dae815cd3e89ee6cdf493a5fab2cc093 # v3` (file was rewritten after edit corruption)
- release.yml: `google-github-actions/.github/.github/workflows/release.yml@v3` → `@29c6d38eeb974133b4b66401985f7c70cf4a6681 # v3`
- unit.yml: `google-github-actions/auth@v3` → `@7c6bc770dae815cd3e89ee6cdf493a5fab2cc093 # v3`

