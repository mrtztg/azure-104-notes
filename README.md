# ☁️ azure-104-notes

## 🏗️ Azure Concepts

> **Note**: Not everything in this section will appear on the AZ-104 exam. This is an overview of Azure services.

### VM Abstractions (Services Built on VMs)

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

## 🖥️ Azure Virtual Machines (VMs)

### VM Creation

**CLI/PowerShell:**

- **Azure CLI**: `az vm create -n <vm-name> -g <resource-group> --image <image-name>`
- **PowerShell**: `New-AzVM -Name <vm-name> -Credential (Get-Credential)`
- **PowerShell script**: Upload and run shell scripts for automated provisioning

**Basics Tab:**

- **Availability options**:
  - **No infrastructure redundancy required**: Single VM, no high availability
  - **Availability zone**: Deploy VM in specific zone within region (protects against datacenter failures)
  - **Virtual machine scale set**: Auto-scaling group of identical VMs with load balancing
  - **Availability set**: Group VMs across fault domains and update domains within single datacenter (99.95% SLA)
- **Security type**:
  - **Standard**: Regular VM without additional security features
  - **Trusted launch**: Secure boot, vTPM (virtual Trusted Platform Module), integrity monitoring (protects against boot kits and rootkits)
  - **Confidential**: Encrypted in-memory processing for sensitive workloads (requires specific VM sizes)
- **Run with Azure Spot discount**:
  - Uses Azure's excess capacity at significant discount (up to 90% off)
  - VMs can be evicted when Azure needs capacity back
  - **Eviction type**:
    - **Capacity only**: Evict only when Azure needs capacity
    - **Price or capacity**: Evict when capacity needed or when price exceeds your max price
  - **Eviction policy**: Deallocate (stop) or Delete
  - **Use cases**: Batch processing, dev/test environments, interruptible workloads
  - **Not suitable for**: Production workloads requiring high availability

**Disks Tab:**

- **OS disk type**: Premium SSD, Standard SSD, Standard HDD
- **Encryption at host**: Encrypts data at rest on VM host (Additional encryption by OS. Is not required, unless you want maximum encryption)
- **Delete with VM**: Auto-delete disk when VM deleted
- **Key management**: Platform-managed key (PMK) or Customer-managed key (CMK)
- **Enable Ultra Disk compatibility**: Allows attaching Ultra Disks for highest performance
- **Data disks**: Attach additional disks for data storage (number limited by VM size/type)

**Networking Tab:**

- **Virtual network**: Select existing VNet or create new (required - every VM must be in a VNet)
  - **Address space**: IP range for entire VNet (e.g., 10.0.0.0/16 provides 65,536 IPs)
  - **Subnet**: Subdivision of VNet for organizing resources (e.g., 10.0.1.0/24 provides 256 IPs)
- **NIC network security group**: Network interface-level firewall
  - **None**: No NSG attached
  - **Basic**: Auto-created NSG with simple inbound port rules (This option is enough for most cases)
  - **Advanced**: Attach existing NSG with custom rules
- **Public inbound ports**: Allow selected ports (SSH 22, RDP 3389, HTTP 80, HTTPS 443) or None
- **Delete public IP and NIC when VM is deleted**: Auto-delete networking resources with VM
- **Enable accelerated networking**: SR-IOV bypasses host for direct VM-to-network communication
  - **Use cases**: High-performance workloads, low-latency applications, high packet-per-second scenarios
  - Requires supported VM size
- **Load balancing options**:
  - **Azure load balancer**: Layer 4 (TCP/UDP) load balancer, like AWS NLB
  - **Application Gateway**: Layer 7 (HTTP/HTTPS) load balancer, like AWS ALB

**Management Tab:**

- **Enable system assigned managed identity**: Enable if VM needs to access other Azure services (Storage, Key Vault, databases) without storing credentials
- **Login with Microsoft Entra ID**: Allow users to login using Entra ID credentials
- **Enable auto-shutdown**: Schedule automatic VM shutdown for cost savings
- **Enable backup**: Configure Azure Backup for VM protection
- **Enable periodic assessment**: Regular security and health assessments
- **Enable hotpatch**: Apply security patches without rebooting (Windows Server only)
- **Patch orchestration options**: Control how OS patches are applied (manual, automatic, Azure-orchestrated)
- **Reboot setting**: Control VM behavior after patching (if required, never, always)

**Monitoring Tab:**

- **Enable recommended alert rules**: Auto-configure common alerts (CPU, memory, disk) - costs per alert
- **Boot diagnostics**: Capture boot logs and screenshots (disable is okay for most cases)
- **Enable guest diagnostics**: Collect detailed performance metrics from inside VM
- **Enable application health monitoring**: Monitor application-level health status

**Advanced Tab:**

- **Extensions**: Install VM extensions for additional functionality (antivirus, monitoring agents, etc.)
- **VM Applications**: Pre-package needed applications to deploy automatically during VM creation
- **Custom data**: Pass startup scripts or configuration data to VM
- **User data**: Similar to custom data, but retrievable from within VM after creation
- **Performance (NVMe)**: Enable NVMe storage for highest disk performance
- **Host group**: Place VM on dedicated physical host
- **Capacity reservations**: Reserve compute capacity in advance (beneficial when guaranteed capacity is needed for critical workloads or during high-demand periods)
- **Proximity placement group**: Group VMs for low latency by placing them physically close

### 🔗 Connecting to VMs

- **Connection options**: VM → **Connect** → Choose method (RDP, SSH, Bastion, Serial console)
- **RDP (Windows)**: Port 3389, download `.rdp` file → Open with Remote Desktop client
- **SSH (Linux)**: Port 22, use `ssh -i <key-path> <user>@<public-ip>` or password auth
- **Serial Console**: Text-based access for troubleshooting unresponsive VMs (requires boot diagnostics)
- **Reset access**: VM → **Help** → **Reset password** (if locked out)

### 🏰 Azure Bastion

- **Purpose**: Secure browser-based RDP/SSH to VMs without public IPs, no NSG rules needed, protects against port scanning
- **Creation settings**:
  - **Availability zone**: Select zone for redundancy (or none)
  - **Tier**: Developer, Basic, Standard, Premium
  - **Instance count**: Number of scale units (Standard/Premium only)
  - **Virtual network**: Must be in the same VNet as target VMs
  - **Subnet**: Requires dedicated subnet named `AzureBastionSubnet` (minimum /26)
- **Connect via Bastion**: VM → **Connect** → **Bastion** → Enter credentials → **Connect**

### 💾 Adding Data Disks

- **Location**: VM → **Settings** → **Disks** → **+ Create and attach a new disk** or **Attach existing disks**
- **Disk types**: Premium SSD (fastest), Standard SSD, Standard HDD, Ultra Disk (highest IOPS)
- **vs Temporary disk**: Data disks persist across reboots/deallocations; temporary disk (D: drive) is lost on deallocation
- **After attaching**: Must initialize and format disk inside VM (Windows: Disk Management, Linux: `fdisk`/`mkfs`)
- **Max disks**: Limited by VM size (check VM specs for maximum data disk count)
- **Detach**: Can detach and reattach to different VMs (same region)

### 📈 Virtual Machine Scale Sets (VMSS)

- **Purpose**: Auto-scaling group of identical VMs with built-in load balancing and availability zone support

#### VMSS Creation

Most settings similar to regular VM creation.

**Basics Tab:**

- **Orchestration mode**:
  - **Flexible**: Mix VM sizes, more control but requires manual configuration
  - **Uniform**: Identical VMs optimized for large-scale workloads
- **Scaling mode**:
  - **Manual**: Set fixed instance count
  - **Autoscaling**: Scale based on metrics (CPU, memory, schedule)
  - **No scaling profile**: Create empty scale set
- **Instance count**: Initial number of VMs, minimum 2 for high availability

**Spot Tab:**

- **Spot VMs**: Use Azure's excess capacity at discounted prices (up to 90% off), can be evicted anytime
- **Eviction type**:
  - **Capacity only**: Evict when Azure needs capacity
  - **Price or capacity**: Evict when capacity needed or price exceeds max
- **Eviction policy**: Stop/Deallocate or Delete
- **Try to restore instances**: Attempt to recreate evicted Spot VMs when capacity available

**Disks Tab:** Similar to VM (OS disk type, encryption, data disks)

**Networking Tab:** Similar to VM (VNet, subnet, NSG, load balancing)

**Management Tab:** Similar to VM, plus:

- **Enable standby pools**: Pre-provisioned instances ready to scale quickly
- **Enable overprovisioning**: Create extra VMs during scaling, delete extras after success (faster scaling)

**Health Tab:**

- **Enable application health monitoring**: Monitor app health via extension or load balancer probes
- **Resilient VM create/delete**: Retry failed operations automatically
- **Automatic instance repairs**: Replace unhealthy instances automatically
- **Repair actions**: Reimage, Restart, or Replace (when health monitoring enabled)
- **Grace period**: Wait time before monitoring starts (allow app to initialize)

**Advanced Tab:** Similar to VM (extensions, custom data, proximity placement groups)

#### Managing VMSS

- **Instances**: View all VM instances, their status, and details (VMSS → **Instances** in left menu)
- **Scaling**: VMSS → **Availability + scale** → **Scaling** (manual scaling or configure autoscaling)
  - **Autoscaling**: Define rules based on metrics (e.g., scale up if CPU > X%, scale down if memory < Y%), set min/max/default instance counts
- **CLI manual scaling**: `az vmss scale --new-capacity <count> --name <vmss-name> --resource-group <rg-name>`

## 📦 Azure Storage

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
- **Pricing comparison**: https://azure.microsoft.com/en-us/pricing/details/storage/blobs/

#### Storage Account Creation Considerations

- **Storage account name**: Must be globally unique across all of Azure (3-24 characters, lowercase letters and numbers only)
- **Region selection**: Consider three factors:
  - Distance from users (lower latency)
  - Region-limited features (not all features available in all regions)
  - Cost (pricing varies by region)
- **Performance tiers**:
  - **Standard**: Lower storage cost, higher transaction/operation cost
  - **Premium**: Higher storage cost, lower read operation cost (better for high-transaction workloads)
- **Redundancy options** (some regions may not support all options):
  - **LRS (Locally Redundant Storage)**: 3 copies within single data center (Microsoft maintains 2 additional copies as disks fail over time)
  - **ZRS (Zone-Redundant Storage)**: 3 copies across availability zones within a region
  - **GRS (Geo-Redundant Storage)**: 6 copies across 2 regions (3 in primary, 3 in secondary)
    - **"Make read access to data available in the event of regional unavailability"**: Enables RA-GRS (slightly higher cost)
    - **"Geo priority replication guarantees Blob storage data is geo-replicated within 15 minutes"**
  - **GZRS (Geo-Zone-Redundant Storage)**: Safest option, combines ZRS and GRS

**Advanced Tab Settings:**

- **Security**:
  - **"Require secure transfer for REST API operations"**: Disables HTTP access, allows only HTTPS
  - **"Allow enabling anonymous access on individual containers"**: Check only when files need to be publicly accessible (e.g., website assets like videos, JS, CSS files)
  - **"Enable storage account key access"**: Allows key-based authentication (disable to enforce Entra ID only)
  - **"Default to Microsoft Entra Authentication in the Azure portal"**: Uses Entra ID (RBAC) instead of storage keys
- **Blob storage access tiers** (default tier for new blobs, can be set per file):
  - **Hot**: Frequent access, optimized for data accessed regularly, higher storage cost, lower access cost
  - **Cool**: Infrequent access (30+ days retention), lower storage cost, higher access cost
  - **Cold**: Rarely accessed (90+ days retention), even lower storage cost, even higher access cost

**Networking Tab Settings:**

- **Network access**:
  - **Enable**: Public access enabled (access key still required)
    - **Enable for all networks**: Accessible from internet
    - **Enable for selected virtual networks and IP addresses**: Restrict to specific VNets and IP ranges (use case: corporate network or specific Azure resources)
  - **Disable**: No public access, requires private endpoints
  - **Secured by Perimeter** (Most restricted): Strictest security, only accessible through configured security perimeter
- **Routing preference**:
  - **Microsoft network routing**: Traffic through Microsoft's global network (better performance, slightly higher cost)
  - **Internet routing**: Traffic through public internet (lower cost, may have higher latency)

**Data Protection Tab Settings:**

- **Recovery** (prevent accidental or malicious data loss):
  - **Enable point-in-time restore for containers**: Restore blob data to earlier state within retention period (requires versioning, change feed, and blob soft delete enabled)
  - **Enable soft delete for blobs**: Retain deleted blobs for specified days (can recover within retention period)
  - **Enable soft delete for containers**: Retain deleted containers for specified days
  - **Enable soft delete for file shares**: Retain deleted file shares for specified days
- **Tracking**:
  - **Enable versioning for blobs**: Maintains previous versions of blobs when modified or deleted
  - **Enable blob change feed**: Logs all changes to blobs (creates, modifications, deletions) for auditing and compliance
- **Access control**:
  - **Enable version-level immutability support**: Prevents files from deletion or modification (WORM - Write Once Read Many). Use case: Store access logs that cannot be deleted, even if hacker gains access and attempts to erase their activity traces

**Encryption Tab Settings:**

- **Encryption types**:
  - **MMK (Microsoft-Managed Keys)**: Default option, Microsoft manages and rotates encryption keys automatically
  - **CMK (Customer-Managed Keys)**: Customer controls encryption keys for greater security and compliance
- **Enable support for customer-managed keys** (available regardless of encryption type):
  - Scope options:
    - Blobs and files only
    - All service types (blobs, files, tables, and queues)
  - **Encryption key** source:
    - From URI: Specify key URI directly
    - From Key Vault: Select key from Azure Key Vault
- **Enhanced security**:
  - **Enable infrastructure encryption**: Adds second layer of encryption at infrastructure level (double encryption for highly sensitive data)

**Review + Create:**

- Review all configuration settings across all tabs
- Azure validates the configuration (checks policies, naming, quotas)
- Click **Create** to deploy the storage account

#### Blob Containers and Management

- **Containers**: Organize blobs within storage account (like folders/boxes to put blobs in)
- **Blob access tiers**: Can be set during file upload or changed later from top menu:
  - **Hot**: Frequent access
  - **Cool**: Infrequent access (30+ days)
  - **Cold**: Rarely accessed (90+ days)
  - **Archive**: Long-term archival storage, lowest cost, but data must be rehydrated before access (not available as default during storage account creation)

#### Lifecycle Management

- **What it is**: Automated policy-based data management to transition blobs between tiers or delete them
- **Location**: Storage account → **Data management** → **Lifecycle management**
- **Benefits**: Great for saving huge money on storage costs by automatically managing data lifecycle
- **Rule configuration** (first tab):
  - **Rule Scope**:
    - **Apply rule to all blobs**: Applies to entire storage account
    - **Limit blobs with filters**: Opens **Filter set** tab for additional filtering
  - **Blob type**: Block blobs or Append blobs
  - **Blob subtype**: Base blobs, Snapshots, or Versions
- **Filter set** tab (when "Limit blobs with filters" is selected):
  - **Blob prefix**: Apply to specific blob name patterns (e.g., "logs/")
  - **Blob index match**: Filter by blob index tags
- **Rule actions** (can define multiple actions in sequence):
  - **Move to cool tier**: After X days since creation/last modification/last access
  - **Move to cold tier**: After X days since creation/last modification/last access
  - **Move to archive tier**: After X days since creation/last modification/last access
  - **Delete blobs**: After X days since creation/last modification/last access
- **Minimum retention requirements** (charged for minimum period even if moved/deleted earlier):
  - **Cool tier**: Minimum 30 days
  - **Cold tier**: Minimum 90 days
  - **Archive tier**: Minimum 180 days

#### Object Replication

- Asynchronously copies block blobs between source and destination storage accounts
- **Use cases**: Disaster recovery, minimize latency, compliance
- **Requirements**: Blob versioning must be enabled on both accounts
- **Location**: Storage account → **Data management** → **Object replication**

#### AzCopy

- Command-line utility for copying data to/from Azure Storage
- **Use cases**:
  - Copy from local machine to storage account
  - Copy between storage accounts
  - Copy between containers
  - Sync directories
- **Authentication methods**:
  - **Entra ID (Microsoft Entra)**: `azcopy login` (recommended for interactive use)
  - **SAS token**: Append SAS token to destination URL
  - **Access key**: Via connection string or storage account key
- **Common commands**:
  - `azcopy copy <source> <destination>`: Copy files
  - `azcopy sync <source> <destination>`: Sync directories (one-way)
  - `azcopy remove <target>`: Delete blobs
- **Benefits**: Fast, resumable transfers with parallelism and automatic retries

#### Storage Browser

- Azure Portal GUI tool for managing storage files across multiple accounts
- **Features**: Upload, download, copy, paste, delete operations with graphical interface
- **Location**: Search "Storage browser" in Azure Portal

#### Azure File Share

- Fully managed cloud file shares accessible via SMB and NFS protocols
- **Use cases**: Shared application files, configuration files, lift-and-shift scenarios
- **Access**: Mount as network drive on Windows/Linux/macOS (SMB requires port 445 open)
- **Location**: Storage account → **Data storage** → **File shares** (also accessible via Storage browser)
- **Snapshots**: Manual one-time copy of files, browse/restore individual files (File share → **Operations** → **Snapshots**, or Windows "Previous Versions" tab)
- **Backup**: Scheduled/frequent backup with configurable policies and retention (File share → **Operations** → **Backup**)

#### Storage Authentication Methods

- **Token-Based Access (Access Keys)**:
  - Access uses storage account keys (tokens)
  - View keys: Storage account → **Security + Networking** → **Access Keys**
  - Keys must be shared with applications or users who need access
  - Less granular control compared to RBAC
- **Role-Based Access (RBAC)**:
  - Access through Azure Entra ID roles (IAM)
  - Provides granular access control through Azure roles
  - Recommended for better security and access management
- **Default to Microsoft Entra Auth in Azure Portal**: Controls which method the Azure Portal uses by default when you access storage (doesn't disable either method)

#### Storage Access Keys

- **Two access keys** (key1 and key2) provide full access to storage account and all data
- **Why two keys**: Enables key rotation without downtime
  - Applications use key1 → Regenerate key2 → Switch apps to key2 → Regenerate key1
- **Location**: Storage account → **Security + Networking** → **Access Keys**
- **Security**: Access keys are like root passwords - store securely (Azure Key Vault), rotate regularly

#### Shared Access Signature (SAS)

- URI token granting restricted access to storage resources
- **Benefits**: Granular control (specific resources, permissions, time period) vs full access with keys
- **SAS Types**:
  - **User Delegation SAS**: Secured with Entra ID (most secure)
  - **Service SAS**: Access to specific service (Blob, Queue, Table, File)
  - **Account SAS**: Access to multiple services
- **Creating SAS**:
  - **Container level**: Container → **Shared access tokens**
  - **Blob level**: Open blob → **Generate SAS** (from top menu or right-click context menu)
  - **Storage account level**: Storage account → **Shared access signature**
- **SAS Configuration Parameters**:
  - **Signing key**: Choose which access key to use (key1 or key2)
  - **Permissions**: Read, Write, Delete, List, Add, Create, Update, Process (select only needed)
  - **Start date/time**: When SAS becomes valid
  - **Expiry date/time**: When SAS expires
  - **Allowed IP addresses**: Restrict to specific IP ranges
  - **Allowed protocols**: HTTPS only (recommended) or HTTP and HTTPS
  - **Stored access policy**: Reference to predefined access policy (optional)
- **Stored Access Policy**:
  - Created at container level: Container → **Settings** → **Access policy** → **Add policy**
  - Define permissions, start/expiry times centrally in the policy
  - When generating SAS, select the stored access policy instead of defining parameters directly
  - **Benefits**: Centralized management - modify or revoke permissions for all SAS tokens using that policy without regenerating access keys
  - **Revoking**: Delete or modify the stored access policy to immediately affect all associated SAS tokens
- **Revoking SAS without Stored Access Policy**: Cannot revoke individual SAS tokens - only way is regenerating the access key used to create it (revokes all SAS tokens created with that key)
- **Best practices**: Use stored access policies for easier management, shortest expiry time, minimum permissions, HTTPS-only, prefer User Delegation SAS

#### Controlling Storage Access Methods

- **"Default to Microsoft Entra Auth in Azure Portal"**: Storage account → **Settings** → **Configurations**
  - When enabled: Azure Portal defaults to using Entra ID (RBAC) authentication
  - When disabled: Azure Portal defaults to using access keys
  - Note: This only controls the default method in the portal; both methods remain available
- **"Allow Storage account key access"**: Storage account → **Settings** → **Configurations**
  - When disabled: Completely blocks access key authentication (enforces Entra ID only)
  - Best practice: Disable if you want to enforce RBAC-only access

#### Storage RBAC - Resource Permissions vs Data Permissions

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

#### Assigning Storage Roles

- **Method 1**: Storage account → **IAM** → **Roles** tab → Select role checkbox → Click **Add role assignment** → Assign to user/group
- **Method 2**: Storage account → **IAM** → **Add role assignment** button → Select role → Assign to user/group/service principal
- **Scope inheritance**: Roles assigned at storage account level apply to all containers, queues, and tables within that account
- **Resource group level**: Assign roles at resource group level for environment-specific access (e.g., separate prod/dev resource groups)
- **Permission combination**: Role assignments are cumulative across scopes (union of all permissions)

#### Container-Level Role Assignment

- **Container-specific permissions**: Open container → **IAM** (left menu) → Assign roles from this screen
- **Scope**: Role assignments at container level affect only that specific container, not all containers in the storage account
- **Inheritance**: Storage-level role assignments are inherited by all containers, but container-level assignments override for that specific container
- **Access method switch**: When viewing container data (**Data Storage** → container), toggle switch at top of page to switch between:
  - **Access key**: Uses storage account keys (token-based)
  - **IAM role**: Uses assigned RBAC roles

### 🗄️ Azure Data Services

- **SQL Services**:
  - **Azure SQL Database**: Managed relational database (PaaS), serverless and provisioned tiers
  - **Azure SQL Managed Instance**: Near 100% compatibility with SQL Server, VNet integration
  - **SQL Server on Azure VMs**: IaaS option for full SQL Server control
- **NoSQL Services**:
  - **Azure Cosmos DB**: Globally distributed, multi-model NoSQL database
  - **Azure Table Storage**: NoSQL key-value store (part of Azure Storage)
- **Azure Database for PostgreSQL Flexible Server**: Managed PostgreSQL with flexible compute and storage scaling, supports zone-redundant high availability
- **Azure Synapse Analytics**: Analytics service combining data warehousing and big data analytics (formerly SQL Data Warehouse)

### 🧩 Resource

- An entity managed by Azure
- **Expected examples**: Virtual Machine (VM), web app, storage account
- **Unexpected examples**: Public IP address, network interface card (NIC), network security group (NSG)
- Accounts can be given read, update, and owner rights to resources

### 📂 Resource Group

- A way of organising resources in a subscription
- Acts as a folder structure for resources
- All resources must belong to only one resource group
- Deleting a resource group also deletes all resources within it
- A way to separate out projects, keeping unrelated things separate

### 🚚 Moving Resources

- Resources can be moved between resource groups, subscriptions, or regions
- Navigate to resource group → Select resources → **Move** button (top menu)
- Move options: Move to another resource group, Move to another subscription, Move to another region
- **Important**: Some resources cannot be moved (e.g., certain networking resources, classic resources)
- Azure validates move compatibility before allowing the operation

![tenant-sub-resourcegroup](assets/tenant-sub-resourcegroup.jpg)

## 📜 Azure Resource Manager (ARM)

- **What it is**: Deployment and management layer for all Azure resources (every action goes through ARM)
- **ARM Templates**: JSON files for repeatable infrastructure deployments
  - **Template file**: Resource definitions with `parameters` section
  - **Parameters file**: Define parameter values separately (reuse templates with different configs)
- **Get templates**: In **Review + Create** → **Download a template for automation**
- **View past deployments**: Resource group → **Settings** → **Deployments**

**Sample ARM Template (template.json):**

```json
{
  "$schema": "https://schema.management.azure.com/...", // Schema version
  "contentVersion": "1.0.0.0", // Your template version
  "parameters": {
    // Configurable inputs (values from parameters.json)
    "environment": {
      "type": "string",
      "defaultValue": "dev"
    },
    "skuName": {
      "type": "string" // No default - must be in parameters.json
    }
  },
  "variables": {
    // Reusable values within template
    "location": "[resourceGroup().location]", // Built-in function
    "storageName": "[concat('storage', parameters('environment'))]" // concat() joins strings
  },
  "resources": [
    // Resources to deploy
    {
      // Find type & apiVersion: Azure docs or download template from portal
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2021-02-01",
      "name": "[variables('storageName')]",
      "location": "[variables('location')]",
      // Find available properties: Review+Create → View automation template
      "sku": { "name": "[parameters('skuName')]" }, // Value from parameters.json
      "kind": "StorageV2"
    },
    {
      "type": "Microsoft.Storage/storageAccounts/blobServices/containers",
      "apiVersion": "2021-02-01",
      "name": "[concat(variables('storageName'), '/default/mycontainer')]",
      "dependsOn": [
        // Wait for storage account to be created first
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageName'))]"
      ]
    }
  ],
  "outputs": {
    // Return values after deployment
    "storageEndpoint": {
      "type": "string",
      "value": "[reference(variables('storageName')).primaryEndpoints.blob]"
    }
  }
}
```

**Sample Parameters File (parameters.json):**

```json
{
  "$schema": "https://schema.management.azure.com/.../deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "environment": { "value": "prod" },
    "skuName": { "value": "Standard_GRS" }
  }
}
```

**Modifying templates**: When removing or duplicating resources, update all related references:

- Remove/update `dependsOn` references in other resources
- Remove/update `variables` that reference the changed resource
- Update `outputs` that use `reference()` to the changed resource
- Example: Removing a Public IP? Update the NIC that references it, and any outputs using it

**Common ARM functions**: `concat()`, `parameters()`, `variables()`, `resourceGroup()`, `reference()`, `uniqueString()`, `resourceId()`

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

#### Automation and Common Operations

- PowerShell enables automation of Azure tasks through scripting
- Store resource information in variables for reuse

**Working with Resource Groups:**

```powershell
# Get resource group and store in variable
$rg = Get-AzResourceGroup -Name '<resource-group-name>' -Location '<location>'

# Access properties
$rg.ResourceGroupName
$rg.Location
$rg.ResourceId
```

**Assigning Policy to Resource Group:**

```powershell
# Step 1: Find policy in Azure Portal → Policy → Definitions → Copy Definition ID
# Step 2: Get policy definition by ID
$definition = Get-AzPolicyDefinition -Id '<definition-id-from-portal>'

# Step 3: Assign policy to resource group
New-AzPolicyAssignment -Scope $rg.ResourceId `
                       -PolicyDefinition $definition `
                       -Name 'RGLocationMatch' `
                       -DisplayName 'Enforce resource group location policy'
```

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
- **Creating a new tenant**: If your account is not a global administrator (e.g., on dev plans), create a new Entra ID tenant via **Entra ID dashboard → Manage Tenants → Create a new tenant** to gain necessary access. Now, this tenant will be yours.
- **Switching tenants**: You can switch between tenants (e.g., from your company's tenant where you're a developer to your own tenant where you're a global administrator). Two methods:
  - Navigate to **Entra ID → Manage Tenants**, select the desired tenant, and click **Switch**
  - Click your **profile picture** in the right corner → **Switch directory** → click **Switch** on the desired directory (tenant)

### Management Groups

- Containers for organizing subscriptions within a tenant for governance at scale
- Hierarchy: Tenant → Management Groups → Subscriptions → Resource Groups → Resources
- Each tenant has a **Root Management Group** at the top level
- Can create **child management groups** under the root or under other management groups (up to 6 levels deep)

#### Creating Child Management Groups

- Navigate to **Management Groups** service → **Create** (or select parent group → **Add management group**)
- Specify parent management group (defaults to root)
- Assign subscriptions to management groups for organizational structure

#### Inheritance and Benefits

- **Policy inheritance**: Policies assigned to parent management group automatically apply to all child management groups and subscriptions
- **RBAC inheritance**: Role assignments at management group level inherited by child groups and subscriptions
- **Benefits**: Centralized governance across multiple subscriptions, apply policies/RBAC once at scale instead of per subscription

### Subscription

- An agreement with Microsoft to use Azure services and how to pay
- All Azure resource usage gets billed to the payment method of the subscription
- Types: Free subscription, Pay-As-You-Go (PAYG), Enterprise agreements
- Every subscription can be assigned to ONLY one tenant, but each tenant **can** have multiple subscriptions.
- To change tenant of a subscription, open the subscription, and use "Change directory" button on top.
- From **IAM** menu, we can define who can have access to this subscription (e.g., see invoices and billings, forecasts, etc.)
- From **Cost Management > Cost Analysis** menu, we can see details of the costs like which services cost how much
- **Cost Management > Cost Alert** is for setting up cost alerts. From top of the page, click **Add > Add Budget**
- There is also **Add Anomaly alert**, which is defined by Azure itself (e.g., for new charges that were not there before, or charges were there and now stopped, or significant changes on cost). Alert sent as email
- **Cost Management > Budgets**: Define budget, reset budget reset period, when to receive the alert (threshold percentage)
- **Cost Management > Advisor Recommendations**: Gives recommendations across number of categories like cost
- In **Settings > Usage + Quotas** menu, we can see different limitations of how much compute in different regions. Under "Adjustable" column, for the ones that are "yes", we can click on pen button and ask for change

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

#### Assigning Roles

- **Resource level**: Resource → **IAM** → **Add role assignment** → Select role → Assign to user/group/service principal
- **Resource group level**: Resource Groups → Select resource group → **IAM** → Assign roles
  - Useful for environment-specific access (e.g., separate prod/dev resource groups)
- **Subscription level**: Subscription → **IAM** → Assign roles
- **Scope inheritance**: Roles assigned at higher levels (subscription) are inherited by lower levels (resource groups, resources)

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

#### Permission Combination

- **Cumulative permissions**: Role assignments are combined across different scopes
- **Example**: User with "Reader" at subscription level + "Contributor" at resource group level = User has contributor access to that resource group (union of all permissions)

#### Viewing Access Assignments

- **Method 1**: Resource/Resource group/Subscription → **IAM** → View role assignments
- **Method 2**: Entra ID → **Users** → Open user → Check both:
  - **Assigned Roles**: Entra ID roles (e.g., Global Administrator, User Administrator)
  - **Azure role assignments**: Azure RBAC role assignments across subscriptions, resource groups, and resources

## 🛡️ Governance & Compliance

### 🏷️ Tags

- Key-value pair metadata applied to Azure resources for organization and management
- Can be applied to resources, resource groups, and subscriptions
- Common uses: cost tracking, environment identification (prod/dev), ownership, automation
- Apply tags via resource **Settings → Tags** menu or during resource creation
- **Important**: Tags are NOT inherited from resource groups to resources by default
- Use Azure Policy to enforce tagging requirements or automatically apply tags

### 🔒 Resource Lock

- Can prevent deletion or modification at Subscription level, Resource Group level, or Resource level
- Navigate to **Settings > Locks** menu, and add either **Read-Only** or **Delete** lock type
- These locks help protect critical resources from accidental deletion or modification

### 📋 Azure Policy

- Service for defining and enforcing governance rules at subscription or resource group level
- Azure provides hundreds of built-in policies ready to use
- During resource creation, **Review + Create** step validates against assigned policies

#### Finding and Assigning Built-in Policies

- Navigate to **Policy** service → **Authoring → Definitions**
- Search by keyword (e.g., "Allowed locations"), click to view JSON definition
- Assign to desired scope (subscription, management group, or resource group)
- Fill predefined parameters (e.g., allowed regions) and custom non-compliance message

#### Definition Types

- **Policy**: Single policy definition with specific rule
- **Initiative**: Collection of multiple policies grouped together
- Can create custom policy or initiative definitions

#### Creating Custom Policy Definitions

- Copy JSON from built-in policies to learn structure and syntax
- **Finding resource property keys**: Create resource (e.g., storage) → **Review + Create** → **Download a template for automation** or **View automation template** to see JSON keys
- **Role definitions**: If policy requires permissions (e.g., for "DeployIfNotExists" or "Modify" effects), specify required role definitions during policy creation to grant necessary permissions

**Basic policy template:**

```json
{
  "mode": "All", // "All" applies to all resource types, "Indexed" applies to resources that support tags and location
  "policyRule": {
    "if": {
      // Condition to evaluate
      "field": "<resource-property-key>", // Property to check (e.g., "type", "location")
      "equals": "<value>" // Value to match
    },
    "then": {
      // Action to take if condition is true
      "effect": "[parameters('effect')]" // References parameter below
    }
  },
  "parameters": {
    // Define configurable parameters for flexibility
    "effect": {
      "type": "String",
      "allowedValues": ["<option1>", "<option2>"], // Options like "Deny", "Audit", "Disabled"
      "defaultValue": "<default-option>"
    }
  }
}
```

**Example - Require HTTPS for Storage Accounts:**

```json
{
  "mode": "All",
  "policyRule": {
    "if": {
      "allOf": [
        // All conditions must be true
        {
          "field": "type", // Check resource type
          "equals": "Microsoft.Storage/storageAccounts"
        },
        {
          "field": "Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly", // Check HTTPS property
          "notEquals": "true" // If HTTPS is NOT enabled
        }
      ]
    },
    "then": {
      "effect": "[parameters('effect')]" // Apply deny or disabled effect
    }
  },
  "parameters": {
    "effect": {
      "type": "String",
      "allowedValues": ["Deny", "Disabled"], // Either block creation or just disable policy
      "defaultValue": "Deny", // Default: prevent non-HTTPS storage accounts
      "metadata": {
        "displayName": "Effect",
        "description": "Enable or disable the execution of the policy"
      }
    }
  }
}
```

#### Viewing Assignments

- **Authoring → Assignments**: View all assigned policies, their scopes, and compliance status
