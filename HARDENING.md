<!-- markdownlint-disable -->

# Hardening Report: google-github-actions--deploy-cloud-functions/v3.0.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **google-github-actions--deploy-cloud-functions/v3.0.9** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags (e.g. @v2, @v3) instead of full 40-character commit SHA digests. This exposes the workflow to supply-chain attacks where a tag could be silently moved to point to malicious code.

Failing references:
- cleanup.yml: `google-github-actions/auth@v2` (line 22), `google-github-actions/setup-gcloud@v2` (line 27)
- draft-release.yml: `google-github-actions/.github/.github/workflows/draft-release.yml@v3` (line 17)
- integration.yml: `google-github-actions/auth@v2` (line 35, line 57)
- release.yml: `google-github-actions/.github/.github/workflows/release.yml@v3` (line 11)
- unit.yml: `google-github-actions/auth@v2` (line 48)

All are marked `# ratchet:exclude` but remain unpinned. Each should be replaced with a full SHA pin, e.g. `google-github-actions/auth@71f986410dfbc7added4569d411d040a91dc6935 # v2`.

Locations:

- `.github/workflows/cleanup.yml:22`
- `.github/workflows/cleanup.yml:27`
- `.github/workflows/draft-release.yml:17`
- `.github/workflows/integration.yml:35`
- `.github/workflows/integration.yml:57`
- `.github/workflows/release.yml:11`
- `.github/workflows/unit.yml:48`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all unpinned GitHub Actions references to full SHA digests:
- cleanup.yml: google-github-actions/auth@v2 → @c200f3691d83b41bf9bbd8638997a462592937ed # v2; google-github-actions/setup-gcloud@v2 → @e427ad8a34f8676edf47cf7d7925499adf3eb74f # v2
- draft-release.yml: google-github-actions/.github/.github/workflows/draft-release.yml@v3 → @29c6d38eeb974133b4b66401985f7c70cf4a6681 # v3
- integration.yml: both occurrences of google-github-actions/auth@v2 → @c200f3691d83b41bf9bbd8638997a462592937ed # v2
- release.yml: google-github-actions/.github/.github/workflows/release.yml@v3 → @29c6d38eeb974133b4b66401985f7c70cf4a6681 # v3
- unit.yml: google-github-actions/auth@v2 → @c200f3691d83b41bf9bbd8638997a462592937ed # v2
All ratchet:exclude comments were replaced with human-readable # v2 / # v3 version comments.

