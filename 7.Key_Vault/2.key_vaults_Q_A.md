### **1\. What is Azure Key Vault?**

Azure Key Vault is a cloud-based service by Microsoft Azure used to securely store and manage sensitive information such as cryptographic keys, secrets (like passwords and connection strings), and digital certificates. It helps protect encryption keys and secrets used by cloud applications and services.

### **2\. Why should we use Azure Key Vault instead of storing secrets in configuration files?**

Storing secrets in config files or code increases the risk of exposure through source control leaks or unauthorized access. Key Vault centralizes secret management, enforces access control, supports auditing, and prevents hardcoding, thereby improving security and compliance.

### **3\. What are the three main types of objects stored in Azure Key Vault?**

The three main types are:

*   **Keys**: Cryptographic keys for encryption, signing, etc.
    
*   **Secrets**: Sensitive data like passwords, API keys, tokens.
    
*   **Certificates**: X.509 certificates that can be auto-renewed and managed.
    

### **4\. What is the difference between a key and a secret in Key Vault?**

A **key** is a cryptographic object used for operations like encryption, decryption, or signing. A **secret** is any sensitive data stored as an encrypted string, such as a password or connection string. Keys support cryptographic operations; secrets do not.

### **5\. What is soft delete in Azure Key Vault?**

Soft delete is a security feature that allows you to recover deleted keys, secrets, or certificates within a retention period (default 90 days). It prevents accidental or permanent loss of critical data.

### **6\. What is purge protection?**

Purge protection prevents the permanent deletion of a soft-deleted item or vault until the retention period expires. Once enabled, you cannot manually purge the item, adding an extra layer of protection against malicious attacks.

### **7\. Can you disable soft delete after enabling it?**

No, soft delete cannot be disabled once enabled. It’s a one-way security enhancement designed to protect data.

### **8\. What is the difference between access policies and Azure RBAC for Key Vault?**

*   **Access Policies**: Legacy model where permissions are granted directly to users, apps, or service principals at the vault level.
    
*   **Azure RBAC**: Modern role-based model using built-in roles (e.g., Key Vault Reader, Contributor) that integrates better with Azure’s overall identity and governance system.
    

> **Best Practice**: Use Azure RBAC when possible for consistency and granular control.

### **9\. How do applications authenticate to Azure Key Vault?**

Applications authenticate using Azure Active Directory (Azure AD). They typically use either:

*   **Managed Identity** (recommended): Automatically managed identity for Azure resources.
    
*   **Service Principal**: App registration in Azure AD with client ID and secret/certificate.
    

### **10\. What is a managed identity, and how does it work with Key Vault?**

A managed identity is an automatically managed identity in Azure AD assigned to Azure resources (like App Service or VM). It allows the resource to authenticate to Key Vault without storing credentials. You just grant the identity access via RBAC or access policy.

### **11\. How do you reference a secret from Azure App Service without hardcoding it?**

Use a **Key Vault reference** in the application settings:

1@Microsoft.KeyVault(SecretUri=https://myvault.vault.azure.net/secrets/mysecret/)

App Service fetches the secret at startup using its managed identity.

### **12\. Can you use Key Vault with Infrastructure as Code (IaC) tools like Terraform?**

Yes. In Terraform, you can retrieve secrets from Key Vault using the azurerm\_key\_vault\_secret data source, provided the deployment environment has proper access (e.g., service principal with permissions).

### **13\. What is the purpose of private endpoints in Key Vault?**

Private endpoints allow Key Vault to be accessed over a private IP address within a virtual network, eliminating exposure to the public internet. This enhances security, especially in production environments.

### **14\. How do you secure Key Vault from public internet access?**

*   Disable public network access.
    
*   Configure firewall rules to allow only specific IPs.
    
*   Use **private endpoints** for private connectivity.
    
*   Enable **network service endpoints** for VNet integration.
    

### **15\. What are the pricing tiers in Azure Key Vault?**

*   **Standard**: Software-protected keys and secrets.
    
*   **Premium**: HSM-backed (Hardware Security Module) keys for higher security and compliance (e.g., FIPS 140-2 Level 2/3).
    

> Premium is used for highly sensitive workloads like financial or government systems.

### **16\. What is HSM in the context of Key Vault?**

HSM stands for Hardware Security Module. In Key Vault, the Premium tier uses HSMs to protect cryptographic keys, ensuring they never leave the secure hardware and are resistant to extraction.

### **17\. Can you back up and restore keys in Key Vault?**

Yes, you can back up keys (especially software-protected ones) using the backup command. However, HSM-backed keys cannot be exported or backed up outside the HSM.

### **18\. How do you audit access to secrets in Key Vault?**

Enable **diagnostic settings** to send logs to:

*   Log Analytics (for querying)
    
*   Azure Storage (for long-term retention)
    
*   Event Hubs (for SIEM integration)
    

Logs capture who accessed what, when, and whether the operation succeeded.

### **19\. What permissions are needed to read a secret from Key Vault?**

The identity must have:

*   **Get** permission on secrets (via access policy), or
    
*   **Key Vault Secrets User** role (if using RBAC)
    

Also requires network access and authentication via Azure AD.

### **20\. Can multiple Azure subscriptions access the same Key Vault?**

Yes, but the accessing subscription's resources (e.g., VMs, apps) must have proper **RBAC or access policy permissions** and network connectivity (e.g., private link or firewall rules).

### **21\. How do you rotate secrets automatically in Key Vault?**

Use **Azure Automation**, **Azure Functions**, or **Logic Apps** triggered by a schedule or alert. You can also set up **certificate auto-renewal policies** that trigger notifications before expiry.

### **22\. What is a Key Vault reference?**

A Key Vault reference is a special syntax used in Azure services (like App Service, Functions, or VMs) to dynamically pull a secret from Key Vault at runtime instead of storing it directly.

### **23\. Can you store large files or blobs in Key Vault?**

No. Key Vault is not a data store. Secrets have a size limit (~25 KB). For large encrypted data, encrypt it using a key from Key Vault and store the ciphertext elsewhere (e.g., Blob Storage).

### **24\. How do you integrate Key Vault with Azure DevOps pipelines?**

Use the **Azure Key Vault task** in your pipeline to fetch secrets into pipeline variables. The service connection must have access to the vault via managed identity or service principal.

> This avoids storing secrets in pipeline variables.

### **25\. What happens when a secret is deleted in Key Vault?**

If soft delete is enabled, the secret goes into a **recovered state** and can be restored within the retention period. Otherwise, it's permanently deleted.

### **26\. Can you replicate Key Vault across regions for high availability?**

No, Key Vault is a **regional service**. For redundancy, you must manually create and synchronize separate vaults in different regions.

### **27\. What is the default retention period for soft-deleted items?**

The default retention period is **90 days**. It can be configured between 7–90 days.

### **28\. Can you restore a deleted Key Vault?**

Yes, if soft delete is enabled, you can restore a deleted vault using PowerShell, CLI, or portal within the retention period.

### **29\. How do you set up auto-renewal for SSL certificates in Key Vault?**

When creating a certificate, define a **lifetime action** (e.g., “notify” or “auto-renew”) at a percentage of the validity period (e.g., 80%). Key Vault will trigger the action automatically.

### **30\. What is the role of Azure AD in Key Vault access?**

Azure AD handles **authentication**. Only identities (users, apps, managed identities) authenticated via Azure AD can request access. **Authorization** is then enforced via RBAC or access policies.

### **31\. Can on-premises applications access Azure Key Vault?**

Yes, if they can authenticate via Azure AD (e.g., using a service principal) and have network connectivity (e.g., via ExpressRoute, VPN, or public internet with firewall rules).

### **32\. What is the maximum size of a secret in Key Vault?**

Approximately **25,000 characters** (25 KB). Larger values should be stored elsewhere (e.g., storage), with only the encryption key in Key Vault.

### **33\. How do you grant a VM access to Key Vault?**

Assign a **system-assigned or user-assigned managed identity** to the VM, then grant that identity **Get** permissions on secrets/keys via access policy or RBAC.

### **34\. What is customer-managed key (CMK) encryption for Key Vault?**

CMK allows you to encrypt the Key Vault’s own encryption keys using a key you control in another Key Vault. This adds an extra layer of control and meets strict compliance requirements.

> Note: CMK encrypts the encryption keys, not the data directly.

### **35\. What are some best practices for using Azure Key Vault?**

*   Use **managed identities** instead of secrets.
    
*   Enable **soft delete** and **purge protection**.
    
*   Use **private endpoints** in production.
    
*   Apply **least privilege access** via RBAC.
    
*   Separate vaults by **environment** (dev, prod).
    
*   Enable **logging and monitoring**.
    
*   Automate **secret rotation** and **certificate renewal**.
    

### **36\. How do you monitor Key Vault performance and usage?**

Use **Azure Monitor** to view metrics like:

*   Number of requests
    
*   Throttled requests
    
*   Latency
    
*   Authentication failures
    

Set up alerts based on thresholds.

### **37\. Can you use Key Vault to encrypt data at rest in your application?**

Yes. Retrieve a key from Key Vault and use it to encrypt sensitive data in your application before storing it in databases or storage. The key never leaves Key Vault if HSM-protected.

### **38\. What is the difference between a self-signed and CA-issued certificate in Key Vault?**

*   **Self-signed**: Generated and signed by Key Vault itself. Suitable for internal/testing use.
    
*   **CA-issued**: Signed by a trusted Certificate Authority (e.g., DigiCert). Used for public websites and production SSL.
    

Key Vault supports both and can manage renewal for CA-issued certs.