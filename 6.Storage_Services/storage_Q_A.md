# Azure Storage Interview Questions & Answers

### **Q1. What are the five core Azure Storage services, and what primary data type does each handle?**

**Answer:** The five core Azure Storage services are:  
- **Blob Storage** for unstructured data like images, videos, backups, and logs.  
- **Azure Files** for fully managed file shares using SMB or NFS, great for replacing on-prem file servers.  
- **Queue Storage** for asynchronous messaging between application components, such as a web app sending tasks to a background worker.  
- **Table Storage** for NoSQL key-value data like user profiles or device telemetry.  
- **Managed Disks**, which provide persistent block storage for Azure Virtual Machines, abstracting the underlying storage management.

### **Q2. Explain the key differences between Azure Blob Storage and Azure Files. When would you choose one over the other?**

**Answer:** Blob Storage is ideal for storing large amounts of unstructured data accessed over HTTP, like images or backup files. It uses REST APIs and isn’t directly mountable. Azure Files provides a real file share you can mount like a network drive using SMB or NFS. For legacy apps needing UNC paths (`\\server\share`), Azure Files is preferred. Blob is suited for web content, Azure Files for shared config files across VMs or hybrid environments.

### **Q3. Describe the three Blob Storage access tiers: Hot, Cool, and Archive. What are their primary cost and performance trade-offs, and ideal use cases?**

**Answer:**  
- **Hot tier:** Data accessed frequently; highest storage cost but lowest access cost and fastest retrieval. Use for active app data.  
- **Cool tier:** Data accessed less often (e.g., backups, older logs); lower storage cost but higher access cost and latency.  
- **Archive tier:** Rarely accessed data like compliance archives; cheapest to store but hours to retrieve and highest retrieval cost.  
Lifecycle policies automate moving data between tiers based on age.

### **Q4. What is the difference between Block Blobs, Append Blobs, and Page Blobs? Which one is used for Azure VM disks?**

**Answer:**  
- **Block Blobs:** Best for large files (videos, backups), uploaded in blocks.  
- **Append Blobs:** Optimized for append-only scenarios like logging.  
- **Page Blobs:** Designed for random read/write, used for Azure VM Managed Disks.

### **Q5. What is Azure Data Lake Storage Gen2? How does it relate to standard Blob Storage?**

**Answer:** ADLS Gen2 is Blob Storage with hierarchical namespace enabled, allowing real folders and subfolders, which is ideal for big data analytics tools like Azure Synapse or Databricks. Enabled on StorageV2 accounts.

### **Q6. Explain Locally Redundant Storage (LRS) vs. Zone-Redundant Storage (ZRS) vs. Geo-Redundant Storage (GRS). What are their durability guarantees and recovery implications?**

**Answer:**  
- **LRS:** 3 copies in one datacenter; cheapest but vulnerable to datacenter failure.  
- **ZRS:** Replicates across 3 availability zones in the same region; survives datacenter outages.  
- **GRS:** Asynchronously copies data to a secondary region far away; protects against regional disasters.

### **Q7. What is the purpose of Azure Queue Storage? Describe its core messaging pattern — producers, consumers, and visibility timeout.**

**Answer:** Decouples application components. Producers add messages; consumers process them. Messages become invisible during processing (visibility timeout) to prevent duplicate processing. If not deleted after processing, messages reappear.

### **Q8. What are the core components of an entity in Azure Table Storage — PartitionKey, RowKey, and Properties? Why is partitioning strategy critical for performance?**

**Answer:**  
- **PartitionKey + RowKey:** Form a unique ID for each entity.  
- **Properties:** Additional data fields.  
Partitioning ensures queries within partitions are fast; uneven partitions cause bottlenecks.

### **Q9. How does Azure Managed Disks simplify storage management for Virtual Machines compared to unmanaged disks?**

**Answer:** Managed Disks abstract storage accounts, handle replication, scaling, and availability. Simplifies snapshot, backup, and encryption operations, reducing operational complexity.

### **Q10. What is the maximum size for a standard Azure File Share?**

**Answer:** 5 TiB for Standard tier. Premium Files can support up to 100 TiB with large file shares enabled.

### **Q11. How do you secure an Azure Storage Account to prevent public internet access? Mention at least three methods.**

**Answer:**  
- Disable public blob access.  
- Restrict network access via IP/firewall rules.  
- Use Private Endpoints to keep traffic within a VNet.  
- Use Azure AD or Managed Identities for access control.

### **Q12. What is the difference between using Shared Access Signatures (SAS) and Azure Role-Based Access Control (RBAC) for granting access to storage resources?**

**Answer:** SAS provides temporary token-based access, easy to share but hard to revoke. RBAC uses Azure AD roles for long-term, auditable, identity-based access control.

### **Q13. Explain Azure Storage Service Encryption (SSE). What are the options for managing the encryption keys — Microsoft-managed vs. Customer-Managed Keys (CMK)?**

**Answer:** SSE encrypts data at rest by default. Keys can be Microsoft-managed or Customer-Managed via Azure Key Vault for more control and compliance.

### **Q14. What is Soft Delete for Blobs? Why is it considered a critical feature for operational safety?**

**Answer:** Keeps deleted blobs recoverable for a set retention period. Prevents accidental data loss and aids recovery.

### **Q15. What is Blob Versioning? How does it differ from Soft Delete, and how would you use both together?**

**Answer:** Versioning saves previous blob versions on overwrite. Soft Delete recovers deleted blobs. Use together for robust data protection.

### **Q16. What are Immutable Blob Policies (WORM)? Which compliance standards do they help meet?**

**Answer:** Write Once Read Many policies prevent modification/deletion for compliance (e.g., SEC 17a-4, HIPAA, GDPR).

### **Q17. How can you optimize the performance and cost of a large-scale logging system storing data in Blob Storage?**

**Answer:** Compress logs, use lifecycle policies to tier storage, organize data for efficient queries, and use parallel uploads.

### **Q18. What factors influence the performance — IOPS and throughput — of Azure Premium Files? How does it differ from Standard Files?**

**Answer:** Premium Files use SSDs with scalable IOPS; Standard Files use HDDs with burstable performance suitable for general use.

### **Q19. What is Azure Storage Scale Target? How does it impact the design of high-scale applications using Table Storage or Queues?**

**Answer:** Scale target is 20,000 TPS per account. High-scale apps should shard across accounts or design partitions to avoid throttling.

### **Q20. How can you monitor the performance and health of an Azure Storage Account? Name specific tools and metrics.**

**Answer:** Use Azure Monitor for metrics (transactions, latency, errors), Storage Analytics Logs, and Azure Advisor for recommendations.

### **Q21. How would you migrate 50 TB of on-premises file server data to Azure Files with minimal downtime?**

**Answer:** Use Azure Data Box for initial transfer, then Azure File Sync for continuous sync and minimal downtime during cutover.

### **Q22. Describe how Azure Storage is used as an artifact repository in a CI/CD pipeline — like Azure Pipelines. What features, like lifecycle policies, are important?**

**Answer:** Store build outputs in Blob Storage, enable lifecycle policies for cost management, use versioning and soft delete for recovery, control access via RBAC/SAS.

### **Q23. Why is storing Terraform state in Azure Blob Storage a common practice? What security and management features — like versioning or immutable policies — should you enable?**

**Answer:** Provides shared state backend; enable versioning, soft delete, restrict access via firewall and Managed Identity, enable immutable policies if needed.

### **Q24. How does Azure File Sync work? What are its primary benefits for hybrid cloud scenarios?**

**Answer:** Syncs on-prem file servers with Azure Files, using cloud as source of truth and caching frequently used files locally. Benefits: disaster recovery, tiered storage, unified namespace.

### **Q25. Explain how Point-in-Time Restore for Blobs works and when you would use it instead of Soft Delete or Versioning.**

**Answer:** Rolls back entire storage account to a point in time, useful for large-scale corruption incidents. Requires versioning to be enabled.

### **Q26. What is the Change Feed in Blob Storage? Provide a real-world use case for it.**

**Answer:** Log of all blob changes used for event-driven processing, e.g., triggering Azure Functions to process new blobs.

### **Q27. How can you enforce that all new blobs written to a container are automatically moved to the Cool tier after 30 days?**

**Answer:** Set up a lifecycle management policy targeting blobs older than 30 days to transition to Cool tier.

### **Q28. What are the key considerations when choosing between Azure Table Storage and Azure Cosmos DB (Table API) for a new application?**

**Answer:** Use Table Storage for simple, low-cost, predictable workloads; Cosmos DB for low latency, global distribution, and guaranteed throughput with higher cost.

### **Q29. Your application is experiencing high latency when reading blobs from a Storage Account in West US 2. The client is in Europe. What are potential causes and solutions?**

**Answer:** Network distance causes latency. Solutions: use Azure CDN, enable RA-GRS for reads from secondary region, or move storage closer to clients.

### **Q30. You need to store user-uploaded profile pictures — small images, accessed frequently. Which storage service and tier would you choose, and why?**

**Answer:** Blob Storage in Hot tier with Azure CDN for fast delivery; Azure Files is unnecessary unless SMB access is required.

### **Q31. A developer accidentally deleted a critical container holding application logs. Soft Delete is enabled with a 14-day retention period. How do you recover the container?**

**Answer:** Use Azure Portal “Show deleted” option on containers, select the deleted container, and click “Undelete” within retention window.

### **Q32. Your Azure Function processing Queue messages is taking too long, causing messages to become visible again and be processed multiple times. How can you prevent this?**

**Answer:** Increase visibility timeout beyond max processing time and make the function idempotent to handle duplicate processing safely.

### **Q33. You are designing a system to handle 10,000 concurrent users uploading small files (1–5 MB) to Blob Storage. What storage account configuration — performance, replication, networking — and client-side techniques like chunking would you recommend for optimal performance and cost?**

**Answer:** Use StorageV2 with Standard performance and ZRS replication, restrict networking with Private Endpoints, use parallel uploads with SDK/AzCopy, shard containers to distribute load, and lifecycle policies to tier older files.

