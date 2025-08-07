#### **Q1. What are the five core Azure Storage services, and what primary data type does each handle?**

**Answer:**The five core Azure Storage services are:**Blob Storage** for unstructured data like images, videos, backups, and logs.**Azure Files** for fully managed file shares using SMB or NFS, great for replacing on-prem file servers.**Queue Storage** for asynchronous messaging between application components, such as a web app sending tasks to a background worker.**Table Storage** for NoSQL key-value data like user profiles or device telemetry.And **Managed Disks**, which provide persistent block storage for Azure Virtual Machines, abstracting the underlying storage management.

#### **Q2. Explain the key differences between Azure Blob Storage and Azure Files. When would you choose one over the other?**

**Answer:**Blob Storage is ideal for storing large amounts of unstructured data that’s accessed over HTTP, like images for a website or backup files. It uses REST APIs and is not directly mountable. Azure Files, on the other hand, provides a real file share that you can mount like a network drive using SMB or NFS. So if you're migrating a legacy app that expects a UNC path like \\\\server\\share, Azure Files is the way to go. I’d use Blob for web content and Azure Files for shared configuration files across VMs or hybrid environments.

#### **Q3. Describe the three Blob Storage access tiers: Hot, Cool, and Archive. What are their primary cost and performance trade-offs, and ideal use cases?**

**Answer:**The **Hot tier** is for data accessed frequently — it has the highest storage cost but the lowest access cost and fastest retrieval. I use it for active application data. The **Cool tier** is for data accessed less often, like backups or older logs. It has lower storage cost but higher access cost and slightly higher latency. Finally, the **Archive tier** is for data rarely accessed, like compliance archives. It’s the cheapest to store but takes hours to retrieve and has the highest retrieval cost. I always pair these with lifecycle policies to automatically move data between tiers based on age.

#### **Q4. What is the difference between Block Blobs, Append Blobs, and Page Blobs? Which one is used for Azure VM disks?**

**Answer:****Block Blobs** are best for large files like videos or backups — they’re split into blocks for efficient upload. **Append Blobs** are optimized for append-only scenarios like logging, where you only add data and never modify it. **Page Blobs** are designed for random read/write operations, and they’re exactly what Azure uses behind the scenes for **Managed Disks** in Virtual Machines. So yes, VM disks are stored as Page Blobs, but that’s abstracted away from us.

#### **Q5. What is Azure Data Lake Storage Gen2? How does it relate to standard Blob Storage?**

**Answer:**Azure Data Lake Storage Gen2 isn’t a separate service — it’s actually Blob Storage with a hierarchical namespace enabled. Standard Blob Storage is flat, like a key-value store. But with ADLS Gen2, you get real folders and subfolders, which makes it much more efficient for big data analytics tools like Azure Synapse or Databricks. You enable it by checking a box when creating a StorageV2 account, and once enabled, you can use file system operations like renaming or deleting entire directories atomically.

#### **Q6. Explain Locally Redundant Storage (LRS) vs. Zone-Redundant Storage (ZRS) vs. Geo-Redundant Storage (GRS). What are their durability guarantees and recovery implications?**

**Answer:****LRS** stores three copies within a single datacenter. It’s the cheapest but offers the least protection — if that datacenter fails, you could lose data. **ZRS** replicates your data across three availability zones in the same region, so it can survive a full datacenter outage. **GRS** goes further by asynchronously copying your data to a secondary region hundreds of miles away, protecting against regional disasters. I use ZRS for production apps that need high availability within a region, and GRS when compliance or business continuity requires disaster recovery across regions.

#### **Q7. What is the purpose of Azure Queue Storage? Describe its core messaging pattern — producers, consumers, and visibility timeout.**

**Answer:**Queue Storage decouples application components. For example, a web app can drop a message into a queue, and a background worker can process it later. The **producer** adds the message. The **consumer** picks it up, and the message becomes invisible for a set **visibility timeout** — this prevents other consumers from processing it simultaneously. Once the work is done, the consumer deletes the message. If it’s not deleted within the timeout, it reappears, which prevents data loss if the worker crashes. It’s a simple but powerful pattern for building scalable, resilient systems.

#### **Q8. What are the core components of an entity in Azure Table Storage — PartitionKey, RowKey, and Properties? Why is partitioning strategy critical for performance?**

**Answer:**Each entity has a **PartitionKey** and **RowKey** that together form a unique identifier. The PartitionKey acts like a logical group, and the RowKey is the unique ID within that group. Then you can have up to 252 additional properties. The reason partitioning is so important is that all operations within a single partition are fast and efficient, but cross-partition queries are slow and expensive. Also, throughput limits apply per partition, so if one partition gets all the traffic, it can become a bottleneck. I always design PartitionKeys to distribute load evenly, like using a hash of a user ID.

#### **Q9. How does Azure Managed Disks simplify storage management for Virtual Machines compared to unmanaged disks?**

**Answer:**With unmanaged disks, you had to manage storage accounts yourself — worrying about limits, replication, and performance bottlenecks. Managed Disks completely abstract that away. Azure handles the storage accounts, replication, scaling, and even integrates with Availability Zones for high availability. You just say “give me a 128 GB SSD disk,” and Azure takes care of the rest. It also makes snapshots, backups, and disk encryption much simpler. From a DevOps standpoint, it removes a whole layer of operational complexity.

#### **Q10. What is the maximum size for a standard Azure File Share?**

**Answer:**For the standard tier, the maximum size is **5 TiB** per file share. If you need more, you can use **Premium Files**, which supports up to 100 TiB when you enable large file shares. For scenarios beyond that, I’d recommend Azure File Sync, which lets you keep a larger namespace with cloud tiering and on-prem caching.

#### **Q11. How do you secure an Azure Storage Account to prevent public internet access? Mention at least three methods.**

**Answer:**First, I disable public access entirely by setting “Allow public blob access” to disabled. Second, I restrict network access to specific IP ranges or virtual networks using the firewall. Third, I use **Private Endpoints** to expose the storage account only within my Azure VNet, so it’s not accessible from the public internet at all. As a bonus, I also use Azure AD authentication or Managed Identity instead of shared keys for applications, which is more secure and auditable.

#### **Q12. What is the difference between using Shared Access Signatures (SAS) and Azure Role-Based Access Control (RBAC) for granting access to storage resources?**

**Answer:**SAS gives time-limited access via a token — great for temporary access, like letting a user upload a file for one hour. RBAC is role-based and integrates with Azure AD, so I use it for long-term access, like giving an application the “Storage Blob Data Reader” role. SAS is easier to share but harder to revoke; RBAC is more secure, auditable, and works well with identity management. In modern DevOps, I prefer RBAC with Managed Identity for services and reserve SAS for short-lived user scenarios.

#### **Q13. Explain Azure Storage Service Encryption (SSE). What are the options for managing the encryption keys — Microsoft-managed vs. Customer-Managed Keys (CMK)?**

**Answer:**All data in Azure Storage is encrypted at rest by default — that’s Storage Service Encryption. By default, Microsoft manages the keys. But for more control, especially for compliance, I can use **Customer-Managed Keys (CMK)** stored in Azure Key Vault. This lets me rotate keys, audit access, and ensure that even Microsoft can’t decrypt my data without my permission. It’s a small performance overhead, but for regulated workloads, it’s essential.

#### **Q14. What is Soft Delete for Blobs? Why is it considered a critical feature for operational safety?**

**Answer:**Soft Delete keeps deleted blobs or containers in a recoverable state for a period I define — usually 7 to 365 days. If someone accidentally deletes a container, I can restore it with a few clicks. It’s critical because it turns a potential disaster into a minor incident. I always enable it on production accounts — it’s one of the first things I check during security and operations reviews.

#### **Q15. What is Blob Versioning? How does it differ from Soft Delete, and how would you use both together?**

**Answer:**Blob Versioning keeps previous versions of a blob whenever it’s overwritten. So if I update a config file and break the app, I can roll back to the previous version. Soft Delete, on the other hand, recovers blobs that were deleted. I use them together: versioning to undo bad updates, and soft delete to recover from accidental deletions. They’re both essential for protecting data integrity in production.

#### **Q16. What are Immutable Blob Policies (WORM)? Which compliance standards do they help meet?**

**Answer:**Immutable Blob Policies, also known as WORM — Write Once Read Many — prevent blobs from being deleted or modified for a set period. This is crucial for meeting compliance requirements like SEC 17a-4, FINRA, HIPAA, or GDPR, where you must prove that audit logs or financial records haven’t been tampered with. I use it for storing logs, legal documents, or any data that needs to be preserved for compliance.

#### **Q17. How can you optimize the performance and cost of a large-scale logging system storing data in Blob Storage?**

**Answer:**I start by compressing logs before uploading to reduce size. Then I use lifecycle policies to automatically move logs from Hot to Cool after 30 days, and to Archive after 180 days. I organize logs in date-based folders for efficient querying. And for ingestion, I use tools like AzCopy with parallel uploads to handle high throughput. This keeps storage costs low while maintaining performance for recent logs.

#### **Q18. What factors influence the performance — IOPS and throughput — of Azure Premium Files? How does it differ from Standard Files?**

**Answer:**Premium Files uses SSDs and offers predictable, high-performance IOPS that scales with the size of the share. For example, a 1 TiB share gives you 1,000 IOPS. Standard Files uses HDDs and has burstable performance — fine for general use but not for latency-sensitive apps. I use Premium Files for databases or high-performance workloads, and Standard for general file sharing or dev/test environments.

#### **Q19. What is Azure Storage Scale Target? How does it impact the design of high-scale applications using Table Storage or Queues?**

**Answer:**The scale target is 20,000 transactions per second per storage account. If your app exceeds that, you’ll get throttled. For high-scale apps, this means I can’t rely on a single account. I either shard data across multiple storage accounts or design my partitioning strategy carefully to distribute load. For example, with Table Storage, I use hashed partition keys to avoid hot partitions. It’s a key constraint to plan for early.

#### **Q20. How can you monitor the performance and health of an Azure Storage Account? Name specific tools and metrics.**

**Answer:**I use **Azure Monitor** to track key metrics like transaction count, egress volume, server latency, and error rates. I set up alerts for 5xx errors and throttling. I also enable **Storage Analytics Logs** for detailed operation logs. And I check **Azure Advisor** for optimization recommendations, like enabling lifecycle policies. This gives me full visibility into performance and cost.

#### **Q21. How would you migrate 50 TB of on-premises file server data to Azure Files with minimal downtime?**

**Answer:**For 50 TB, I’d use **Azure Data Box** — ship a physical device to Azure for secure, fast transfer. Once the initial sync is done, I’d set up **Azure File Sync** to keep everything in sync with minimal downtime. During cutover, I’d redirect users to the Azure File Share UNC path. File Sync also caches frequently used files on-prem, so users don’t notice a performance drop.

#### **Q22. Describe how Azure Storage is used as an artifact repository in a CI/CD pipeline — like Azure Pipelines. What features, like lifecycle policies, are important?**

**Answer:**In CI/CD, I store build outputs — binaries, Docker images, Helm charts — in a Blob container. I enable **lifecycle policies** to move old artifacts to Cool tier after 30 days to save cost. I also enable **versioning** and **soft delete** so I can recover if a build is accidentally deleted. Access is controlled via RBAC or SAS tokens. It’s a simple, cost-effective alternative to dedicated artifact tools for smaller teams.

#### **Q23. Why is storing Terraform state in Azure Blob Storage a common practice? What security and management features — like versioning or immutable policies — should you enable?**

**Answer:**Storing Terraform state in Blob Storage allows teams to share a single source of truth. I enable **versioning** to recover from bad applies, and **soft delete** to prevent accidental loss. For security, I restrict access via firewall rules and use **Managed Identity** instead of access keys. In regulated environments, I might enable **immutable blob policies** to meet compliance. It’s reliable, scalable, and integrates well with DevOps workflows.

#### **Q24. How does Azure File Sync work? What are its primary benefits for hybrid cloud scenarios?**

**Answer:**Azure File Sync links an on-prem file server to an Azure File Share. The cloud is the single source of truth, and the on-prem server caches frequently used files. This gives users fast local access while keeping everything backed up in Azure. Benefits include disaster recovery, cloud tiering to save on-prem storage, and a single namespace across locations. It’s perfect for organizations moving to the cloud but still needing local performance.

#### **Q25. Explain how Point-in-Time Restore for Blobs works and when you would use it instead of Soft Delete or Versioning.**

**Answer:**Point-in-Time Restore lets me roll back an entire storage account to a specific moment — say, 15 minutes ago. It’s different from Soft Delete, which recovers individual deleted blobs, or Versioning, which rolls back a single blob. I’d use PITR in a major incident — like a ransomware attack or a script that corrupts thousands of files. It’s a last-resort recovery tool, and it requires versioning to be enabled.

#### **Q26. What is the Change Feed in Blob Storage? Provide a real-world use case for it.**

**Answer:**The Change Feed is a log of every create, update, or delete operation on blobs. It’s useful for building event-driven systems. For example, I can have an Azure Function triggered by the change feed to process new uploaded images — like generating thumbnails or indexing metadata. It’s more efficient than polling and ensures I don’t miss any changes.

#### **Q27. How can you enforce that all new blobs written to a container are automatically moved to the Cool tier after 30 days?**

**Answer:**I’d create a **lifecycle management policy** that says: for any block blob in this container, if it hasn’t been modified in over 30 days, move it to the Cool tier. I can apply this policy at the container level and it runs automatically in the background. It’s a simple way to optimize cost without any code changes.

#### **Q28. What are the key considerations when choosing between Azure Table Storage and Azure Cosmos DB (Table API) for a new application?**

**Answer:**I’d use **Table Storage** for simple, low-cost scenarios with predictable traffic — like storing device metadata. But if I need low latency, global distribution, or guaranteed throughput, I’d go with **Cosmos DB (Table API)**. Cosmos DB offers SLAs, multi-region writes, and dedicated throughput, but at a higher cost. For most new apps with scale or performance needs, I lean toward Cosmos DB.

#### **Q29. Your application is experiencing high latency when reading blobs from a Storage Account in West US 2. The client is in Europe. What are potential causes and solutions?**

**Answer:**The main cause is network distance — the data is far from the user. I’d first check if the blobs are in Cool or Archive tier, which have higher retrieval latency. Then I’d enable **Azure CDN** to cache blobs at edge locations in Europe. Alternatively, I could use **RA-GRS** to allow reads from the secondary region, or even move the storage account closer to users, like to West Europe.

#### **Q30. You need to store user-uploaded profile pictures — small images, accessed frequently. Which storage service and tier would you choose, and why?**

**Answer:**I’d use **Blob Storage** with the **Hot tier** because the images are small and accessed often. I’d also enable **Azure CDN** to serve them from edge locations for faster load times. Blob Storage is perfect for this — it’s cost-effective, scalable, and integrates well with web apps. Azure Files would be overkill since we don’t need SMB access.

#### **Q31. A developer accidentally deleted a critical container holding application logs. Soft Delete is enabled with a 14-day retention period. How do you recover the container?**

**Answer:**I’d go to the storage account in the Azure portal, navigate to Containers, and click “Show deleted.” I’d find the container, select it, and click “Undelete.” The container and all its blobs would be restored within seconds. As long as it’s within the 14-day window, we’re good. It’s one of the reasons I never skip enabling Soft Delete.

#### **Q32. Your Azure Function processing Queue messages is taking too long, causing messages to become visible again and be processed multiple times. How can you prevent this?**

**Answer:**The issue is that the visibility timeout is shorter than the processing time. I’d increase the visibility timeout to something longer than the longest expected processing time — say, 10 minutes instead of 5. As a safety net, I’d also make the function **idempotent**, so processing the same message twice doesn’t cause issues, like updating a database record with the same data.

#### **Q33. You are designing a system to handle 10,000 concurrent users uploading small files (1–5 MB) to Blob Storage. What storage account configuration — performance, replication, networking — and client-side techniques like chunking would you recommend for optimal performance and cost?**

**Answer:**I’d use a **StorageV2** account with **Standard performance** and **ZRS replication** for high availability within the region. Networking would be locked down with a **Private Endpoint**. On the client side, I’d use the Azure SDK or AzCopy with parallel uploads — maybe 5–10 concurrent threads per user. I’d also distribute uploads across multiple containers to avoid hitting scale limits. And I’d set a lifecycle policy to move files to Cool tier after a week to save cost.