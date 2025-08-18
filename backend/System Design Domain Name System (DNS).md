# System Design: Domain Name System (DNS)

DNS translates a domain name like www.example.com into an IP address that machines use to identify each other on the network.

---
## **Key Concepts**

**DNS is hierarchical**:
It starts from root servers (”.”) at the top, moves to TLDs (like .com, .org), and finally to authoritative servers for specific domains.

**Resolvers and caching**:

* Your **router** or **ISP** configures which DNS resolver to use
* Resolvers **cache results** to reduce lookup time
* Results can also be cached by your **browser** or **operating system**

**TTL (Time to Live)**:
Each DNS record comes with a TTL that defines how long it should be cached before a re-query is made.

---
## DNS Resolvers

> A **DNS resolver** is the component responsible for initiating and sequencing the process of translating a domain name into an IP address.

There are **three main types** of resolvers:

### 1. Recursive Resolver

* Acts on behalf of the client (e.g., your browser)
* Handles the full query process: from root → TLD → authoritative server
* Caches results to speed up future lookups

* Usually provided by:

  * ISPs
  * Public DNS services (e.g., **Google DNS: 8.8.8.8**, **Cloudflare DNS: 1.1.1.1**)

### 2. Root Resolver (Root Hints)

* Knows how to contact the **13 root server clusters** worldwide
* Root servers don’t resolve full domain names — they redirect to appropriate TLD servers (.com, .net, etc.)

### 3. Authoritative Resolver

* Final stop in the chain
* Knows the **exact IP address** for the requested domain
* Responds with definitive answers (not guesses or cached data)

---
### **Resolver Lookup Path (Simplified)**

1. **Your device** asks the configured **recursive resolver** (often your router or ISP)
2. If not cached, the resolver asks a **root server**
3. The root server points to the correct **TLD server**
4. The TLD server points to the **authoritative name server**
5. The authoritative server returns the **IP address**
6. The recursive resolver **caches** and returns the result to your device

---
## **Common DNS Records**

|  **Record Type**  |  **Purpose**  |  **Example**  | 
|---|---|---|
|  **A**  |  Maps a domain to an IPv4 address  |  example.com → 93.184.216.34  | 
|  **CNAME**  |  Maps a domain to another domain  |  www.example.com → example.com  | 
|  **MX**  |  Specifies mail servers for email delivery  |  mail.example.com  | 
|  **NS**  |  Delegates a domain to a DNS server  |  ns1.exampledns.com  | 

> A CNAME must always point to another name, **never directly to an IP**.

> A records are terminal and must point directly to an IP.

---
## **Managed DNS Services**

Platforms like **Cloudflare** and **AWS Route 53** offer advanced DNS management features:

* **Weighted round robin** – Distribute traffic based on assigned weights
* **Failover routing** – Avoid routing to servers under maintenance
* **Cluster-aware balancing** – Account for differing server capacities
* **A/B testing** – Route fractions of traffic for experiments
* **Latency-based routing** – Send users to the closest low-latency server
* **Geolocation routing** – Route users based on region

---

## **Disadvantages of DNS**

**Lookup Delay**:
Each DNS query introduces a small delay, although **caching** usually helps.

**Staleness / Propagation Delays**:
DNS changes don’t propagate instantly due to **cached records** and TTL settings.

**Complex Management**:
Managing DNS zones and delegations across a hierarchy of servers can be complex. Typically handled by:

* Governments (for root zones)
* ISPs
* Large companies or DNS providers

**Security Risks**:
DNS servers are targets for **DDoS attacks**.
*For example, attacks on DNS providers like Dyn have taken major sites like Twitter offline (unless the user knew their raw IP).*

---

#backend