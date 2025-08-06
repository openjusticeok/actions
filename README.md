# Github Actions Templates

This repo stores Custom Actions and Reusable Workflows for the organization.

## Custom Action vs. Reusable Workflow
The fundamental difference is what you are creating:

- **Custom Action**: A single, self-contained, and reusable step that you can use within a job. It's usually written in JavaScript or as a Docker container and is defined by an action.yml file.

- **Reusable Workflow**: An entire, pre-defined workflow file (including jobs and steps) that can be called and run by another workflow. It's defined by a standard .yml file with a workflow_call trigger.

## Using Workflows and Custom Actions

- To call the workflow:
  ```bash
  uses: openjusticeok/actions/.github/workflows/tofu/plan-apply.yml@main
  ```
- To call the action:
  ```bash
  uses: openjusticeok/actions/setup-custom-tool@v1
  ```

## Repository Structure

Reusable Workflows live inside the .github/workflows/ directory.
Each custom action lives in its own directory at the root of the repository. Inside that directory there is an action.yml file and any scripts or Dockerfiles it needs.

Example Repository Layout
Your actions repository could look something like this:

```
actions/
│
├── .github/
│   └── workflows/
│       └── tofu/
│           └── plan-apply.yml      # <-- Your Reusable Workflow
│
├── setup-custom-tool/
│   ├── action.yml                  # <-- Definition for a custom action
│   └── main.js                     # <-- The script for the action
│
└── another-action/
    ├── action.yml
    └── entrypoint.sh
```
