# 🧠 The Application Layer – Full Developer-Oriented Notes

---

## 7.1 🔹 The Domain Name System (DNS)

### 📜 History & Motivation

Before DNS, systems used `hosts.txt` files from Stanford. It didn’t scale. DNS was introduced in 1983 as a hierarchical, distributed system to map **domain names to IP addresses**, separating **naming** from **addressing**.

### 🧱 DNS Architecture

* **Root Servers** (.)
* **Top-Level Domains (TLD)** – `.com`, `.org`, `.net`, etc.
* **Authoritative Servers** – responsible for `example.com`
* **Resolvers** – typically on your ISP/local system. Handles recursion.

### 🔁 The Lookup Process

1. You type `www.example.com`.
2. Browser asks the OS, which asks the **resolver**.
3. Resolver queries the **root** → gets `.com` TLD name server.
4. Resolver queries `.com` → gets `example.com` authoritative server.
5. Resolver queries it → gets `A record`: IP of `www.example.com`.

### 🔍 Query & Response Format

* **Query**: contains `QNAME`, `QTYPE`, `QCLASS`
* **Response**: contains answers, authority, additional sections

Packet is in binary format with fields like:

```txt
Header:
- ID
- QR (query/response)
- Opcode
- Flags: Recursion Desired, Auth Answer

Questions:
- example.com, IN, A

Answers:
- 93.184.216.34
```

### ⚙️ Name Resolution

* **Iterative** (resolver asks each server directly)
* **Recursive** (resolver asks one, which handles the rest) – most common

### 🔐 DNS Privacy

* DNS is plaintext → vulnerable to spying, spoofing
* **DNSSEC** – signs responses
* **DoH (DNS over HTTPS)** – encrypted in HTTPS
* **DoT (DNS over TLS)** – encrypted but not mixed in HTTP

---

## 7.2 🔹 Electronic Mail

### 📦 Architecture

* **User Agent (UA)**: Gmail, Thunderbird – lets users send/read
* **Mail Transfer Agent (MTA)**: sends mail (SMTP)
* **Mail Delivery Agent (MDA)**: stores mail on server

### 🔄 Flow

1. Alice composes email in her **UA**
2. UA sends mail to **MTA** via **SMTP**
3. MTA relays to **Bob’s MTA**
4. Bob’s **UA** fetches via **POP3** or **IMAP**

### 💌 Message Format

* Headers:

  * `From`, `To`, `Subject`, `Date`, `Received`, etc.
* Body:

  * Plain text or MIME multipart (for HTML, attachments)

### 📤 SMTP

Command-based protocol:

```
S: 220 smtp.example.com
C: HELO alice.com
S: 250 Hello
C: MAIL FROM:<alice@example.com>
C: RCPT TO:<bob@example.com>
C: DATA
   (message body)
.
C: QUIT
```

### 📥 POP3 vs IMAP

* **POP3**: downloads and deletes from server
* **IMAP**: syncs and keeps mail on server

### 🔐 Security

* **SPF**: Who can send for a domain
* **DKIM**: Signs email with domain key
* **DMARC**: Tells receivers how to handle failures

---

## 7.3 🔹 The World Wide Web

### 🏗️ Architecture

* **Client–Server** over **HTTP/HTTPS** using **URLs**
* Static and dynamic resources
* **Web Server** (Apache, Nginx) → serves content

### 📄 Static vs Dynamic

* **Static**: HTML, CSS, images – same for everyone
* **Dynamic**: JS-based, APIs, rendered per request

---

## ✅ 7.3.4 HTTP & HTTPS (Already Covered)

### 🌐 HTTP

* Application layer protocol over TCP
* Stateless, request-response model

#### Example Request:

```
GET /index.html HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
```

#### Example Response:

```
HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: sessionId=abc123; HttpOnly
```

### 🔐 HTTPS Stack

* Built over TLS (Transport Layer Security)
* Adds:

  * Encryption (AES, ChaCha20)
  * Integrity (HMAC)
  * Authentication (Certificates)

**Handshake:**

1. ClientHello → supported ciphers
2. ServerHello → picks cipher, sends certificate
3. Key exchange (e.g. ECDHE)
4. Encrypted communication starts

---

### 🧠 Important Headers

| Header                                   | Use                                    |
| ---------------------------------------- | -------------------------------------- |
| `Content-Type`                           | Type of returned content (`text/html`) |
| `Accept`                                 | What client accepts                    |
| `Authorization`                          | Token-based login                      |
| `Set-Cookie` / `Cookie`                  | Sessions                               |
| `Origin` / `Access-Control-Allow-Origin` | CORS                                   |
| `Cache-Control`                          | Caching behavior                       |
| `Content-Length`                         | Size of response                       |
| `User-Agent`                             | Browser identity                       |

---

### 🍪 Cookies

* Sent by server:
  `Set-Cookie: id=abc; HttpOnly; Secure`
* Stored by browser
* Sent on next request:
  `Cookie: id=abc`
* Used for:

  * Sessions
  * Tracking
  * Preferences

---

### 🌐 HTTP/2

* Multiplexed streams (no head-of-line blocking)
* Binary framing layer
* Header compression (HPACK)

### 🌐 HTTP/3

* Based on QUIC over UDP
* TLS handshake in **0-RTT**
* Handles retransmissions at user level

---

## 7.4 🔹 Streaming Media

### 🔊 Digital Audio

* Encodings: PCM, MP3, AAC
* Sample rate (44.1kHz), bitrate, compression

### 🎥 Digital Video

* Encodings: H.264, VP9, AV1
* Frames, keyframes, compression

### 🗂️ Streaming Stored Media

* Progressive: buffer as it downloads
* Adaptive (DASH, HLS): based on connection speed

### 📡 Real-Time Streaming

* Protocols: RTP, RTCP, WebRTC
* Needs jitter buffers, low-latency paths

---

## 7.5 🔹 Content Delivery

### 📈 Internet Content

* Majority: video > images > documents

### 🛠️ Server Farms

* Load-balanced data centers
* Horizontal scaling

### 🚀 CDN (Content Delivery Networks)

* Caches data at edge
* Examples: Cloudflare, Akamai, Fastly
* Reduces latency, offloads origin servers

### 🤝 P2P Networks

* BitTorrent, WebRTC, DHT
* Decentralized content sharing

---

## 7.6 🔹 Summary

Application layer is the **interface between users and the network**. It provides:

* DNS to resolve names
* Email protocols to send messages
* HTTP to browse web
* TLS/SSL to secure communication
* CDNs and streaming for fast, scalable delivery

---
