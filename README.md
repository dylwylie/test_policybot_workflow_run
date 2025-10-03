# Policy Bot Workflow Run Demo

This repository demonstrates how Policy Bot can enforce both parent workflows and dependent `workflow_run` triggered workflows.

## Setup

### Workflows
- **Parent Workflow** (`.github/workflows/parent.yml`): Runs on pull requests
- **Dependent Workflow** (`.github/workflows/dependent.yml`): Triggered by completion of Parent Workflow using `workflow_run`

### Policy Configuration
The `.policy.yml` file requires both workflows to succeed:
- Parent Workflow must complete successfully
- Dependent Workflow must complete successfully (only runs from main branch due to workflow_run security restrictions)

## Testing
1. Create a pull request
2. Parent Workflow will run automatically
3. Dependent Workflow will trigger after Parent Workflow completes (runs from main branch)
4. Policy Bot will enforce both workflows are successful before allowing merge