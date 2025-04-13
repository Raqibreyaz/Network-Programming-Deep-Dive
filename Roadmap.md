### 🚧 **Phase 0: Pre-requisites Review**
(you've likely covered this already, but quick brush-up won't hurt)

- OS basics: processes, threads, memory, files, signals
- C programming: pointers, memory mgmt, structs, casting
- Linux terminal & basic system calls

---

### ⚙️ **Phase 1: Core Networking Concepts (Tanenbaum)**
> Focus: **Understanding how the internet works**

Read from Tanenbaum:
- **Chapter 1–3:** Introduction, Physical, Data Link layers
- **Chapter 4:** Network Layer (IP, routing, fragmentation)
- **Chapter 5:** Transport Layer (UDP, TCP, ports, connection)
- **Chapter 6 (if curious):** Application Layer (DNS, HTTP, SMTP)

Key topics to deeply grasp:
- Encapsulation (data → segment → packet → frame)
- IP & subnetting
- Routing tables & NAT
- TCP 3-way handshake, sliding windows, congestion control
- UDP vs TCP (when and why)
- Socket pair: (IP:Port, IP:Port)

---

### ⚡️ **Phase 2: Socket Programming Foundations (TLPI + Stevens Vol 1)**
> Focus: **System calls + hands-on**

🔹 Start from **TLPI Chapter 56–60**  
🔹 Then go deeper with **Stevens: Volume 1 Chapters 1–6, 11–13**

Topics:
- `socket()`, `bind()`, `listen()`, `accept()`, `connect()`
- `read()` / `write()` / `send()` / `recv()`
- `struct sockaddr`, `inet_ntop`, `getaddrinfo`
- `select()` / `poll()` / `epoll()`
- IPv4 and IPv6 sockets
- TCP vs UDP sockets

Practice ideas:
- 🌐 TCP Echo Server + Client
- 📦 UDP Datagram Server + Client
- 🧪 Test error cases: invalid addresses, disconnects, etc.

---

### 🧠 **Phase 3: Internal Engineering Concepts**
> This is where backend magic starts feeling real.

Study from Stevens + your own experiments:
- TCP connection lifecycle: **SYN–SYN/ACK–ACK**
- Socket options: `setsockopt()` (e.g., SO_REUSEADDR, TCP_NODELAY)
- I/O Models: **blocking vs non-blocking vs multiplexed**
- Signal-driven I/O and `SIGIO`
- Network byte order: `htons()`, `htonl()`, etc.
- Persistent connections, Keep-alives

Implement:
- ⚙️ **Chat app** using TCP (multiple clients using `select`)
- 🔌 **Port scanner** using raw sockets
- 🧠 Reverse DNS lookup tool

---

### 🕸 **Phase 4: Event-Driven / Scalable Networking**
> Optional but important if you aim for backend performance

- Non-blocking sockets + `epoll`
- `kqueue` (BSD) for portability learning
- Thread pools for concurrent connections
- Using `fork()` or `pthread` per client (and downsides)
- Compare models: threaded vs event loop vs async (Node.js style)

---

### 🚀 **Phase 5: Combine Everything into a Project**
Ideas:
- 🌐 Simple HTTP Server (GET + keep-alive)
- 🧰 Custom Proxy Server (HTTP caching + filtering)
- 🔐 SSH-style encrypted chat using `OpenSSL` sockets
- 🪝 Build something like `netcat`, `curl`, or a packet sniffer

---

### 🛠 Tools You’ll Use Alongside:
- `netstat`, `ss`, `lsof`
- `tcpdump`, `wireshark` (for deep packet analysis)
- `strace` + `ltrace` for syscall tracking
- `gdb` for debugging socket-level issues
- `telnet`, `nc`, and custom clients for testing servers

---