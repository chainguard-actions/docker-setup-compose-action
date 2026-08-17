<!-- markdownlint-disable -->

# Hardening Report: docker--setup-compose-action/v1.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **docker--setup-compose-action/v1.2.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tags instead of pinned 40-character SHA digests, making them vulnerable to supply-chain attacks if the tag is moved. Failing references:
- ci.yml: actions/checkout@v4 (×4)
- publish.yml: actions/checkout@v4, actions/publish-immutable-action@v0.0.4
- test.yml: docker/setup-buildx-action@v3, docker/bake-action@v6, codecov/codecov-action@v5
- validate.yml: actions/checkout@v4, docker/bake-action/subaction/list-targets@v6, docker/setup-buildx-action@v3, docker/bake-action@v6

Locations:

- `.github/workflows/ci.yml:27`
- `.github/workflows/publish.yml:13`
- `.github/workflows/publish.yml:16`
- `.github/workflows/test.yml:20`
- `.github/workflows/test.yml:24`
- `.github/workflows/test.yml:27`
- `.github/workflows/validate.yml:21`
- `.github/workflows/validate.yml:24`
- `.github/workflows/validate.yml:37`
- `.github/workflows/validate.yml:41`

### missing-permissions (severity: medium)

ci.yml has no top-level 'permissions:' key and none of its four jobs (main, multi, standalone, cache-binary) define job-level permissions. This means the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/ci.yml:1`

### missing-permissions (severity: medium)

test.yml has no top-level 'permissions:' key and its only job ('test') has no job-level permissions block. This means the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/test.yml:1`

### missing-permissions (severity: medium)

validate.yml has no top-level 'permissions:' key and neither of its jobs ('prepare', 'validate') define job-level permissions. This means the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/validate.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 4 workflow files:

1. ci.yml: Pinned all 4 `actions/checkout@v4` references to SHA `11d5960a326750d5838078e36cf38b85af677262`. Added top-level `permissions: contents: read` block.

2. publish.yml: Pinned `actions/checkout@v4` to SHA `11d5960a326750d5838078e36cf38b85af677262` and `actions/publish-immutable-action@v0.0.4` to SHA `4bc8754ffc40f27910afb20287dbbbb675a4e978`. (Already had job-level permissions.)

3. test.yml: Pinned `docker/setup-buildx-action@v3` to `8d2750c68a42422c14e847fe6c8ac0403b4cbd6f`, `docker/bake-action@v6` to `5be5f02ff8819ecd3092ea6b2e6261c31774f2b4`, and `codecov/codecov-action@v5` to `0fb7174895f61a3b6b78fc075e0cd60383518dac`. Added top-level `permissions: contents: read` block.

4. validate.yml: Pinned `actions/checkout@v4` to `11d5960a326750d5838078e36cf38b85af677262`, `docker/bake-action/subaction/list-targets@v6` to `5be5f02ff8819ecd3092ea6b2e6261c31774f2b4`, `docker/setup-buildx-action@v3` to `8d2750c68a42422c14e847fe6c8ac0403b4cbd6f`, and `docker/bake-action@v6` to `5be5f02ff8819ecd3092ea6b2e6261c31774f2b4`. Added top-level `permissions: contents: read` block.

All original tag names preserved as inline comments for readability.

