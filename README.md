# GitHub Actions - Mixed Reusable Workflows

<!-- LOGO -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://nuvibit.com/images/logo/logo-nuvibit-banner.png">
  <source media="(prefers-color-scheme: light)" srcset="https://nuvibit.com/images/logo/logo-nuvibit-banner-dark.png">
  <img alt="Nuvibit - Sovereign AWS Platforms" src="https://nuvibit.com/images/logo/logo-nuvibit-banner.png" width="400">
</picture>
<br/>
<br/>

<!-- DESCRIPTION -->
Collection of mixed reusable [GitHub Workflows](https://docs.github.com/en/actions/learn-github-actions/) for various automation tasks. 
Built by **Nuvibit**, the leading specialist in **sovereign AWS platforms** and creator of the **Nuvibit Terraform Collection (NTC)**.

## 🚀 **Available Workflows**

This collection provides workflows for **different automation scenarios**:

### **📄 Documentation & Static Site Workflows**
- **`hugo-deploy.yml`** - Deploy Hugo static sites to AWS with OIDC authentication
- **`npm-frontend-deploy.yml`** - Deploy NPM-based frontend applications to AWS S3 + CloudFront
- **`drawio-export.yml`** - Export Draw.io diagrams to PNG and commit back to repository

### **📜 License Management Workflows**
- **`license-update.yml`** - Automatically update license files and headers across repositories

## 📋 **Workflow Documentation**

### Hugo Deploy Workflow
**Purpose**: Deploy Hugo static sites to AWS with secure OIDC authentication

**Features**:
- Hugo build with optional minification
- AWS S3 deployment using OIDC (no long-lived credentials)
- Support for Hugo extended version
- Dry-run capability for testing
- Configurable Hugo versions

**Usage**:
```yaml
name: HUGO DEPLOY

on:
  push:
    branches:
      - main

jobs:
  hugo-deploy:
    permissions:
      id-token: write
      contents: read
    uses: nuvibit/github-workflows/.github/workflows/hugo-deploy.yml@v2
    with:
      hugo_dry_run: false
      hugo_target_name: "aws"
      hugo_version: "0.136.5"
      hugo_extended: true
      aws_default_region: "eu-central-1"
      aws_oidc_role_arn: "arn:aws:iam::ACCOUNT:role/hugo-oidc-deployment-role"
```

### NPM Frontend Deploy Workflow
**Purpose**: Deploy NPM-based frontend applications to AWS S3 + CloudFront

**Features**:
- NPM build process
- S3 sync with change tracking
- CloudFront cache invalidation (configurable: ALL, UPDATED, NONE)
- OIDC authentication with AWS
- Automatic CloudFront distribution detection

**Usage**:
```yaml
name: NPM FRONTEND DEPLOYMENT

on:
  push:
    branches:
      - main

jobs:
  npm-frontend-deploy:
    permissions:
      id-token: write
      contents: read
    uses: nuvibit/github-workflows/.github/workflows/npm-frontend-deploy.yml@v2
    with:
      aws_default_region: "eu-central-1"
      aws_oidc_role_arn: "arn:aws:iam::ACCOUNT:role/oidc-deployment-role"
      domain_name: "docs.nuvibit.com"
      local_build_dir: "build"
      s3_deployment_dir: "build"
      cloudfront_invalidate_paths: "ALL"
```

### Draw.io Export Workflow
**Purpose**: Automatically export Draw.io diagrams to PNG format and commit back to repository

**Features**:
- Exports all Draw.io files to PNG format
- High-quality export settings (scale: 2, quality: 95%)
- Automatic commit back to main branch
- Concurrent workflow management

**Usage**:
```yaml
name: DRAW.IO EXPORT

on:
  push:
    paths:
      - '**.drawio'

jobs:
  drawio-export:
    uses: nuvibit/github-workflows/.github/workflows/drawio-export.yml@v2
    with:
      drawio_path: "."
```

### License Update Workflow
**Purpose**: Automatically update license files and headers across repositories

**Features**:
- Pulls license file from a template repository
- Updates license headers in specified file types
- GitHub App authentication for cross-repository access
- Automatic commit and push to PR branch

**Usage**:
```yaml
name: LICENSE UPDATE

on:
  pull_request:
    branches:
      - main

jobs:
  license-update:
    uses: nuvibit/github-workflows/.github/workflows/license-update.yml@v2
    with:
      license_file_source_repo: "ntc-module.tpl"
      license_file_name: "LICENSE"
      company_name: "Nuvibit AG"
      insert_header_file_extension: ".tf"
    secrets:
      GH_APP_ID: ${{ secrets.GH_APP_ID }}
      GH_APP_PRIVATE_KEY: ${{ secrets.GH_APP_PRIVATE_KEY }}
```

## 🔧 **Common Features**

All workflows in this collection share these characteristics:

- **🔐 Secure Authentication**: OIDC for AWS, GitHub Apps for repository access
- **⚡ Efficient Execution**: Optimized for performance and resource usage
- **🛡️ Best Practices**: Following GitHub Actions and cloud security best practices
- **📊 Comprehensive Logging**: Detailed output and error reporting
- **🔄 Reusable Design**: Easy integration across multiple repositories

## 🔗 **Related Collections**

- **[github-terraform-workflows](https://github.com/nuvibit/github-terraform-workflows)** - Terraform/OpenTofu specific workflows
- **[github-tflint-config](https://github.com/nuvibit/github-tflint-config)** - TFLint configuration for Terraform projects
- **[github-terratest-config](https://github.com/nuvibit/github-terratest-config)** - Terratest configuration for module testing

<!-- AUTHORS -->
## Authors
This collection is maintained by [Nuvibit](https://nuvibit.com) with help from [these amazing contributors](https://github.com/nuvibit/github-terraform-workflows/graphs/contributors)

<!-- LICENSE -->
## License
This collection is licensed under Apache 2.0
<br />
See [LICENSE](https://github.com/nuvibit/github-terraform-workflows/tree/master/LICENSE) for full details

<!-- COPYRIGHT -->
<br />
<br />
<p align="center">Copyright &copy; Nuvibit AG</p>