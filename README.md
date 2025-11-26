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

### Account / User
- **Person**: User account with username, password, and MFA (multi-factor authentication)
  - Create a user from **Users** menu → **New User**
  - **User creation options** (radio group):
    - **Create user** (default): For users within your organization
      - By default, new users are created with the format `username@XXX.onmicrosoft.com` (e.g., `johndoe@example.onmicrosoft.com`)
      - To use a custom domain, add it from the **Custom Domain Names** menu in Entra ID before creating users
      - From the **Custom Domain Names** screen, you can make a custom domain primary
      - With a custom domain, users can be created as `username@yourdomain.com` (e.g., `johndoe@example.com`)
    - **Invite user**: For users whose email is not within your organization (e.g., contractors using personal accounts or external email addresses)
      - Useful when the user's email is not `@yourcompany.onmicrosoft.com` or `@yourcompany.com`
      - Most other options are similar to "Create user" (force MFA, using location, roles, groups, etc.)
  - New users have no permissions by default, only the ability to login to the Azure portal
- **Bulk Operations**: Available in multiple areas to perform bulk actions:
  - **Users menu**: Bulk create (download CSV template, fill with user data, and upload), bulk delete, bulk invite, download users
  - **Group members**: Import members, remove members, download members
  - Other areas may also have bulk operations
- **User Groups**: Groups are an organizational structure for putting users in. If you enable **"Azure AD roles can be assigned to the group"** during group creation, you can assign roles to the group (which will apply to all users in the group). By choosing **"Dynamic User"** in Membership Type during group creation, you can create dynamic groups with rules (e.g., if display name contains something, or if department is something, etc.)
- **App - Managed Identity**: Represents a program or service, used for authentication without storing credentials

### Tenant
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

### Subscription
- An agreement with Microsoft to use Azure services and how to pay
- All Azure resource usage gets billed to the payment method of the subscription
- Types: Free subscription, Pay-As-You-Go (PAYG), Enterprise agreements
- Every subscription can be assigned to ONLY one tenant, but each tenant cant have multiple subscriptions.
- To change tenant of a subscription, open the subscription, and use "Change directory" button on top.
- From **IAM** menu, we can define who can have access to this subscription (e.g., see invoices and billings, forecasts, etc.)
- From **Cost Management > Cost Analysis** menu, we can see details of the costs like which services cost how much
- **Cost Management > Cost Alert** is for setting up cost alerts. From top of the page, click **Add > Add Budget**
- There is also **Add Anomaly alert**, which is defined by Azure itself (e.g., for new charges that were not there before, or charges were there and now stopped, or significant changes on cost). Alert sent as email
- **Cost Management > Budgets**: Define budget, reset budget reset period, when to receive the alert (threshold percentage)
- **Cost Management > Advisor Recommendations**: Gives recommendations across number of categories like cost
- In **Settings > Usage + Quotas** menu, we can see different limitations of how much compute in different regions. Under "Adjustable" column, for the ones that are "yes", we can click on pen button and ask for change

### Resource
- An entity managed by Azure
- **Expected examples**: Virtual Machine (VM), web app, storage account
- **Unexpected examples**: Public IP address, network interface card (NIC), network security group (NSG)
- Accounts can be given read, update, and owner rights to resources

### Resource Group
- A way of organising resources in a subscription
- Acts as a folder structure for resources
- All resources must belong to only one resource group
- Resource groups can be deleted (which deletes the resources inside)
- A way to separate out projects, keeping unrelated things separate

![tenant-sub-resourcegroup](assets/tenant-sub-resourcegroup.jpg)

### Entra ID Management

#### Assigned Roles
- Access the **Assigned Roles** menu in Entra ID to view and manage role assignments for users and groups
- Shows all roles assigned to a user or group across the tenant
- Common roles include: Global Administrator, User Administrator, Application Administrator, and custom roles
- Used to verify permissions and troubleshoot access issues

#### Licenses and Paid Features
- Some Azure features are available only in paid plans (e.g. self-service password change)
- If you have a paid license (like P2), you can enable those paid features for specific users
- Better to enable only for users who need them, as paid features cost per person
- To view licenses: Navigate to **Entra ID → Licenses** menu
  - **All products**: Shows all available licenses
  - **Licensed Features**: Shows features included in each license
- To assign a license to users: **Users** menu → open user → **Licenses** (left menu) → **Assignments** (top) → assign licenses
- **Important**: If **"Using Location"** is not defined for the user, license assignment will fail
- To define Using Location: Open user → **Edit Properties** → change **Using Location** in the form

#### Password Reset (Self-Service Password Reset)
- **Password Reset** menu in Entra ID enables users to reset their passwords if forgotten or lost
- Configure who can use it (all users, selected users, or none) and required authentication methods

#### Administrative Units
- Used to segregate Active Directory for administrative purposes
- Create boundaries within your tenant to delegate administrative permissions
- Can contain both users and groups
- Unlike Roles (which are organization-wide), Administrative Units are scoped
- Administrators assigned to an Administrative Unit can only manage users, groups, and devices within that unit
- Allows for decentralized administration and scoped role assignments
- Useful for large organizations with multiple departments or geographic locations
- **Creation process**: During creation, in the **Assign Roles** screen, choose the desired roles and select the administrators. After creation, click **Add Member** and add users that should be in that scope
- **Examples**:
  - **Department-based**: Create an Administrative Unit for "Sales Department", add Sales users and groups to it, then assign a User Administrator role scoped to that unit - they can only manage users and groups in Sales
  - **Geographic-based**: Create an Administrative Unit for "North America Region", add users and groups from that region, then assign administrators scoped to that unit - they can only manage resources in that region

#### Devices
- **Devices** menu in Entra ID shows devices that users used to login to applications (office, home, phone) as long as they authenticated through Azure
- Can define requirements for devices that can connect to Azure (e.g., antivirus should be installed, password strength, etc.)
- Can see all devices users used to connect
- Devices can be used in other security features like **Conditional Access** (e.g., geographic-based access, force enable MFA if user is connecting from home device, etc.)
- **Device Settings** (inside Devices menu): Configure settings such as:
  - Who can access Entra ID (all users, selected users, or none)
  - Require MFA on register
  - Max devices per user

### RBAC (Role-Based Access Control)
- **Benefits**:
  - **Level of abstraction**: Permissions are managed at the role level rather than individual user level
  - **Simpler management**: Fewer errors due to centralized permission management
  - **Consistent access**: All users with the same role are treated identically
  - **Easier user onboarding**: New users can be quickly added to the system by assigning appropriate roles
  - **Scalability**: Easy to manage permissions for large numbers of users

#### Storage Authentication Methods
- **Token-Based Access (CBAC - Claim Based Access Control)**:
  - Default authentication method when "Default to Microsoft Entra Auth in Azure Portal" is disabled
  - Access uses storage account keys (tokens)
  - View keys: Storage account → **Security + Networking** → **Access Keys**
  - Keys must be shared with applications or users who need access
  - Less granular control compared to RBAC
- **Role-Based Access (RBAC)**:
  - Enabled by setting "Default to Microsoft Entra Auth in Azure Portal" to enabled
  - Provides granular access control through Azure roles
  - Recommended for better security and access management

#### Enabling RBAC for Storage Accounts
- **During creation**: Enable "Default to Microsoft Entra Auth in Azure Portal" option
- **After creation**: Storage account → **Settings** → **Configurations** → Enable "Default to Microsoft Entra Auth in Azure Portal"
- **Best practice**: After enabling RBAC, disable "Allow Storage account key access" to immediately block token-based access (unless applications still require key access)

#### Resource Permissions vs Data Permissions
- **Resource permissions**: Control management operations on the storage account (create, delete, modify settings, assign roles)
  - Examples: Owner, Contributor, Reader roles
  - Owner can manage everything but cannot read/write data by default
  - Contributor can manage resources but cannot assign roles
  - Reader can only view resource properties
- **Data permissions**: Control access to data within storage (blobs, queues, tables, files)
  - Examples: Storage Blob Data Owner, Storage Blob Data Contributor, Storage Blob Data Reader
  - Separate from resource permissions
  - Required to read/write data even if you have Owner role on the resource
- **View your access**: Storage account → **IAM** → **View my access** button to see current permissions

#### Storage Roles
- **Resource management roles**:
  - **Owner**: Full management access (manage everything, assign roles)
  - **Contributor**: Manage resources but cannot assign roles
  - **Reader**: Read-only access to resource properties
- **Data access roles** (Storage-specific):
  - **Storage Blob Data Owner**: Full access to blob containers and data (read, write, delete, manage)
  - **Storage Blob Data Contributor**: Read, write, and delete blob data
  - **Storage Blob Data Reader**: Read-only access to blob data
  - Similar roles exist for Queues, Tables, and Files (e.g., Storage Queue Data Contributor)
- **Role descriptions**: Check role descriptions in **IAM** → **Roles** tab to understand access levels

#### Assigning Roles (Storage-Specific)
- **Method 1**: Storage account → **IAM** → **Roles** tab → Select role checkbox → Click **Add role assignment** → Assign to user/group
- **Method 2**: Storage account → **IAM** → **Add role assignment** button → Select role → Assign to user/group/service principal
- **Scope inheritance**: Roles assigned at storage account level apply to all containers, queues, and tables within that account

#### Custom Roles
- **Creation location**: Subscription level → **IAM** → **Roles** → **+ Add** → **Add custom role**
- **Creation options**:
  - **Clone a role**: Start from an existing role that closely matches your needs, then modify permissions
  - **Start from scratch**: Build a role from the ground up with specific permissions
  - **Start from JSON**: Define the role using JSON format
- **Assignable scopes**: Define where the custom role can be assigned during creation
  - Can specify subscription(s), management group(s), or resource group(s)
  - Limits the scope where this role can be used
- **Limit**: Maximum of 5000 custom roles per tenant (avoid creating excessive custom roles)

#### Resource Group Level Role Assignment
- Assign roles at resource group level for environment-specific access (e.g., separate prod/dev resource groups)
- Navigate to **Resource Groups** → Select resource group → **IAM** → Assign roles
- Useful for granting access to specific environments without affecting other resource groups

#### Permission Combination
- **Cumulative permissions**: Role assignments are combined across different scopes
- **Example**: User with "Storage Blob Data Reader" at storage level + "Storage Blob Data Contributor" at resource group level = User has delete permission (union of all permissions)

#### Viewing Access Assignments
- **Method 1**: Resource group → **IAM** → View role assignments for that resource group
- **Method 2**: Entra ID → **Users** → Open user → Check both:
  - **Assigned Roles**: Entra ID roles (e.g., Global Administrator, User Administrator)
  - **Azure role assignments**: Azure RBAC role assignments across subscriptions, resource groups, and resources

#### Container-Level Role Assignment
- **Container-specific permissions**: Open container → **IAM** (left menu) → Assign roles from this screen
- **Scope**: Role assignments at container level affect only that specific container, not all containers in the storage account
- **Inheritance**: Storage-level role assignments are inherited by all containers, but container-level assignments override for that specific container
- **Access method switch**: When viewing container data (**Data Storage** → container), toggle switch at top of page to switch between:
  - **Access key**: Uses storage account keys (token-based)
  - **IAM role**: Uses assigned RBAC roles
