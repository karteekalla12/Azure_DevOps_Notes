# Azure Interview Questions & Answers

## Table of Contents
- [SDLC in Azure](#sdlc-in-azure)
- [Methodologies](#methodologies)
- [Resource Groups](#resource-groups)
- [Virtual Machines](#virtual-machines)
- [Networking](#networking)
- [IAM (Identity and Access Management)](#iam-identity-and-access-management)
- [Storage Services](#storage-services)
- [Key Vault](#key-vault)

---

## SDLC in Azure

### 1. How does Azure support the Software Development Life Cycle (SDLC)?
**Answer:** Azure provides integrated services across all SDLC phases:  
- **Planning**: Azure Boards for agile planning  
- **Development**: GitHub Actions/Azure DevOps Repos for version control  
- **Testing**: Azure Test Plans and integration with testing frameworks  
- **Deployment**: Azure Pipelines for CI/CD  
- **Monitoring**: Azure Monitor and Application Insights  
- **Maintenance**: Azure Automation for patching and updates  
This creates a seamless cloud-native SDLC environment with infrastructure as code (IaC) support.

### 2. What Azure services would you use for a DevOps CI/CD pipeline?
**Answer:**  
- **Source Control**: Azure Repos or GitHub  
- **Build**: Azure Pipelines (YAML-based)  
- **Artifact Storage**: Azure Artifacts  
- **Release Management**: Azure Pipelines (multi-stage)  
- **Infrastructure as Code**: Azure Resource Manager (ARM) templates or Bicep  
- **Testing**: Azure Test Plans integrated with pipelines  
- **Monitoring**: Application Insights for post-deployment validation

### 3. How would you implement infrastructure as code (IaC) in Azure SDLC?
**Answer:**  
- Use **ARM templates** or **Bicep** for declarative resource provisioning  
- Store templates in version control (GitHub/Azure Repos)  
- Integrate with CI/CD pipelines:  
  1. Validate templates during build phase  
  2. Deploy via Azure Pipelines using `AzureResourceManagerTemplateDeployment` task  
  3. Implement deployment slots for safe rollouts  
- Use **Azure Policy** to enforce IaC standards  
- Implement **Azure Blueprints** for enterprise-scale compliance

### 4. What is the role of Azure DevOps in SDLC?
**Answer:** Azure DevOps provides end-to-end SDLC management:  
- **Azure Boards**: Agile planning (sprints, backlogs)  
- **Azure Repos**: Git repositories with PR workflows  
- **Azure Pipelines**: CI/CD with 100+ deployment targets  
- **Azure Artifacts**: Package management (NuGet, npm)  
- **Azure Test Plans**: Manual and exploratory testing  
- **Azure Monitor**: Integrated with Application Insights for telemetry  
It creates a unified platform replacing multiple point solutions.

### 5. How do you handle database changes in Azure SDLC?
**Answer:**  
- Use **Database Projects** in Visual Studio for schema versioning  
- Implement **migration scripts** (e.g., Flyway, DbUp) stored in source control  
- Integrate with CI/CD:  
  - Validate scripts during build  
  - Deploy via pipelines using SQLPackage.exe or Azure SQL Deployment task  
- Use **Azure SQL Database** with:  
  - Deployment slots for testing migrations  
  - Point-in-time restore for rollback  
- Implement **Azure Database for PostgreSQL/MySQL** with similar patterns

### 6. What Azure services support shift-left security in SDLC?
**Answer:**  
- **Azure Security Center**: Integrated security posture management  
- **Azure Defender**: For containers and SQL databases  
- **Azure Policy**: Enforce security standards during deployment  
- **GitHub Advanced Security**: Secret scanning and dependency review  
- **Azure Container Registry**: Vulnerability scanning for images  
- **Azure DevOps**: Pre-merge security checks in PR policies  
- **Azure Key Vault**: Secure secret management in pipelines

### 7. How would you implement blue-green deployments in Azure?
**Answer:**  
1. Deploy new version to "green" environment (identical to production)  
2. Route 10% of traffic using **Azure Traffic Manager** or **Application Gateway**  
3. Validate with **Application Insights** metrics  
4. If successful, shift 100% traffic using routing rules  
5. Decommission "blue" environment  
*Key services*: App Service slots, Traffic Manager, Application Gateway, Azure Monitor

### 8. What is the significance of Azure Policy in SDLC governance?
**Answer:** Azure Policy enforces organizational standards:  
- **Pre-deployment**: Prevent non-compliant resource creation  
- **Continuous compliance**: Audit existing resources  
- **Remediation**: Auto-correct violations (e.g., unencrypted storage)  
- **Policy as Code**: Store policies in source control  
- **Integration**: Works with Azure Blueprints for enterprise governance  
Example: Enforce "All VMs must use managed disks" via policy initiative.

### 9. How do you manage configuration drift in Azure SDLC?
**Answer:**  
- **Infrastructure as Code**: Treat infrastructure as versioned code  
- **Azure Policy**: Continuously audit configuration  
- **Azure Automation State Configuration**: Enforce DSC configurations  
- **Deployment Gates**: Require policy compliance before production deployment  
- **Drift Detection**: ARM template comparison to detect changes  
- **Immutable Infrastructure**: Use containers/VM scale sets where possible

### 10. What Azure services support canary deployments?
**Answer:**  
- **Application Gateway**: Weighted routing to different backend pools  
- **Azure Traffic Manager**: Percentage-based traffic distribution  
- **App Service**: Deployment slots with traffic routing percentages  
- **Azure Front Door**: Session affinity for canary testing  
- **Azure Monitor**: Real-time metrics to validate canary version  
- **Feature Flags**: Implement with Azure App Configuration for gradual rollouts

---

## Methodologies

### 11. How does Azure support Agile methodology implementation?
**Answer:**  
- **Azure Boards**: Backlogs, sprints, and Kanban boards  
- **Integrated Work Tracking**: Link code commits to work items  
- **Dashboards**: Real-time sprint progress visualization  
- **Retrospectives**: Built-in templates in Azure DevOps  
- **Scalability**: SAFe support through Portfolio Backlogs  
- **Automation**: Auto-update work items on PR completion  
- **Reporting**: Velocity charts, burn-down reports

### 12. What Azure tools support DevOps methodology?
**Answer:**  
- **CI/CD**: Azure Pipelines with YAML workflows  
- **IaC**: ARM/Bicep templates in source control  
- **Monitoring**: Application Insights + Azure Monitor  
- **Collaboration**: Azure Boards + Wiki integration  
- **Security**: Azure Security Center + GitHub Advanced Security  
- **Testing**: Azure Test Plans with pipeline integration  
- **Package Management**: Azure Artifacts for dependencies

### 13. How would you implement Scrum in Azure DevOps?
**Answer:**  
1. Create **Project** with Scrum process template  
2. Configure **Sprints** (iterations) with start/end dates  
3. Use **Backlog** for user stories and tasks  
4. Plan **Sprint** with capacity planning  
5. Track daily progress with **Taskboard**  
6. Run **Sprint Retrospective** using built-in template  
7. Generate **Velocity Report** for forecasting  
8. Automate sprint ceremonies with **Azure Boards extensions**

### 14. What Azure services support Lean methodology principles?
**Answer:**  
- **Value Stream Mapping**: Azure Boards + Value Stream Extension  
- **Waste Reduction**: Azure Advisor for cost optimization  
- **Continuous Improvement**: Azure Monitor anomaly detection  
- **Amplify Learning**: Azure DevOps Wiki + Knowledge Base  
- **Decide Late**: Feature flags in Azure App Configuration  
- **Deliver Fast**: Azure Pipelines with parallel jobs  
- **Empower Teams**: RBAC for granular permissions

### 15. How does Azure facilitate Waterfall methodology?
**Answer:** While Azure excels at Agile, Waterfall can be implemented by:  
- Using **Azure Boards** with sequential phases (Requirements → Design → Build → Test → Deploy)  
- Creating **custom workflows** with state transitions  
- Using **Gantt chart extensions** for timeline visualization  
- Implementing **manual approval gates** between stages  
- Generating **comprehensive reports** for phase sign-offs  
*Note: Azure is optimized for iterative approaches, making Waterfall less efficient.*

### 16. What Azure tools support SAFe (Scaled Agile Framework)?
**Answer:**  
- **Portfolio Backlogs**: For Epic and Feature management  
- **Program Boards**: Visualize dependencies across teams  
- **Roadmaps**: Strategic planning timelines  
- **Team Backlogs**: Standard sprint planning  
- **DevOps Integration**: Link features to pipelines  
- **Reporting**: PI (Program Increment) metrics  
- **Extensions**: Third-party SAFe tools from Azure Marketplace

### 17. How would you implement GitFlow in Azure Repos?
**Answer:**  
1. Create `main` (production) and `develop` branches  
2. Configure branch policies:  
   - Require PRs for `main` and `develop`  
   - Minimum 1 reviewer  
   - Build validation  
3. Feature branches from `develop`  
4. Release branches for staging  
5. Hotfix branches from `main`  
6. Use **Azure Pipelines** to trigger builds on feature branches  
7. Automate merge to `develop` on PR completion

### 18. What Azure services support Continuous Delivery?
**Answer:**  
- **Azure Pipelines**: Multi-stage YAML pipelines  
- **Deployment Slots**: For App Service zero-downtime deployments  
- **Azure Artifact**: Package versioning  
- **Azure Key Vault**: Secure secret injection  
- **Azure Monitor**: Deployment validation with health checks  
- **Feature Management**: Gradual rollouts with Azure App Configuration  
- **Infrastructure as Code**: ARM/Bicep for environment parity

### 19. How does Azure support Value Stream Management?
**Answer:**  
- **Azure Boards**: End-to-end work tracking  
- **Azure Pipelines**: Visualize CI/CD flow  
- **Application Insights**: Monitor deployment impact  
- **Azure Monitor**: Track lead time and deployment frequency  
- **Value Stream Extension**: Dedicated VSM visualization  
- **Integration**: Connect to Jira, GitHub, etc.  
- **Metrics**: Cycle time, failure rate, MTTR

### 20. What methodology would you recommend for a legacy migration to Azure?
**Answer:**  
**Hybrid Agile Approach**:  
1. **Assessment Phase** (Waterfall): Inventory, TCO analysis  
2. **Lift-and-Shift** (Iterative): Move VMs in batches using Azure Migrate  
3. **Modernization** (Agile): Refactor apps in sprints  
4. **Optimization** (Lean): Continuous cost/performance tuning  
*Key Azure tools*: Azure Migrate, Azure Advisor, Azure Cost Management

---

## Resource Groups

### 21. What is the primary purpose of Azure Resource Groups?
**Answer:** Resource Groups are logical containers that:  
- Organize related resources (VMs, storage, networks)  
- Enable collective management (deploy/delete/update as a unit)  
- Provide scope for Azure RBAC permissions  
- Define deployment boundaries for ARM templates  
- Simplify cost tracking via billing reports  
- Must reside in a single Azure region (though resources can span regions)

### 22. What are the naming convention best practices for Resource Groups?
**Answer:**  
- Prefix with environment: `rg-prod-`, `rg-dev-`  
- Include application/function: `rg-prod-networking`  
- Add region: `rg-prod-eus-web`  
- Avoid special characters (use hyphens)  
- Keep under 90 characters  
- Example: `rg-prod-weu-appgateway-001`

### 23. Can resources from different regions be in the same Resource Group?
**Answer:** Yes. Resource Groups have a **location** (for metadata storage), but resources within them can be deployed to **any Azure region**. Example:  
- RG location: East US  
- Contains:  
  - VM in West Europe  
  - Storage account in Southeast Asia  
  - SQL DB in Australia East  
*Note: Some services like Availability Sets require same region.*

### 24. What happens when you delete a Resource Group?
**Answer:** All resources within the Resource Group are **permanently deleted**. This is:  
- Irreversible (no soft delete for RGs)  
- Atomic operation (all or nothing)  
- Requires Contributor+ permissions on the RG  
- Best practice: Use Resource Locks (`CanNotDelete`) for production RGs

### 25. How do Resource Groups relate to Azure Policy?
**Answer:**  
- Azure Policy can be assigned at Resource Group scope  
- Policies evaluate resources within the RG  
- Initiatives (policy sets) can target specific RGs  
- Compliance reports are viewable per RG  
- Example: Policy to enforce "All storage accounts must be encrypted" applied to `rg-prod-storage`

### 26. When should you use multiple Resource Groups?
**Answer:** When:  
- Separating environments (dev/test/prod)  
- Isolating by application component (web/api/db)  
- Meeting security boundaries (different teams)  
- Managing billing separation  
- Implementing different lifecycle policies  
*Anti-pattern*: Creating one RG per resource (defeats purpose)

### 27. What is the maximum number of resources per Resource Group?
**Answer:** Azure allows up to **800 resources** per Resource Group. However:  
- Best practice: Keep under 200 for manageability  
- Exceeding limits causes deployment failures  
- Use multiple RGs if approaching limit  
- Some resource types (like Network Security Groups) have lower effective limits

### 28. How do Resource Groups impact Azure Cost Management?
**Answer:**  
- Costs are reported per Resource Group in Cost Analysis  
- Tags on RGs propagate to resources (for chargeback)  
- Budgets can be set at RG scope  
- Example: Tag `rg-prod-finance` with `costcenter:12345` for finance department billing

### 29. What are Resource Locks and how do they work with Resource Groups?
**Answer:** Resource Locks prevent accidental deletion/modification:  
- **CanNotDelete**: Authorized users can read/edit but not delete  
- **ReadOnly**: Only read operations allowed  
- Applied at RG level to protect all contained resources  
- Must be removed before deleting RG  
- Bypasses RBAC (requires Owner permissions to manage locks)

### 30. How would you structure Resource Groups for a multi-tier application?
**Answer:**  
```
rg-app-prod-weu (West US)  
├── rg-app-prod-weu-web (Web tier)  
├── rg-app-prod-weu-app (App tier)  
└── rg-app-prod-weu-db (DB tier)  
```
*Benefits*:  
- Tier-specific scaling policies  
- Isolated backup strategies  
- Granular RBAC (DB team only accesses DB RG)  
- Simplified cost allocation

---

## Virtual Machines

### 31. What are the key differences between Azure VM Scale Sets and Availability Sets?
**Answer:**  
| **Feature**          | **Availability Sets**       | **VM Scale Sets**          |  
|----------------------|-----------------------------|----------------------------|  
| Purpose              | High availability           | Auto-scaling + HA          |  
| VM Count             | Max 200 VMs                 | Max 1,000 VMs              |  
| Scaling              | Manual                      | Automatic (CPU/memory)     |  
| Orchestration        | None                        | Rolling updates, health    |  
| Use Case             | Stateful apps (SQL clusters)| Stateless apps (web farms) |  
| Update Domain        | 5 max                       | Configurable (up to 20)    |

### 32. How do you secure Azure Virtual Machines?
**Answer:**  
- **Network Security**: NSGs, Azure Firewall, DDoS protection  
- **OS Security**: Azure Security Center recommendations, JIT VM access  
- **Disk Encryption**: Azure Disk Encryption (ADE) with Key Vault  
- **Patch Management**: Azure Automation Update Management  
- **Identity**: Managed identities instead of passwords  
- **Monitoring**: Azure Monitor for VMs (Insights)  
- **Backup**: Azure Backup with recovery points

### 33. What is the difference between Managed and Unmanaged Disks?
**Answer:**  
| **Managed Disks**                     | **Unmanaged Disks**                |  
|---------------------------------------|-------------------------------------|  
| Azure manages storage accounts        | You manage storage accounts         |  
| No storage account limits             | Limited by storage account capacity |  
| Built-in high availability            | Requires manual replication setup   |  
| Simplified scaling                    | Complex scaling operations          |  
| Recommended for all new deployments   | Legacy (pre-2016)                   |  
| Supports ultra disks and shared disks | Limited disk type options           |

### 34. How would you automate VM deployment in Azure?
**Answer:**  
1. Create ARM template/Bicep file defining VM configuration  
2. Store in source control (GitHub/Azure Repos)  
3. Deploy via Azure Pipelines:  
   ```yaml
   - task: AzureResourceManagerTemplateDeployment@3
     inputs:
       deploymentScope: 'Resource Group'
       azureResourceManagerConnection: 'AzureServiceConnection'
       action: 'Create Or Update Resource Group'
       resourceGroupName: 'rg-app-prod'
       location: 'eastus'
       templateLocation: 'Linked artifact'
       csmFile: 'vm-deployment.json'
4. Use parameter files for environment-specific values  
5. Implement approvals for production deployments

### 35. What are Azure VM Availability Zones?
**Answer:**  
- Physically separate datacenters within a region (3+ zones)  
- Each zone has independent power, cooling, networking  
- Protects against datacenter-level failures  
- Required for zone-redundant services (e.g., Standard Load Balancer)  
- VMs deployed in zones use `Availability Zone` instead of `Availability Set`  
- Supported regions: All major regions (e.g., East US, West Europe)

### 36. How do you resize an Azure VM?
**Answer:**  
1. Stop the VM (deallocation required for most size changes)  
2. In Azure Portal:  
   - Go to VM → Size → Select new size → Resize  
3. Using Azure CLI:  
   ```bash
   az vm resize --resource-group myRG --name myVM --size Standard_D4s_v3
   ```
*Note*:  
- Can't resize between series (e.g., Dv2 to Ev3)  
- Premium Storage VMs require Premium Storage-compatible sizes  
- Check available sizes first: `az vm list-vm-resize-options`

### 37. What is Azure Spot VM and when would you use it?
**Answer:**  
- **Definition**: Low-cost VMs using unused Azure capacity (up to 90% discount)  
- **Use Cases**:  
  - Fault-tolerant workloads (batch processing, stateless apps)  
  - Short-duration tasks (<14 days)  
  - Big data analytics (Spark clusters)  
- **Limitations**:  
  - Can be evicted with 30-second warning  
  - No SLA guarantee  
  - Max 6-hour runtime without eviction protection  
- **Best Practice**: Use with scale sets and eviction policy `Deallocate`

### 38. How do you monitor Azure VM performance?
**Answer:**  
- **Azure Monitor for VMs**:  
  - Dependency map visualization  
  - Performance counters (CPU, memory, disk)  
  - Log queries for custom analysis  
- **Log Analytics**:  
  - Custom alerts on thresholds  
  - VM insights for OS-level metrics  
- **Application Insights**: For app-level monitoring  
- **Diagnostic Settings**: Stream metrics to Event Hubs for external tools

### 39. What is Azure Hybrid Benefit and how does it work?
**Answer:**  
- **Definition**: Use existing Windows Server/SQL Server licenses on Azure VMs  
- **Savings**: Up to 85% on Windows VM costs  
- **Requirements**:  
  - Software Assurance or eligible subscription  
  - Bring-your-own-license (BYOL) model  
- **Implementation**:  
  - During VM creation: Select "Azure Hybrid Benefit"  
  - For existing VMs: `az vm update --license-type Windows_Server`  
- **Applies To**: Windows Server, SQL Server, SUSE Linux (RHEL via Red Hat Cloud Access)

### 40. How would you implement backup for Azure VMs?
**Answer:**  
1. Enable **Azure Backup** for the VM  
2. Configure:  
   - Recovery Services Vault  
   - Backup policy (daily/weekly, retention)  
3. Backup process:  
   - Uses VM snapshots + incremental backups  
   - Application-consistent via VSS (Windows) or fsfreeze (Linux)  
4. Restore options:  
   - Full VM restore  
   - File-level recovery  
   - Cross-region restore (with CRR)  
*Best Practice*: Test restores quarterly using "Restore Test" feature

## Networking

### 41. What is Azure Virtual Network (VNet) and why is it important?
**Answer:**  
- **Definition**: Isolated network environment in Azure (like a private datacenter)  
- **Key Functions**:  
  - IP address space definition (CIDR blocks)  
  - Subnet segmentation (e.g., web, app, db)  
  - On-premises connectivity via VPN/ExpressRoute  
  - Network security via NSGs and firewalls  
  - DNS resolution within the network  
- **Importance**: Foundation for all Azure networking; required for most PaaS services

### 42. How do Network Security Groups (NSGs) differ from Azure Firewall?
**Answer:**  
| **Network Security Groups**           | **Azure Firewall**                  |  
|---------------------------------------|-------------------------------------|  
| Basic L3-L4 filtering (IP/port)       | Advanced L3-L7 filtering (FQDNs)    |  
| Free (included with resources)        | Paid service                        |  
| Applied to subnets/VM NICs            | Standalone resource                 |  
| No threat intelligence                | Integrated threat intelligence      |  
| Limited rule count (1,000)            | Higher scale (10,000 rules)         |  
| No forced tunneling                   | Supports forced tunneling           |  
*Best Practice*: Use NSGs for basic segmentation, Firewall for perimeter security.

### 43. What is Azure Application Gateway and when would you use it?
**Answer:**  
- **Definition**: Layer 7 (HTTP/HTTPS) load balancer with WAF capabilities  
- **Key Features**:  
  - SSL termination  
  - URL-based routing  
  - Web Application Firewall (WAF)  
  - Redirection and rewrite rules  
- **Use Cases**:  
  - Web app load balancing  
  - Microservices routing (path-based)  
  - SSL offloading  
  - Protection against OWASP top 10 threats  
- **Alternatives**: Azure Load Balancer (L4), Front Door (global)

### 44. How does Azure DNS Private Zones work?
**Answer:**  
- Provides **private DNS resolution** within VNets  
- Integrates with Azure Virtual Network:  
  - Automatic registration of VMs  
  - Custom DNS records for PaaS services (e.g., private endpoints)  
- **Benefits**:  
  - No need for custom DNS servers  
  - Secure name resolution without public DNS  
  - Cross-VNet resolution via VNet peering  
- **Use Case**: Resolving `app.database.windows.net` to private IP in hybrid environment

### 45. What is VNet Peering and what are its limitations?
**Answer:**  
- **Definition**: Connects two VNets (same/different regions) for private traffic  
- **Key Features**:  
  - Non-transitive (A↔B and B↔C doesn't connect A↔C)  
  - No gateways required  
  - Low latency (same region: <2ms)  
- **Limitations**:  
  - Cannot peer VNets with overlapping IP ranges  
  - Max 500 peerings per VNet  
  - Global peering (across regions) has higher latency  
  - No support for on-premises networks (use ExpressRoute/VPN)

### 46. How do you connect on-premises networks to Azure?
**Answer:** Three primary methods:  
1. **Site-to-Site VPN**:  
   - Uses IPSec over public internet  
   - Best for dev/test or low-criticality workloads  
   - Max throughput: 1.25 Gbps per connection  
2. **ExpressRoute**:  
   - Private fiber connection via partner  
   - SLA-backed (99.95%)  
   - Throughput: 50 Mbps - 10 Gbps  
   - Supports private peering for Azure services  
3. **Azure Virtual WAN**:  
   - Hub-spoke architecture for global networks  
   - Integrates VPN/ExpressRoute/Branch connectivity  

### 47. What is Azure Front Door and how does it differ from Traffic Manager?
**Answer:**  
| **Azure Front Door**                  | **Traffic Manager**                 |  
|---------------------------------------|-------------------------------------|  
| Layer 7 (HTTP/S) routing              | Layer 4 (DNS) routing               |  
| SSL offloading, WAF integration       | No SSL offloading                   |  
| Real-time health probes (HTTP)        | Basic health probes (TCP/HTTP)      |  
| URL path-based routing                | Only DNS-level routing              |  
| Global Anycast network (edge POPs)    | DNS-based routing                   |  
| Session affinity support              | No session affinity                 |  
*Use Front Door for web apps, Traffic Manager for general DNS routing.*

### 48. How do you secure traffic between Azure services?
**Answer:**  
- **Private Endpoints**: PaaS services via private IP in your VNet  
- **Service Endpoints**: Secure subnet-to-service traffic (no public internet)  
- **NSGs**: Restrict traffic to specific ports/IPs  
- **Azure Firewall**: Filter traffic to/from internet  
- **Private Link Service**: Expose your services privately  
- **TLS 1.2 Enforcement**: For service-to-service communication  
- **Network Watcher**: Monitor traffic with IP Flow Verify

### 49. What is Azure Bastion and why use it?
**Answer:**  
- **Definition**: Fully managed service for secure RDP/SSH access to VMs  
- **Key Benefits**:  
  - No public IP on VMs  
  - Browser-based access (no client software)  
  - SSH key management via Key Vault  
  - Audit logs via Azure Monitor  
  - TLS 1.2 encrypted session  
- **Use Case**: Secure administrative access without jump boxes or public IPs

### 50. How would you troubleshoot connectivity issues in Azure?
**Answer:** Use **Network Watcher** tools:  
1. **Connection Troubleshoot**: Test end-to-end connectivity  
2. **IP Flow Verify**: Check if NSG blocks traffic  
3. **Packet Capture**: Save network packets for analysis  
4. **NSG Diagnostic Logs**: Analyze traffic flow logs  
5. **Flow Logs**: Visualize traffic patterns in Log Analytics  
*Process*:  
- Verify IP configuration  
- Check NSG rules (effective rules)  
- Test connectivity with Connection Monitor  
- Review DNS resolution  

## IAM (Identity and Access Management)

### 51. What are the key differences between Azure AD and Azure RBAC?
**Answer:**  
| **Azure Active Directory**            | **Azure RBAC**                      |  
|---------------------------------------|-------------------------------------|  
| Identity provider (authentication)    | Authorization system                |  
| Manages users/groups/apps             | Controls resource access            |  
| On-premises sync via AD Connect       | Scope: Management plane only        |  
| SSO for SaaS applications             | Uses roles (Contributor, Reader)    |  
| Conditional Access policies           | Data plane requires other controls  |  
*Relationship*: Azure AD authenticates users, RBAC authorizes their access to Azure resources.

### 52. What is the principle of least privilege in Azure IAM?
**Answer:** Grant users **minimum permissions necessary** to perform their job:  
- Avoid using Owner/Contributor roles globally  
- Prefer custom roles over built-in roles  
- Assign permissions at narrowest scope (e.g., resource not subscription)  
- Use Azure AD PIM for just-in-time privileged access  
- Regularly review access with Access Reviews  
*Example*: Grant "Storage Blob Data Reader" instead of "Storage Account Contributor" for read-only access.

### 53. How do you implement Just-In-Time (JIT) access in Azure?
**Answer:** Using **Azure AD Privileged Identity Management (PIM)**:  
1. Enable PIM for Azure AD roles and Azure resources  
2. Configure eligible assignments (not active by default)  
3. Set approval requirements and multi-factor authentication  
4. Define maximum activation duration (e.g., 4 hours)  
5. Users request access via Azure portal  
6. Admins approve (or auto-approve based on policies)  
7. Access automatically expires after duration  

### 54. What are Azure Custom Roles and when would you use them?
**Answer:**  
- **Definition**: User-defined roles with specific permissions  
- **Use Cases**:  
  - When built-in roles are too broad (e.g., need "VM start/stop only")  
  - Industry compliance requirements (e.g., PCI DSS)  
  - Separation of duties (e.g., DBAs need SQL permissions but not network access)  
- **Implementation**:  
  - Define via JSON with `Actions` and `NotActions`  
  - Assign at subscription/resource group scope  
  - Example: Role allowing only backup operations  

### 55. How does Azure AD Conditional Access work?
**Answer:** Policy-based access control that:  
1. **Identifies**: User, device, location, app  
2. **Evaluates**: Risk level (from Identity Protection)  
3. **Enforces**: Access controls:  
   - Require MFA  
   - Require compliant device (Intune)  
   - Block access from untrusted locations  
   - Require approved client app  
- **Key Scenarios**:  
  - Block legacy authentication  
  - Require MFA for admin roles  
  - Restrict access to specific countries  

### 56. What is the difference between Azure AD B2B and B2C?
**Answer:**  
| **Azure AD B2B**                      | **Azure AD B2C**                    |  
|---------------------------------------|-------------------------------------|  
| Collaboration with external users     | Customer identity management        |  
| Invites external users (e.g., partners)| Scales to millions of consumers     |  
| Uses organization's Azure AD          | Separate tenant for customer auth   |  
| Limited customization                 | Custom branding, user flows         |  
| No social logins (by default)         | Supports social identity providers  |  
| Free (based on Azure AD)              | Consumption-based pricing           |  

### 57. How do you audit Azure IAM changes?
**Answer:**  
- **Azure AD Audit Logs**: Track role assignments, user changes  
- **Azure Activity Log**: Monitor RBAC changes at resource level  
- **Diagnostic Settings**: Stream logs to Log Analytics/Storage  
- **Alerts**: Create alerts for high-risk changes (e.g., Owner role assignment)  
- **Azure Policy**: Detect non-compliant role assignments  
- **Access Reviews**: Periodic verification of access rights  

### 58. What is Azure Managed Identity and why use it?
**Answer:**  
- **Definition**: Automatically managed identity for Azure services  
- **Types**:  
  - System-assigned (tied to resource lifecycle)  
  - User-assigned (reusable across resources)  
- **Benefits**:  
  - Eliminates secret management (no credentials in code)  
  - Automatic credential rotation  
  - Tighter security than service principals  
- **Use Case**: App Service accessing Key Vault without storing secrets  

### 59. How would you secure service principals in Azure?
**Answer:**  
- **Use Managed Identities** instead where possible  
- **For service principals**:  
  - Apply strict RBAC (least privilege)  
  - Use certificates instead of secrets  
  - Rotate credentials automatically (90 days)  
  - Restrict sign-in IPs via Conditional Access  
  - Monitor usage with Azure AD sign-in logs  
  - Store secrets in Key Vault (never in code)  
- **Best Practice**: Use Azure AD App Registration with client certificates  

### 60. What is Azure AD Privileged Access Workstations (PAW)?
**Answer:**  
- **Definition**: Dedicated hardened workstations for privileged tasks  
- **Purpose**: Isolate high-risk admin activities from internet browsing  
- **Azure Integration**:  
  - Conditional Access policies requiring PAW for admin roles  
  - Intune compliance policies for PAW devices  
  - Azure Security Center recommendations  
- **Key Features**:  
  - No internet access  
  - Application whitelisting  
  - Enhanced monitoring  
  - Multi-factor authentication required  


## Storage Services

### 61. What are the key differences between Azure Blob, File, and Disk Storage?
**Answer:**  
| **Service**       | **Use Case**                     | **Access Protocol**     | **Max Size**       |  
|--------------------|----------------------------------|-------------------------|--------------------|  
| **Blob Storage**   | Unstructured data (images, logs) | HTTP/HTTPS (REST)       | 4.75 TB per blob   |  
| **File Storage**   | Lift-and-shift file shares       | SMB/NFS (3.0)           | 100 TB per share   |  
| **Disk Storage**   | VM disks (OS/data)               | iSCSI (via VM)          | 32 TB per disk     |  
| **Queue Storage**  | Messaging (decoupled apps)       | REST                    | 64 KB per message  |  
| **Table Storage**  | NoSQL key-value storage          | OData REST              | 4 TB per partition |  

### 62. How do you choose between Hot, Cool, and Archive storage tiers?
**Answer:** Based on **access patterns and cost**:  
- **Hot**: Frequently accessed data (≥30 days)  
  - Low access cost, high storage cost  
  - Minimum retention: None  
- **Cool**: Infrequently accessed (≥30 days)  
  - 30% lower storage cost than Hot  
  - Higher access cost, minimum 30-day retention  
- **Archive**: Rarely accessed (≥180 days)  
  - 80% lower storage cost than Hot  
  - Highest access cost, minimum 180-day retention  
*Rule of thumb*:  
- Hot: Active application data  
- Cool: Backup copies, older photos  
- Archive: Compliance data, medical archives  

### 63. What is Azure Data Lake Storage Gen2 and how does it differ from Blob Storage?
**Answer:**  
- **Definition**: Blob Storage with hierarchical namespace (like a file system)  
- **Key Differences**:  
  | **Feature**          | **Blob Storage**          | **ADLS Gen2**               |  
  |----------------------|---------------------------|-----------------------------|  
  | Namespace            | Flat (containers/blobs)   | Hierarchical (directories)  |  
  | ACLs                 | Limited (container level) | POSIX-like ACLs             |  
  | Performance          | Standard                  | Optimized for big data      |  
  | Analytics Integration| Basic                     | Native for Synapse, Databricks |  
- **Use Case**: Big data analytics workloads requiring file system semantics  

### 64. How do you secure Azure Storage accounts?
**Answer:**  
- **Network Security**:  
  - Enable "Secure transfer required"  
  - Configure firewall rules (allow specific IPs/VNets)  
  - Use Service Endpoints/Private Endpoints  
- **Authentication**:  
  - Azure AD integration (preferred over keys)  
  - SAS tokens with limited permissions/duration  
  - Always use HTTPS  
- **Encryption**:  
  - Azure Storage Service Encryption (SSE) at rest  
  - Customer-managed keys (CMK) in Key Vault  
- **Additional**:  
  - Immutable storage for legal compliance  
  - Soft delete for blobs (14-day recovery)  

### 65. What is Azure Storage Redundancy and which type should you choose?
**Answer:**  
| **Redundancy**       | **Description**                          | **Durability** | **Use Case**               |  
|----------------------|------------------------------------------|----------------|----------------------------|  
| **LRS**              | 3 copies in single datacenter            | 99.999999999%  | Non-critical data          |  
| **ZRS**              | 3 copies across zones in region          | 99.9999999999% | Stateful apps requiring HA |  
| **GRS**              | 6 copies (LRS in primary + secondary)    | 99.99999999999%| Disaster recovery          |  
| **GZRS**             | 6 copies (ZRS in primary + secondary)    | Highest        | Critical apps with zone HA |  
*Recommendation*:  
- Use ZRS for production apps in multi-zone regions  
- GRS/GZRS for critical data requiring geo-redundancy  

### 66. How would you migrate large datasets to Azure Storage?
**Answer:**  
- **Small datasets (<1TB)**: Azure Storage Explorer or AzCopy  
- **Medium datasets (1-50TB)**:  
  - AzCopy with parallel transfers  
  - Azure Data Box Disk (5TB/75TB devices)  
- **Large datasets (>50TB)**:  
  - Azure Data Box (100TB) or Data Box Heavy (1PB)  
  - Offline import via physical shipment  
- **Ongoing sync**:  
  - Azure File Sync for file shares  
  - Azure Migrate for server migration  

### 67. What are Shared Access Signatures (SAS) and when should you use them?
**Answer:**  
- **Definition**: Time-limited URI granting specific permissions to storage resources  
- **Types**:  
  - **Service SAS**: Access to single resource (blob/container)  
  - **Account SAS**: Access to multiple services (blob, queue, table)  
- **Best Practices**:  
  - Always use HTTPS  
  - Set short expiration times (minutes/hours)  
  - Use stored access policies for revocation  
  - Never embed in client apps (use backend to generate)  
- **Avoid**: When Azure AD authentication is possible  

### 68. How do you monitor Azure Storage performance?
**Answer:**  
- **Metrics in Azure Monitor**:  
  - Ingress/Egress (bytes)  
  - Transactions (count)  
  - Server latency (ms)  
  - Availability (%)  
- **Alerts**: Configure thresholds (e.g., latency > 100ms)  
- **Logs**:  
  - Storage Analytics Logs (detailed operation logs)  
  - Diagnostic settings to Log Analytics  
- **Tools**:  
  - Azure Storage Explorer (performance counters)  
  - Metrics Advisor for anomaly detection  

### 69. What is Azure Blob Versioning and how does it work?
**Answer:**  
- **Definition**: Automatically maintains previous versions of blobs  
- **How it works**:  
  1. Enable versioning on storage account  
  2. When blob is modified/deleted, previous version is saved  
  3. Versions are immutable (can't be modified)  
  4. Access via `versionId` parameter in URL  
- **Benefits**:  
  - Accidental deletion recovery  
  - Ransomware protection  
  - Compliance with data retention policies  
- **Cost**: Storage for all versions (use lifecycle management to delete old versions)  

### 70. How would you implement lifecycle management for Blob Storage?
**Answer:** Create rules to automate tiering/deletion:  
```json
{
  "rules": [
    {
      "enabled": true,
      "name": "move-to-cool",
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": ["blockBlob"],
          "prefixMatch": ["logs/"]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 7 }
          }
        }
      }
    },
    {
      "enabled": true,
      "name": "archive-old",
      "type": "Lifecycle",
      "definition": {
        "filters": { "blobTypes": ["blockBlob"] },
        "actions": {
          "baseBlob": {
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 365 }
          }
        }
      }
    }
  ]
}
```
*Benefits*: Reduces costs by 65%+ through automatic tiering.


## Key Vault

### 71. What is Azure Key Vault and what can it store?
**Answer:**  
- **Definition**: Managed service for safeguarding cryptographic keys and secrets  
- **Stores**:  
  - **Secrets**: Passwords, API keys, connection strings  
  - **Keys**: RSA/EC keys for encryption/signing  
  - **Certificates**: SSL/TLS certs (auto-renewal)  
- **Key Features**:  
  - Hardware Security Modules (HSM) option  
  - Access policies with granular permissions  
  - Audit logging via Azure Monitor  
  - Integration with Azure services (App Service, VMs)  

### 72. How do you secure access to Azure Key Vault?
**Answer:**  
- **Network Security**:  
  - Enable firewall (allow specific IPs/VNets)  
  - Use Private Endpoints (no public internet access)  
- **Access Control**:  
  - Azure RBAC (preferred) or Access Policies  
  - Principle of least privilege (e.g., "Get Secret" only)  
  - Azure AD Conditional Access for vault access  
- **Additional**:  
  - Enable soft delete (90-day recovery)  
  - Purge protection to prevent permanent deletion  
  - Monitor access with diagnostic logs  

### 73. What is the difference between Key Vault Access Policies and Azure RBAC?
**Answer:**  
| **Access Policies**                   | **Azure RBAC**                      |  
|---------------------------------------|-------------------------------------|  
| Legacy model (vault-specific)         | Modern (consistent with Azure)      |  
| Permissions: Keys/Secrets/Certificates| Built-in roles (Key Vault Reader)   |  
| No support for Azure AD groups        | Full Azure AD group support         |  
| No conditional access integration     | Works with Conditional Access       |  
| Being phased out                      | Recommended approach                |  
*Best Practice*: Use Azure RBAC for new deployments.

### 74. How would you rotate secrets in Key Vault?
**Answer:**  
1. **Store secret** with versioning enabled  
2. **Create new version** of secret (never update in-place)  
3. **Update applications** to use new version ID:  
   - App Service: Reference via `@Microsoft.KeyVault(SecretUri=...)`  
   - VMs: Use Managed Identity to fetch secret  
4. **Automate rotation**:  
   - Use Azure Functions with timer trigger  
   - Example: Rotate SQL password monthly  
5. **Cleanup**: Delete old versions after application update  

### 75. What is Key Vault soft delete and purge protection?
**Answer:**  
- **Soft Delete**:  
  - Deleted items retained for 90 days (configurable)  
  - Can be recovered during retention period  
  - Enabled by default (cannot be disabled)  
- **Purge Protection**:  
  - Prevents permanent deletion during soft delete period  
  - Must be enabled manually (recommended for production)  
  - Requires 90-day wait after soft delete before purge  
*Critical*: Without purge protection, malicious users can delete and purge vault contents.

### 76. How do you integrate Key Vault with Azure App Service?
**Answer:** Two methods:  
1. **App Settings Reference**:  
   ```txt
   MySecretSetting = @Microsoft.KeyVault(SecretUri=https://myvault.vault.azure.net/secrets/mysecret/7c735...)
   ```  
   - App Service fetches secret at startup  
   - Automatic refresh every 24 hours  
2. **Managed Identity + SDK**:  
   - Enable system-assigned identity for App Service  
   - Grant identity "Key Vault Secrets User" role  
   - Use Azure SDK in code:  
     ```csharp
     var secretClient = new SecretClient(new Uri("https://myvault.vault.azure.net"), new DefaultAzureCredential());
     KeyVaultSecret secret = await secretClient.GetSecretAsync("mysecret");
     ```  

### 77. What are Key Vault certificates and how do they differ from importing PEM?
**Answer:**  
- **Key Vault Certificates**:  
  - Managed lifecycle (auto-renewal from issuer)  
  - Stored as certificate + private key + public key  
  - Issued by integrated CAs (DigiCert, GlobalSign)  
  - Can configure auto-rotation  
- **Imported PEM**:  
  - Static file import (no auto-renewal)  
  - Must manually manage renewal  
  - Stored as secret (not certificate object)  
*Best Practice*: Use Key Vault certificates for SSL termination with App Gateway.

### 78. How would you audit Key Vault access?
**Answer:**  
1. Enable **diagnostic settings** for Key Vault  
2. Stream logs to:  
   - Log Analytics (for queries/alerts)  
   - Storage account (long-term retention)  
   - Event Hubs (SIEM integration)  
3. Key log types:  
   - **AuditEvent**: All data plane operations (Get, Set, Delete)  
   - **ServiceAudit**: Management plane operations  
4. Critical queries:  
   - Failed access attempts  
   - Secret deletions  
   - Key exports  

### 79. What is the difference between Key Vault keys and HSM-backed keys?
**Answer:**  
| **Standard Keys**                     | **HSM-Backed Keys (Premium Tier)**  |  
|---------------------------------------|-------------------------------------|  
| Software-protected                    | FIPS 140-2 Level 2 validated HSMs   |  
| Lower cost ($0.03/key/month)          | Higher cost ($1/key/month)          |  
| Suitable for most scenarios           | Required for compliance (HIPAA, PCI)|  
| No physical key extraction            | Keys never leave HSM boundary       |  
| Supported by all services             | Limited service support             |  
*Note*: HSM requires Premium Key Vault SKU.

### 80. How do you handle Key Vault dependencies during application startup?
**Answer:** Implement **retry patterns** with exponential backoff:  
```csharp
var retryPolicy = Policy
    .Handle<KeyVaultErrorException>()
    .WaitAndRetryAsync(3, retryAttempt => 
        TimeSpan.FromSeconds(Math.Pow(2, retryAttempt)));

await retryPolicy.ExecuteAsync(async () => {
    var secret = await secretClient.GetSecretAsync("mysecret");
});
```
- **Best Practices**:  
  - Cache secrets in-memory (with refresh timer)  
  - Fail startup if critical secret unavailable  
  - Use local fallback (encrypted) for disaster scenarios  
  - Monitor Key Vault availability with Application Insights  

### 81. What is Key Vault access via Managed Identities?
**Answer:**  
- **Process**:  
  1. Enable system/user-assigned identity on resource (VM, App Service)  
  2. Grant identity permissions in Key Vault (Azure RBAC)  
  3. Resource authenticates to Key Vault using identity  
  4. No secrets stored in code/configuration  
- **Benefits**:  
  - Eliminates credential management  
  - Automatic token refresh  
  - Audit trail shows resource name (not generic service principal)  
- **Implementation**: Use `DefaultAzureCredential` in Azure SDKs  

### 82. How would you recover a deleted Key Vault?
**Answer:**  
1. **List deleted vaults**:  
   ```bash
   az keyvault list-deleted --location eastus
   ```
2. **Recover vault** (within soft delete retention period):  
   ```bash
   az keyvault recover --name myvault --location eastus
   ```
3. **If purge protection enabled**:  
   - Must wait 90 days after soft delete before purge  
   - Cannot recover after purge (permanent deletion)  
*Critical*: Always enable purge protection in production.

### 83. What are Key Vault secrets vs. Azure App Configuration?
**Answer:**  
| **Key Vault Secrets**                 | **Azure App Configuration**         |  
|---------------------------------------|-------------------------------------|  
| For sensitive data (passwords, keys)  | For application settings (non-secret)|  
| Strict access controls                | Simpler RBAC model                  |  
| Audit logs for all access             | Limited auditing                    |  
| Higher latency (5-10ms)               | Lower latency (1-2ms)               |  
| Limited to 10,000 secrets per vault   | Higher scale limits                 |  
