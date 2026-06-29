Great. We'll make this like **Senior Azure DevOps Engineer Interview Notes** (8–12 YOE level).

---

# Azure Landing Zone Interview Handbook

# Part 1 — Landing Zone, Management Groups, Subscription & Resource Organization (Q1–Q23)

---

# 1. What is an Azure Landing Zone?

### Interview Answer

An **Azure Landing Zone** is a **pre-configured, secure, scalable, and governed Azure environment** that provides the foundational infrastructure required before deploying business workloads.

It implements Microsoft Cloud Adoption Framework (CAF) best practices and includes:

* Identity
* Networking
* Governance
* Security
* Monitoring
* Connectivity
* Resource Organization

Instead of creating resources manually, Landing Zone provides a standardized platform where application teams can deploy workloads consistently.

### Key Components

```
Management Groups
       │
Subscriptions
       │
Hub Network
       │
Policies
       │
RBAC
       │
Azure Monitor
       │
Security
```

### Interview One-liner

> "Azure Landing Zone is the enterprise-ready foundation that standardizes governance, security, networking, and operations before onboarding workloads."

---

# 2. Why do organizations implement Landing Zones before workloads?

### Interview Answer

Without a Landing Zone, every application team creates resources differently, resulting in:

* No governance
* Security gaps
* Cost overruns
* Inconsistent networking
* Difficult compliance

Landing Zones solve these problems by enforcing enterprise standards from day one.

### Benefits

* Standardized deployments
* Consistent RBAC
* Central networking
* Cost management
* Compliance
* Secure connectivity
* Faster application onboarding

### Example

Instead of every team creating its own VNet:

```
Without Landing Zone

App1 --> Own VNet
App2 --> Own Firewall
App3 --> Own VPN

Messy
```

```
With Landing Zone

Hub Network
      │
 ┌────┴─────┐
App1 App2 App3
```

---

# 3. What are the core design areas of Azure Landing Zone?

Microsoft recommends **8 design areas**.

## 1. Identity

* Microsoft Entra ID
* RBAC
* PIM
* MFA

---

## 2. Resource Organization

* Management Groups
* Subscriptions
* Resource Groups

---

## 3. Network Topology

* Hub-Spoke
* ExpressRoute
* VPN
* DNS

---

## 4. Security

* Defender for Cloud
* NSGs
* Azure Firewall
* WAF
* Key Vault

---

## 5. Governance

* Azure Policy
* Blueprints (legacy)
* Tags
* Naming standards

---

## 6. Platform Automation

* Terraform
* Bicep
* Azure DevOps
* GitHub Actions

---

## 7. Management

* Azure Monitor
* Log Analytics
* Alerts
* Backup

---

## 8. Business Continuity

* Azure Backup
* Site Recovery
* Multi-region DR

---

### Interview Tip

Remember:

> **Identity → Network → Governance → Security → Operations**

---

# 4. Difference between Platform Landing Zone and Application Landing Zone

| Platform Landing Zone    | Application Landing Zone |
| ------------------------ | ------------------------ |
| Shared infrastructure    | Business application     |
| Managed by Platform Team | Managed by App Team      |
| Hub Network              | Spoke Network            |
| Firewall                 | VM/AKS/App Service       |
| Policies                 | Application resources    |
| Shared Services          | Business Logic           |

### Platform Includes

* Hub
* Firewall
* VPN
* DNS
* Log Analytics
* Key Vault
* Identity
* Policies

### Application Includes

* AKS
* VM
* SQL
* Storage
* App Service
* Function Apps

---

# 5. How do you design a production-grade Landing Zone?

### Interview Answer

I follow Microsoft's Cloud Adoption Framework.

### Step 1

Management Group hierarchy

```
Tenant

│

Platform

Landing Zones

Sandbox
```

---

### Step 2

Separate subscriptions

```
Platform

Connectivity

Management

Identity

Production

Non-Production

Sandbox
```

---

### Step 3

Deploy Hub Network

* Azure Firewall
* VPN
* ExpressRoute
* Bastion
* DNS

---

### Step 4

Create spoke VNets

One spoke per application/domain.

---

### Step 5

Governance

* Azure Policies
* RBAC
* Tags
* Budgets

---

### Step 6

Monitoring

* Azure Monitor
* Log Analytics
* Alerts
* Defender

---

### Step 7

Automation

Deploy everything using Terraform.

---

### Interview Summary

> Production Landing Zone = Governance + Identity + Hub Network + Security + Monitoring + Automation.

---

# 6. Benefits of Landing Zone

### Technical

✔ Standard architecture

✔ Faster deployments

✔ Reusable infrastructure

✔ Secure networking

✔ Central monitoring

---

### Business

✔ Reduced risk

✔ Easier audits

✔ Lower operational cost

✔ Faster project onboarding

✔ Better compliance

---

### Enterprise Benefit

A new application can be onboarded in **hours instead of weeks** because the platform already exists.

---

# 7. Challenges of implementing Landing Zones

### Common Challenges

* Designing Management Groups
* Subscription strategy
* IP address planning
* Policy conflicts
* RBAC complexity
* Legacy application migration
* Cost optimization
* Multi-region networking

### Interview Answer

The biggest challenge is balancing governance with developer agility. Overly restrictive policies can slow teams, while weak governance increases security and compliance risks.

---

# 8. Why do we use Management Groups?

Management Groups provide governance above subscriptions.

Instead of assigning policies individually:

```
Subscription A

Subscription B

Subscription C
```

You assign once:

```
Management Group

│

├── Subscription A

├── Subscription B

└── Subscription C
```

Policies automatically inherit.

### Used For

* RBAC
* Azure Policy
* Compliance
* Cost Governance

---

# 9. How would you structure Management Groups?

Example Enterprise

```
Tenant

│

├── Platform

│   ├── Identity

│   ├── Connectivity

│   └── Management

│

├── Landing Zones

│   ├── Production

│   ├── NonProduction

│   └── Shared Services

│

└── Sandbox
```

### Why?

Platform resources remain isolated from application subscriptions while governance flows from the top.

---

# 10. Difference between Subscription and Management Group

| Management Group     | Subscription                  |
| -------------------- | ----------------------------- |
| Governance container | Billing & deployment boundary |
| Holds subscriptions  | Holds resources               |
| Policies inherited   | Contains Resource Groups      |
| No billing           | Billing unit                  |
| RBAC inheritance     | Resource deployment           |

### Interview One-liner

> Management Groups organize subscriptions, while subscriptions organize resources.

---

# 11. How do Azure Policies inherit from Management Groups?

Policy assignment at a Management Group flows down automatically:

```
Management Group

↓

Subscription

↓

Resource Group

↓

Resource
```

Example:

Policy:

```
Allowed Region

India Central

India South
```

Every subscription under the Management Group must comply unless an exemption is granted.

---

# 12. How do you enforce governance across multiple subscriptions?

Use a combination of:

* Management Groups
* Azure Policy
* Policy Initiatives
* RBAC
* Tags
* Budgets
* Azure Monitor
* Defender for Cloud

### Production Example

Assign at the Management Group level:

* Allowed Locations
* Required Tags
* Disable Public IP
* Enforce Diagnostics
* Restrict VM Sizes

This ensures consistency across hundreds of subscriptions.

---

# 13. How would you separate Prod, Non-Prod, and Sandbox environments?

### Best Practice

```
Management Group

│

├── Production Subscription

├── NonProduction Subscription

└── Sandbox Subscription
```

### Why?

* Security isolation
* Separate RBAC
* Separate budgets
* Different policies
* Reduced blast radius

Never mix Production and Development workloads in the same subscription.

---

# 14. How many subscriptions would you create for a 5000-user enterprise?

There is no fixed number. It depends on business units, environments, compliance, and scale.

A typical enterprise design might include:

```
Platform
├── Identity
├── Connectivity
├── Management

Landing Zones
├── Prod-App1
├── Prod-App2
├── NonProd-App1
├── NonProd-App2
├── Shared Services
└── Sandbox
```

For a 5000-user organization, **20–100+ subscriptions** is common, depending on the number of applications and business domains.

---

# 15. What factors determine subscription boundaries?

* Environment (Prod/Non-Prod)
* Business Unit
* Cost ownership
* Compliance requirements
* Quotas and limits
* Geographic location
* Security isolation
* Operational ownership

### Rule

A subscription should represent a logical administrative and billing boundary, not just an application.

---

# 16. Why should Production and Non-Production be in separate subscriptions?

Reasons:

* Separate RBAC
* Separate budgets
* Different policies
* Independent quotas
* Security isolation
* Reduced blast radius
* Easier auditing

### Interview One-liner

> Separate subscriptions reduce operational risk and allow environment-specific governance.

---

# 17. What are subscription limits and quotas?

Azure subscriptions have limits on resources, for example:

* vCPU quotas
* Public IP limits
* VNet limits
* Route table limits
* Storage account limits (service-specific)
* API request throttling

### Best Practice

Monitor quota usage and request increases before large deployments to avoid failures.

---

# 18. How do you handle subscription sprawl?

* Define subscription creation standards.
* Automate provisioning with Terraform/Bicep.
* Use Management Groups for organization.
* Apply naming conventions.
* Assign budgets and cost centers.
* Periodically review and retire unused subscriptions.
* Use Azure Policy to enforce baseline configurations.

---

# 19. How do you design Resource Groups in a Landing Zone?

### Principles

* Group resources with the same lifecycle.
* Keep related application components together.
* Separate shared infrastructure from application resources.
* Avoid mixing production and non-production.

### Example

```
RG-Network-Hub
RG-Identity
RG-Monitoring
RG-App1-Prod
RG-App1-Dev
RG-App2-Prod
```

---

# 20. What naming conventions would you implement?

### Pattern

```
<Org>-<Environment>-<Region>-<Workload>-<ResourceType>-<Instance>
```

### Examples

```
contoso-prod-eastus-vnet-01
contoso-prod-eastus-vm-01
contoso-dev-centralindia-aks-01
```

### Best Practices

* Use lowercase where required.
* Keep names predictable.
* Include environment and region.
* Follow Azure naming constraints.

---

# 21. What tagging strategy would you recommend?

Mandatory tags:

| Tag                | Purpose                      |
| ------------------ | ---------------------------- |
| Environment        | Prod/Dev/Test                |
| Application        | Application name             |
| BusinessUnit       | Owning department            |
| CostCenter         | Chargeback/showback          |
| Owner              | Team or contact              |
| Project            | Project identifier           |
| Criticality        | High/Medium/Low              |
| DataClassification | Public/Internal/Confidential |

Enforce tags using Azure Policy to ensure consistency.

---

# 22. How do tags help in governance and cost management?

Tags enable:

* Cost allocation by team or project
* Chargeback/showback
* Resource ownership tracking
* Automation (e.g., backups based on tags)
* Compliance reporting
* Inventory management

Example: Filter monthly costs by `CostCenter=Finance` to generate department-specific reports.

---

# 23. What tags are mandatory in enterprise environments?

A common mandatory baseline:

| Tag                | Example         |
| ------------------ | --------------- |
| Environment        | Prod            |
| CostCenter         | FIN-1001        |
| Owner              | PlatformTeam    |
| Application        | InternetBanking |
| BusinessUnit       | RetailBanking   |
| Project            | CoreBanking     |
| Criticality        | High            |
| DataClassification | Confidential    |
| Backup             | Enabled         |
| DR                 | Yes             |

### Interview Tip

> Mandatory tags should be enforced through Azure Policy using **Deny** or **Modify** effects to prevent non-compliant resource creation.

---

## 📌 Part 1 Revision (30-Second Recap)

* **Landing Zone**: Enterprise-ready Azure foundation with governance, networking, identity, security, and monitoring.
* **Management Groups**: Governance layer above subscriptions; policies and RBAC inherit downward.
* **Subscriptions**: Administrative, billing, and isolation boundaries.
* **Production Design**: Separate Platform, Prod, Non-Prod, and Sandbox subscriptions.
* **Resource Groups**: Organize resources by lifecycle and ownership.
* **Naming**: Standardized format including organization, environment, region, workload, and resource type.
* **Tags**: Essential for governance, automation, ownership, and cost management; enforce with Azure Policy.
