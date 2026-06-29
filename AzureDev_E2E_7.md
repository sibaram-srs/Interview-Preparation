# Azure Landing Zone Interview Notes (Q108–Q125)

# Monitoring, Observability, Security, Compliance, Backup & Disaster Recovery

---

# Q108. What monitoring components are mandatory in a Landing Zone?

## Answer

A production Azure Landing Zone should include centralized monitoring and observability from day one.

### Core Components

| Component                         | Purpose                                  |
| --------------------------------- | ---------------------------------------- |
| Azure Monitor                     | Collects metrics and logs                |
| Log Analytics Workspace           | Central log storage and querying         |
| Application Insights              | Application performance monitoring (APM) |
| Azure Monitor Alerts              | Proactive alerting                       |
| Diagnostic Settings               | Send resource logs to Log Analytics      |
| Microsoft Sentinel (Optional/SOC) | SIEM and threat detection                |
| Storage Account                   | Long-term log retention                  |
| Event Hub                         | Stream logs to external SIEM             |

### Architecture

```text
Azure Resources
      │
Diagnostic Settings
      │
Azure Monitor
      │
Log Analytics Workspace
      │
Alerts / Dashboards / Sentinel
```

### Best Practices

* Central Log Analytics Workspace
* Enable Diagnostic Settings for all resources
* Monitor platform + application logs
* Configure action groups for alerts

### Interview Tip

> Monitoring is not optional in production. Every critical resource should send logs and metrics to a centralized Log Analytics Workspace.

---

# Q109. Azure Monitor vs Log Analytics?

## Answer

Although closely related, they serve different purposes.

| Azure Monitor             | Log Analytics           |
| ------------------------- | ----------------------- |
| Monitoring platform       | Log storage & analytics |
| Collects metrics and logs | Stores and queries logs |
| Creates alerts            | Uses KQL for analysis   |
| Dashboards                | Troubleshooting         |

### Flow

```text
Azure Resource
      │
Azure Monitor
      │
Metrics + Logs
      │
Log Analytics Workspace
      │
KQL Queries
```

### Example

Azure Monitor:

* CPU > 80%
* Disk utilization
* Memory usage

Log Analytics:

* Failed logins
* NSG flow logs
* Firewall logs
* VM events

### Easy Interview Line

> Azure Monitor collects telemetry, while Log Analytics stores and analyzes logs using Kusto Query Language (KQL).

---

# Q110. What data should be collected centrally?

## Answer

Enterprise Landing Zones should centralize operational, security, and compliance logs.

### Infrastructure Logs

* VM logs
* Storage logs
* Key Vault logs
* SQL logs
* Network logs

### Security Logs

* Azure AD Sign-ins
* Defender alerts
* PIM activity
* RBAC changes
* Policy violations

### Network Logs

* NSG Flow Logs
* Azure Firewall logs
* Application Gateway logs
* VPN Gateway logs
* ExpressRoute metrics

### Application Logs

* Exceptions
* Response times
* API failures
* Dependency tracking

### Best Practice

Retain logs based on compliance requirements (e.g., 90 days, 1 year, or longer).

---

# Q111. How do you monitor network traffic?

## Answer

Network monitoring combines Azure-native tools for visibility into east-west and north-south traffic.

### Services

* Azure Network Watcher
* NSG Flow Logs
* Azure Firewall Logs
* Traffic Analytics
* Connection Monitor
* Packet Capture (when needed)

### Architecture

```text
VNets
 │
NSG Flow Logs
 │
Log Analytics
 │
Traffic Analytics
```

### What to Monitor

* Allowed/Denied traffic
* Top talkers
* Latency
* Packet loss
* Routing issues

### Best Practices

* Enable NSG Flow Logs.
* Monitor firewall rules.
* Set alerts for unusual traffic patterns.

---

# Q112. How do you monitor security events?

## Answer

Security monitoring should be centralized using Microsoft Defender for Cloud and Microsoft Sentinel.

### Sources

* Azure AD Sign-ins
* Defender Alerts
* Azure Firewall
* Key Vault
* Azure Policy
* RBAC Changes
* PIM Activations

### Flow

```text
Azure Resources
      │
Security Logs
      │
Log Analytics
      │
Microsoft Sentinel
      │
Incident Response
```

### Best Practices

* Enable Defender plans.
* Integrate Sentinel.
* Create automated incident playbooks.
* Monitor privileged account activity.

---

# Q113. How do you create enterprise alerting?

## Answer

Enterprise alerting should be actionable, prioritized, and routed to the correct teams.

### Components

* Azure Monitor Alerts
* Action Groups
* Email
* SMS
* Teams
* Webhooks
* ITSM integration

### Alert Types

| Severity | Example                  |
| -------- | ------------------------ |
| Critical | Production VM Down       |
| High     | Firewall Failure         |
| Medium   | CPU > 80%                |
| Low      | Storage nearing capacity |

### Best Practices

* Avoid alert fatigue.
* Define severity levels.
* Use dynamic thresholds where appropriate.
* Integrate with ticketing systems.

---

# Q114. What is Microsoft Defender for Cloud?

## Answer

Microsoft Defender for Cloud is Azure's cloud security posture management (CSPM) and cloud workload protection platform (CWPP).

It provides:

* Security recommendations
* Vulnerability assessments
* Threat detection
* Regulatory compliance
* Secure Score
* Just-In-Time VM Access
* Attack path analysis

### Features

* VM Protection
* SQL Protection
* Storage Protection
* Kubernetes Protection
* Containers Protection

### Best Practice

Enable Defender plans for production workloads.

### Interview Tip

> Defender for Cloud continuously assesses resources, provides recommendations, and detects active threats.

---

# Q115. How do you measure security posture?

## Answer

Security posture is measured using multiple indicators.

### Key Metrics

* Microsoft Secure Score
* Regulatory Compliance Dashboard
* Azure Policy Compliance
* Defender Recommendations
* Vulnerability Assessments
* Identity Risk
* Patch Compliance

### Dashboard

```text
Resources
      │
Defender for Cloud
      │
Secure Score
Compliance
Recommendations
```

### Best Practice

Track Secure Score trends rather than focusing on a single number.

---

# Q116. What is Secure Score?

## Answer

Secure Score is a percentage-based metric provided by Defender for Cloud that measures how well Azure resources follow Microsoft's security best practices.

### Example

Recommendations:

* Enable MFA
* Enable Defender
* Restrict public access
* Encrypt storage
* Enable backups

Implementing recommendations increases Secure Score.

### Important Note

Secure Score is a guidance metric—not a compliance certification.

---

# Q117. How do you implement CIS compliance?

## Answer

CIS (Center for Internet Security) Benchmarks define security best practices for Azure.

### Implementation Steps

1. Enable Defender for Cloud.
2. Assign CIS Regulatory Compliance Initiative.
3. Deploy Azure Policies.
4. Monitor compliance reports.
5. Remediate non-compliant resources.

### Common Controls

* MFA
* Logging
* Encryption
* Least Privilege
* Network Security
* Backup

### Best Practice

Automate remediation where possible using Azure Policy.

---

# Q118. How do you implement Zero Trust Architecture?

## Answer

Zero Trust follows the principle:

> **Never Trust, Always Verify**

### Pillars

* Verify Identity
* Least Privilege Access
* Assume Breach

### Azure Services

| Requirement       | Azure Service         |
| ----------------- | --------------------- |
| Identity          | Microsoft Entra ID    |
| MFA               | Conditional Access    |
| Privileged Access | PIM                   |
| Secrets           | Key Vault             |
| Network           | NSGs + Azure Firewall |
| Governance        | Azure Policy          |
| Threat Detection  | Defender for Cloud    |

### Best Practices

* Enforce MFA.
* Use Managed Identities.
* Disable public endpoints where possible.
* Continuously monitor and validate access.

---

# Q119. What controls are mandatory in production?

## Answer

A production Landing Zone should include mandatory governance and security controls.

### Identity

* MFA
* PIM
* Conditional Access

### Governance

* Azure Policy
* RBAC
* Management Groups
* Tagging

### Network

* Azure Firewall
* NSGs
* DDoS Protection
* Private Endpoints

### Monitoring

* Azure Monitor
* Log Analytics
* Alerts
* Defender for Cloud

### Backup

* Azure Backup
* Azure Site Recovery
* Immutable backups (where applicable)

### DevSecOps

* IaC Scanning
* Secret Management
* Approval Gates
* Security Testing

---

# Q120. What backup strategy would you recommend?

## Answer

Use a layered backup strategy based on business criticality.

### Components

* Azure Backup
* Recovery Services Vault
* SQL Backups
* Storage Snapshots
* VM Backups
* Geo-Redundant Storage (GRS)

### Best Practices

* Daily incremental backups.
* Weekly full backups (where applicable).
* Long-term retention for compliance.
* Encrypt backup data.
* Regularly test restores.

---

# Q121. What is Azure Site Recovery (ASR)?

## Answer

Azure Site Recovery (ASR) provides disaster recovery by replicating workloads to another Azure region or from on-premises to Azure.

### Workflow

```text
Primary Region
      │
Continuous Replication
      │
Secondary Region
      │
Failover
```

### Features

* VM replication
* Planned failover
* Unplanned failover
* Failback
* Recovery Plans

### Use Case

Protect mission-critical applications against regional outages.

---

# Q122. RTO vs RPO?

## Answer

| Metric                         | Meaning                      |
| ------------------------------ | ---------------------------- |
| RTO (Recovery Time Objective)  | Maximum acceptable downtime  |
| RPO (Recovery Point Objective) | Maximum acceptable data loss |

### Example

Banking System

* RTO = 30 minutes
* RPO = 5 minutes

This means:

* Service must recover within 30 minutes.
* Maximum of 5 minutes of data can be lost.

### Interview Tip

> RTO measures **time to recover**, while RPO measures **acceptable data loss**.

---

# Q123. How would you design DR for critical applications?

## Answer

For critical workloads, use a multi-region architecture with automated failover.

### Components

* Availability Zones
* Azure Site Recovery
* Geo-Redundant Storage
* SQL Geo-Replication
* Traffic Manager or Front Door
* Backup Strategy

### Architecture

```text
Region A
      │
Replication
      │
Region B
      │
Automatic/Manual Failover
```

### Best Practices

* Define RTO/RPO.
* Replicate databases.
* Test failover regularly.
* Document recovery procedures.

---

# Q124. Active-Active vs Active-Passive DR?

## Answer

| Active-Active              | Active-Passive            |
| -------------------------- | ------------------------- |
| Both regions serve traffic | One region serves traffic |
| Higher availability        | Simpler architecture      |
| Higher cost                | Lower cost                |
| Faster failover            | Slightly slower failover  |

### Use Cases

**Active-Active**

* Banking
* E-commerce
* Global SaaS

**Active-Passive**

* Internal business apps
* Medium-criticality workloads

### Best Practice

Choose based on business SLA and budget.

---

# Q125. How do you test disaster recovery?

## Answer

Disaster recovery testing validates that recovery procedures actually work.

### Steps

1. Schedule DR drills.
2. Execute test failover.
3. Validate application functionality.
4. Measure actual RTO and RPO.
5. Document results.
6. Perform failback.
7. Update runbooks.

### Best Practices

* Test at least annually (or more frequently for critical systems).
* Automate recovery plans where possible.
* Include application owners in testing.
* Review lessons learned after each exercise.

### Interview Tip

> A DR plan is only effective if it is tested regularly. Untested recovery procedures cannot be assumed to work.

---

# Senior Architect Quick Revision

* **Azure Monitor** collects metrics and logs; **Log Analytics** stores and analyzes logs with KQL.
* **Mandatory Monitoring**: Azure Monitor, Log Analytics, Application Insights, Alerts, Diagnostic Settings, and Defender for Cloud.
* **Network Monitoring**: Network Watcher, NSG Flow Logs, Traffic Analytics, and Azure Firewall logs.
* **Security Monitoring**: Defender for Cloud + Microsoft Sentinel with centralized logging.
* **Enterprise Alerting**: Severity-based alerts using Action Groups integrated with email, Teams, SMS, or ITSM.
* **Defender for Cloud** improves security posture through recommendations, threat detection, and Secure Score.
* **Secure Score** measures alignment with Microsoft's security best practices but is not a compliance certification.
* **CIS Compliance** is implemented using Azure Policy initiatives and continuous compliance monitoring.
* **Zero Trust**: Verify explicitly, enforce least privilege, and assume breach.
* **Production Controls**: MFA, PIM, Azure Policy, Firewall, Private Endpoints, Monitoring, Backup, and DevSecOps.
* **Backup Strategy**: Azure Backup with Recovery Services Vault, encryption, retention, and regular restore testing.
* **Azure Site Recovery** provides regional disaster recovery with replication and failover.
* **RTO** = Recovery Time Objective; **RPO** = Recovery Point Objective.
* **Critical DR Design**: Multi-region architecture with replication, geo-redundant storage, and tested failover plans.
* **Active-Active** offers maximum availability, while **Active-Passive** is simpler and more cost-effective.
* **DR Testing** should be scheduled regularly, validated against RTO/RPO targets, and followed by documented improvements.
