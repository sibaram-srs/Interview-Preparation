Excellent. This is the section where **Azure Architect/DevOps interviews (8–15 YOE)** spend the most time. If you answer these confidently, you're already covering a major portion of Azure infrastructure interviews.

---

# Azure Landing Zone Interview Handbook

# Part 2 — Hub-and-Spoke, Connectivity, Routing & Network Security (Q24–Q50)

---

# 24. What is Hub-and-Spoke Architecture?

## Interview Answer

Hub-and-Spoke is a **network topology** where shared infrastructure is deployed in a **central Hub VNet**, while applications are deployed in separate **Spoke VNets**.

Instead of every application managing its own networking, security, and connectivity, these services are centralized in the Hub.

### Architecture

```text
                 On-Premises
                      │
            ExpressRoute / VPN
                      │
              Azure Firewall
                      │
              Hub Virtual Network
        ┌──────────┼──────────┐
        │          │          │
     Spoke-1    Spoke-2    Spoke-3
     (App1)     (AKS)      (App3)
```

### Advantages

* Centralized security
* Easier governance
* Lower operational cost
* Shared connectivity
* Better scalability
* Reduced duplication

### Interview One-Liner

> "The Hub provides shared services, while each Spoke hosts isolated application workloads."

---

# 25. Why is Hub-and-Spoke preferred in Azure Landing Zones?

Because enterprise environments require centralized control.

Without Hub:

```text
App1 → VPN
App2 → Firewall
App3 → DNS
App4 → ExpressRoute
```

Lots of duplicated infrastructure.

With Hub:

```text
Hub
├── Firewall
├── VPN
├── ExpressRoute
├── Bastion
├── DNS
└── Monitoring

All applications consume shared services.
```

### Benefits

* Lower cost
* Easier management
* Consistent security
* Better compliance
* Central internet breakout

---

# 26. What resources are deployed in the Hub?

Typical Hub contains:

| Service                | Purpose               |
| ---------------------- | --------------------- |
| Azure Firewall         | Central security      |
| VPN Gateway            | Site-to-site VPN      |
| ExpressRoute Gateway   | Private connectivity  |
| Bastion                | Secure VM access      |
| Private DNS            | Name resolution       |
| Azure Monitor Agents   | Monitoring            |
| Route Tables           | Traffic routing       |
| DDoS Protection        | Network protection    |
| NAT Gateway (optional) | Outbound connectivity |

### Rule

**Only shared infrastructure belongs in the Hub.**

---

# 27. What resources are deployed in the Spokes?

Application-specific resources:

* AKS
* Virtual Machines
* App Services
* SQL Managed Instance
* Storage Accounts
* Key Vault
* Load Balancer
* Private Endpoints

### Rule

One Spoke generally represents:

* One application
* One business domain
* One environment

Example:

```text
Spoke-CRM
Spoke-SAP
Spoke-HR
Spoke-Ecommerce
```

---

# 28. How does VNet Peering work?

VNet Peering privately connects two VNets over the Microsoft backbone network.

Traffic:

* Never goes over the Internet
* Has low latency
* High bandwidth
* No VPN required

Example:

```text
Hub VNet
      │
 ┌────┴────┐
 │         │
App1     App2
```

### Characteristics

✔ Private IP communication

✔ No gateways required (unless gateway transit is used)

✔ Supports cross-subscription

✔ Supports cross-region (Global Peering)

---

# 29. What are VNet Peering limitations?

### Common limitations

* Non-transitive by default
* Cannot have overlapping IP ranges
* Requires explicit peering in both directions
* Charges apply for peering traffic
* Route propagation must be planned

### Interview Question

Can Spoke1 automatically reach Spoke2?

**Answer:**

No.

By default:

```text
Hub ↔ Spoke1

Hub ↔ Spoke2

Spoke1 ❌ Spoke2
```

Communication requires:

* Azure Firewall
* Azure Route Server
* Azure Virtual WAN
* Appropriate routing

---

# 30. What is Global VNet Peering?

Global VNet Peering connects VNets located in different Azure regions.

Example:

```text
East US Hub
        │
        │
West Europe Spoke
```

Uses Microsoft's backbone network.

### Benefits

* Low latency
* High bandwidth
* No VPN
* Supports DR architectures

---

# 31. How do you connect multiple spokes securely?

Best practice:

```text
              Firewall
                 │
          Hub Virtual Network
       ┌─────────┼─────────┐
       │         │         │
    Spoke1    Spoke2    Spoke3
```

Instead of direct Spoke-to-Spoke communication:

* Route traffic through Azure Firewall
* Apply NSGs
* Inspect traffic
* Log traffic
* Apply threat intelligence

### Interview Tip

Never fully mesh 100 VNets.

Use centralized routing.

---

# 32. How would you design networking for 100 applications?

### Recommended Architecture

```text
Hub

│

├── Firewall

├── VPN

├── ExpressRoute

├── Bastion

├── DNS

│

├── Spoke-1

├── Spoke-2

├── Spoke-3

...

└── Spoke-100
```

### Best Practices

* Hub-and-Spoke
* One VNet per application/domain where appropriate
* Non-overlapping CIDR ranges
* Centralized internet egress
* Private Endpoints
* Azure Firewall
* NSGs + ASGs
* Central DNS
* Infrastructure as Code

---

# 33. ExpressRoute vs VPN Gateway?

| Feature     | ExpressRoute                       | VPN Gateway                      |
| ----------- | ---------------------------------- | -------------------------------- |
| Connection  | Private                            | Internet                         |
| Speed       | Up to 100 Gbps (service-dependent) | Lower than ExpressRoute          |
| Reliability | High                               | Depends on Internet              |
| SLA         | Higher                             | Lower                            |
| Security    | Private connectivity               | Encrypted over public Internet   |
| Cost        | Higher                             | Lower                            |
| Enterprise  | Yes                                | Small/Medium workloads or backup |

### Interview Answer

Use VPN for smaller deployments or as a backup. Use ExpressRoute for mission-critical enterprise workloads requiring predictable performance and private connectivity.

---

# 34. When would you choose ExpressRoute?

Use when:

* Banking
* Healthcare
* Government
* SAP
* Large enterprise
* Low latency required
* Regulatory compliance
* High bandwidth
* Hybrid cloud

### Interview One-Liner

> ExpressRoute is the preferred choice for production enterprise connectivity because it provides private, predictable, and highly available network connectivity.

---

# 35. What is Forced Tunneling?

Forced tunneling routes all outbound traffic through on-premises or a centralized security appliance instead of allowing direct Internet access.

Example:

```text
VM

↓

Firewall

↓

On-Prem

↓

Internet
```

### Benefits

* Central inspection
* Compliance
* Logging
* DLP
* Enterprise security

---

# 36. How do you connect Azure to on-premises?

Options:

### Site-to-Site VPN

* Small offices
* Quick setup
* Internet-based

### ExpressRoute

* Enterprise
* Private MPLS/direct connectivity
* High availability

### Point-to-Site VPN

* Remote users
* Individual devices

### Virtual WAN

* Global branch connectivity
* Simplified large-scale networking

---

# 37. How would you design hybrid connectivity?

```text
Data Center

│

ExpressRoute

│

Hub VNet

├── Firewall

├── DNS

├── Bastion

│

Application Spokes
```

### Best Practices

* Primary: ExpressRoute
* Backup: VPN Gateway
* BGP enabled
* Private DNS integration
* Firewall inspection
* Redundant gateways

---

# 38. What is Route Propagation?

Azure automatically learns routes from gateways using BGP and propagates them to route tables unless disabled.

Example:

```text
On-Prem

↓

BGP

↓

ExpressRoute

↓

Azure Route Tables
```

Benefits:

* Dynamic routing
* Less manual configuration
* Easier failover

---

# 39. What is BGP and why is it used?

**Border Gateway Protocol (BGP)** exchanges routing information between networks.

Azure uses BGP with:

* ExpressRoute
* VPN Gateway

Advantages:

* Automatic route updates
* Redundancy
* Failover
* Dynamic routing

### Interview Tip

Without BGP, you would manage static routes manually, which is error-prone in large environments.

---

# 40. What is a User Defined Route (UDR)?

A UDR is a custom route that overrides Azure's default system routes.

Example:

Default:

```text
VM → Internet
```

UDR:

```text
VM

↓

Azure Firewall

↓

Internet
```

### Use Cases

* Firewall inspection
* Forced tunneling
* Custom appliances
* Network Virtual Appliances (NVAs)

---

# 41. When would you use custom routes?

Use UDRs when you need to:

* Force traffic through Azure Firewall
* Route via NVAs
* Control east-west traffic
* Implement forced tunneling
* Customize hybrid routing

---

# 42. What happens if multiple routes match?

Azure route selection priority:

1. Longest Prefix Match (most specific route)
2. User Defined Route (UDR)
3. BGP Route
4. System Route

### Example

```text
10.0.0.0/16

10.0.1.0/24
```

Azure selects:

```text
10.0.1.0/24
```

because it is more specific.

---

# 43. How does Azure route traffic by default?

Azure creates system routes automatically for:

* Local VNet traffic
* Internet
* Virtual Network Gateway
* Peered VNets

You only create UDRs when custom routing is required.

---

# 44. How would you force traffic through Azure Firewall?

Architecture:

```text
Subnet

↓

Route Table (UDR)

↓

Azure Firewall

↓

Internet
```

Steps:

1. Deploy Azure Firewall in the Hub.
2. Create a route table.
3. Add default route `0.0.0.0/0` with next hop = Azure Firewall.
4. Associate the route table with application subnets.
5. Configure Firewall rules (Network, Application, DNAT).

This ensures all outbound traffic is inspected.

---

# 45. Difference between NSG and ASG?

| NSG                        | ASG                          |
| -------------------------- | ---------------------------- |
| Filters traffic            | Groups NICs/VMs logically    |
| Applied to Subnet/NIC      | Referenced inside NSG rules  |
| Uses IPs, ports, protocols | Uses application group names |
| Security enforcement       | Simplifies rule management   |

### Example

Instead of allowing traffic to IPs:

```text
10.1.1.4
10.1.1.5
10.1.1.6
```

Use:

```text
ASG-WebServers
```

---

# 46. Where should NSGs be applied?

Best practice:

* **Subnet level**: Common security baseline for all resources.
* **NIC level**: Exceptional cases requiring VM-specific rules.

Typical approach:

```text
VNet
└── Subnet
      ├── NSG (baseline)
      ├── VM1
      └── VM2
```

Avoid unnecessary complexity by minimizing NIC-level NSGs.

---

# 47. What are best practices for NSG design?

* Follow least privilege.
* Deny by default (Azure has default deny inbound).
* Use ASGs instead of IP addresses.
* Document all rules.
* Remove unused rules.
* Use service tags (e.g., AzureLoadBalancer, Storage).
* Avoid overly broad rules like `Any -> Any`.

---

# 48. How do ASGs simplify security management?

Application Security Groups allow you to define rules based on application roles instead of IP addresses.

Example:

```text
ASG-Web
ASG-App
ASG-DB
```

Rule:

```text
ASG-Web → ASG-App → TCP 443
```

Benefits:

* Easier maintenance
* No IP updates when VMs change
* Cleaner NSG rules
* Better readability

---

# 49. How do you secure East-West traffic?

East-West traffic is communication between workloads inside Azure.

Best practices:

* Segment applications into separate subnets/VNets.
* Use NSGs and ASGs to restrict lateral movement.
* Route sensitive traffic through Azure Firewall using UDRs.
* Use Private Endpoints for PaaS services.
* Enable Microsoft Defender for Cloud recommendations.
* Log traffic with NSG Flow Logs (or equivalent network visibility solutions where applicable).

### Goal

Prevent unauthorized communication between internal workloads.

---

# 50. How do you secure North-South traffic?

North-South traffic is communication between Azure and external networks (Internet or on-premises).

Recommended architecture:

```text
Internet
    │
Azure DDoS Protection
    │
Application Gateway (WAF)
    │
Azure Firewall
    │
Spoke VNet
    │
Application
```

Security controls:

* Azure DDoS Protection
* Web Application Firewall (WAF)
* Azure Firewall
* NSGs
* Private Endpoints (where applicable)
* TLS/HTTPS
* Threat Intelligence mode
* Central logging and monitoring

### Interview One-Liner

> "North-South traffic is protected with layered security—DDoS, WAF, Firewall, and NSGs—while East-West traffic is segmented and controlled using NSGs, ASGs, UDRs, and Azure Firewall."

---

# 📝 Part 2 Revision (60-Second Cheat Sheet)

| Topic                | Key Takeaway                                                 |
| -------------------- | ------------------------------------------------------------ |
| Hub-and-Spoke        | Centralized shared services with isolated application VNets  |
| Hub Resources        | Firewall, VPN, ExpressRoute, Bastion, DNS, Monitoring        |
| Spoke Resources      | AKS, VMs, App Services, Databases, Storage                   |
| VNet Peering         | Private, low-latency connectivity; non-transitive            |
| Global Peering       | Connects VNets across Azure regions                          |
| ExpressRoute         | Private, enterprise-grade hybrid connectivity                |
| VPN Gateway          | Internet-based encrypted connectivity                        |
| Forced Tunneling     | Send all outbound traffic through a central inspection point |
| BGP                  | Dynamic route exchange for VPN/ExpressRoute                  |
| UDR                  | Custom routing to override Azure system routes               |
| Route Priority       | Longest Prefix → UDR → BGP → System                          |
| NSG                  | Packet filtering at subnet/NIC level                         |
| ASG                  | Logical grouping for simpler NSG rules                       |
| East-West Security   | Segmentation + NSGs + Firewall + Private Endpoints           |
| North-South Security | DDoS + WAF + Firewall + NSGs + Monitoring                    |
