# Policy Bot Reusable Workflow Demo

This repository demonstrates how to use reusable workflows with concurrency control to enable matrix jobs while maintaining single-job concurrency semantics.

## Problem Solved

When you have matrix jobs with concurrency control, each matrix job tries to acquire the same lock, causing issues. The solution is to wrap the matrix jobs in a reusable workflow and apply concurrency control to the calling job.

## Setup

### Workflows
- **Parent Workflow** (`.github/workflows/parent.yml`): Calls reusable workflow with concurrency control at job level
- **Reusable Matrix Workflow** (`.github/workflows/matrix-jobs.yml`): Contains matrix jobs without individual concurrency controls

### Key Pattern
```yaml
jobs:
  call-reusable-workflow:
    concurrency:
      group: deploy-${{ github.ref }}
      cancel-in-progress: false
    uses: ./.github/workflows/matrix-jobs.yml
```

### Policy Configuration
The `.policy.yml` file requires the parent workflow (which includes all matrix jobs) to succeed.

## Benefits
- Matrix jobs run in parallel within the reusable workflow
- Concurrency control ensures only one deployment per ref
- All jobs appear as part of the same workflow run for policy enforcement