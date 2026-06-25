# GitHub Actions Workflows

This directory contains automated workflows for the demo-workload repository.

## Available Workflows

### 1. YAML Syntax Check (`yaml-lint.yml`)

**Purpose**: Validates YAML syntax in all repository files

**Triggers**:
- Push to `main` branch
- Pull requests to `main` branch

**What it checks**:
- YAML syntax validity
- Indentation (2 spaces)
- Line length (max 120 characters)
- Key files: `catalog-info.yaml`, `deployment.yaml`, `service.yaml`, `route.yaml`, `namespace.yaml`, `mkdocs.yml`

**Tool**: [yamllint](https://github.com/adrienverge/yamllint)

---

### 2. OpenSSF Scorecard (`scorecard.yml`)

**Purpose**: Security and best practices analysis using OpenSSF Scorecard

**Triggers**:
- Push to `main` branch
- Weekly on Sundays at 01:30 UTC
- Manually via workflow dispatch
- Branch protection rule changes

**What it checks**:
- Security vulnerabilities
- Code review practices
- Dependency management
- CI/CD security
- Branch protection
- And 15+ other security checks

**Results**: 
- Published to GitHub Security tab (Code scanning alerts)
- Stored as artifacts for 30 days
- SARIF format for integration with security tools

**Tool**: [OpenSSF Scorecard](https://github.com/ossf/scorecard)

---

## Viewing Results

### YAML Lint Results
1. Go to **Actions** tab
2. Click on "YAML Syntax Check" workflow
3. View the latest run

### Scorecard Results
1. Go to **Security** tab
2. Click **Code scanning alerts**
3. Filter by tool: "OpenSSF Scorecard"

Or view in **Actions** tab > "OpenSSF Scorecard Analysis" workflow

---

## Configuration

Both workflows use standard configurations optimized for Kubernetes/OpenShift YAML files and security best practices.

### YAML Lint Config
- Indentation: 2 spaces
- Line length: 120 chars (warning)
- Ignores: `.github/`, `node_modules/`

### Scorecard Config
- Results format: SARIF
- Publish results: Disabled (set to `true` for public repos)
- Retention: 30 days

---

## Badges

Add these badges to your README.md:

```markdown
[![YAML Lint](https://github.com/cristianocasella/demo-app/actions/workflows/yaml-lint.yml/badge.svg)](https://github.com/cristianocasella/demo-app/actions/workflows/yaml-lint.yml)
[![OpenSSF Scorecard](https://github.com/cristianocasella/demo-app/actions/workflows/scorecard.yml/badge.svg)](https://github.com/cristianocasella/demo-app/actions/workflows/scorecard.yml)
```
