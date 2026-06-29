# Senior Azure DevOps Engineer Interview Questions & Answers
# File 1 - Member 1 Questions

---

# 1. Tell me about yourself and your Roles & Responsibilities.

## Answer

"I have around **X years of experience** as an Azure DevOps Engineer. My primary responsibility is designing, automating, and maintaining secure cloud infrastructure and CI/CD pipelines on Azure.

Currently, my day-to-day responsibilities include:

- Developing and maintaining Azure DevOps and GitHub Actions pipelines
- Provisioning Azure infrastructure using Terraform
- Managing Azure Kubernetes Service (AKS)
- Building and pushing Docker images to Azure Container Registry (ACR)
- Implementing Azure Landing Zone governance
- Managing Azure Policies and RBAC
- Monitoring infrastructure using Azure Monitor, Log Analytics, Prometheus, and Grafana
- Supporting production deployments and troubleshooting deployment issues
- Working closely with developers, QA teams, and security teams."

---

# 2. What do you do at the end of your day? How do you close your tickets?

## Answer

At the end of my workday, I ensure that all assigned tasks are updated and documented.

Typical activities include:

- Update Jira/Azure DevOps work items
- Add implementation notes
- Attach evidence (pipeline screenshots, logs, PR links)
- Update deployment status
- Verify monitoring after deployment
- Handover any pending production issues
- Send daily status update to stakeholders
- Ensure code has been committed and PRs are raised

I never close a ticket without:

- Successful validation
- Peer review (if required)
- User/UAT confirmation
- Documentation updates

### Interview Highlight

> "I ensure every ticket has proper documentation, validation evidence, and stakeholder confirmation before closing it."

---

# 3. Explain the Terraform Lifecycle.

## Answer

Terraform follows a lifecycle to provision and manage infrastructure.

```text
Write Code
      │
terraform init
      │
terraform fmt
      │
terraform validate
      │
terraform plan
      │
Approval
      │
terraform apply
      │
Infrastructure Created
      │
terraform destroy (if required)
```

### Commands

### terraform init

- Downloads providers
- Configures backend
- Initializes modules

### terraform fmt

Formats Terraform code.

### terraform validate

Validates syntax.

### terraform plan

Shows proposed infrastructure changes.

### terraform apply

Creates or updates resources.

### terraform destroy

Deletes managed infrastructure.

### Interview Highlight

> "In production, we always review the Terraform plan and require approval before running `terraform apply`."

---

# 4. Terraform is Open Source. Can you name 2–3 other Open Source tools?

## Answer

Common open-source DevOps tools include:

| Tool | Purpose |
|------|----------|
| Jenkins | CI/CD Automation |
| Kubernetes | Container Orchestration |
| Docker | Containerization |
| Helm | Kubernetes Package Management |
| Prometheus | Monitoring |
| Grafana | Visualization |
| Argo CD | GitOps Deployments |
| Ansible | Configuration Management |
| SonarQube Community | Code Quality |
| Harbor | Container Registry |

### Interview Highlight

> "Our stack primarily includes Terraform, Kubernetes, Helm, Prometheus, Grafana, and Argo CD, all of which are widely adopted open-source tools."

---

# 5. What are the stages of a CI/CD Pipeline?

## Answer

A typical enterprise CI/CD pipeline consists of:

```text
Developer

↓

Git Push

↓

Build

↓

Unit Testing

↓

Static Code Analysis

↓

Security Scan

↓

Build Docker Image

↓

Push Image to ACR

↓

Terraform Plan

↓

Approval

↓

Terraform Apply

↓

Deploy to AKS

↓

Smoke Test

↓

Production
```

### CI Stage

- Checkout Code
- Build
- Unit Testing
- SonarQube
- Docker Build
- Push Artifact

### CD Stage

- Infrastructure Deployment
- Application Deployment
- Validation
- Monitoring
- Rollback (if required)

---

# 6. Build is successful but deployment failed. How will you troubleshoot?

## Answer

I follow a structured troubleshooting approach.

### Step 1

Check deployment logs.

Examples:

- Azure DevOps Logs
- GitHub Actions Logs

---

### Step 2

Identify failure point.

Examples:

- Authentication
- Infrastructure
- Kubernetes
- Application

---

### Step 3

Verify infrastructure.

```bash
terraform plan
```

or

```bash
kubectl get pods
```

---

### Step 4

Check Kubernetes.

```bash
kubectl get pods

kubectl describe pod

kubectl logs pod-name
```

---

### Step 5

Check Azure resources.

- Resource Group
- AKS
- ACR
- Key Vault
- Networking

---

### Step 6

Rollback if production impact exists.

---

### Interview Highlight

> "My first priority is restoring service by rolling back if needed, then performing root cause analysis to prevent recurrence."

---

# 7. Production deployment failed after release. What will you do?

## Answer

Production incidents should follow an Incident Management process.

### Immediate Actions

- Stop further deployments
- Notify stakeholders
- Check monitoring dashboards
- Identify impact

---

### Restore Service

Depending on deployment strategy:

- Rollback Deployment
- Blue-Green Switchback
- Canary Rollback

---

### Investigation

Review:

- Deployment Logs
- Application Logs
- Infrastructure Changes
- Recent PRs

---

### Root Cause Analysis

Document:

- Root Cause
- Resolution
- Preventive Actions

---

### Interview Highlight

> "Restoring production service is always my first priority. Root cause analysis comes after business continuity is ensured."

---

# 8. What is GitHub? Explain the components of GitHub Actions.

## Answer

GitHub is a Git-based platform for:

- Source Code Management
- Version Control
- CI/CD
- Collaboration
- Issue Tracking
- Pull Requests

---

## GitHub Actions Components

### Workflow

Main automation file.

```text
.github/workflows/build.yml
```

---

### Event

Triggers workflow.

Examples:

```yaml
push

pull_request

workflow_dispatch
```

---

### Job

Logical unit of work.

Example:

```yaml
jobs:

 build:

 deploy:
```

---

### Step

Individual task.

Example:

```yaml
- Checkout

- Build

- Test

- Deploy
```

---

### Runner

Executes workflow.

Examples:

- Ubuntu
- Windows
- Self-hosted Runner

---

### Action

Reusable automation.

Example:

```yaml
actions/checkout
```

---

## Workflow Flow

```text
Developer

↓

Git Push

↓

Workflow Trigger

↓

Runner

↓

Jobs

↓

Steps

↓

Deployment
```

---

# 9. Workflow is not triggering after code push. What will you check?

## Answer

I verify the following:

### Trigger

```yaml
on:

  push:

    branches:

      - main
```

---

### Branch Name

Ensure push is to the correct branch.

---

### Workflow Location

Must exist in:

```text
.github/workflows/
```

---

### YAML Syntax

Validate YAML formatting.

---

### GitHub Actions Enabled

Check repository settings.

---

### Workflow Permissions

Verify:

- Read permissions
- Workflow permissions

---

### Repository Settings

Check if Actions are disabled by organization policy.

---

### Interview Highlight

> "I first verify the trigger, branch, workflow location, and repository settings before investigating runner or permission issues."

---

# 10. Can we hardcode credentials in GitHub Actions?

## Answer

**No.**

Hardcoding credentials is a major security risk.

### Problems

- Credentials exposed in Git
- Difficult rotation
- Compliance violations
- Insider threats

---

## Best Practices

### GitHub Secrets

Store secrets securely.

Example:

```yaml
env:

 AZURE_CLIENT_ID: ${{ secrets.AZURE_CLIENT_ID }}
```

---

### Azure Key Vault

Production environments should retrieve secrets from Azure Key Vault.

---

### OpenID Connect (OIDC)

Preferred authentication method.

Benefits:

- No long-lived secrets
- Temporary tokens
- Better security

---

### Authentication Options

| Method | Recommended |
|---------|-------------|
| Hardcoded Password | ❌ No |
| GitHub Secrets | ✅ Yes |
| Azure Key Vault | ✅ Yes |
| OIDC | ⭐ Best Practice |

---

## Interview Highlight

> "In enterprise environments, we never hardcode credentials. We use GitHub Secrets or Azure Key Vault, and preferably OpenID Connect (OIDC) for passwordless authentication."

---

# Final Interview Tips

Always answer with:

- Security First
- Automation
- Infrastructure as Code
- Monitoring
- Rollback Strategy
- Documentation
- Root Cause Analysis

These are the qualities interviewers expect from a **Senior Azure DevOps Engineer**.
