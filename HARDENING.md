<!-- markdownlint-disable -->

# Hardening Report: docker--setup-compose-action/v2.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **docker--setup-compose-action/v2.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of immutable full-length SHA commit hashes. This exposes the workflow to supply-chain attacks where a tag could be silently moved to point to malicious code. Unpinned references found:

- ci.yml: `actions/checkout@v6` (×4)
- test.yml: `actions/checkout@v6`, `docker/setup-buildx-action@v3`, `docker/bake-action@v6`, `codecov/codecov-action@v5`
- publish.yml: `actions/checkout@v6`, `actions/publish-immutable-action@v0.0.4`
- update-dist.yml: `actions/create-github-app-token@v2`, `actions/checkout@v6`, `docker/bake-action@v6`
- validate.yml: `actions/checkout@v6`, `docker/bake-action/subaction/list-targets@v6`, `docker/setup-buildx-action@v3`, `docker/bake-action@v6`

All should be pinned to full 40-character SHA digests (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`).

Locations:

- `.github/workflows/ci.yml:33`
- `.github/workflows/test.yml:19`
- `.github/workflows/test.yml:22`
- `.github/workflows/test.yml:26`
- `.github/workflows/test.yml:30`
- `.github/workflows/publish.yml:14`
- `.github/workflows/publish.yml:17`
- `.github/workflows/update-dist.yml:14`
- `.github/workflows/update-dist.yml:20`
- `.github/workflows/update-dist.yml:27`
- `.github/workflows/validate.yml:24`
- `.github/workflows/validate.yml:27`
- `.github/workflows/validate.yml:40`
- `.github/workflows/validate.yml:44`

### missing-permissions (severity: medium)

Four workflow files have no top-level `permissions:` block and no job-level `permissions:` block on any of their jobs. Without explicit permissions, workflows run with the repository's default token permissions, which may be overly broad (e.g. `write` access to contents). Each workflow should declare the minimal set of permissions required.

- ci.yml: jobs `main`, `multi`, `standalone`, `cache-binary` — none have permissions
- test.yml: job `test` — no permissions
- update-dist.yml: job `update-dist` — no permissions
- validate.yml: jobs `prepare`, `validate` — none have permissions

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/test.yml:1`
- `.github/workflows/update-dist.yml:1`
- `.github/workflows/validate.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 workflow files:

**unpinned-uses**: Pinned all action references to full 40-char SHAs:
- actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803 # v6
- docker/setup-buildx-action@v3 → @8d2750c68a42422c14e847fe6c8ac0403b4cbd6f # v3
- docker/bake-action@v6 → @5be5f02ff8819ecd3092ea6b2e6261c31774f2b4 # v6
- codecov/codecov-action@v5 → @0fb7174895f61a3b6b78fc075e0cd60383518dac # v5
- actions/publish-immutable-action@v0.0.4 → @4bc8754ffc40f27910afb20287dbbbb675a4e978 # v0.0.4
- actions/create-github-app-token@v2 → @fee1f7d63c2ff003460e3d139729b119787bc349 # v2
- docker/bake-action/subaction/list-targets@v6 → @5be5f02ff8819ecd3092ea6b2e6261c31774f2b4 # v6

**missing-permissions**: Added top-level `permissions: {}` and minimal job-level permissions to ci.yml, test.yml, update-dist.yml, and validate.yml. update-dist.yml gets `contents: write` (needs to push commits); all others get `contents: read`. publish.yml already had job-level permissions.

