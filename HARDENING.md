<!-- markdownlint-disable -->

# Hardening Report: Eomm--why-don-t-you-tweet/v1.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Eomm--why-don-t-you-tweet/v1.1.0** was hardened automatically. 6 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in ci.yml use mutable tag refs instead of pinned 40-character SHA digests, making the workflow vulnerable to supply-chain attacks if the referenced tags are moved or compromised. Failing references: `actions/checkout@v3` (line 7), `actions/setup-node@v3` (line 8), `fastify/github-action-merge-dependabot@v3` (line 21).

Locations:

- `.github/workflows/ci.yml:7`
- `.github/workflows/ci.yml:8`
- `.github/workflows/ci.yml:21`

### unpinned-uses (severity: high)

All `uses:` references in release.yml use mutable tag refs instead of pinned 40-character SHA digests. Failing references: `actions/checkout@v3` (line 22), `actions/setup-node@v3` (line 23), `nearform/optic-release-automation-action@v3` (line 26).

Locations:

- `.github/workflows/release.yml:22`
- `.github/workflows/release.yml:23`
- `.github/workflows/release.yml:26`

### unpinned-uses (severity: high)

All `uses:` references in test.yml use mutable tag refs instead of pinned 40-character SHA digests. Failing references: `actions/checkout@v3` (line 15, line 23).

Locations:

- `.github/workflows/test.yml:15`
- `.github/workflows/test.yml:23`

### missing-permissions (severity: medium)

ci.yml has no top-level `permissions:` block, and the `build` job (line 4) has no job-level `permissions:` key. Only the `automerge` job defines permissions. Without explicit permissions, the `build` job inherits the default broad token permissions, violating least-privilege.

Locations:

- `.github/workflows/ci.yml:1`

### missing-permissions (severity: medium)

release.yml has no top-level `permissions:` block and the `release` job has no job-level `permissions:` key. The workflow uses `secrets.github_token` and calls an external release automation action without restricting token scope.

Locations:

- `.github/workflows/release.yml:1`

### missing-permissions (severity: medium)

test.yml has no top-level `permissions:` block and neither the `units` job nor the `test` job defines a job-level `permissions:` key. The workflow is triggered on `pull_request` and `push`, so it runs with default broad token permissions.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 6 findings across 3 workflow files:

**ci.yml**: Added top-level `permissions: {}` and job-level `permissions: { contents: read }` to the `build` job. Pinned `actions/checkout@v3` → SHA `a37ce9120846195fa4ece8f58b268e6043cb2f26`, `actions/setup-node@v3` → SHA `3235b876344d2a9aa001b8d1453c930bba69e610`, and `fastify/github-action-merge-dependabot@v3` → SHA `73ec4cbb5e56df5591eae286972d5b2201ffe90f`.

**release.yml**: Added top-level `permissions: {}` and job-level `permissions: { contents: write, pull-requests: write }` to the `release` job (write permissions needed for the release automation action). Pinned `actions/checkout@v3` and `actions/setup-node@v3` to the same SHAs as above, and `nearform/optic-release-automation-action@v3` → SHA `1f7a3866e58c58f41c7ed420e270894b0f2c089c`.

**test.yml**: Added top-level `permissions: {}` and job-level `permissions: { contents: read }` to both the `units` and `test` jobs. Pinned both instances of `actions/checkout@v3` to SHA `a37ce9120846195fa4ece8f58b268e6043cb2f26`.

