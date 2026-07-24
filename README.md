# Github Actions Templates

This repo stores Custom Actions and Reusable Workflows for the organization.

## Custom Action vs. Reusable Workflow
The fundamental difference is what you are creating:

- **Custom Action**: A single, self-contained, and reusable step that you can use within a job. It's usually written in JavaScript or as a Docker container and is defined by an action.yml file.

- **Reusable Workflow**: An entire, pre-defined workflow file (including jobs and steps) that can be called and run by another workflow. It's defined by a standard .yml file with a workflow_call trigger.

## Available Workflows

### Tofu Plan & Apply for GCP
Located at: `.github/workflows/tofu-gcp-plan-apply.yml`

This workflow handles OpenTofu `plan` and `apply` operations for Google Cloud Platform. It supports Workload Identity Federation for authentication and GCS for state storage.

#### Inputs

| Input | Description | Required | Default |
| :--- | :--- | :---: | :---: |
| `gcp_project_id` | The GCP project ID. | Yes | - |
| `gcp_wif_provider` | The full resource name of the WIF provider for Github Actions. | Yes | - |
| `gcp_service_account` | The service account email for the Github Actions workflow to use. | Yes | - |
| `tofu_version` | OpenTofu version to use. | No | `1.8.1` |
| `gcp_state_bucket_name` | The name of the GCS bucket for Tofu state. | Yes | - |
| `gcp_state_prefix` | Optional prefix (folder) in the GCS bucket for Tofu state. | No | `''` |
| `working_directory` | The directory where the Tofu commands will be run. | Yes | - |
| `tfvars_file` | Optional name of the .tfvars file to use. | No | `''` |
| `allow_apply` | Set to true to allow the apply step to run. | No | `true` |

#### Usage Example

```yaml
name: Deploy Infrastructure

on:
  push:
    branches: [ main ]
  pull_request:

permissions:
  contents: read
  id-token: write
  pull-requests: write

jobs:
  tofu-gcp:
    name: Call Tofu Workflow
    uses: openjusticeok/actions/.github/workflows/tofu-gcp-plan-apply.yml@v1
    with:
      gcp_project_id: 'my-gcp-project'
      gcp_wif_provider: 'projects/123456789/locations/global/workloadIdentityPools/my-pool/providers/my-provider'
      gcp_service_account: 'my-sa@my-gcp-project.iam.gserviceaccount.com'
      tofu_version: '1.8.1'
      gcp_state_bucket_name: 'my-terraform-state-bucket'
      gcp_state_prefix: 'prod/infrastructure'
      working_directory: './infrastructure'
      allow_apply: ${{ github.ref == 'refs/heads/main' }}
```

### R CI
Located at: `.github/workflows/r-ci.yml`

This workflow performs style checks for R projects using the tools from the OPI Nix flake:
1.  **Format Check:** Checks if all R files are formatted correctly with `air`.
2.  **Lint:** Runs `jarl` to catch common R issues.

#### Inputs

| Input | Description | Required | Default |
| :--- | :--- | :---: | :---: |
| `r_version` | R version to use. | No | `4.5` |
| `working_directory` | Directory to run checks in. | No | `.` |

#### Usage Example

```yaml
name: R CI

on:
  push:
  pull_request:

permissions:
  contents: read

jobs:
  r-ci:
    uses: openjusticeok/actions/.github/workflows/r-ci.yml@v1
    with:
      working_directory: '.'
```

### Targets CI
Located at: `.github/workflows/targets-ci.yml`

This workflow validates a [`targets`](https://docs.ropensci.org/targets/) pipeline for R projects:
1.  **Dependency Sync:** Runs `rv sync` to install the R packages captured in `rv.lock`.
2.  **Pipeline Validation:** Runs `targets::tar_validate()` to check the pipeline definition.
3.  **Optional Run:** Can optionally run `targets::tar_make()` via the `run_tar_make` input.

The `rv/` library is cached between runs using the hash of `rv.lock` as the cache key.

#### Inputs

| Input | Description | Required | Default |
| :--- | :--- | :---: | :---: |
| `r_version` | R version to use. | No | `4.5` |
| `working_directory` | Directory to run checks in. | No | `.` |
| `run_tar_make` | Run `tar_make()` after validation. | No | `false` |

#### Usage Example

```yaml
name: Targets CI

on:
  push:
  pull_request:

permissions:
  contents: read

jobs:
  targets-ci:
    uses: openjusticeok/actions/.github/workflows/targets-ci.yml@v1
    with:
      working_directory: '.'
      run_tar_make: false
```

### OpenTofu CI
Located at: `.github/workflows/tofu-ci.yml`

This workflow performs basic Continuous Integration checks for OpenTofu projects:
1.  **Format Check:** Checks if all configuration files are formatted correctly (`tofu fmt`).
2.  **Validation:** Runs `tofu validate` in specified directories to check for syntax and configuration validity.

#### Inputs

| Input | Description | Required | Default |
| :--- | :--- | :---: | :---: |
| `tofu_version` | OpenTofu version to use. | No | `1.8.1` |
| `directories` | JSON list of directories to run validation in. | Yes | - |

#### Usage Example

```yaml
name: CI

on:
  pull_request:

jobs:
  tofu-ci:
    uses: openjusticeok/actions/.github/workflows/tofu-ci.yml@v1
    with:
      tofu_version: '1.8.1'
      directories: '["modules/network", "modules/compute", "envs/dev"]'
```

## Repository Structure

Reusable Workflows live inside the `.github/workflows/` directory.

```
actions/
├── .github/
│   └── workflows/
│       └── tofu-gcp-plan-apply.yml  # <-- Reusable Workflow for GCP Tofu
├── README.md
└── NEWS.md
```