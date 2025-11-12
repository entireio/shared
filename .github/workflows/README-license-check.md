# Organization-Wide License Check

This directory contains a reusable workflow for checking dependency licenses across all repositories in the organization.

## Reusable Workflow

**File**: `license-check-reusable.yml`

This workflow can be called from any repository in the organization to enforce license compliance.

## How It Works

The workflow performs these checks in order:

1. **Check for `mise lint:licenses` task**
   - Looks for the mise task in the repository
   - Fails if it not exists

2. **Run the license check**
   - Executes `mise run lint:licenses` if the task exists
   - Reports any license violations

## Usage in Other Repositories

### 1: Implement License Checking

To implement license checking in your repository:

1. **Copy the required files** from this repo:
   - `.allowed-licenses` - List of permitted license types

2. **Create a mise task called "lint:licenses"

3. **Customize `.allowed-licenses`** for your needs:
   ```
   MIT
   BSD-2-Clause
   BSD-3-Clause
   Apache-2.0
   ```

4. **Test locally**:
   ```bash
   mise run lint:licenses
   ```

5. **Add the workflow**

    Create `.github/workflows/license-check.yml` in your repository:
    
    ```yaml
    name: License Check
    
    on:
      pull_request:
        branches:
          - main
      push:
        branches:
          - main
    
    jobs:
      check-licenses:
        uses: entirehq/shared/.github/workflows/license-check-reusable.yml@main
        # No inputs needed
    ```
