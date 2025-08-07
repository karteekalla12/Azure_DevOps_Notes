Azure Networking Deep Dive: VNet, Subnets, NSG, ASG, IP, CIDR (DevOps Focus)
----------------------------------------------------------------------------

### Core Concepts Explained

**1\. Virtual Network (VNet)**

*   **What it is:** The fundamental building block for private networking in Azure. It's an isolated, software-defined network _within_ your Azure subscription, analogous to a traditional on-premises network you'd build in your own datacenter. Resources deployed within a VNet (like VMs, databases) can communicate securely with each other using private IP addresses.
    
*   **Why it matters (DevOps):** Provides the secure foundation for _all_ your cloud resources. DevOps teams define VNet topology (address space, subnets) as Infrastructure as Code (IaC - Terraform, ARM, Bicep) to ensure consistent, repeatable environments (Dev, Test, Prod). Crucial for network security, segmentation, and hybrid connectivity (VPN/ExpressRoute).
    
*   **UI Creation Experience:** When creating a VNet via Azure Portal:
    
    *   **Basics Tab:** You select Subscription, Resource Group (often rg-network-), Region, and give it a name (e.g., vnet-prod-centralus).
        
    *   **IP Addresses Tab (CRITICAL):** This is where you define the **Address Space** using **CIDR notation** (e.g., 10.0.0.0/16). You see a text box to enter this. The portal validates if the space is available within Azure's constraints. You might also see options for IPv6 (less common initially).
        
    *   **Security Tab:** Basic options like enabling DDoS Protection Standard (usually enabled in Prod).
        
    *   **Review + Create:** Final validation before deployment. You'll see a summary confirming your chosen CIDR block.
        

**2\. Subnet**

*   **What it is:** A segmented portion of a VNet's address space (CIDR block). Think of the VNet as a large city block (e.g., 10.0.0.0/16 = 65,536 addresses) and subnets as individual streets or neighborhoods within it (e.g., 10.0.1.0/24 = 251 usable addresses for Web Servers). Subnets isolate resources for security, routing, and service requirements (e.g., separate subnets for Web, App, DB tiers).
    
*   **Why it matters (DevOps):** Enables micro-segmentation – a core security principle. DevOps teams define subnet layouts in IaC, associating specific NSGs and route tables. Critical for deploying resources to the correct tier (e.g., placing a VM in the snet-app-prod subnet), enabling service endpoints (e.g., for Azure SQL), and managing network traffic flow securely. Subnet design directly impacts scalability and security posture.
    
*   **UI Creation Experience:** Subnets are created _within_ a VNet:
    
    *   Navigate to your VNet -> Subnets under Settings.
        
    *   Click + Subnet.
        
    *   **Name:** (e.g., snet-web-prod, snet-db-prod)
        
    *   **Subnet address range:** Enter the **CIDR block** _within_ the parent VNet's space (e.g., VNet=10.0.0.0/16, Subnet=10.0.1.0/24). The portal shows available space and validates overlap. _This is where CIDR math happens!_
        
    *   **Network security group (NSG):** Dropdown to select an existing NSG or create a new one (highly recommended). You _see_ the association happen here.
        
    *   **Route table:** Dropdown to associate a custom route table (if needed for advanced routing).
        
    *   **Service endpoints / Delegation:** Options to enable service endpoints (e.g., for Storage, SQL) or delegate the subnet to a specific service (e.g., Azure Kubernetes Service - AKS).
        

**3\. Network Security Group (NSG)**

*   **What it is:** A stateful firewall _at the subnet or individual resource (NIC) level_ that controls inbound and outbound traffic using rules based on source/destination IP, port, and protocol. Rules are evaluated in priority order (lowest number first). Default rules allow VNet traffic and outbound traffic, but block most inbound traffic.
    
*   **Why it matters (DevOps):** The primary tool for implementing network security policies. DevOps teams define NSG rules meticulously in IaC to enforce the principle of least privilege (e.g., "Only App subnet can talk to DB subnet on port 1433"). Critical for passing security audits, reducing attack surface, and enabling zero-trust architectures. Managing NSG rules via IaC prevents configuration drift and human error.
    
*   **UI Creation Experience:** Creating an NSG standalone or during subnet/VM creation:
    
    *   **Basics Tab:** Name (e.g., nsg-web-prod), Resource Group, Region.
        
    *   **Inbound security rules Tab:** The core interface.
        
        *   You see a list of existing rules (including default rules).
            
        *   Click + Add to create a new rule.
            
        *   **Source/Destination:** Dropdowns for Any, IP Addresses, Service Tag (e.g., VirtualNetwork, Internet, AzureLoadBalancer), Application Security Group (ASG).
            
        *   **Source/Destination port ranges:** Enter ports (e.g., 80, 443, 1024-65535).
            
        *   **Protocol:** Any, TCP, UDP, ICMP.
            
        *   **Action:** Allow or Deny.
            
        *   **Priority:** Enter a number (100-4096, lower = higher priority).
            
        *   **Name:** Descriptive name for the rule (e.g., Allow\_HTTP\_From\_Internet).
            
    *   **Outbound security rules Tab:** Similar structure to Inbound rules.
        
    *   **Review + Create:** Validation of rule logic (e.g., no duplicate priorities).
        

**4\. Application Security Group (ASG)**

*   **What it is:** A logical grouping of resources (VM NICs) _based on application function_, not network location. Instead of managing complex IP lists in NSG rules, you assign resources to ASGs (e.g., asg-app-servers, asg-db-servers) and then reference the ASG name in NSG rules (e.g., "Allow traffic _from_ asg-app-servers _to_ asg-db-servers on port 1433").
    
*   **Why it matters (DevOps):** **Revolutionizes NSG management for dynamic environments.** DevOps teams define ASGs in IaC alongside resources. When VMs scale in/out (e.g., in an Autoscale Set), their NICs are automatically added/removed from the ASG. NSG rules using ASGs _never need updating_ for IP changes – the rule "Allow from asg-app" always points to the correct current set of app servers. Huge reduction in complexity, errors, and manual effort, especially with IaC.
    
*   **UI Creation Experience:**
    
    *   Create ASG: Resource Group, Region, Name (e.g., asg-api-prod).
        
    *   **Associate Resources:** After creation, go to the ASG -> Associated resources.
        
    *   Click + Associate.
        
    *   You see a list of _Network Interfaces (NICs)_ within the subscription/region. Select the NICs of the VMs that belong to this application tier (e.g., all API server VMs). _This is the key step – linking resources to the logical group._
        

**5\. IP Address (Internet Protocol Address)**

*   **What it is:** A unique numerical identifier assigned to every device (interface) participating in a network using the Internet Protocol. In Azure context:
    
    *   **Public IP:** Routable on the public internet. Assigned to resources like Load Balancers, NAT Gateways, or directly to VMs (not recommended for security). Can be Basic or Standard SKU (Standard required for many features like zonal redundancy, NSG association).
        
    *   **Private IP:** Used _within_ a VNet or on-premises network. Assigned from the VNet/Subnet's CIDR block. Resources communicate internally using private IPs. Never routed publicly.
        
*   **Why it matters (DevOps):** IP management is foundational. DevOps uses IaC to allocate Public IPs (for endpoints) and ensure proper Private IP assignment within subnets. Understanding IP types is crucial for designing secure architectures (e.g., never exposing DB VMs directly via Public IP). Automation handles IP allocation, reducing manual errors. Private IP consistency is vital for configuration management (Ansible, Chef).
    

**6\. CIDR (Classless Inter-Domain Routing)**

*   **What it is:** The notation system (x.x.x.x/y) used to define IP address ranges (blocks) and their subnet masks. The /y (prefix length) specifies how many bits of the IP address are used for the _network portion_, with the remaining bits for _host addresses_.
    
    *   Example: 10.0.0.0/24
        
        *   10.0.0.0 = Network Address
            
        *   /24 = First 24 bits = Network portion (255.255.255.0 mask)
            
        *   Usable Hosts: 10.0.0.1 to 10.0.0.254 (254 addresses; .0 is network, .255 is broadcast).
            
    *   Key Math: Number of Usable Hosts = 2^(32 - y) - 2 (e.g., /24 = 2^8 - 2 = 254).
        
*   **Why it matters (DevOps):** **The language of network design.** DevOps engineers _must_ understand CIDR to:
    
    *   Plan VNet and Subnet address spaces without overlap.
        
    *   Calculate required address space for scaling (e.g., "We need 500 VMs, so subnet needs at least /23").
        
    *   Troubleshoot connectivity issues (is the IP in the expected subnet?).
        
    *   Write accurate IaC (Terraform cidrsubnet() function is essential).
        
    *   Configure NSG rules correctly (source/dest IP ranges).
        
*   **UI Relevance:** You constantly _see_ and _enter_ CIDR blocks when creating VNets (10.0.0.0/16), Subnets (10.0.1.0/24), and even in NSG rules (source/dest IP ranges like 10.0.2.0/24). The portal validates CIDR syntax and overlap.
    

### DevOps Context

*   **What is the core purpose of a VNet?** (What)
    
    *   _DevOps Answer:_ "It's the secure, isolated private network foundation for all Azure resources. We define it in IaC to ensure consistent network topology across environments, enabling secure communication and integration with on-premises via ExpressRoute/VPN."
        
*   **Why do we subnet?** (Why)
    
    *   _DevOps Answer:_ "For security segmentation (least privilege), meeting service requirements (e.g., AKS needs its own subnet), enabling service endpoints, and managing broadcast domains. In IaC, we define tiered subnets (Web, App, DB) with specific NSGs, making environments reproducible and audit-compliant."
        
*   **How do NSGs enforce security?** (How)
    
    *   _DevOps Answer:_ "By stateful rules evaluating source/dest IP, port, protocol. We automate NSG rule creation via Terraform/Bicep, using ASGs heavily to avoid IP lists. Rules are strict: only allow necessary traffic (e.g., App subnet -> DB port 1433), default deny. We test rules pre-deployment using tools like az network watcher test-connectivity."
        
*   **When would you use an ASG instead of IP ranges in an NSG?** (When)
    
    *   _DevOps Answer:_ "Always for dynamic workloads! If VMs scale in/out (like VMSS or AKS nodes), ASGs are mandatory. Defining a rule like 'Allow from asg-app-tier' means the NSG _automatically_ uses the current set of app server IPs. Using static IPs here would break scaling and require constant manual NSG updates – a major anti-pattern we avoid in IaC."
        
*   **Where are Private IPs used vs Public IPs?** (Where)
    
    *   _DevOps Answer:_ "Private IPs are _everywhere internally_: VMs talk via private IPs within the VNet/subnet. Public IPs are _only_ on edge resources like Application Gateways, Load Balancers, or Bastion hosts – never directly on backend VMs like DBs. Our IaC strictly enforces this: frontend tiers get Public IPs (via LB), backend tiers only get Private IPs within secure subnets."
        
*   **Who manages CIDR planning?** (Who)
    
    *   _DevOps Answer:_ "Collaboratively. Network Architects define the overall addressing scheme (e.g., 10.0.0.0/8 for cloud), but DevOps Engineers own the _implementation_ in IaC for their applications. We calculate subnet sizes (e.g., /24 for 251 hosts) using cidrsubnet(), validate for overlap during terraform plan, and ensure consistency across environments. It's a core DevOps responsibility to get this right."