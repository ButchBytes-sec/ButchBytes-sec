# AZ-900 Cheat Sheet

A quick-reference glossary of concepts likely to appear on the exam. Skim a section whenever you have a few spare minutes.

## 1. Cloud Concepts

- **CapEx vs OpEx**: CapEx = upfront spending on physical infrastructure. OpEx = pay-as-you-go operating expense (the cloud model).
- **Consumption-based model**: You pay only for what you use, when you use it.
- **High availability**: System stays up and accessible with minimal downtime.
- **Scalability**: Vertical = bigger machine (more CPU/RAM). Horizontal = more machines (scale out/in).
- **Elasticity**: Resources automatically grow or shrink based on demand.
- **Agility**: Ability to quickly provision and deprovision resources.
- **Fault tolerance**: System keeps running even if a component fails.
- **Disaster recovery**: Restoring service after a major outage or disaster.
- **IaaS**: Rent infrastructure (VMs, storage, networking) — most control, most management.
- **PaaS**: Rent a platform to build/deploy apps — Microsoft manages the OS/runtime.
- **SaaS**: Fully managed software you just use (e.g., Microsoft 365).
- **Public cloud**: Resources owned/run by a third-party provider, shared infrastructure.
- **Private cloud**: Dedicated infrastructure for a single organization.
- **Hybrid cloud**: Mix of public + private/on-premises.

## 2. Core Azure Architecture

- **Region**: A geographic area with one or more datacenters.
- **Region pair**: Two regions in the same geography paired for disaster recovery.
- **Availability Zone**: Physically separate datacenters within a region, each with independent power/cooling/networking.
- **Availability Set**: Logical grouping of VMs within a datacenter to avoid a single point of failure.
- **Resource Group**: A container that holds related Azure resources.
- **Subscription**: Billing and access boundary; contains resource groups.
- **Management Group**: Organizes multiple subscriptions for governance at scale.
- **Tenant**: An organization's dedicated instance of Azure AD (Entra ID).
- **Azure Resource Manager (ARM)**: The deployment/management layer — all requests (portal, CLI, SDK) go through it.

## 3. Compute, Networking & Storage

- **Virtual Machine (VM)**: On-demand, scalable IaaS compute resource.
- **VM Scale Set**: Group of identical, load-balanced VMs that auto-scale.
- **App Service**: PaaS for hosting web apps/APIs without managing servers.
- **Azure Container Instances (ACI)**: Run containers without managing VMs.
- **Azure Kubernetes Service (AKS)**: Managed Kubernetes for container orchestration.
- **Azure Functions**: Serverless, event-driven compute — pay only when code runs.
- **Virtual Network (VNet)**: Private network for Azure resources to communicate.
- **VNet Peering**: Connects two VNets so resources can communicate directly.
- **VPN Gateway**: Encrypted connection between Azure and on-premises over the public internet.
- **ExpressRoute**: Private, dedicated connection between on-premises and Azure (not over public internet).
- **Load Balancer**: Distributes traffic across VMs at the network layer (Layer 4).
- **Application Gateway**: Web traffic load balancer with URL routing, SSL termination (Layer 7).
- **Azure DNS**: Hosts and resolves domain names.
- **Azure CDN**: Caches content closer to users to reduce latency.
- **Blob Storage**: Unstructured data (images, video, backups, logs).
- **File Storage**: Managed file shares accessible via SMB/NFS.
- **Queue Storage**: Messages between application components.
- **Table Storage**: NoSQL key-value structured data.
- **Disk Storage**: Persistent storage for VMs.
- **Storage tiers**: Hot (frequent access), Cool (infrequent, 30+ days), Archive (rare, hours to retrieve).
- **Redundancy**: LRS (one datacenter), ZRS (across zones), GRS (paired region, not readable), RA-GRS (paired region, readable).

## 4. Identity & Security

- **Azure AD (Microsoft Entra ID)**: Cloud-based identity and access management service.
- **Authentication**: Proving who you are (login).
- **Authorization**: Determining what you're allowed to do.
- **MFA (Multi-Factor Authentication)**: Requires 2+ verification methods.
- **Conditional Access**: Policies that grant/block access based on conditions (location, device, risk).
- **RBAC (Role-Based Access Control)**: Assigns permissions to users based on role, scoped to a resource/group/subscription.
- **Zero Trust model**: "Never trust, always verify" — assume breach, verify explicitly, least privilege.
- **Defense in depth**: Layered security controls (physical, identity, network, app, data).
- **Network Security Group (NSG)**: Filters inbound/outbound traffic to resources.
- **Azure Firewall**: Managed, cloud-based network security service.
- **DDoS Protection**: Mitigates distributed denial-of-service attacks.
- **Microsoft Defender for Cloud**: Security posture management and threat protection.
- **Microsoft Sentinel**: Cloud-native SIEM for security analytics and threat detection.
- **Key Vault**: Securely stores secrets, keys, and certificates.

## 5. Management & Governance

- **Azure Policy**: Enforces organizational rules/compliance on resources (e.g., "only allow VMs in East US").
- **Azure Blueprints**: Packages policies, roles, and resource templates for repeatable environment setup.
- **Resource Locks**: Prevent accidental deletion or modification (CanNotDelete, ReadOnly).
- **Tags**: Name/value pairs for organizing and tracking resource costs/ownership.
- **Azure Cost Management**: Monitors and analyzes spending.
- **Azure Advisor**: Personalized recommendations for cost, security, reliability, performance.
- **Azure Monitor**: Collects/analyzes telemetry (metrics, logs) from resources.
- **Azure Service Health**: Alerts about Azure service issues affecting your resources.

## 6. Pricing & Support

- **Pricing Calculator**: Estimates cost of Azure services before deploying.
- **TCO Calculator**: Compares cost of on-premises vs. running the same workload in Azure.
- **Free Account**: Limited free services + credit for 30 days.
- **Support Plans**: Basic (free) → Developer → Standard → Professional Direct → Premier (fastest response, dedicated support).
- **SLA (Service Level Agreement)**: Microsoft's guaranteed uptime/performance commitment.
- **Service lifecycle**: Private Preview → Public Preview → General Availability (GA).

---
*Tip: if two terms still feel blurry after a read-through (e.g., NSG vs Firewall, or Policy vs RBAC), that's your cue to dig into Microsoft Learn's section on it again.*
