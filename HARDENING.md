<!-- markdownlint-disable -->

# Hardening Report: Eomm--why-don-t-you-tweet/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Eomm--why-don-t-you-tweet/v2.0.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags instead of full 40-character SHA commit digests, making them vulnerable to supply-chain attacks if the tag is moved.

.github/workflows/ci.yml:
  - actions/checkout@v4
  - actions/setup-node@v4
  - fastify/github-action-merge-dependabot@v3

.github/workflows/release.yml:
  - actions/checkout@v4
  - actions/setup-node@v4
  - nearform/optic-release-automation-action@v4

.github/workflows/test.yml:
  - actions/checkout@v4

Locations:

- `.github/workflows/ci.yml:7`
- `.github/workflows/ci.yml:8`
- `.github/workflows/ci.yml:20`
- `.github/workflows/release.yml:20`
- `.github/workflows/release.yml:21`
- `.github/workflows/release.yml:24`
- `.github/workflows/test.yml:14`

### missing-permissions (severity: medium)

ci.yml has no top-level `permissions:` key and the `build` job has no job-level `permissions:` block. Only the `automerge` job defines permissions, leaving `build` with the default (potentially broad) token permissions.

Locations:

- `.github/workflows/ci.yml:1`

### missing-permissions (severity: medium)

release.yml has no top-level `permissions:` key and the `release` job has no job-level `permissions:` block. The workflow runs on `pull_request` and `workflow_dispatch` triggers with default (potentially broad) token permissions.

Locations:

- `.github/workflows/release.yml:1`

### missing-permissions (severity: medium)

test.yml has no top-level `permissions:` key and neither the `units` nor the `test` job defines a job-level `permissions:` block, leaving both jobs with default (potentially broad) token permissions.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 3 workflow files:

1. ci.yml: Pinned actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020, fastify/github-action-merge-dependabot@v3 → @73ec4cbb5e56df5591eae286972d5b2201ffe90f. Added top-level `permissions: {}` and job-level `permissions: contents: read` to the `build` job.

2. release.yml: Pinned actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020, nearform/optic-release-automation-action@v4 → @08642f7889f3bb519fb69ce2c3bf58e4f900c018. Added top-level `permissions: {}` and job-level `permissions: contents: write, pull-requests: write` to the `release` job.

3. test.yml: Pinned actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 (used twice). Added top-level `permissions: {}` and job-level `permissions: contents: read` to both the `units` and `test` jobs.

