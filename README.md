# terraform-shared-workflows

Shared reusable GitHub workflows for Terraform projects.

## Prerequisites

To use these workflows in your repository, you need to enable external workflows:

1. Go to your repository **Settings** → **Actions** → **General**
2. Under **Actions permissions**, select:
   - ✅ **Allow pblsnchzrmro actions and reusable workflows**

Without this setting, the workflows won't be able to run.

## Available Workflows

### 1. Security Scan

Runs KICS security scanner on your Terraform code.

```yaml
name: Security Scan
on:
  pull_request:
    branches: [main]

jobs:
  security:
    uses: pblsnchzrmro/terraform-shared-workflows/.github/workflows/security-scan.yml@main
```

### 2. Pre-Commit

Validates Terraform code with pre-commit hooks using min/max supported versions.

```yaml
name: Pre-Commit
on:
  pull_request:
    branches: [main]

permissions:
  contents: read

jobs:
  pre-commit:
    uses: pblsnchzrmro/terraform-shared-workflows/.github/workflows/pre-commit.yml@main
```

### 3. Generate Documentation

Automatically generates and updates Terraform documentation in README.md.

```yaml
name: Documentation
on:
  pull_request:
    branches: [main]

permissions:
  contents: write
  pull-requests: write

jobs:
  docs:
    uses: pblsnchzrmro/terraform-shared-workflows/.github/workflows/doc.yml@main
```

### 4. Release

Creates semantic versioning releases automatically.

```yaml
name: Release
on:
  push:
    branches: [main]

jobs:
  release:
    uses: pblsnchzrmro/terraform-shared-workflows/.github/workflows/release.yml@main
    secrets:
      github_token: ${{ secrets.GITHUB_TOKEN }}
```

## Complete Example

Create a `.github/workflows/terraform.yml` in your repository:

```yaml
name: Terraform CI/CD

on:
  pull_request:
    branches: [main]
  push:
    branches: [main]

permissions:
  contents: write
  pull-requests: write

jobs:
  security:
    if: github.event_name == 'pull_request'
    uses: pblsnchzrmro/terraform-shared-workflows/.github/workflows/security-scan.yml@main

  pre-commit:
    if: github.event_name == 'pull_request'
    uses: pblsnchzrmro/terraform-shared-workflows/.github/workflows/pre-commit.yml@main

  docs:
    if: github.event_name == 'pull_request'
    uses: pblsnchzrmro/terraform-shared-workflows/.github/workflows/doc.yml@main

  release:
    if: github.event_name == 'push'
    needs: [security, pre-commit, docs]
    uses: pblsnchzrmro/terraform-shared-workflows/.github/workflows/release.yml@main
    secrets:
      github_token: ${{ secrets.GITHUB_TOKEN }}
```
