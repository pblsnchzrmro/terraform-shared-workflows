# Examples

Examples of how to use the shared workflows in your Terraform repositories.

## Usage Options

You have **2 ways** to use these workflows:

### Option 1: Separate Files (4 files)

Copy each file individually to `.github/workflows/` in your repository:

- **security-scan.yml** - Runs KICS security scanner on PRs
- **pre-commit.yml** - Runs Terraform validation on PRs
- **doc.yml** - Auto-generates documentation on PRs
- **release.yml** - Creates semantic releases on push to main

**Use this when:** You want granular control and only need some workflows.

### Option 2: Combined Files

- **pull-request.yml** - Runs all 3 PR checks (security + pre-commit + docs) in parallel
- **all-in-one.yml** - Single file that handles both PR checks AND releases

**Use this when:** You want simplicity and need all workflows.

## Quick Start

1. Create `.github/workflows/` directory in your repository if it doesn't exist
2. Copy `all-in-one.yml` to `.github/workflows/terraform.yml`
3. Commit and push
4. Done! Workflows will run automatically

## What Happens When?

**When you open a PR:**

```text
Pull Request opened → 3 workflows run in parallel:
├── Security Scan (KICS)
├── Pre-Commit (Terraform validate)
└── Documentation (terraform-docs)
```

**When you merge to main:**

```text
Push to main → 1 workflow runs:
└── Semantic Release (creates version tag)
```

## Customization

You can adjust triggers by modifying the `on:` section:

```yaml
on:
  pull_request:
    branches:
      - main
      - develop  # Add more branches
    paths:
      - '**/*.tf'  # Only run if .tf files change
```
