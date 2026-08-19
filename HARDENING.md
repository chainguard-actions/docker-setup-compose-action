<!-- markdownlint-disable -->

# Hardening Report: docker--setup-compose-action/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **docker--setup-compose-action/v2.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the action is compromised.

.github/workflows/ci.yml: actions/checkout@v6 (×4 occurrences)
.github/workflows/publish.yml: actions/checkout@v6, actions/publish-immutable-action@v0.0.4
.github/workflows/test.yml: actions/checkout@v6, docker/setup-buildx-action@v3, docker/bake-action@v6, codecov/codecov-action@v5
.github/workflows/update-dist.yml: actions/create-github-app-token@v2, actions/checkout@v6, docker/bake-action@v6
.github/workflows/validate.yml: actions/checkout@v6, docker/bake-action/subaction/list-targets@v6, docker/setup-buildx-action@v3, docker/bake-action@v6

Locations:

- `.github/workflows/ci.yml:32`
- `.github/workflows/ci.yml:41`
- `.github/workflows/ci.yml:52`
- `.github/workflows/ci.yml:74`
- `.github/workflows/publish.yml:17`
- `.github/workflows/publish.yml:19`
- `.github/workflows/test.yml:20`
- `.github/workflows/test.yml:23`
- `.github/workflows/test.yml:28`
- `.github/workflows/test.yml:33`
- `.github/workflows/update-dist.yml:14`
- `.github/workflows/update-dist.yml:22`
- `.github/workflows/update-dist.yml:29`
- `.github/workflows/validate.yml:19`
- `.github/workflows/validate.yml:22`
- `.github/workflows/validate.yml:33`
- `.github/workflows/validate.yml:38`

### missing-permissions (severity: medium)

Four workflow files have no top-level `permissions:` block and no job-level `permissions:` block on any of their jobs. Without explicit permissions, workflows run with the default (potentially broad) token permissions, violating the principle of least privilege.

- ci.yml: jobs `main`, `multi`, `standalone`, `cache-binary` all lack permissions
- test.yml: job `test` lacks permissions
- update-dist.yml: job `update-dist` lacks permissions
- validate.yml: jobs `prepare` and `validate` lack permissions

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

**unpinned-uses** (17 occurrences pinned):
- actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803 (×8 across ci.yml, publish.yml, test.yml, update-dist.yml, validate.yml)
- actions/publish-immutable-action@v0.0.4 → @4bc8754ffc40f27910afb20287dbbbb675a4e978 (publish.yml)
- docker/setup-buildx-action@v3 → @8d2750c68a42422c14e847fe6c8ac0403b4cbd6f (×2 in test.yml, validate.yml)
- docker/bake-action@v6 → @5be5f02ff8819ecd3092ea6b2e6261c31774f2b4 (×4 in test.yml, update-dist.yml, validate.yml ×2)
- docker/bake-action/subaction/list-targets@v6 → @5be5f02ff8819ecd3092ea6b2e6261c31774f2b4 (validate.yml)
- codecov/codecov-action@v5 → @0fb7174895f61a3b6b78fc075e0cd60383518dac (test.yml)
- actions/create-github-app-token@v2 → @fee1f7d63c2ff003460e3d139729b119787bc349 (update-dist.yml)

**missing-permissions** (4 workflow files):
- ci.yml: added top-level `permissions: {}` + `contents: read` on all 4 jobs
- test.yml: added top-level `permissions: {}` + `contents: read` on test job
- update-dist.yml: added top-level `permissions: {}` + `contents: write` on update-dist job (needs to push commits)
- validate.yml: added top-level `permissions: {}` + `contents: read` on both prepare and validate jobs

publish.yml already had job-level permissions so only the action pins were needed there.

