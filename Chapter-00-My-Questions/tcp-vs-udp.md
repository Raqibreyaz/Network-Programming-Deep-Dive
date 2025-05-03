# 🔥 TCP vs UDP – Ultimate Notes for Devs & System-Level Understanding

---

## 🧠 Why Use TCP or UDP in the First Place?

Both **TCP (Transmission Control Protocol)** and **UDP (User Datagram Protocol)** sit at the **Transport Layer (Layer 4)** of the OSI model and help us **deliver data between applications across machines**, using:

* `Source IP + Source Port`
* `Destination IP + Destination Port`

Ports allow the OS to decide **which process** should receive the data, just like addresses on envelopes.

---

## 🔗 TCP – Reliable Byte Stream with Connection

TCP is a **connection-oriented**, **stream-based**, and **reliable** protocol designed for applications that cannot afford data loss or disorder.

### 🔁 Connection Establishment – The 3-Way Handshake (with Sequence Numbers)

Before any real data is exchanged, TCP performs a **3-way handshake** using **sequence numbers**:

1. **Client → Server:** `SYN`, **Seq = x\`**
   (Client says: I want to connect; I’ll start counting data from x)

2. **Server → Client:** `SYN-ACK`, **Seq = y, Ack = x+1**
   (Server says: I accept; my count starts from y; I received your x)

3. **Client → Server:** `ACK`, **Ack = y+1**
   (Client confirms: Got your y)

🔥 After this handshake:

* Both sides know where to begin tracking data.
* This is not just “hi-hello” — it’s establishing a **mutual agreement on byte-level tracking** of the upcoming stream.

---

## 🚀 TCP Data Transfer – Stream-Based, Reliable Flow

Once connection is made:

* You don’t send individual packets/messages.
* You send a **continuous stream of bytes**.
* OS may split or coalesce them arbitrarily before sending.

### 💡 Why Stream and Not Messages?

In TCP:

* There is **no notion of message boundaries**.
* If you call `send("Hello")` and then `send("World")`, the receiver may `recv("HelloWo")` and then `recv("rld")`.

Your app must define how to split messages (e.g., via delimiters like `\n`, JSON parsing, or fixed-length headers).

### 🧃 Reliable Delivery – TCP ACKs After Data Too

TCP doesn’t just do ACKs in the handshake — it:

* **Acknowledges every segment of data**.
* If ACK isn’t received in time, it **retransmits** the data.
* This ensures **no loss, no duplication**.

### 🪟 Sliding Window Protocol (Not the DSA One!)

* TCP uses **sliding window** for:

  * Controlling how much unacknowledged data can be in-flight.
  * Preventing buffer overflows on the receiver.
* Sender window slides forward as ACKs are received.

🧠 DSA-style sliding window is for arrays. TCP’s sliding window is for managing **network throughput** efficiently.

---

## 🌉 TCP Graceful Shutdown – Clean Disconnection

When either side is done, it doesn’t just close. Instead:

1. Sends `FIN` to say "I’m done sending".
2. Peer sends `ACK`, then later its own `FIN`.
3. You reply with `ACK`.

✅ Now both sides know:

* Data has been fully sent.
* No data is being lost in mid-air.

💣 Abrupt disconnects (`RST`) skip this and may cause data loss.

---

## ⚡ UDP – Stateless, Fast, Fire-and-Forget

UDP skips all connection setup and tracking — it simply sends **datagrams**:

* No handshake
* No ACK
* No tracking of order or duplication

### 📫 Message Boundaries – Fully Preserved

UDP **preserves datagrams**. Every send = 1 receive.

🧠 So unlike TCP, if you send 3 packets, the receiver either gets 3 packets or fewer — never half a packet or merged ones.

### ⛓️ recvfrom() in UDP – No Loop Needed for Boundaries

Since UDP messages are **not streams**, your app **doesn’t need to loop** to get a full message.

* If you `recvfrom()` 1 datagram, you get that whole message (or nothing, if lost).
* No need to worry about merging/streaming/partial reads.

🌊 But yes, for continuous communication (like video or voice), **you still loop to read the next packet**, but **not for assembling a full message**.

---

## 📊 TCP vs UDP – Complete Summary Table

| Feature                | TCP                            | UDP                           |
| ---------------------- | ------------------------------ | ----------------------------- |
| **Connection**         | Yes (3-way handshake)          | No                            |
| **Handshake**          | Required once                  | None                          |
| **Sequence Numbers**   | Yes (track every byte)         | No                            |
| **ACKs**               | Yes, for reliability           | No                            |
| **Retransmission**     | Yes                            | No                            |
| **Ordering Guarantee** | Yes                            | No                            |
| **Message Boundaries** | No (byte stream)               | Yes (each datagram preserved) |
| **Sliding Window**     | Yes                            | No                            |
| **Graceful Shutdown**  | Yes (`FIN`/`ACK`)              | No (just stops sending)       |
| **Use Cases**          | Web (HTTP), File Transfer, SSH | DNS, Gaming, VoIP             |
| **Speed**              | Slower but reliable            | Very fast, but no guarantees  |
| **Loop in recv()?**    | Yes, to assemble full messages | No, each datagram is atomic   |

---

## 🧠 Real-World Analogies

* **TCP**: Phone call (you say hello, talk, confirm messages, then hang up).
* **UDP**: Throwing notes from a window (some land, some don’t, no way to confirm).

---

## 🛠 Bonus: When to Use What?

| Application Type             | Use TCP                             | Use UDP |
| ---------------------------- | ----------------------------------- | ------- |
| Login systems, chat, bank    | ✅                                   | ❌       |
| Video call, multiplayer game | ❌                                   | ✅       |
| File transfer                | ✅                                   | ❌       |
| Streaming (YouTube, Netflix) | ✅ (but over UDP via QUIC sometimes) | ✅       |

---

## ✅ Final Takeaways

* TCP gives **connection, reliability, and order**, but at a performance cost.
* UDP gives **speed and simplicity**, but you sacrifice delivery guarantees.
* TCP is **stream-based** — messages are like pipes.
* UDP is **message-based** — each datagram is an envelope.

---