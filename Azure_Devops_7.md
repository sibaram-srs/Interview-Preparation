# Senior Azure DevOps Engineer Interview Questions & Answers
# File 3 – GitHub, Automation, Python & Enterprise GitHub Actions + Terraform

---

# 1. Explain Pull Request (PR) in GitHub.

## Answer

A **Pull Request (PR)** is a GitHub feature that allows developers to propose changes from one branch into another (usually `main` or `develop`) for review before merging.

It helps teams collaborate by enabling:

- Code Review
- Automated Validation
- CI/CD Checks
- Security Scanning
- Approval Workflow
- Merge Control

---

## Pull Request Workflow

```text
Developer

│

Creates Feature Branch

│

Makes Code Changes

│

Git Push

│

Create Pull Request

│

Automated CI Pipeline

│

Code Review

│

Approval

│

Merge into Main Branch

│

Deployment Pipeline
```

---

## Typical PR Checks

Before merging, the following checks are executed:

- Build Validation
- Unit Tests
- SonarQube Scan
- Secret Scanning
- Dependency Scan
- Docker Image Build
- Terraform Validation (Infrastructure Repositories)

---

## Interview Highlight

> "A Pull Request ensures that code is reviewed, validated, and tested before merging into the main branch, improving code quality and reducing production issues."

---

# 2. A Pull Request cannot be merged. How will you troubleshoot?

## Answer

I follow a structured troubleshooting approach.

---

## Step 1 – Check Merge Conflicts

```text
This branch has conflicts that must be resolved.
```

Resolve conflicts locally:

```bash
git checkout feature-branch

git pull origin main

git merge main

git push
```

---

## Step 2 – Review Required Checks

Verify if mandatory checks have passed:

- Build
- Unit Tests
- Security Scan
- Code Quality

---

## Step 3 – Required Reviewers

Many repositories require approvals.

Example:

```text
Required Reviewers = 2
```

Ensure approvals are completed.

---

## Step 4 – Branch Protection Rules

Check whether the repository enforces:

- Required approvals
- Passing status checks
- Signed commits
- Linear history

---

## Step 5 – Permissions

Verify:

- Write access
- Repository permissions
- Branch restrictions

---

## Step 6 – CI Pipeline Failures

If the pipeline failed:

- Review workflow logs
- Fix build/test issues
- Push updated commits

---

## Interview Highlight

> "I first check merge conflicts, required approvals, CI validation, and branch protection rules before investigating repository permissions."

---

# 3. Many teams are using the same GitHub workflow. How will you avoid duplication?

## Answer

I use **Reusable GitHub Workflows** or **Composite Actions**.

This follows the **DRY (Don't Repeat Yourself)** principle.

---

## Repository Structure

```text
.github

└── workflows

    ├── build.yml

    ├── deploy.yml

    └── reusable-build.yml
```

---

## Reusable Workflow

```yaml
name: Build

on:

  workflow_call:
```

---

## Calling Workflow

```yaml
jobs:

  build:

    uses: company/devops/.github/workflows/reusable-build.yml@main
```

---

## Benefits

- Standardized CI/CD
- Easier maintenance
- Centralized updates
- Reduced duplication

---

## Interview Highlight

> "Instead of maintaining the same workflow across multiple repositories, I centralize it as a reusable workflow and let each project consume it."

---

# 4. What are Python Modules?

## Answer

A **Python Module** is a file containing reusable Python code such as functions, classes, and variables.

Modules help organize code into logical units.

---

## Example

```python
# math_operations.py

def add(a, b):
    return a + b
```

Import:

```python
import math_operations

math_operations.add(5,2)
```

---

## Types

### Built-in Modules

- os
- sys
- json
- datetime
- math

---

### Third-party Modules

- requests
- pandas
- boto3
- azure-identity
- azure-mgmt-compute

---

## If You Haven't Worked on Python

A professional answer:

> "I haven't worked extensively with Python scripting in my current project. My primary automation experience is with Bash scripting, YAML pipelines, Terraform, and GitHub Actions. However, I'm familiar with Python modules and can read or modify automation scripts when required."

---

## Interview Highlight

Never say:

> "I don't know Python."

Instead say:

> "Python hasn't been my primary automation language, but I'm comfortable with Bash scripting, YAML, Terraform, and I'm currently learning Python for Azure automation."

---

# 5. Suppose you need to check the status of 100 Azure VMs every hour. How will you automate it?

## Answer

There are multiple enterprise approaches.

---

## Option 1 (Preferred)

Azure Automation Runbook

```text
Automation Account

↓

Runbook

↓

Authenticate

↓

Get VM Status

↓

Email/Teams Alert
```

Schedule:

```text
Every 1 Hour
```

---

## Option 2

Azure Logic Apps

Trigger every hour.

Check VM status.

Send notification.

---

## Option 3

Azure Functions

Timer Trigger

↓

Check VM Health

↓

Send Alert

---

## Option 4

Bash Script + GitHub Actions

Workflow:

```text
Schedule

↓

Azure Login

↓

Run Bash Script

↓

Check VM Status

↓

Email Report
```

---

## Bash Example

```bash
az vm list -d \
--query "[].{Name:name,Status:powerState}" \
-o table
```

---

## Azure CLI

```bash
az vm get-instance-view
```

---

## Interview Highlight

> "Although I haven't implemented this in Python, I would automate it using Azure Automation Runbooks or Azure CLI with Bash scripts, scheduled through GitHub Actions or Azure Automation."

---

# 6. Explain how you would deploy Azure infrastructure securely using GitHub Actions and Terraform in an Enterprise Environment.

## Answer

In enterprise environments, I follow a secure **Infrastructure as Code (IaC)** approach using GitHub Actions, Terraform, Azure Key Vault, and Azure Landing Zones.

---

# High-Level Architecture

```text
Developer

│

Git Push

│

GitHub Repository

│

Pull Request

│

GitHub Actions

│

Terraform Validate

│

Terraform Format

│

Terraform Plan

│

Security Scan

│

Approval

│

Terraform Apply

│

Azure Landing Zone

│

Azure Resources
```

---

# Step 1 – GitHub Repository Structure

```text
terraform/

modules/

network/

aks/

storage/

keyvault/

environments/

dev/

qa/

prod/

.github/

workflows/

terraform.yml
```

---

# Step 2 – Authentication

I never use hardcoded credentials.

Preferred authentication:

- OpenID Connect (OIDC) ⭐
- Managed Identity (where applicable)
- Azure Service Principal (fallback)

Secrets stored in:

- GitHub Secrets
- Azure Key Vault

---

# Step 3 – Workflow

```text
Developer Push

↓

Terraform Init

↓

Terraform Format

↓

Terraform Validate

↓

TFLint

↓

Checkov

↓

Terraform Plan

↓

Approval

↓

Terraform Apply

↓

Deployment Verification
```

---

# Step 4 – Security

Security controls include:

- Azure Policy
- RBAC
- Azure Key Vault
- Private Endpoints
- Microsoft Defender for Cloud
- Azure Monitor
- Log Analytics

---

# Step 5 – Remote State

Terraform State stored in:

```text
Azure Storage Account

↓

Blob Container

↓

terraform.tfstate
```

Enable:

- State Locking
- Blob Versioning
- RBAC
- Encryption

---

# Step 6 – Multi-Environment Deployment

```text
Git Branch

↓

Environment

↓

Variable File

↓

Terraform Backend

↓

Azure Subscription
```

Example:

| Branch | Environment |
|---------|-------------|
| develop | Dev |
| release | QA |
| main | Production |

---

# Step 7 – Approval Gates

Production requires:

- Terraform Plan Review
- Security Approval
- CAB Approval (if applicable)
- Manual Approval
- Terraform Apply

---

# Step 8 – Monitoring

Post-deployment:

- Azure Monitor
- Log Analytics
- Alerts
- Managed Prometheus
- Managed Grafana
- Application Insights

---

# Enterprise CI/CD Flow

```text
Developer

↓

GitHub

↓

Pull Request

↓

Code Review

↓

GitHub Actions

↓

Terraform Validate

↓

Terraform Plan

↓

Security Scan

↓

Approval

↓

Terraform Apply

↓

Azure Landing Zone

↓

Azure Resources

↓

Monitoring

↓

Production
```

---

# Best Practices

- Use reusable GitHub workflows.
- Authenticate with Azure using OIDC.
- Store secrets in Azure Key Vault or GitHub Secrets.
- Keep Terraform state remote in Azure Storage.
- Separate environments with different backends and variable files.
- Use Terraform modules for reusable infrastructure.
- Enable Azure Policy and RBAC for governance.
- Protect the `main` branch with required reviews and status checks.
- Integrate security scanning (e.g., Checkov, Trivy, CodeQL) into the pipeline.
- Configure approval gates before production deployments.
- Monitor deployments using Azure Monitor and Application Insights.

---

# Interview Scenario Question

### Interviewer:

**"How would you deploy infrastructure securely in an enterprise?"**

### Sample Answer

> "I would use GitHub Actions with Terraform to provision Azure resources through reusable modules. Authentication would be handled using OpenID Connect instead of long-lived credentials. Terraform state would be stored securely in an Azure Storage Account with RBAC, encryption, versioning, and state locking enabled. Secrets would be managed through Azure Key Vault or GitHub Secrets. Before deployment, the pipeline would run formatting, validation, linting, security scans, and `terraform plan`. Production deployments would require manual approval before `terraform apply`. Governance would be enforced using Azure Landing Zones, Azure Policy, RBAC, and monitoring through Azure Monitor and Log Analytics."

---

# Final Senior Azure DevOps Interview Tips

When answering architecture or design questions, consistently mention these enterprise practices:

- Azure Landing Zone
- Terraform Modules
- GitHub Actions / Azure DevOps YAML Pipelines
- OpenID Connect (OIDC)
- Azure Key Vault
- Azure Policy
- RBAC (Least Privilege)
- Azure Storage Remote State
- Branch Protection Rules
- Pull Request Reviews
- Security Scanning
- Approval Gates
- Hub-and-Spoke Networking
- Azure Monitor & Log Analytics
- Managed Prometheus & Grafana
- Microsoft Defender for Cloud

These topics demonstrate the mindset expected from a Senior Azure DevOps Engineer working in enterprise environments.
