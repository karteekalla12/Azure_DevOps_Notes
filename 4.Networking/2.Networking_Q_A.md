# Azure VNet and NSG Q&A for DevOps

## 1. What is an Azure VNet, and why is it critical for DevOps?  
An Azure Virtual Network (VNet) is an isolated private network foundation in Azure. For DevOps, it's critical because it forms the base network defined and deployed via Infrastructure as Code (IaC). We use tools like Terraform or Bicep to create consistent VNet topologies across Dev, Test, and Prod environments, ensuring network consistency, security segmentation, and enabling reliable hybrid connectivity — all automated and auditable.

## 2. Explain subnetting in Azure. Why not put everything in one big subnet?  
Subnets segment the VNet address space for security and operational reasons. Putting everything in one subnet violates least privilege principles — for example, a compromised web server could attack databases directly. We design tiered subnets (Web, App, DB) with specific Network Security Groups (NSGs). This segmentation reduces blast radius, meets compliance requirements (like PCI), and supports service-specific configurations such as AKS subnet delegation.

## 3. How do you determine the CIDR block size for a subnet?  
We calculate based on the maximum expected resource count plus a buffer for growth. For example, for 300 VMs: 2^(32 - prefix) - 2 ≥ 300. A /23 subnet provides 510 usable hosts (512 - 2). Terraform's `cidrsubnet()` function helps carve subnets from the VNet space. Always validate with `terraform plan` to prevent overlap and ensure adequate address space.

## 4. What's the primary function of an NSG?  
An NSG acts as a stateful firewall controlling traffic to and from subnets or individual network interfaces (NICs). It enforces network security policies via rules, allowing only necessary traffic (e.g., App tier to DB on port 1433) and denying everything else by default. We automate all NSG rules via IaC to prevent configuration drift.

## 5. Describe a common NSG rule mistake you've seen or fixed.  
A classic mistake is using 'Allow * from Internet' on port 22/3389 for admin access. This creates a massive security hole. The fix is strict source IP restriction — only allowing corporate IPs or a jumpbox subnet. In IaC, we parameterize the admin IP range and enforce it via policy. Another common mistake is forgetting outbound rules; we always define explicit outbound rules for dependencies like patching servers.

## 6. What problem does an Application Security Group (ASG) solve that NSGs alone don't?  
NSGs using IP ranges become unmaintainable when resources scale dynamically. ASGs solve this by letting us define rules based on logical application tiers (e.g., 'Allow from asg-app-servers'). When VMs scale in or out, their NICs automatically update ASG membership. This means NSG rules never need changing for IP changes — a huge win for automation and reliability in IaC deployments.

## 7. When would you apply an NSG directly to a NIC instead of a subnet?  
Rarely, and only for very specific exceptions. Subnet-level NSGs are standard for tier security. NIC-level NSGs might be used for a single jumpbox needing stricter rules than its subnet or a legacy app requiring unique rules. But we avoid them because they complicate IaC management and bypass subnet policy. Subnet NSGs are our primary pattern.

## 8. Explain the difference between a Public IP and a Private IP in Azure.  
A Private IP is used within the VNet or connected networks (like on-prem via ExpressRoute) — it’s non-routable on the public internet. Resources communicate internally using private IPs. A Public IP is globally routable on the internet. We only assign Public IPs to edge services like Load Balancers or Application Gateways, never directly to backend VMs like databases, following security best practices enforced in IaC.

## 9. What does /24 mean in CIDR notation (e.g., 10.0.1.0/24)?  
/24 means the first 24 bits of the IP address define the network portion. For 10.0.1.0/24, the network mask is 255.255.255.0. This gives 256 total addresses (2^8), but only 254 are usable for hosts (10.0.1.1 to 10.0.1.254), reserving .0 for network and .255 for broadcast. We use this constantly in IaC for subnet sizing.

## 10. How do you calculate the number of usable hosts in a /27 subnet?  
For /27, the host bits are 32 - 27 = 5. Total addresses = 2^5 = 32. Usable hosts = 32 - 2 = 30 (excluding network and broadcast addresses). We use this math in Terraform with `cidrhost()` and `cidrsubnet()` to plan address space for VM scale sets or container instances.

## 11. Why is using ASGs considered a DevOps best practice for NSG management?  
Because ASGs decouple security rules from infrastructure volatility. In dynamic environments where VMs scale constantly, managing NSG rules via IP lists is error-prone and manual. ASGs let us define rules like 'Allow from asg-web-tier' — the membership updates automatically. This is perfect for IaC; we define the ASG once, associate resources via IaC, and the security policy becomes infrastructure-agnostic and resilient to scale events.

## 12. Describe how you would structure subnets for a 3-tier web application in IaC.  
We define the VNet (e.g., 10.1.0.0/16). Then key subnets: snet-frontend (/24) for LB/App Gateway, snet-web (/24) for web servers, snet-app (/24) for app servers, snet-db (/25 for 126 hosts) for databases. Each subnet gets a dedicated NSG: Web NSG allows 80/443 from LB subnet; App NSG allows only from Web subnet on app port; DB NSG allows only from App subnet on DB port. All defined in Terraform modules.

## 13. What is a service endpoint, and why enable it on a subnet?  
A service endpoint extends your VNet identity to Azure PaaS services (like Storage, SQL). Enabling it on a subnet (e.g., snet-app) means traffic to that PaaS service originates from the VNet's private IP space, not the public internet. This allows us to secure the PaaS service with firewall rules that only allow traffic from that specific subnet — a key security pattern enabled via IaC.

## 14. How do you troubleshoot 'No Network Connectivity' to an Azure VM?  
First, check the VM's NSG (subnet and NIC level) for blocking rules — is the port open and is the source IP allowed? Verify the VM's OS firewall. Check if the VM has a Public IP (if needed) and if the Load Balancer/frontend config is correct. Use Network Watcher's 'Connection Troubleshoot' from the portal or CLI — it tests the full path, including NSGs and route tables, pinpointing the exact block.

## 15. What's the difference between Basic and Standard SKU Public IPs?  
Basic SKU is simple but limited: no zonal redundancy, can't attach directly to VMs (only via Load Balancers), and cannot be associated with an NSG. Standard SKU is required for most production scenarios: zonal aware, can attach to VMs/NICs/LBs, supports outbound rules, and can be associated with an NSG for direct VM security. We exclusively use Standard SKU in IaC for production resources.

## 16. Why is VNet Peering important for DevOps?  
It enables secure, low-latency, private communication between VNets (e.g., Prod VNet to Log Analytics VNet, or Spoke VNets to a Hub). For DevOps, it's vital for connecting environments securely without public exposure. We automate peering setup in IaC, ensuring transitive routing (via UDRs in Hub) and consistent security policies across the network mesh.

## 17. How do you ensure NSG rules are consistent across Dev, Test, Prod?  
Strictly through Infrastructure as Code. We define NSG rule sets as Terraform/Bicep modules parameterized by environment (e.g., allowed_ssh_ips). The same module is called for each environment, with environment-specific variables (like stricter IP ranges in Prod). `terraform plan` shows differences, and policy-as-code (like Azure Policy) blocks non-compliant manual changes.

## 18. What is 'default allow VNet inbound' in NSGs? Is it secure?  
NSGs have a default rule (priority 65000) allowing all traffic within the VNet. While convenient, it's often too permissive for production. Best practice is to deny all VNet traffic by default (using a high-priority deny rule) and then explicitly allow only necessary tier-to-tier communication (e.g., App to DB). We implement this strict model in our IaC for Prod.

## 19. Explain how ASGs simplify NSG rules for a scaling VM Scale Set.  
Without ASGs, if a VM Scale Set scales from 5 to 50 instances, you'd need to manually update every NSG rule referencing those IPs — impossible to automate reliably. With ASGs: 1) Define ASG asg-app-servers, 2) Associate VMSS NICs to this ASG in IaC, 3) Create NSG rule "Allow from asg-app-servers to DB port 1433". As the VMSS scales, NICs auto-join/leave the ASG, and NSG rules use current IPs automatically.

## 20. What is the role of CIDR in VNet Peering?  
CIDR is critical for avoiding overlap. Peered VNets must have non-overlapping address spaces (e.g., VNet1=10.1.0.0/16, VNet2=10.2.0.0/16). If CIDR blocks overlap, peering fails. During IaC design, we meticulously plan the overall address space (e.g., 10.0.0.0/8) and allocate non-overlapping /16 blocks to each VNet using `cidrsubnet()`, validated during `terraform plan`.

## 21. How would you restrict SSH access to Linux VMs in a subnet?  
In the subnet's NSG: 1) Create a high-priority deny rule blocking all inbound SSH (port 22) from the internet, 2) Create a lower-priority allow rule permitting SSH only from a specific admin IP range (e.g., corporate office) or a dedicated snet-jumpbox subnet. Never allow 0.0.0.0/0. We enforce this pattern in all environment IaC templates, with the admin IP range as a variable.

## 22. What's a common pitfall when defining NSG rules?  
Rule priority mistakes. Lower numbers mean higher priority. A common error is creating a broad "Allow Internet" rule with priority 100, then a specific "Deny Bad IP" rule with priority 200 — the allow rule matches first, so the deny is ignored. We always design rules top-down: specific denies first, then specific allows, broad denies last. IaC reviews catch this.

## 23. Why might you use multiple address spaces in a single VNet?  
Primarily for adding non-contiguous address ranges later without recreating the VNet. For example, initial VNet=10.0.0.0/16, but you need more space; you can add 10.1.0.0/16. However, this is complex. Best practice is to size the initial address space large enough (e.g., /8) using `cidrsubnet()` in IaC to avoid this need. We add spaces only if absolutely necessary.

## 24. How does DevOps handle NSG rule changes safely?  
Via IaC pull requests. Changes are made in the Terraform/Bicep code, reviewed for security impact (e.g., opening unnecessary ports), tested in non-prod, and deployed through CI/CD pipelines. We use `az network watcher show-next-hop` pre-deployment to simulate traffic. Never make manual portal changes in production — policy-as-code enforces this.

## 25. What is the significance of the 'VirtualNetwork' service tag in NSGs?  
It represents all IP addresses within the current VNet and any peered VNets. Using it as a source/destination (e.g., "Allow from VirtualNetwork to port 8080") simplifies rules instead of listing all VNet CIDRs. It's dynamic — if you peer a new VNet, the rule automatically includes it. We use this extensively for internal tier communication rules.

## 26. How do you secure a database subnet?  
Strictly: 1) NSG on DB subnet denies all inbound by default, 2) Only one allow rule: from the specific App tier subnet (or ASG) on the exact DB port (e.g., 1433 for SQL), 3) No Public IP on DB VMs, 4) Enable service endpoints for PaaS DB if used, restricting its firewall to the App subnet. This is codified in our secure baseline IaC module.

## 27. Explain the relationship between a VNet, Subnet, and NSG.  
The VNet is the overall private network container (CIDR block). Subnets are segments within the VNet (smaller CIDR blocks), grouping resources by function or tier. An NSG is a firewall attached to either a Subnet (protecting all resources in it) or a NIC (protecting one resource). We attach NSGs to Subnets as the standard pattern for tier security in IaC.

## 28. Why is understanding CIDR essential for a DevOps engineer?  
It's the language of network design. You need it to plan VNet/subnet sizes without overlap, calculate host capacity for scaling, write correct IaC (Terraform's `cidrsubnet()`), configure NSG rules accurately (source/destination ranges), troubleshoot connectivity (is the IP in the right subnet?), and communicate effectively with network teams. It's foundational.

## 29. What's the first thing you check if an application can't reach an Azure SQL DB?  
The NSG on the App server subnet — is there an allow rule for outbound traffic to SQL port (1433/TCP)? Next, check the Azure SQL Server firewall: is "Allow Azure services" ON? If not, is the App subnet's CIDR explicitly allowed? We automate SQL firewall rules in IaC to include the App subnet's CIDR.

## 30. How do you manage NSG rules for ephemeral environments (e.g., feature branch deployments)?  
We use the same IaC modules as Prod, but with environment-specific parameters. For a feature branch VNet, we might allow broader access (e.g., dev team IPs) for debugging, but still enforce tier segmentation. The key is consistent structure: same subnet/NSG pattern, just relaxed rules via variables. Terraform destroys the entire environment cleanly, including NSGs.

## 31. What is the impact of not associating an NSG to a subnet?  
The subnet inherits the default NSG behavior: all inbound traffic from the internet is blocked, but all traffic within the VNet is allowed. This is often too permissive for production (e.g., Web tier could directly access DB). Best practice is always associate a custom NSG to every subnet to enforce least privilege — a standard in our IaC templates.

## 32. How would you allow only your corporate network to access an internal API in Azure?  
1) Place the API backend in a dedicated subnet (snet-api), 2) Create an NSG for snet-api, 3) Add an inbound allow rule: Source = your corp public IP range (e.g., 203.0.113.0/24), Destination = snet-api CIDR, Port = API port (e.g., 8080), Protocol = TCP, Action = Allow, Priority = 100, 4) Add a deny all inbound rule (priority 4096). We parameterize the corp IP range in IaC.
