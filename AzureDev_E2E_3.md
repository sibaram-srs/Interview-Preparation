Awesome! This section is **one of the highest-weight topics in Azure DevOps/Cloud Architect interviews**. Interviewers often ask scenario-based questions like:

* *How do you secure internet-facing applications?*
* *When do you use Azure Firewall vs NSG?*
* *Application Gateway vs Load Balancer?*
* *Design a secure banking application architecture.*

If you answer this part confidently, you'll stand out as a senior engineer.

---

# Azure Landing Zone Interview Handbook

# Part 3 – Azure Firewall, Application Gateway & Load Balancing (Q51–69)

---

# 51. Why use Azure Firewall in Landing Zones?

## Interview Answer

**Azure Firewall** is a **fully managed, stateful Layer 3–Layer 7 network security service** that provides centralized traffic inspection and control for Azure workloads.

Instead of configuring security separately in each VNet, Azure Firewall enforces a **single point of control** for inbound, outbound, and east-west traffic.

### Typical Architecture

```text
                 Internet
                     │
             Azure Firewall
                     │
          Hub Virtual Network
          ┌─────────┼─────────┐
          │         │         │
      Spoke-1   Spoke-2   Spoke-3
```

### Why Enterprises Use It

* Centralized security
* Stateful inspection
* Application rules (FQDN/URL)
* Network rules (IP/Port)
* DNAT
* Threat Intelligence
* High Availability (built-in)
* Autoscaling
* Central logging

### Interview One-Liner

> "Azure Firewall provides centralized network security for all workloads, eliminating the need to manage distributed firewalls across multiple VNets."

---

# 52. Azure Firewall vs NSG?

| Feature             | Azure Firewall                        | NSG                  |
| ------------------- | ------------------------------------- | -------------------- |
| Layer               | L3–L7                                 | L3–L4                |
| Stateful            | Yes                                   | Yes                  |
| Centralized         | Yes                                   | No                   |
| FQDN filtering      | Yes                                   | No                   |
| Threat Intelligence | Yes                                   | No                   |
| DNAT/SNAT           | Yes                                   | No                   |
| TLS Inspection      | Supported with Azure Firewall Premium | No                   |
| Best Use            | Enterprise network security           | Subnet/NIC filtering |

### Interview Answer

* **NSG** controls traffic entering and leaving a subnet or NIC.
* **Azure Firewall** provides centralized inspection, application filtering, logging, and advanced security features.

### Best Practice

Use both:

```text
Internet

↓

Firewall

↓

NSG

↓

Application
```

Firewall protects the network.

NSG protects individual subnets.

---

# 53. What are Firewall Policies?

Firewall Policy is a reusable configuration that contains:

* Network Rules
* Application Rules
* NAT Rules
* Threat Intelligence settings
* DNS settings
* TLS Inspection settings (Premium)

### Benefits

* Reusable
* Version-controlled
* Consistent across multiple firewalls
* Easier management

### Example

```text
Firewall Policy

├── Network Rules

├── Application Rules

├── DNAT Rules

└── Threat Intelligence
```

---

# 54. What is DNAT?

DNAT (**Destination Network Address Translation**) changes the **destination IP/Port** of incoming traffic.

Example:

```text
Internet

↓

20.x.x.x:443

↓

Firewall

↓

10.0.1.4:443
```

### Use Cases

* Publish internal web applications
* SSH/RDP access (restricted)
* Load-balanced applications

### Interview Tip

DNAT is primarily used for **inbound** traffic.

---

# 55. What is SNAT?

SNAT (**Source Network Address Translation**) changes the **source IP** of outbound traffic.

Example:

```text
VM

↓

Firewall

↓

Internet

Source becomes Firewall Public IP
```

### Why?

* Hide private IP addresses
* Centralize outbound identity
* Simplify firewall rules on external systems

---

# 56. How do you implement outbound Internet control?

### Best Practice

```text
VM

↓

Route Table (0.0.0.0/0)

↓

Azure Firewall

↓

Internet
```

### Steps

1. Deploy Firewall in Hub.
2. Create UDR.
3. Route all traffic to Firewall.
4. Allow only approved destinations using Application Rules.
5. Block all others.
6. Log traffic to Log Analytics.

### Benefits

* Prevent unauthorized Internet access
* Compliance
* Malware protection
* Audit trail

---

# 57. How do you enable Threat Intelligence mode?

Azure Firewall integrates with Microsoft's threat intelligence feeds.

### Modes

* **Off** – No threat filtering
* **Alert** – Log suspicious traffic
* **Alert and Deny** – Log and block malicious traffic

### Recommendation

Production:

```text
Threat Intelligence = Alert and Deny
```

This helps block known malicious IPs and domains.

---

# 58. How would you design a highly available firewall architecture?

Azure Firewall is highly available by design within a region.

For higher resilience:

```text
Internet

↓

Azure Front Door (optional for global apps)

↓

Regional Firewall

↓

Hub

↓

Spokes
```

### Best Practices

* Multiple Availability Zones
* Azure Firewall Policy
* Azure Monitor alerts
* Log Analytics
* ExpressRoute redundancy
* Regional disaster recovery

### Interview Tip

You don't deploy multiple firewalls in one region for HA—Azure Firewall provides built-in high availability. Multi-region resilience is achieved through regional deployments.

---

# 59. Why use Application Gateway?

Azure Application Gateway is a **Layer 7 load balancer**.

It understands:

* HTTP
* HTTPS
* URL paths
* Cookies
* Host headers

### Features

* SSL termination
* URL-based routing
* Cookie affinity
* Web Application Firewall (WAF)
* End-to-end TLS
* Path-based routing

Example:

```text
Internet

↓

Application Gateway

├── /api → API Servers

├── /images → Storage

└── /admin → Admin Servers
```

---

# 60. Application Gateway vs Azure Load Balancer?

| Azure Load Balancer | Application Gateway |
| ------------------- | ------------------- |
| Layer 4             | Layer 7             |
| TCP/UDP             | HTTP/HTTPS          |
| No WAF              | Supports WAF        |
| No URL routing      | URL-based routing   |
| No SSL termination  | SSL termination     |
| Best for VMs        | Best for Web Apps   |

### Interview Answer

* Use **Load Balancer** for network traffic.
* Use **Application Gateway** for web traffic.

---

# 61. What is WAF?

**Web Application Firewall (WAF)** protects web applications from Layer 7 attacks.

### Protects Against

* SQL Injection
* Cross-Site Scripting (XSS)
* Remote Code Execution patterns
* Command Injection
* HTTP Protocol violations
* OWASP Top 10 threats

### Architecture

```text
Internet

↓

Application Gateway + WAF

↓

Application
```

---

# 62. How do you protect applications from OWASP attacks?

### Recommended Stack

```text
Internet

↓

DDoS Protection

↓

Application Gateway WAF

↓

Azure Firewall

↓

NSG

↓

Application
```

Additional controls:

* TLS 1.2/1.3
* Secure headers
* Private Endpoints
* Microsoft Defender for Cloud
* Regular vulnerability scanning

---

# 63. WAF Detection vs Prevention mode?

### Detection

```text
Attack

↓

Log Only
```

No blocking.

Good for learning traffic patterns.

---

### Prevention

```text
Attack

↓

Blocked
```

Used in production after validating rules.

### Best Practice

* Development → Detection
* Production → Prevention

---

# 64. How would you publish an internal application securely?

Architecture:

```text
Internet
     │
Application Gateway + WAF
     │
Azure Firewall
     │
Private Load Balancer
     │
Application Servers
```

### Security Measures

* No Public IP on VMs
* Private Backend Pool
* TLS termination
* NSGs
* Azure Firewall
* Microsoft Entra ID authentication (where applicable)
* Private DNS
* Central monitoring

---

# 65. Azure Load Balancer vs Application Gateway?

| Feature         | Load Balancer | Application Gateway |
| --------------- | ------------- | ------------------- |
| Layer           | L4            | L7                  |
| Protocol        | TCP/UDP       | HTTP/HTTPS          |
| SSL Offload     | No            | Yes                 |
| URL Routing     | No            | Yes                 |
| Cookie Affinity | No            | Yes                 |
| WAF             | No            | Yes                 |

### Rule

* Network traffic → Load Balancer
* Web traffic → Application Gateway

---

# 66. Internal vs Public Load Balancer?

### Public Load Balancer

```text
Internet

↓

Public Load Balancer

↓

VMs
```

Used for public-facing services.

---

### Internal Load Balancer

```text
Internal Users

↓

Internal Load Balancer

↓

Backend Servers
```

Used for private workloads and multi-tier applications.

---

# 67. What are Health Probes?

Health Probes continuously check backend instance health.

Checks include:

* TCP
* HTTP
* HTTPS

Example:

```text
Load Balancer

↓

Health Probe

↓

VM1 ✅

VM2 ❌

VM3 ✅
```

Traffic is sent only to healthy instances.

---

# 68. What happens when a backend server fails?

Health probe detects failure.

```text
VM1 Healthy

VM2 Failed

VM3 Healthy
```

Load Balancer automatically removes **VM2** from rotation.

When it becomes healthy again, it is automatically added back.

### Interview Tip

This process is automatic—no manual intervention is required.

---

# 69. How would you design a highly available web application?

### Production Architecture

```text
                     Users
                       │
                Azure Front Door
                       │
             Application Gateway (WAF)
                       │
                Azure Firewall
                       │
             Internal Load Balancer
                       │
         ┌─────────────┴─────────────┐
         │                           │
     VM/AKS/App1                 VM/AKS/App2
         │                           │
         └─────────────┬─────────────┘
                       │
          Azure SQL / Cosmos DB / Storage
```

### Best Practices

* Multi-AZ deployment
* Application Gateway with WAF
* Azure Firewall
* Internal Load Balancer
* Autoscaling
* Health Probes
* Private Endpoints for PaaS
* Azure Monitor & Log Analytics
* Backup & Disaster Recovery
* Infrastructure as Code (Terraform/Bicep)

### Interview Summary

> "For a production web application, I use Azure Front Door for global entry and failover (if multi-region), Application Gateway with WAF for Layer 7 routing, Azure Firewall for centralized security, an Internal Load Balancer for backend distribution, deploy workloads across Availability Zones, and enable monitoring, autoscaling, backups, and Private Endpoints."

---

# 📝 Part 3 Revision (60-Second Cheat Sheet)

| Topic                   | Key Takeaway                                                                    |
| ----------------------- | ------------------------------------------------------------------------------- |
| Azure Firewall          | Centralized L3–L7 security, stateful inspection, DNAT/SNAT, Threat Intelligence |
| NSG vs Firewall         | NSG = subnet/NIC filtering, Firewall = centralized network security             |
| Firewall Policy         | Reusable rules for multiple firewalls                                           |
| DNAT                    | Publishes internal services by changing destination IP                          |
| SNAT                    | Changes source IP for outbound traffic                                          |
| Outbound Control        | UDR → Azure Firewall → Internet                                                 |
| Threat Intelligence     | Alert or Alert & Deny using Microsoft threat feeds                              |
| Application Gateway     | Layer 7 load balancer with SSL termination and URL routing                      |
| WAF                     | Protects against OWASP Top 10 attacks                                           |
| Detection vs Prevention | Detect = log only, Prevention = block attacks                                   |
| Load Balancer           | Layer 4 TCP/UDP load balancing                                                  |
| Internal LB             | Private backend workloads                                                       |
| Public LB               | Internet-facing services                                                        |
| Health Probes           | Remove unhealthy instances automatically                                        |
| HA Web App              | Front Door → App Gateway (WAF) → Firewall → Internal LB → Multi-AZ backend      |

---

## 🎯 Senior Interview Tip

A common production architecture answer that impresses interviewers is:

```text
Internet
   │
Azure Front Door (Global Entry + Failover)
   │
Application Gateway (WAF)
   │
Azure Firewall
   │
Internal Load Balancer
   │
AKS / App Service / VM Scale Set
   │
Private Endpoint
   │
Azure SQL / Storage / Key Vault
```

Mention **Availability Zones, Private Endpoints, centralized monitoring, Infrastructure as Code, and least-privilege RBAC** to demonstrate production-grade design thinking.
