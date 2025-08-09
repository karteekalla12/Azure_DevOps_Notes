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
