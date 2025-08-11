# Networking for DevOps — Complete, Detailed Guide

---

## 1. What is a Network?

A **network** is two or more computers or devices connected so they can exchange data. Connections can be wired (Ethernet, fiber) or wireless (Wi-Fi). Networks let devices:

* Communicate (send/receive data).
* Share resources (files, printers, servers).
* Provide centralized services (DNS, DHCP, authentication).

**The Internet** is the largest network: a “network of networks.”

---

## 2. Types of Networks (by scope)

* **LAN (Local Area Network):** small area (home, office, school). Technologies: Ethernet, Wi-Fi.
* **MAN (Metropolitan Area Network):** covers a city or campus.
* **WAN (Wide Area Network):** covers large geographic areas (intercity, international). Often uses leased lines, MPLS, or Internet links.
* **PAN (Personal Area Network):** very small (Bluetooth, personal devices).
* **SONET/SDH:** synchronous optical networks used in carrier backbones and undersea links.

---

## 3. Common Network Devices

* **NIC (Network Interface Card):** hardware required to connect a device. Has a MAC address.
* **Hub:** legacy device that broadcasts all incoming frames to all ports (obsolete).
* **Switch:** connects devices in a LAN; uses MAC addresses to forward frames to the correct port.
* **Router:** forwards packets between different networks (uses IP addressing and routing tables).
* **Bridge:** older device to connect LAN segments (largely replaced by switches).
* **Modem:** converts between digital signals (computer) and analog signals (phone line or cable).
* **Firewall:** enforces security rules for traffic entering/leaving networks.
* **Load balancer:** distributes incoming requests across multiple servers.
* **VPN gateway:** provides secure tunnels for remote access.

---

## 4. Network Protocols — short overview

A **protocol** is a set of rules for communication. Examples:

* **Ethernet:** data link + physical layer in LANs.
* **ARP:** resolves IPv4 addresses to MAC addresses.
* **IP (IPv4/IPv6):** network layer, addressing and routing.
* **TCP:** reliable, connection-oriented transport protocol (three-way handshake).
* **UDP:** connectionless, low latency (no guaranteed delivery).
* **HTTP/HTTPS:** web protocols (app layer).
* **DNS:** maps domain names to IP addresses.
* **DHCP:** assigns IP addresses dynamically.
* **SSH:** secure shell for remote command execution.

---

## 5. IP Addressing — IPv4 and IPv6

### IPv4

* 32 bits, shown as four octets: `192.168.1.10`
* Approximately 4.29 billion total addresses (2^32).
* Usual notation: dotted decimal.

### IPv6

* 128 bits, hex groups: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
* Huge address space; designed to replace IPv4.

### Public vs Private

* **Public IP:** routable on the internet (assigned by ISP).
* **Private IP:** used inside networks (not routable on internet). Common ranges:

  * `10.0.0.0/8`
  * `172.16.0.0/12`
  * `192.168.0.0/16`

### Static vs Dynamic

* **Static IP:** manually assigned; used for servers, devices needing stable addressing.
* **Dynamic IP:** assigned by DHCP; changes over time.

---

## 6. IPv4 Address Classes (historical — useful for learning)

| Class |                       Range | Typical Use                   |
| ----- | --------------------------: | ----------------------------- |
| A     |   1.0.0.0 — 126.255.255.255 | Very large networks           |
| B     | 128.0.0.0 — 191.255.255.255 | Medium networks               |
| C     | 192.0.0.0 — 223.255.255.255 | Small networks, home/business |
| D     | 224.0.0.0 — 239.255.255.255 | Multicast                     |
| E     | 240.0.0.0 — 255.255.255.255 | Experimental                  |

**Special blocks**

* `127.0.0.1` — loopback (localhost).
* `0.0.0.0` — unspecified / default route representation in some contexts.

> Note: Modern networks use **CIDR** instead of strict classes.

---

## 7. Network ID and Host ID (how to tell if two devices are on same network)

An IPv4 address has two parts: the **network** portion and the **host** portion. The division is determined by the subnet mask (or prefix length).

Example:

* IP: `192.168.1.10`
* Mask: `255.255.255.0` (`/24`)

  * Network: `192.168.1.0`
  * Host range: `192.168.1.1` — `192.168.1.254`

If two devices share the same network portion, they are on the same LAN and can communicate directly. If not, communication goes via a router.

---

## 8. Subnetting — step-by-step (worked examples)

**Goal:** Learn how to split `192.168.1.0/24` into two `/25` subnets.

### Definitions

* Prefix `/24` means 24 bits for network, 8 bits for host.
* Number of addresses in `n` host bits = `2^n`.
* Usable hosts = `2^n - 2` (remove network and broadcast addresses).

### Example: `/24` -> `/25`

* `/24` → host bits = 8 → addresses `2^8 = 256`, usable `254`.
* `/25` → host bits = 7 → addresses `2^7 = 128`, usable `126`.

**Subnets:**

1. `192.168.1.0/25`

   * Range: `192.168.1.0` — `192.168.1.127`
   * Network = `192.168.1.0`, Broadcast = `192.168.1.127`
   * Usable hosts: `192.168.1.1` — `192.168.1.126` (126 hosts)

2. `192.168.1.128/25`

   * Range: `192.168.1.128` — `192.168.1.255`
   * Network = `192.168.1.128`, Broadcast = `192.168.1.255`

**How to calculate number of subnets when you borrow bits:**

* Original mask `/24`. If we move to `/25`, we borrowed 1 bit for subnetting → `2^1 = 2` subnets.

**General formula for usable hosts for prefix /p:**

* host bits `h = 32 - p`
* usable hosts `= 2^h - 2`

Worked: `/25` → `h = 7` → `2^7 - 2 = 128 - 2 = 126`.

---

## 9. CIDR (Classless Inter-Domain Routing)

CIDR lets you specify prefixes like `/19`, `/22` instead of classful ranges.

Common table:

| Prefix | Netmask         | Total addresses |   Usable hosts |
| -----: | --------------- | --------------: | -------------: |
|  `/32` | 255.255.255.255 |               1 | 1 (host route) |
|  `/30` | 255.255.255.252 |               4 |              2 |
|  `/29` | 255.255.255.248 |               8 |              6 |
|  `/28` | 255.255.255.240 |              16 |             14 |
|  `/27` | 255.255.255.224 |              32 |             30 |
|  `/26` | 255.255.255.192 |              64 |             62 |
|  `/25` | 255.255.255.128 |             128 |            126 |
|  `/24` | 255.255.255.0   |             256 |            254 |
|  `/16` | 255.255.0.0     |          65,536 |         65,534 |

---

## 10. OSI Model — clear breakdown (7 layers)

**Top → Bottom**

1. **Application (7):** user apps, HTTP, FTP, DNS, SMTP.
2. **Presentation (6):** data formats, encryption (SSL/TLS).
3. **Session (5):** session setup/teardown.
4. **Transport (4):** TCP (reliable), UDP (unreliable).
5. **Network (3):** IP addressing and routing.
6. **Data Link (2):** MAC addresses, Ethernet frames, ARP.
7. **Physical (1):** cables, signaling, bits on wire.

**TCP/IP model mapping**

* Application = OSI 7,6,5
* Transport = OSI 4
* Internet = OSI 3
* Link = OSI 2,1

---

## 11. TCP vs UDP

* **TCP**: connection-oriented, reliable, flow control, ordering.

  * Use when data integrity matters (web, email, file transfer).
  * **Three-way handshake**:

    1. Client sends `SYN` (seq=x).
    2. Server replies `SYN+ACK` (seq=y, ack=x+1).
    3. Client sends `ACK` (ack=y+1) — connection established.
* **UDP**: connectionless, low overhead. Use for DNS, streaming, VoIP.

---

## 12. Common Protocols & Ports (practical list)

| Protocol   |  Port | Use                             |
| ---------- | ----: | ------------------------------- |
| HTTP       |    80 | Web (cleartext)                 |
| HTTPS      |   443 | Secure web (TLS)                |
| SSH        |    22 | Secure shell                    |
| DNS        |    53 | Domain resolution               |
| SMTP       |    25 | Sending email                   |
| POP3       |   110 | Receiving email                 |
| IMAP       |   143 | Receiving email                 |
| FTP        | 20/21 | File transfer                   |
| SFTP       |    22 | Secure file transfer (over SSH) |
| RDP        |  3389 | Remote Desktop                  |
| MySQL      |  3306 | DB connection                   |
| PostgreSQL |  5432 | DB connection                   |

---

## 13. DNS — how it works (step-by-step)

1. You type `www.example.com` in browser.
2. Browser checks local cache — OS cache — router cache — ISP resolver cache.
3. If not cached, the resolver asks root servers → root points to TLD servers (`.com`).
4. TLD server points to the authoritative nameserver for `example.com`.
5. Authoritative server returns A/AAAA record: `93.184.216.34`.
6. Browser connects to that IP.

**Common DNS records**

* `A` — IPv4 address
* `AAAA` — IPv6 address
* `CNAME` — alias to another name
* `MX` — mail exchanger (with priority)
* `NS` — nameserver for the zone
* `TXT` — text; used for verification, SPF, DKIM

**DNS TTL**: how long a resolver caches the record.

**Useful commands**

* `dig example.com A +trace`
* `nslookup example.com`

---

## 14. DHCP — basics

* DHCP automates IP assignment and parameters (IP, gateway, DNS, lease time).
* **Lease**: time period the client can use the assigned IP.
* DHCP flow: Discover → Offer → Request → Acknowledge.

---

## 15. NAT and PAT (Network Address Translation)

* **NAT**: maps private IPs to public IPs — used to conserve IPv4 addresses.
* **PAT** (Port Address Translation) aka NAT overload: many private IPs share one public IP, differentiated by port numbers.

Example: `192.168.1.10:50000` -> `203.0.113.5:62000` on internet.

---

## 16. Routing — basic principles

* Routers forward IP packets based on routing tables.
* **Longest prefix match**: the route with the most specific prefix is chosen.
* Default route: `0.0.0.0/0` for IPv4 (all traffic not matched elsewhere).
* Dynamic routing protocols: OSPF, BGP, EIGRP, RIP.

**Simple route table example**

| Destination |       Netmask | Gateway       | Interface |
| ----------- | ------------: | ------------- | --------- |
| `10.0.0.0`  | `255.255.0.0` | 0.0.0.0       | eth0      |
| `0.0.0.0`   |     `0.0.0.0` | `203.0.113.1` | eth0      |

Routing decisions evaluate narrower (more specific) networks first.

---

## 17. VLANs (Virtual LANs)

* VLANs segment a single switch into multiple broadcast domains.
* Useful to separate traffic (e.g., `VLAN 10 = servers`, `VLAN 20 = users`).
* Tagged VLANs use 802.1Q tagging between switches.

---

## 18. Firewalls & Security Basics

* Firewalls filter traffic by IP, port, protocol, and sometimes application.
* Best practices:

  * Default deny inbound, allow necessary services.
  * Use network segmentation and least privilege.
  * Use IDS/IPS for detection.
  * Use TLS for encrypting traffic (HTTPS).
  * For SSH, use key-based auth, disable root login, change default port only as defense-in-depth.

---

## 19. Load Balancers and Reverse Proxies

* **Load Balancer:** Distributes client requests across backend servers.

  * Types: L4 (TCP/UDP), L7 (HTTP/HTTPS).
* **Reverse Proxy:** sits in front of web servers; can do SSL termination, caching, request routing (e.g., NGINX, HAProxy).

---

## 20. Common Network Troubleshooting Tools & Examples

### ping

* Test reachability.
* `ping google.com`
* Output: latency and packet loss.

### traceroute / tracert (Windows)

* Shows the path packets take.
* `traceroute example.com` or `tracert example.com`

### curl

* HTTP requests:

  * `curl http://example.com`
  * `curl -I https://example.com` (headers)
  * `curl -o page.html http://example.com` (save)

### telnet

* Test TCP port connectivity:

  * `telnet example.com 443`
  * If connection opens, port is reachable.

### dig

* DNS queries:

  * `dig A example.com`
  * `dig example.com +trace`

### nslookup

* Interactive DNS lookup in Windows or Linux.

### netstat / ss

* Show listening ports and connections.

  * `netstat -tulpn` (Linux)
  * `ss -tuln`

### nmap

* Network scanning and host discovery.

  * `nmap -sn 192.168.1.0/24` (ping sweep)
  * `nmap -sV example.com` (service/version detection)

### tcpdump / wireshark

* Capture and analyze network packets.

  * `tcpdump -i eth0 port 80` (capture HTTP)

### ssh / scp

* `ssh user@host`
* `scp file user@host:/path`

---

## 21. Practical Troubleshooting Flow (step-by-step)

1. **Is the device online?** `ping <gateway>`, `ping 8.8.8.8`
2. **Is DNS working?** `dig example.com` or `nslookup example.com`
3. **Is port/service reachable?** `telnet host port` or `curl -I https://host`
4. **Trace the route if remote:** `traceroute host`
5. **Inspect local interface config:** `ip addr` (Linux) or `ipconfig /all` (Windows)
6. **Check routing table:** `ip route` or `route -n`
7. **Look for dropped packets or errors on NIC:** `ifconfig` or `ip -s link`
8. **Capture packets if needed:** `tcpdump -i eth0 host <ip> and port <p>`

---

## 22. HTTP and Web Concepts (detail)

* **Stateless:** each HTTP request is independent.
* **Cookies** and **sessions** maintain state between requests.
* **Common methods:** `GET`, `POST`, `PUT`, `DELETE`, `HEAD`, `OPTIONS`.
* **Status codes:**

  * `2xx` success (200 OK)
  * `3xx` redirection (301 Moved Permanently, 302 Found)
  * `4xx` client errors (400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found)
  * `5xx` server errors (500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable)
* **TLS:** Use certs for HTTPS. Use modern ciphers and avoid deprecated ones.

---

## 23. Useful CLI Examples (Linux)

Show IP config:

```
ip addr show
```

Show routing:

```
ip route show
```

Ping with count:

```
ping -c 5 8.8.8.8
```

Traceroute:

```
traceroute example.com
```

Curl headers:

```
curl -I https://example.com
```

DNS lookup:

```
dig +short example.com
```

List listening ports:

```
ss -tuln
```

Network capture (first 100 packets):

```
sudo tcpdump -i eth0 -c 100 -w capture.pcap
```

---

## 24. Worked Examples & Practice

### Example 1 — Determine network and broadcast:

* IP: `192.168.10.73`
* Mask: `255.255.255.192` → `/26` → host bits = 6.

Calculate:

* Block size = `256 - 192 = 64`
* Subnets start at: `0, 64, 128, ...`
* Which block contains .73? It's in `192.168.10.64/26`.

  * Network: `192.168.10.64`
  * Broadcast: `192.168.10.127`
  * Usable hosts: `192.168.10.65` — `192.168.10.126` (62 hosts)

Step-by-step binary method is available if you want to see bit-level math.

### Example 2 — How many hosts in `/22`?

* `h = 32 - 22 = 10` host bits
* Total addresses `2^10 = 1024`
* Usable hosts `1024 - 2 = 1022`

---

## 25. DevOps Best Practices (networking-related)

* **Infrastructure as Code**: declare networks, subnets, security groups in Terraform/ARM.
* **Network segmentation**: separate prod/dev/test networks; use VLANs or subnets.
* **Least privilege**: security group rules should be minimal (whitelist necessary ports).
* **Monitoring**: collect metrics (latency, packet loss), set alerts for anomalies.
* **Automate config**: manage firewall rules and routing via IaC to avoid drift.
* **Use private endpoints**: avoid public exposure of services when possible (VPC endpoints, private link).
* **Secrets**: use secret managers for keys, avoid storing creds in code.

---

## 26. Quick Reference Cheat Sheet

**Useful commands**

* `ping <host>`
* `traceroute <host>`
* `dig <domain> A`
* `curl -I <url>`
* `ss -tuln` or `netstat -tulpn`
* `nmap -sV <host>`

**Subnet formulas**

* Host bits `h = 32 - prefix`
* Total addresses `= 2^h`
* Usable hosts `= 2^h - 2` (except for `/31` and `/32` special cases)

**Remember**

* Loopback = `127.0.0.1`
* Default route = `0.0.0.0/0`
* Private ranges: `10/8`, `172.16/12`, `192.168/16`

---

## 27. Learning Path & Next Steps

If you want a curated study plan, I can:

* Create a one-week, two-week, or month-long study plan.
* Produce practice labs (commands and expected outputs) you can run on your machine or in cloud VMs.
* Convert this doc into a printable PDF or slide deck.

---

## 28. Appendix — Extra Details (if you want them)

I can expand any of the following on request (I didn’t dump them all here to keep the document readable):

* Binary-to-decimal subnet conversions with worked bitwise math.
* Full list of HTTP status codes with meanings and examples.
* Deep dive into BGP and OSPF.
* Example Terraform scripts for VPC/subnets/security groups and examples for Azure VNet.
* Packet capture walkthrough (tcpdump + Wireshark analysis).
* Example NGINX reverse proxy and load balancer config.

---
