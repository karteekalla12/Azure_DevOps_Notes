1.  _Users & Groups:_ Human identities (employees, partners) and collections (Security Groups, Microsoft 365 Groups).
    
2.  _Service Principals & Managed Identities:_ Non-human identities for applications/services (critical for DevOps).
    
3.  _Authentication:_ Verifying identity (passwords, MFA, SSO).
    
4.  _Conditional Access:_ Policy-based access controls (e.g., "Require MFA from untrusted networks").
    
5.  **Role-Based Access Control (RBAC):** The core authorization model _on top_ of Azure AD identities. Defines _permissions_ via:
    

7.  _Roles:_ Collections of permissions (e.g., Reader, Contributor, Owner, custom roles). Permissions are defined as Actions (what you _can_ do) and NotActions (what you _cannot_ do, overrides Actions).
    
8.  _Scopes:_ The Azure resource hierarchy where a role assignment applies (Management Group > Subscription > Resource Group > Individual Resource).
    
9.  _Role Assignments:_ The binding of a _Security Principal_ (User, Group, Service Principal, Managed Identity) to a _Role_ at a specific _Scope_.
    
10.  II. Applying the W/H Questions to Azure IAM
    
11.  **WHO (Security Principals):**
    
12.  Who gets access? Users, Groups, Service Principals (apps), Managed Identities (Azure resources like VMs, Functions).
    
13.  _Groups are strongly preferred for manageability._
    
14.  **WHAT (Permissions via Roles):** What can they do? Defined by RBAC roles. Reader (view), Contributor (create/modify/delete _except_ access control), Owner (full control _including_ access control), User Access Administrator (manage access control). Custom roles for precise needs.
    
15.  **WHERE (Scope):** Where does the access apply? Crucial hierarchy: Management Group (broadest), Subscription, Resource Group, Specific Resource (most granular). Assignments at higher levels inherit down.
    
16.  **WHEN (Conditions & PIM):** When is access granted? Standard RBAC is persistent. For just-in-time (JIT) access:
    
    *   _Azure AD Privileged Identity Management (PIM):_ Requires approval & MFA to _activate_ eligible roles (like Owner) for a limited time.
        
    *   _Conditional Access Policies:_ Enforce conditions (device compliance, location, risk level) _before_ granting access, even if RBAC allows it.
        
17.  **HOW (Authentication & Authorization Flow):**
    
    1.  User/App authenticates via Azure AD (proves _who_ they are).
        
    2.  Azure AD issues token containing identity & group memberships.
        
    3.  User/App requests Azure resource operation (e.g., Microsoft.Storage/storageAccounts/write).
        
    4.  Azure Resource Manager (ARM) checks:
        
        *   Is the token valid? (AuthN)
            
        *   Does the user have a role assignment at the resource's scope granting the required permission? (AuthZ via RBAC)
            
        *   Do Conditional Access policies allow the session? (AuthZ)
            
    5.  Access granted or denied (AuthorizationFailed error).
        
18.  III. Azure IAM in DevOps: Critical Integration Points
    
19.  **Non-Human Identities are King:**
    

21.  **Service Principals (SPNs):** Manually created identities for CI/CD pipelines (Azure DevOps, GitHub Actions, Jenkins). _Must be granted minimal RBAC roles_ (e.g., Contributor on a specific Resource Group for deployment). _Secrets/Certs must be securely managed (Key Vault!)._
    
22.  **Managed Identities (MIs):** **Preferred method.** Azure _automatically_ creates and manages the identity for PaaS services (App Service, Functions, AKS, VMs). No secrets to manage! Assign RBAC roles directly to the MI. _Huge security win for DevOps._
    
23.  **Pipeline Security:**
    
    *   **Principle of Least Privilege (PoLP):** Pipeline SPN/MI should _only_ have permissions needed for its specific task (e.g., Reader for validation, Contributor on _only_ the target RG for deployment). **NEVER use Owner in pipelines.**
        
    *   **Secure Secret Handling:** Pipeline secrets (SPN passwords/certs) MUST come from Azure Key Vault, not hardcoded in YAML/variables.
        
    *   **Infrastructure as Code (IaC):** Terraform/ARM/Bicep define RBAC role assignments _as code_. Crucial for reproducibility and auditability. _Validate IaC templates for excessive permissions._
        
24.  **Common DevOps Scenarios:**
    
    *   _Deploying to Azure:_ Pipeline SPN/MI needs Contributor on target RG/Subscription.
        
    *   _Reading Secrets from Key Vault:_ Pipeline SPN/MI needs Key Vault Secrets User role on the KV.
        
    *   _Managing Container Registries:_ Pipeline needs AcrPush/AcrPull roles on the ACR.
        
    *   _Terraform State:_ Backend storage (e.g., Storage Account) requires specific RBAC (Storage Blob Data Contributor) for the TF service principal.
        
25.  IV. Creating Azure IAM Role Assignment via UI: Step-by-Step Breakdown
    
26.  _(Navigate to any Azure Resource -> Access control (IAM) blade)_
    
27.  **Access Control (IAM) Blade:**
    

29.  _Role assignments tab:_ Shows _all_ current role assignments for this scope (resource/RG/subscription). Columns: Role, Type (User, Group, ServicePrincipal, MSI), Name, Status (Active, Eligible via PIM).
    
30.  _Custom roles tab:_ Manage custom RBAC roles defined at this scope.
    
31.  _Add button:_ Primary entry point for new assignments.
    
32.  **Add Role Assignment:**
    
    *   _Step 1: Role Selection:_
        
        *   **Role List:** Comprehensive list of built-in roles (filterable by name, service). _Critical to understand what each role actually permits!_ Hover for description. _Avoid Owner/Contributor if possible; seek least privilege._
            
        *   _Custom Roles:_ Appear here if defined at this scope or higher.
            
33.  **Step 2: Members:**
    
    *   _Assign access to:_ Dropdown: Azure AD user, group, or service principal OR Managed identity. _Choosing the right type is vital._
        
    *   _Select members:_ Search box. **Best Practice:** _Always select a Group_, not individual users. Add users to groups; assign roles to groups. Easier management, auditing, and onboarding/offboarding.
        
    *   _Managed Identity Selection:_ If chosen, search for the _resource_ (VM, Function App) whose Managed Identity you want to assign. The MI is tied to that resource.
        
34.  **Step 3: Conditions (Preview - Highly Recommended for Security):**
    
    *   _Create condition:_ Allows defining _attribute-based_ access rules _on top_ of RBAC. _Game-changer for security._
        
    *   _Example:_ "Only allow VM start/stop if the VM tag Environment equals Dev". Uses a JSON-like expression builder. _Reduces blast radius significantly._
        
35.  **Step 4: Review + assign:**
    

37.  _Summary:_ Crucial validation step! Shows:
    
    *   **Selected role:** (e.g., Storage Blob Data Contributor)
        
    *   **Assign access to:** (e.g., Group - DevOps-Deploy-Team)
        
    *   **Scope:** (e.g., Resource group - prod-rg). _DOUBLE-CHECK THIS SCOPE!_
        
    *   **Conditions:** (If applied)
        
38.  _Validation:_ Azure checks if the assigner has permission to make this assignment (needs User Access Administrator or Owner at the target scope).
    
39.  _Assign button:_ Final confirmation. _No undo!_ Permissions take effect within minutes.