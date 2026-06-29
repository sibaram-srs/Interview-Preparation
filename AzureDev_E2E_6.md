# Azure Landing Zone Interview Notes (Q96–Q107)

# Terraform (IaC) + CI/CD

---

# Q96. Why Terraform for Landing Zones?

## Answer

**Terraform** is the preferred Infrastructure as Code (IaC) tool for Azure Landing Zones because it enables **consistent, repeatable, and automated infrastructure provisioning** across multiple environments and cloud providers.

Instead of manually creating resources through the Azure Portal, infrastructure is defined in code and deployed through CI/CD pipelines.

## Benefits

* Infrastructure as Code
* Multi-cloud support (Azure, AWS, GCP)
* Version-controlled infrastructure
* Reusable modules
* State management
* Automated deployments
* Easy rollback through Git

## Enterprise Example

A Landing Zone may include:

* Management Groups
* Subscriptions
* VNets
* Azure Firewall
* Azure Policy
* RBAC
* Log Analytics
* Key Vault

Terraform provisions the entire platform from a single pipeline.

## Example

```text
Git
 ↓
Terraform Code
 ↓
Terraform Plan
 ↓
Approval
 ↓
Terraform Apply
 ↓
Azure Landing Zone
```

### Best Practices

* Store code in Git.
* Use reusable modules.
* Use remote state.
* Automate via Azure DevOps.
* Perform code reviews through Pull Requests.

### Interview Tip

> Terraform enables infrastructure to be deployed consistently, repeatedly, and auditable across all environments.

---

# Q97. How would you modularize Landing Zone deployment?

## Answer

Large Terraform projects should be divided into **reusable modules**, where each module manages a specific component.

## Example Structure

```text
terraform/
│
├── modules/
│   ├── networking/
│   ├── firewall/
│   ├── keyvault/
│   ├── monitoring/
│   ├── policy/
│   ├── identity/
│   └── storage/
│
├── dev/
├── test/
└── prod/
```

## Typical Modules

| Module     | Purpose                |
| ---------- | ---------------------- |
| Networking | VNet, Subnets, NSGs    |
| Firewall   | Azure Firewall         |
| Monitoring | Log Analytics, Alerts  |
| Identity   | RBAC, Managed Identity |
| Security   | Key Vault, Defender    |
| Governance | Azure Policy           |

## Benefits

* Reusability
* Easier maintenance
* Team ownership
* Independent testing
* Standardization

### Best Practice

Keep modules **small, reusable, and loosely coupled**.

---

# Q98. How do remote backends work?

## Answer

A **remote backend** stores the Terraform state file in a centralized and secure location.

For Azure, the recommended backend is **Azure Storage Account**.

## Architecture

```text
Terraform
      │
Terraform State
      │
Azure Storage Account
      │
Blob Container
```

## Benefits

* Shared state
* State locking
* Centralized storage
* Encryption
* Backup
* Versioning

## Backend Example

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "rg-tfstate"
    storage_account_name = "stterraformprod"
    container_name       = "tfstate"
    key                  = "prod.tfstate"
  }
}
```

## Best Practices

* Enable blob versioning.
* Enable soft delete.
* Use Private Endpoint.
* Restrict access using RBAC.
* Never store state locally in production.

---

# Q99. How do you manage multiple environments?

## Answer

Each environment should have its **own state file**, variables, and deployment pipeline.

## Recommended Structure

```text
terraform/
│
├── dev/
├── test/
├── uat/
└── prod/
```

Each environment has:

* Separate backend
* Separate tfvars
* Separate subscriptions (recommended)
* Separate approvals

## Example

```text
Dev
 └── dev.tfvars

Test
 └── test.tfvars

Prod
 └── prod.tfvars
```

### Best Practices

* Separate subscriptions.
* Separate state files.
* Separate pipelines.
* Separate service connections.
* Separate approvals.

---

# Q100. Terraform Workspace vs Separate State Files?

## Answer

### Terraform Workspace

One configuration with multiple logical state files.

Example:

```text
default
dev
test
prod
```

### Separate State Files

Each environment maintains an independent backend.

```text
dev.tfstate

test.tfstate

prod.tfstate
```

## Comparison

| Workspace      | Separate State      |
| -------------- | ------------------- |
| Easier setup   | Better isolation    |
| Shared backend | Separate backend    |
| Small projects | Enterprise projects |
| Higher risk    | Lower risk          |

## Recommendation

For enterprise Azure Landing Zones:

> Use **Separate State Files** instead of Workspaces.

Reason:

* Better isolation
* Better security
* Easier disaster recovery
* Cleaner access control

---

# Q101. How do you handle Terraform drift?

## Answer

Terraform drift occurs when infrastructure is modified outside Terraform.

Example:

Terraform creates an NSG.

Administrator manually changes NSG in Azure Portal.

Terraform state no longer matches Azure.

## Detection

```bash
terraform plan
```

Terraform compares:

State File

vs

Actual Azure Resources

## Resolution

* Import manually created resources.
* Remove manual changes.
* Apply Terraform again.
* Restrict portal changes through RBAC.

### Best Practices

* No manual production changes.
* All infrastructure through CI/CD.
* Regular drift detection.
* Review `terraform plan` before apply.

---

# Q102. How do you perform safe Terraform deployments?

## Answer

Production deployments should follow a controlled pipeline.

## Pipeline

```text
Developer
      │
Pull Request
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
```

## Validation Steps

* terraform fmt
* terraform validate
* terraform plan
* Checkov/tfsec
* Manual approval
* terraform apply

### Best Practices

* Never use auto-approve in production.
* Review the execution plan.
* Use deployment approvals.
* Keep state backups.
* Enable rollback through Git.

---

# Q103. How do you deploy infrastructure through Azure DevOps?

## Answer

Infrastructure is deployed using **Azure DevOps YAML Pipelines** integrated with Terraform.

## Pipeline Flow

```text
Git Commit
      │
Azure DevOps Pipeline
      │
Terraform Init
      │
Terraform Validate
      │
Terraform Plan
      │
Approval
      │
Terraform Apply
```

## Typical Stages

1. Checkout Code
2. Install Terraform
3. Authenticate using Service Connection
4. Terraform Init
5. Validate
6. Plan
7. Security Scan
8. Approval
9. Apply

## Best Practices

* Store code in Git.
* Use YAML pipelines.
* Use variable groups.
* Store secrets in Key Vault.
* Enable branch protection.

---

# Q104. How do you implement approvals in production?

## Answer

Production deployments should require **manual approvals** before execution.

## Azure DevOps Environment

```text
Dev
      │
Automatic

↓

Test
      │
Automatic

↓

Production
      │
Manual Approval
      │
Terraform Apply
```

## Approval Options

* Environment approvals
* CAB approval
* Manager approval
* Change ticket validation

## Best Practices

* Separate approvers from developers.
* Require at least one manual approval.
* Log approval history.
* Integrate with ITSM if required.

---

# Q105. What is GitOps?

## Answer

GitOps is an operational model where **Git is the single source of truth** for infrastructure and application deployments.

Any desired infrastructure state is defined in Git.

Changes occur only through Pull Requests.

## Workflow

```text
Git Repository
      │
Pull Request
      │
Approval
      │
Pipeline
      │
Azure
```

## Benefits

* Version control
* Audit trail
* Easy rollback
* Peer reviews
* Consistency
* Automation

## Example

Developer changes Terraform code.

Merge to main.

Pipeline deploys automatically.

Infrastructure matches Git.

---

# Q106. How would you deploy infrastructure across multiple subscriptions?

## Answer

Large enterprises usually have separate subscriptions for:

* Platform
* Production
* Non-Production
* Sandbox
* Shared Services

Terraform can deploy across multiple subscriptions using different provider configurations.

## Example

```text
Management Group
      │
 ├── Platform
 ├── Production
 ├── Non-Production
 └── Sandbox
```

Each subscription uses:

* Separate Service Connection
* Separate Backend
* Separate State File

## Best Practices

* Use Management Groups.
* Assign RBAC at subscription level.
* Isolate state files.
* Use reusable modules.

---

# Q107. How do you implement rollback strategies?

## Answer

Terraform itself does not provide a direct rollback command.

Rollback is achieved by reverting infrastructure code in Git and redeploying.

## Strategy

```text
Version 1
      │
Deploy

↓

Version 2
      │
Problem

↓

Git Revert

↓

Terraform Apply

↓

Infrastructure returns to Version 1
```

## Additional Strategies

* Keep previous Git tags.
* Backup Terraform state.
* Test in lower environments.
* Review execution plans.
* Blue-Green deployment for applications.
* Use feature flags for application changes.

## Best Practices

* Never modify production manually.
* Roll back through Git.
* Keep state version history.
* Validate rollback in non-production first.

---

# Senior Architect Quick Revision

* **Terraform** → Preferred IaC tool for Azure Landing Zones due to automation, consistency, and modularity.
* **Modules** → Organize infrastructure into reusable components (networking, security, monitoring, identity).
* **Remote Backend** → Store Terraform state securely in Azure Storage with RBAC, encryption, versioning, and state locking.
* **Multiple Environments** → Use separate subscriptions, state files, and pipelines for Dev, Test, UAT, and Prod.
* **State Files vs Workspaces** → Prefer separate state files in enterprise environments for better isolation.
* **Terraform Drift** → Detect using `terraform plan`; prevent by enforcing infrastructure changes through CI/CD.
* **Safe Deployments** → Validate, scan, review plan, require approvals, then apply.
* **Azure DevOps CI/CD** → Use YAML pipelines with Service Connections and Key Vault integration.
* **Production Approvals** → Require manual approvals through Azure DevOps Environments before deployment.
* **GitOps** → Git is the single source of truth; all infrastructure changes flow through Pull Requests.
* **Multi-Subscription Deployments** → Use Management Groups, separate providers, and isolated state files.
* **Rollback** → Revert Git changes and re-run Terraform; never rely on manual changes in production.
