### Conceptual & Core Principles

1.  **Q: How would you explain Azure IAM's relationship with Azure Resource Manager (ARM) to a non-technical stakeholder?****A:** Azure IAM is the "bouncer" for Azure resources. When you try to access a resource (like a VM or database), ARM checks your Azure AD identity against IAM rules. It asks: "Do you have the right role (like 'Contributor') for _this specific resource_?" If yes, ARM lets you in; if not, you get locked out. IAM defines the rules, ARM enforces them.
    
2.  **Q: Why can't Azure AD roles (like Global Admin) directly manage Azure resource access?****A:** Azure AD roles control _directory-level_ actions (user management, app registrations), while Azure IAM/RBAC controls _resource-level_ actions (starting VMs, reading storage). Global Admins _can_ assign RBAC roles but don't automatically get Owner on resources. This separation prevents directory admins from accidentally breaking production resources – a critical security boundary.
    
3.  **Q: What's the practical impact of RBAC's "allow" model versus a "deny" model?****A:** RBAC is _permissive by default_: If no role grants access, you're denied. But if _any_ role assignment allows an action, you get access – even if other roles deny it. This means **deny assignments are rarely needed** (they override allows). Most security is achieved by _not granting_ excessive permissions upfront, not by adding denies later.
    
4.  **Q: When would you use Azure AD Privileged Identity Management (PIM) over standard RBAC assignments?****A:** **Always for privileged roles** like Owner or User Access Administrator. PIM enforces just-in-time access: users request activation, get approval/MFA, and access expires automatically. This minimizes "standing privileges" – the #1 cause of cloud breaches. For day-to-day roles (e.g., Reader), standard RBAC is sufficient.
    
5.  **Q: How do Azure Management Groups simplify IAM at enterprise scale?****A:** They let you apply RBAC/Policy _across multiple subscriptions_. Example: Assign Security Reader to the "Compliance Team" group at the Management Group level. Instantly, they can audit _all_ subscriptions below it. Without MGs, you'd manually assign roles to every subscription – error-prone and unscalable.
    

### DevOps & Pipeline Security

1.  **Q: Your pipeline fails with "AuthorizationFailed" when deploying AKS. What RBAC role is** _**actually**_ **required?****A:** Azure Kubernetes Service Cluster Admin Role _or_ Azure Kubernetes Service Cluster User Role – **not** generic Contributor. Contributor manages the AKS _resource_ (scaling node pools), but cluster-level actions (like kubectl apply) require Kubernetes RBAC, granted via these _specific_ Azure roles. A classic DevOps pitfall.
    
2.  **Q: How do you grant a Function App (using Managed Identity) access to** _**only**_ **secrets starting with "prod-" in Key Vault?****A:** Use **Key Vault Access Policies** (not RBAC) for granular secret control. Assign the Function App's Managed Identity the "Get" permission _only_ for secrets with names matching prod-\*. RBAC roles like Key Vault Secrets User grant access to _all_ secrets – too broad for production.
    
3.  **Q: Why is storing pipeline SPN secrets in Azure DevOps Library variables** _**still risky**_ **even with "secret" masking?****A:** Masked variables can leak via:
    
    *   Debug logs (if system.debug=true)
        
    *   Pipeline task output (e.g., echo $(SPN\_SECRET))
        
    *   Compromised agent machines**Secure fix:** Use Azure Key Vault + Managed Identity. The pipeline fetches secrets _at runtime_ with no credentials stored in DevOps.
        
4.  **Q: How would you implement "break-glass" access for a pipeline that** _**must**_ **have elevated permissions?****A:** 1) Create a _separate_ pipeline for emergency tasks. 2) Use PIM to make its SPN _eligible_ for a custom role (e.g., Emergency-Deploy). 3) Require 2 approvers + MFA for activation. 4) Limit activation to 15 minutes. Never bake emergency access into daily pipelines.
    
5.  **Q: What RBAC role is needed for Terraform to manage Azure Policy assignments?****A:** Resource Policy Contributor (Microsoft.Authorization/policyassignments/\*). Owner works but is excessive. Crucially, this role _must_ be assigned at the **Management Group or Subscription level** – Policy assignments can't be scoped to Resource Groups.
    

### Troubleshooting & Real-World Scenarios

1.  **Q: A user in the "Dev" group has Contributor on a Resource Group but can't delete a VM. Why?****A:** Two likely causes:
    
    *   **Resource Lock:** A "CanNotDelete" lock exists on the VM or Resource Group.
        
    *   **Azure Policy:** A policy (e.g., "Deny VM deletion") blocks the action. Check Activity Log for "ForbiddenByPolicy" errors. RBAC grants permission, but locks/policies can override it.
        
2.  **Q: How do you diagnose why a Managed Identity can't access Storage, even with Storage Blob Data Contributor?****A:** 1) Verify the MI is **assigned to the correct resource** (e.g., Function App, not a test VM). 2) Check if **Hierarchical Namespace** is enabled on Storage (requires Storage Blob Data Owner). 3) Confirm **Firewall rules** aren't blocking the MI's IP. 4) Validate **Key Vault references** aren't failing (if using SAS tokens).
    
3.  **Q: After deleting a user, their RBAC assignments still appear as "Unknown principal". Should you worry?****A:** **No immediate risk** – deleted users can't authenticate. But:
    
    *   It clutters audits and inflates role assignment counts.
        
    *   If the UPN is reused, the _new_ user inherits those permissions!**Fix:** Run Remove-AzRoleAssignment -ObjectId to clean up. Automate this with Azure AD Connect sync rules.
        
4.  **Q: Your az storage blob upload command fails with 403, but Portal shows Storage Blob Data Contributor. Why?****A:** The CLI uses **Azure AD auth by default** for blob operations. Storage Blob Data Contributor works _only_ if:
    
    *   You ran az login with a user/SPN (not az storage login).
        
    *   The Storage account has **"Azure AD authorized"** selected in _Firewalls and virtual networks > Authentication_.**Fix:** Use --auth-mode login in the CLI command.
        

### Advanced Security & Design

1.  **Q: When would you choose Azure AD Authorization Manager (preview) over standard RBAC?****A:** For **extreme granularity** where built-in/custom roles aren't enough. Example: Allow "VM Start/Stop" _only_ for VMs tagged CostCenter=Marketing. Authorization Manager uses JSON policies to define conditions like "resource.tags.CostCenter": "Marketing", reducing the need for 50+ custom roles.
    
2.  **Q: How do Azure Lighthouse and IAM interact for multi-tenant management?****A:** Lighthouse _delegates_ RBAC management. Your company (MSP) gets Owner on a _customer's_ subscription via Lighthouse. But:
    
    *   Customer retains ultimate control (can revoke delegation).
        
    *   Your MSP admins **must** be assigned roles _within Lighthouse_ – standard RBAC assignments in the customer's tenant won't work. It's RBAC with an extra delegation layer.
        
3.  **Q: Why is assigning User Access Administrator more dangerous than Owner in some cases?****A:** User Access Administrator can grant _itself_ Owner by creating a role assignment. Owner can't self-escalate – it already has full access. This makes User Access Admin the **most privileged role** for security reviews. Always restrict it to PIM with multi-approver activation.
    
4.  **Q: What's the IAM impact of enabling "Enable system assigned managed identity" on an App Service?****A:** Azure creates a hidden Azure AD app registration. You **must** then:
    
    1.  Assign RBAC roles _to this new identity_ (e.g., Reader on a Storage account).
        
    2.  Update code to use DefaultAzureCredential (no secrets needed!).**Critical:** The App Service's _application settings_ remain accessible to anyone with Owner/Contributor – securing the MI doesn't protect config data.
        

### Identity Federation & Edge Cases

1.  **Q: Can GitHub Actions use Azure Managed Identities? How?****A:** **Yes, via Workload Identity Federation.** Steps:
    
    1.  Configure Azure AD App Registration with federated credentials (issuer = token.actions.githubusercontent.com, subject = repo:org/repo:ref:refs/heads/main).
        
    2.  In GitHub workflow, use azure/login@v1 with client-id and OIDC.**Result:** No SPN secrets – GitHub tokens directly assume Azure roles. The future of secure pipelines.
        
2.  **Q: Why might an Azure AD B2B guest user see "Access denied" even with Owner?****A:** Two common reasons:
    
    *   **Multi-Tenant Confusion:** The guest is signed into _their home tenant_, not your Azure AD tenant. They must use ?tenant= in the login URL.
        
    *   **Conditional Access:** Policies blocking guest access (e.g., "Require compliant device") – check sign-in logs for "blocked by policy".
        
3.  **Q: How does Azure handle RBAC for resources moved between Resource Groups?****A:** **Role assignments** _**do not**_ **move with the resource.** If you move a VM from RG-A to RG-B:
    
    *   Existing assignments on the VM (scoped to VM) remain.
        
    *   Assignments inherited from RG-A _stop applying_.
        
    *   Assignments on RG-B _start applying_.**Always re-audit permissions after moves!**
        
4.  **Q: What permission is required to** _**view**_ **role assignments without modifying them?****A:** Microsoft.Authorization/roleAssignments/read – included in the **Reader** role. Important: Reader can see _who has access_ but can't change assignments. For pure auditing, avoid granting User Access Administrator.
    
5.  **Q: When does Azure AD Application Consent impact Azure IAM?****A:** When your app (e.g., pipeline SPN) needs delegated permissions (like Group.Read.All). If **admin consent** isn't granted:
    
    *   Users get consent prompts (breaks headless pipelines).
        
    *   SPN authentication fails with "Insufficient privileges".**Fix:** Grant admin consent in Azure AD > App Registrations > API Permissions.
        

### Policy, Compliance & Governance

1.  **Q: Why is RBAC ineffective for protecting Azure AD objects (users, groups)?****A:** Azure AD objects are managed by **Azure AD roles** (Global Admin, Privileged Role Admin), _not_ Azure RBAC. RBAC roles like Owner _can_ assign Azure AD roles but don't directly control user/group permissions. This is a frequent source of confusion – always check Azure AD > Roles and administrators.
    
2.  **Q: How do Azure Blueprints handle RBAC during deployment?****A:** Blueprints apply RBAC _as part of the artifact_. When assigning a blueprint:
    
    *   You specify which users/groups get which roles on the _new_ resources.
        
    *   This happens **after** resource creation, so the deployment SPN needs Owner on the target scope.**Risk:** Over-permissive blueprint assignments can bypass your RBAC design.
        

### Real-World Pitfalls & Best Practices

1.  **Q: What's the hidden risk of using az login with a user account in CI/CD pipelines?****A:** **Session token theft.** If an attacker compromises the pipeline agent, they can steal the Azure CLI token cache (usually in ~/.azure/accessTokens.json) and impersonate the user for _hours_. Always use SPNs or Managed Identities – never user accounts in pipelines.
    
2.  **Q: Why should you avoid assigning roles at the Subscription root scope?****A:** It creates **unintended inheritance**. Example: Assigning Reader to "All Employees" at the Subscription level lets _everyone_ see _all_ resources (including production secrets in Storage). **Always start with Resource Group scope** and escalate only if necessary.
    
3.  **Q: What happens if you assign a role to an Azure AD group that contains nested groups?****A:** Azure **flattens group membership**. If Group-A (with Contributor) contains Group-B, and User-X is in Group-B, User-X gets Contributor. But:
    
    *   Nesting depth is limited (15 levels).
        
    *   Changes propagate slowly (up to 30 mins).**Best Practice:** Avoid deep nesting; use flat groups for RBAC.
        

### Critical Thinking & Strategy

1.  **Q: How would you design IAM for a zero-trust environment in Azure?****A:** Four pillars:
    
    1.  **Least Privilege:** Custom roles + Conditions (e.g., "Only access KV from VNet").
        
    2.  **Just-in-Time Access:** PIM for all privileged roles (max 2-hour activation).
        
    3.  **Non-Human Identity Security:** Managed Identities > SPNs; secrets in Key Vault.
        
    4.  **Continuous Verification:** Conditional Access (device compliance, MFA) + real-time audit alerts.
        
2.  **Q: Your audit finds 200+ Owner assignments. How do you remediate without breaking workflows?****A:** 1) **Classify:** Separate human vs. SPN/MI assignments. 2) **SPNs:** Replace with least-privilege roles (e.g., Storage Blob Data Contributor). 3) **Humans:** Move to PIM _eligible_ assignments. 4) **Monitor:** Use Azure AD Access Reviews to auto-remind owners quarterly. 5) **Prevent:** Enforce Azure Policy denying new Owner assignments.
    
3.  **Q: What's the single most overlooked IAM risk in Azure migrations?****A:** **Carrying over on-prem AD groups with excessive permissions.** Example: Migrating a "Domain Admins" group and assigning it Owner on Azure resources. **Fix:** Never map on-prem groups1:1. Recreate _new_ Azure-specific groups with minimal roles during migration.
    
4.  **Q: How do you prove compliance for "separation of duties" in Azure IAM?****A:** 1) Use **Access Reviews** for quarterly attestations. 2) Enforce **PIM activation logs** (who requested what, when, why). 3) Implement **Azure Policy** to prevent conflicts (e.g., "No user can have both Deployer and Approver roles"). 4) Export **Azure AD audit logs** showing role assignment history. Auditors love automated evidence.