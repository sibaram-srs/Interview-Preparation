# Senior Azure DevOps Engineer Interview Questions & Answers
# File 2 – YAML Pipelines, Azure Landing Zone, Governance & Security

---

# 1. Which pipeline are you using in your current project? Why is YAML Pipeline mostly used compared to Classic Pipeline?

## Answer

In my current project, we primarily use **Azure DevOps YAML Pipelines**.

YAML pipelines are preferred because they support **Infrastructure as Code (IaC)** principles, are stored in Git, and allow pipeline changes to be version-controlled just like application code.

### Advantages of YAML Pipelines

- Pipeline as Code
- Version controlled in Git
- Supports reusable templates
- Easy code review through Pull Requests
- Supports branching strategies
- Easier automation and maintenance
- Better scalability across multiple projects

### Classic Pipeline Limitations

- GUI-based configuration
- Difficult to track changes
- Limited reusability
- Hard to maintain for enterprise-scale projects

### Comparison

| YAML Pipeline | Classic Pipeline |
|---------------|------------------|
| Stored in Git | Stored in Azure DevOps UI |
| Version Control | No Version Control |
| Supports Templates | Limited |
| Easy Code Review | Difficult |
| Enterprise Standard | Mostly Legacy |

### Interview Highlight

> "In enterprise environments, YAML pipelines are preferred because they are version-controlled, reusable, and align with Infrastructure as Code practices."

---

# 2. Can you describe the template concept in a YAML Pipeline?

## Answer

A **YAML Template** is a reusable pipeline definition that allows multiple teams or projects to share the same pipeline logic.

Instead of duplicating build or deployment steps, we define them once and reuse them.

---

## Project Structure

```text
azure-pipelines.yml

templates/
│
├── build.yml
├── test.yml
├── deploy.yml
├── terraform-plan.yml
└── terraform-apply.yml
```

---

## Main Pipeline

```yaml
trigger:
- main

stages:

- template: templates/build.yml

- template: templates/deploy.yml
```

---

## Build Template

```yaml
steps:

- task: Maven@4

- task: PublishBuildArtifacts@1
```

---

## Template with Parameters

```yaml
parameters:

- name: environment
  type: string

steps:

- script: echo Deploying to ${{ parameters.environment }}
```

---

## Benefits

- Reusable code
- Easier maintenance
- Standardized deployments
- Reduced duplication
- Centralized updates

### Interview Highlight

> "Templates help us follow the DRY (Don't Repeat Yourself) principle by centralizing common build and deployment logic."

---

# 3. Multiple teams are using the same build process. How will you avoid code duplication?

## Answer

I would create **reusable YAML templates** and maintain them in a central repository.

---

## Example Structure

```text
Shared Repository

templates/

├── build.yml
├── deploy.yml
├── security-scan.yml
└── docker-build.yml
```

Each application pipeline references these templates.

---

## Application Pipeline

```yaml
resources:
  repositories:
  - repository: templates
    type: git
    name: DevOps/Templates

stages:

- template: build.yml@templates

- template: deploy.yml@templates
```

---

## Benefits

- Single source of truth
- Consistent pipelines
- Easier updates
- Better governance

### Interview Highlight

> "If we need to change the build process, we update the template once, and all consuming pipelines automatically benefit."

---

# 4. Explain Azure Landing Zone Hierarchy.

## Answer

Azure Landing Zone provides a secure and governed Azure environment following Microsoft's Enterprise Scale Architecture.

---

## Hierarchy

```text
Tenant Root Management Group
│
├── Platform
│   ├── Identity Subscription
│   ├── Connectivity Subscription
│   └── Management Subscription
│
├── Landing Zones
│   ├── Production Subscription
│   ├── Non-Production Subscription
│   └── Development Subscription
│
└── Sandbox Subscription
```

---

## Platform Subscriptions

### Identity

- Azure AD DS (if required)
- Domain Controllers
- Identity Services

### Connectivity

- Hub Network
- Azure Firewall
- VPN Gateway
- ExpressRoute
- Bastion

### Management

- Azure Monitor
- Log Analytics
- Defender for Cloud
- Backup
- Automation

---

## Workload Subscriptions

Host application resources:

- AKS
- App Services
- SQL Database
- Storage Accounts
- Virtual Machines

---

### Interview Highlight

> "The Landing Zone establishes governance, security, networking, identity, and operational standards before any workloads are deployed."

---

# 5. What do you mean by Hub and Spoke Architecture?

## Answer

Hub-and-Spoke is Microsoft's recommended enterprise networking model for Azure.

---

## Architecture

```text
                On-Premises
                     │
          VPN / ExpressRoute
                     │
                Hub VNet
     ┌──────────┼──────────┐
     │          │          │
Firewall     Bastion      DNS
     │
──────────────┼────────────────
              │
      VNet Peering
              │
   ┌──────────┴──────────┐
   │                     │
Prod Spoke           Dev Spoke
   │                     │
 AKS                 App Service
 SQL                 Storage
```

---

## Hub Contains

- Azure Firewall
- VPN Gateway
- Bastion
- DNS
- Shared Services

---

## Spokes Contain

- Application workloads
- Databases
- AKS
- Storage
- App Services

---

## Benefits

- Centralized security
- Network isolation
- Easier management
- Shared connectivity
- Cost optimization

### Interview Highlight

> "The Hub provides shared networking and security services, while each Spoke isolates application workloads."

---

# 6. What do you mean by Azure Policies? Which policies are you using?

## Answer

Azure Policy helps enforce organizational standards and governance by evaluating Azure resources for compliance.

---

## Common Policies We Use

### Allowed Locations

Restricts deployments to approved Azure regions.

Example:

```text
Allowed Regions:

Central India

East US

West Europe
```

---

### Required Tags

Enforces mandatory tags such as:

- Environment
- Owner
- CostCenter

---

### Allowed VM Sizes

Restricts developers from deploying expensive VM SKUs.

---

### Require HTTPS

Ensures Storage Accounts and App Services use HTTPS only.

---

### Deploy Diagnostic Settings

Automatically enables Azure Monitor diagnostics.

---

## Policy Effects

| Effect | Purpose |
|----------|----------|
| Deny | Blocks deployment |
| Audit | Reports non-compliance |
| Modify | Updates resource properties |
| Append | Adds missing properties |
| DeployIfNotExists | Deploys required resources |

---

### Interview Highlight

> "We assign policies at the Management Group level to ensure consistent governance across all subscriptions."

---

# 7. What is Azure Key Vault? What is its use?

## Answer

Azure Key Vault is a managed service used to securely store and manage sensitive information.

---

## Stores

- Secrets
- Passwords
- Certificates
- Encryption Keys

---

## Common Use Cases

- Database passwords
- API Keys
- Storage Account Keys
- SSL Certificates
- Service Principal Secrets

---

## Access

Applications retrieve secrets securely using:

- Managed Identity
- Service Principal
- RBAC
- Access Policies

---

## Benefits

- Centralized secret management
- Automatic key rotation
- Audit logging
- Secure access
- Integration with Azure services

---

### Interview Highlight

> "Instead of storing secrets in code or pipelines, we retrieve them dynamically from Azure Key Vault using Managed Identity."

---

# 8. What do you mean by RBAC? Explain it.

## Answer

RBAC (Role-Based Access Control) controls who can perform actions on Azure resources.

---

## Components

```text
Security Principal
        │
Role Assignment
        │
Scope
```

---

## Built-in Roles

- Owner
- Contributor
- Reader
- User Access Administrator

---

## Scope Levels

```text
Management Group

↓

Subscription

↓

Resource Group

↓

Resource
```

---

## Principle of Least Privilege

Users receive only the permissions necessary to perform their job.

---

### Interview Highlight

> "We always follow the Principle of Least Privilege and avoid granting excessive permissions."

---

# 9. A developer asks for Owner access in Production. What will you do?

## Answer

I would **not** grant Owner access directly.

---

## Steps

1. Understand the business requirement.
2. Verify whether Contributor or Reader access is sufficient.
3. Follow the organization's approval process.
4. Obtain approval from the Application Owner and Security Team.
5. Assign the minimum required role.
6. If temporary elevated access is needed, use Azure Privileged Identity Management (PIM).

---

## Best Practice

- Avoid permanent Owner access.
- Use Just-In-Time (JIT) access through PIM.
- Audit all privileged access.

---

### Interview Highlight

> "Production Owner access should be exceptional, time-bound, and approved through governance processes."

---

# 10. Explain each component of Azure Landing Zone.

## Answer

### Management Groups

Provide governance across multiple subscriptions.

---

### Subscriptions

Logical boundary for billing and resource isolation.

---

### Azure Policy

Enforces compliance and governance.

---

### RBAC

Controls access to Azure resources.

---

### Networking

- Hub-and-Spoke
- VNet
- NSGs
- Azure Firewall
- VPN Gateway
- ExpressRoute

---

### Identity

- Microsoft Entra ID
- Managed Identity
- Azure AD DS (if required)

---

### Management

- Azure Monitor
- Log Analytics
- Backup
- Defender for Cloud
- Automation

---

### Security

- Azure Policy
- RBAC
- Key Vault
- Private Endpoints
- Defender for Cloud

---

### Interview Highlight

> "The Landing Zone provides a secure, scalable, and governed foundation before deploying any application workloads."

---

# 11. Developers are creating resources in random Azure regions. How will you control it?

## Answer

I would implement an **Azure Policy** using the built-in **Allowed Locations** policy.

---

## Steps

1. Create the Allowed Locations policy.
2. Specify approved regions.
3. Assign the policy at the Management Group level.
4. Set the effect to **Deny**.

---

## Example

Allowed Regions:

```text
Central India

East US

West Europe
```

Any deployment outside these regions is automatically blocked.

---

### Interview Highlight

> "Assigning the policy at the Management Group ensures that every subscription inherits the restriction automatically."

---

# 12. The security team wants common security policies across all subscriptions. What will you do?

## Answer

I would assign Azure Policies at the **Management Group** level.

---

## Architecture

```text
Management Group

│

├── Production Subscription

├── Non-Production Subscription

└── Sandbox Subscription
```

All subscriptions inherit the assigned policies.

---

## Common Enterprise Policies

- Allowed Locations
- Mandatory Tags
- Allowed VM Sizes
- Require Encryption
- Enable Diagnostic Settings
- Restrict Public IPs
- Require HTTPS
- Enforce Private Endpoints

---

## Benefits

- Centralized governance
- Consistent compliance
- Reduced manual effort
- Automatic inheritance
- Easier auditing

---

### Interview Highlight

> "Management Groups are the preferred scope for enterprise-wide governance because policies assigned there automatically cascade to all child subscriptions."

---

# Senior Interview Tips

Always emphasize:

- Azure Landing Zone
- Hub-and-Spoke Architecture
- Azure Policy
- RBAC
- Key Vault
- Least Privilege Access
- Management Groups
- Governance
- Infrastructure as Code (Terraform)
- Security by Design

These concepts demonstrate enterprise-scale Azure architecture knowledge and are commonly expected from Senior Azure DevOps Engineers.
