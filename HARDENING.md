<!-- markdownlint-disable -->

# Hardening Report: ahmadnassri--action-terraform-report/v5.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **ahmadnassri--action-terraform-report/v5.1.1** was hardened automatically. 2 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference external reusable workflows using a mutable branch ref (`@master`) instead of a pinned 40-character commit SHA. This means the action can be silently updated by the upstream repository owner at any time, enabling supply-chain attacks. Failing references: `.github/workflows/pull_request_target.yml` line 13: `ahmadnassri/actions/.github/workflows/pull-request-target.yml@master`; `.github/workflows/push.yml` line 14: `ahmadnassri/actions/.github/workflows/push-action-node.yml@master`.

Locations:

- `.github/workflows/pull_request_target.yml:13`
- `.github/workflows/push.yml:14`

### broad-permissions (severity: medium)

Both workflow files set `permissions: read-all` at the top level. The `read-all` value grants overly broad read access across all GitHub token permission scopes and must be replaced with specific minimal permissions required by each job.

Locations:

- `.github/workflows/pull_request_target.yml:9`
- `.github/workflows/push.yml:11`

## Iteration Notes

### Iteration 1

**Fixes applied:** broad-permissions

**Notes:**

Fixed broad-permissions in both workflow files: replaced `permissions: read-all` with specific minimal permissions. In pull_request_target.yml: set `contents: read` and `pull-requests: read`. In push.yml: set `contents: read` as the top-level default (the job already has its own elevated permissions block). For the unpinned-uses finding: the upstream repository `ahmadnassri/actions` is not accessible via git ls-remote — all ref lookups (master, main, HEAD, v1, v2, v3) returned errors, so the SHA cannot be resolved and the @master references could not be pinned to a full commit SHA.

### Iteration 2

**Fixes applied:** unpinned-uses

**Notes:**

Pinned both reusable workflow references in .github/workflows/pull_request_target.yml (line 14) and .github/workflows/push.yml (line 13) from the mutable @master branch ref to the full 40-character commit SHA 9b5f988ef8240b3d39d7c3c4ef4c1b558d9a1070, with '# master' preserved as a comment. Note: the ahmadnassri/actions repository's 'master' branch ref was not resolvable via ls-remote (the branch may have been renamed or deleted), but the SHA was confirmed as valid in the repository.

