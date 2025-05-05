## 🌐 DNS – The Domain Name System (Deep Dive Notes)

### 🔹 Why DNS Exists

Humans prefer easy-to-remember names like `example.com`, but computers communicate using IP addresses like `93.184.216.34`. This mismatch is where DNS comes in — it acts like the **“phonebook of the internet”**, converting human-friendly domain names into machine-friendly IP addresses.

---

### 🔹 DNS in the Networking Stack

* DNS is part of the **Application Layer** of the OSI and TCP/IP models.
* It translates hostnames into IP addresses so that the **network layer** can route packets.
* Without DNS, every user would need to memorize IP addresses of websites — which is neither scalable nor practical.

---

### 🔹 DNS Resolution: Behind the Scenes

When a browser receives a URL like `https://example.com:443`, it needs to extract the **hostname** (`example.com`) and **port** (`443`) and then resolve the hostname into an IP address.

#### 🧠 The Resolution Flow

1. **Browser Cache**: Checks if it already has the IP cached.
2. **OS Cache**: If the browser doesn't know, the OS may have it cached.
3. **`/etc/hosts`** file: OS checks this static file for mappings.
4. **Stub Resolver**: The resolver in your system uses DNS configuration (like `/etc/resolv.conf`) to contact external DNS servers.
5. **Recursive Resolver**: Your DNS server (usually from your ISP or Google like `8.8.8.8`) queries:

   * **Root DNS Servers**
   * **TLD Servers** (like `.com`)
   * **Authoritative Servers** (like `example.com`)

Once the IP is found, it's returned to the OS → passed to the browser → connection established to the server using the resolved IP and the port.

#### Example:

```bash
dig example.com
```

You might see an IP like `93.184.216.34` returned along with authoritative nameservers and TTLs.

---

### 🔹 Is DNS Server Running on My Machine?

* Your **machine does not run a DNS server** by default.
* When your OS needs DNS resolution, it sends a **UDP (or TCP) packet to port 53** of a **remote DNS server** (like Google’s `8.8.8.8`).
* This DNS server responds with the resolved IP address.

#### Clarification:

Port **53** is where **DNS servers** listen, not your machine. Your system is the client, the DNS resolver is the server.

---

### 🔹 DNS Caching (Where and How?)

DNS resolution is expensive, so results are **cached** at multiple levels:

* **Browser Cache** (Chrome, Firefox, etc.)
* **OS Resolver Cache**
* **System-wide caches** like `nscd`, `systemd-resolved`, `dnsmasq`
* **Local `/etc/hosts`** file (manual overrides)

#### TTL:

Each DNS record has a **TTL (Time To Live)**, after which the cache must be refreshed by querying again.

---

### 🔹 Why Use IP Addresses at All? Why Not Just Use Hostnames?

* **Routing Equipment (Routers)**: Operate at Layer 3 (Network Layer) and only understand IP addresses.
* Hostnames are meaningful to us but have **no significance to routers or switches**.
* IP addresses are required for **packet delivery, fragmentation, routing**, etc.
* Hostnames are just an **abstraction layer** on top of IPs.

Thus, every hostname must eventually be resolved to an IP for actual communication to happen.

---

### 🔹 DNS Resolution in Code: `getaddrinfo()`

* `getaddrinfo()` is a **C library function**, not a system call.
* It internally uses the system’s DNS resolution logic and returns a list of address structures (`sockaddr`).
* Used in both IPv4 and IPv6 resolution.

#### Example:

```c
struct addrinfo *res;
getaddrinfo("example.com", "http", NULL, &res);
```

---

### 🔹 DNS Packet Structure (Query/Response)

#### ➤ DNS Query Packet Fields

| Field   | Description                             |
| ------- | --------------------------------------- |
| ID      | Unique ID for matching query & response |
| QR      | 0 = query, 1 = response                 |
| Opcode  | Type of query (usually 0 for standard)  |
| RD      | Recursion Desired                       |
| QDCOUNT | Number of questions                     |
| QNAME   | Domain name being queried               |
| QTYPE   | Type (A, AAAA, MX, etc.)                |
| QCLASS  | Usually IN (Internet)                   |

#### ➤ DNS Response Packet Fields

| Field   | Description                       |
| ------- | --------------------------------- |
| AA      | Authoritative answer              |
| RA      | Recursion available               |
| RCODE   | Response code (e.g. 0 = No error) |
| ANCOUNT | Answer count                      |
| NSCOUNT | Number of name servers            |
| ARCOUNT | Number of additional records      |
| ANSWERS | Contains IP addresses and TTLs    |

---

### 🔒 Privacy in DNS

Traditional DNS queries are sent in **plain text** over UDP/TCP (port 53), which means:

* ISPs and middleboxes can **see** which domains you're querying.
* Vulnerable to **man-in-the-middle attacks**, spoofing, and surveillance.

#### 🔐 Modern Solutions:

* **DNS over HTTPS (DoH)**: Encrypts DNS using HTTPS (port 443).
* **DNS over TLS (DoT)**: Encrypts using TLS (port 853).
* These protocols improve **privacy** and prevent DNS snooping or manipulation.

---

### 🧪 Real Example

```bash
nslookup google.com
```

Output:

```
Server:         8.8.8.8
Non-authoritative answer:
Name:   google.com
Address: 142.250.64.14
```

Here:

* Query went to Google's public DNS server (`8.8.8.8`).
* The response was cached (hence non-authoritative).
* The IP address returned is what the OS uses to connect.

---

### 📌 Summary Points

* DNS maps domain names to IP addresses.
* Port 53 is used for DNS communication (UDP/TCP).
* Your machine queries an external DNS server, not a local one.
* DNS responses are cached at multiple levels.
* `getaddrinfo()` is a library function, not a system call.
* DNS packets have structured headers and data sections.
* Use of DNS ensures scalability and decouples users from needing to remember IPs.
* DNS privacy is improved using DoH and DoT.

---
