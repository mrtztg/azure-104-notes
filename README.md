# ☁️ azure-104-notes
## 💰 Cost centre

### 💵 Setting Cost Budgets in Azure

To set cost budgets in Azure:

1. Navigate to **Cost Management + Billing** in the Azure portal.
2. Select **Budgets** from the left menu.
3. Click **+ Add** to create a new budget.
4. Configure the budget:
   - **Scope**: Select the subscription, resource group, or management group.
   - **Budget name**: Give it a descriptive name.
   - **Reset period**: Monthly, Quarterly, or Annually.
   - **Budget amount**: Set the spending limit.
5. Set **Alert conditions** (optional):
   - Configure alerts at different thresholds (e.g., 50%, 75%, 90%, 100%).
   - Add email recipients or action groups for notifications. In **Manage Action Groups**, you can define automated actions to take place when a budget threshold is reached (such as **running an automation**, **sending SMS**, **stop all VMs**, or **triggering webhooks**).
6. Click **Create** to save the budget.

Budgets help monitor and control spending by sending alerts and taking automated actions when thresholds are reached.

## 🏗️ Azure Concepts

> **Note**: Not everything in this section will appear on the AZ-104 exam. This is an overview of Azure services.

### 🖥️ Azure Virtual Machines (VMs)
- On-demand scalable compute resources
- Common uses: Windows Server, Linux VMs, jumpboxes, app servers

#### VM Abstractions (Services Built on VMs)
- **Azure Batch**: Large-scale parallel and high-performance computing (HPC) workloads
- **Virtual Machine Scale Sets (VMSS)**: Auto-scale and load-balance sets of identical VMs
- **Azure Kubernetes Service (AKS)**: Managed Kubernetes clusters orchestrating containerized applications (runs on VM node pools)
- **Service Fabric**: Platform for running distributed, scalable microservices on a VM infrastructure
- **App Services**:
  - Fully managed platform for hosting **Web Apps**, **API Apps**, and **Container Apps**
  - No direct remote desktop access to server VMs (PaaS)
  - Supports multiple languages: **.NET**, **Java**, **Node.js**, **Python**, **PHP**, **Ruby**, **Docker containers**
  - Key benefits:
    - Built-in scaling (manual or automatic)
    - Continuous Integration (CI) support (Azure DevOps, GitHub, etc.)
    - Deployment slots for staged rollouts and testing
    - Easy integration with **Visual Studio**
    - SSL support, custom domains, authentication, and diagnostics

### 🌐 Azure Networking

#### Microservices
- **Azure Container Apps**: Serverless container platform, auto-scales
- **API Management (APIM)**: API gateway and lifecycle management
- **Azure Container Instances (ACI)**: Serverless containers, no infrastructure management
- **Service Fabric**: Distributed microservices platform

#### Serverless/Event-Driven
- **Azure Functions**: Serverless event-driven code execution
- **Azure Logic Apps**: Serverless workflow automation

Four major categories:

#### Connectivity
- **Virtual Network (VNet)**: Isolated network environment in Azure
- **Virtual WAN**: Unified hub-and-spoke network connectivity
- **ExpressRoute**: Private, dedicated connection between on-premises and Azure
- **VPN Gateway**: Connects on-premises networks to Azure VNets (Site-to-Site or Point-to-Site)
- **Azure DNS**: DNS domain hosting and name resolution
- **Peering**: Connect VNets together

#### Security
- **Network Security Groups (NSGs)**: Firewall rules for subnet and NIC level traffic control
- **Private Link**: Private connectivity to Azure services via Private Endpoints
- **DDoS Protection**: Protection against distributed denial-of-service attacks
- **Azure Firewall**: Managed network security service with application and network-level filtering
- **Web Application Firewall (WAF)**: Protection for web applications (part of Application Gateway/Front Door)
- **VNet Service Endpoints**: Extends VNet identity to Azure services over optimized route
- **Azure Bastion**: Secure RDP/SSH access to VMs without public IPs

#### Delivery
- **Azure CDN**: Content delivery network for global content distribution
- **Azure Front Door**: Global entry point with WAF, SSL termination, and routing
- **Traffic Manager**: DNS-based traffic distribution across regions
- **Application Gateway**: Layer 7 load balancer with SSL termination, WAF, and URL-based routing
- **Azure Load Balancer**: Layer 4 load balancer for TCP/UDP traffic distribution

#### Monitoring
- **Network Watcher**: Network monitoring and diagnostics tool
- **Metrics and Logs**: Network performance and diagnostic logging
- **Packet Capture**: Capture network traffic for analysis

### 📦 Azure Storage
- Scalable from GBs to PBs (petabytes) for massive growth needs
- Storage types:
  - **Blob Storage**: Stores unstructured data (objects, files, images, backups)
  - **Azure Files**: Managed file shares (SMB/NFS protocol)
  - **Managed Disks**: Persistent storage for Azure VMs
  - **Table Storage**: NoSQL key-value storage
  - **Queue Storage**: Message queuing for distributed apps
- Available as **local** (regionally-redundant) or **global** (geo-redundant/global endpoints) storage
- Storage tiers for Blobs:
  - **Hot**: Frequent access, higher cost
  - **Cool**: Infrequent access, cheaper for storage
  - **Archive**: Rarely accessed, lowest cost, high retrieval time
- Managed vs. Unmanaged Disks:
  - **Managed Disks**: Azure handles storage accounts, high scalability/reliability, easier to manage
  - **Unmanaged Disks**: User manages storage accounts, legacy usage only

#### Data Services
- **SQL Services**:
  - **Azure SQL Database**: Managed relational database (PaaS), serverless and provisioned tiers
  - **Azure SQL Managed Instance**: Near 100% compatibility with SQL Server, VNet integration
  - **SQL Server on Azure VMs**: IaaS option for full SQL Server control
- **NoSQL Services**:
  - **Azure Cosmos DB**: Globally distributed, multi-model NoSQL database
  - **Azure Table Storage**: NoSQL key-value store (part of Azure Storage)
- **Azure Database for PostgreSQL Flexible Server**: Managed PostgreSQL with flexible compute and storage scaling, supports zone-redundant high availability
- **Azure Synapse Analytics**: Analytics service combining data warehousing and big data analytics (formerly SQL Data Warehouse)

## 💻 CLI and PowerShell

> **Exam Tip**: Leave enough time for performance-based CLI tasks in the exam.

- CLI and PowerShell commands follow understandable patterns
- **Azure CLI (Bash)**: `az <service> <action>` pattern
  - Examples: `az vm list`, `az vm create`, `az vm delete`
  - Examples: `az network vnet list`, `az network vnet create`, `az network vnet delete`
  - Examples: `az network vnet subnet list`, `az network vnet subnet create`
- **PowerShell**: Verb-Noun pattern (Get, New, Remove, Set)
  - Examples: `Get-AzVM`, `New-AzVM`, `Remove-AzVM`
  - Examples: `Get-AzVirtualNetwork`, `New-AzVirtualNetwork`, `Remove-AzVirtualNetwork`
  - Examples: `Get-AzVirtualNetworkNetworkSubnetConfig`, `New-AzVirtualNetworkSubnetConfig`

### PowerShell

#### Installation
- Install Az module: `Install-Module -Name Az -AllowClobber -Repository PSGallery -Force`
- Update module: `Update-Module -Name Az -AllowClobber -Repository PSGallery -Force`

#### Connection
- Connect to Azure: `Connect-AzAccount`
- Switch between subscriptions:
  - List subscriptions: `Get-AzSubscription`
  - Set context: `Set-AzContext -Subscription "subscription-id"`

## 🔐 Entra ID

### Basic Concepts of Accounts & Subscriptions

#### Account / User
- **Person**: User account with username, password, and MFA (multi-factor authentication)
- **App - Managed Identity**: Represents a program or service, used for authentication without storing credentials
- **User creation**: By default, new users are created with the format `username@XXX.onmicrosoft.com` (e.g., `johndoe@example.onmicrosoft.com`). To use a custom domain, add it from the **Custom Domain Names** menu in Entra ID before creating users. From the **Custom Domain Names** screen, you can make a custom domain primary. With a custom domain, users can be created as `username@yourdomain.com` (e.g., `johndoe@example.com`).

#### Tenant
- A representation of an organization
- Usually represented by a public domain name (e.g., `example.com`)
- Will be assigned a domain if not specified (e.g., `example.onmicrosoft.com`)
- A dedicated instance of Azure AD (now Entra ID)
- Every Azure account is part of at least one tenant
- Each tenant can have any number of subscriptions (even 0)
- More than one account can be the owner in a tenant
- **Creating a new tenant**: If your account is not a global administrator (e.g., on dev plans), create a new Entra ID tenant via **Entra ID dashboard → Manage Tenants → Create Microsoft Entra ID** to gain necessary access. Now, this tenant will be yours.
- **Switching tenants**: You can switch between tenants (e.g., from your company's tenant where you're a developer to your own tenant where you're a global administrator). Two methods:
  - Navigate to **Entra ID → Manage Tenants**, select the desired tenant, and click **Switch**
  - Click your **profile picture** in the right corner → **Switch directory** → click **Switch** on the desired directory (tenant)

#### Subscription
- An agreement with Microsoft to use Azure services and how to pay
- All Azure resource usage gets billed to the payment method of the subscription
- Types: Free subscription, Pay-As-You-Go (PAYG), Enterprise agreements

#### Resource Group
- Logical container for Azure resources
- Resources within a resource group share the same lifecycle
- Used for organizing, managing, and billing resources

![tenant-sub-resourcegroup](assets/tenant-sub-resourcegroup.jpg)

### Entra ID Management

#### Assigned Roles
- Access the **Assigned Roles** menu in Entra ID to view and manage role assignments for users and groups
- Shows all roles assigned to a user or group across the tenant
- Common roles include: Global Administrator, User Administrator, Application Administrator, and custom roles
- Used to verify permissions and troubleshoot access issues