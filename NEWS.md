# News

## v1.2.1 (2026-06-29)

### Fixes
- Mark `allow_apply` as optional (`required: false`) in `tofu-gcp-plan-apply.yml`. Combining `required: true` with a `default` value caused a `startup_failure` when the reusable workflow was called.

## v1.2.0 (2026-06-29)

### New Features
- PR comments now report `tofu init` failures and deep-link to the failing job step.
- Added job summary output showing init, plan, and apply outcomes.
- Added concurrency controls to serialize runs per working directory and branch.
- Added a 30-minute timeout to prevent stuck runs from holding state locks.
- Plan comments now use `-detailed-exitcode`, distinguishing no changes, changes pending, and errors.
- Large plan output is middle-truncated so the summary remains visible.

### Fixes
- Fixed silent failures in `tofu init` and `tofu plan` by enabling `pipefail` with `shell: bash`.
- Added a fail-closed guard so real plan errors turn the check red instead of passing silently.

### Behavior Changes
- `tofu apply` now only runs on the repository's default branch.
- `allow_apply` defaults to `true`; callers can still pass `false` to opt out.

## v1.1.0 (2026-01-09)

### New Features
- Added the `tofu-ci.yml` reusable workflow for format checks and validation across multiple directories.
- Added a `tofu_version` input to the `tofu-gcp-plan-apply.yml` workflow.

### Updates
- Expanded README documentation for the new CI workflow and GCP workflow versioning/safety details.

## v1.0.1 (2025-08-06)

### Fixes
- Removes directory nesting and keeps workflow files at the top level of `.github/workflows/`

## v1.0.0 (2025-08-05)

### Updates
- Initial release of the `gcp-plan-apply` workflow for planning and applying tofu configurations.
- Adopted semantic versioning for better referencing of the workflows.

