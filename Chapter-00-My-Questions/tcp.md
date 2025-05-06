# 💡 **Complete TCP Deep Notes — Engineering + Intuition + Real Examples**

---

## 🚀 TCP: The Reliable Messenger of the Internet

TCP (Transmission Control Protocol) is like **a professional courier service** — it ensures your message reaches the destination:

* **Completely**
* **Without corruption**
* **In correct order**
* **With acknowledgement**

TCP is a **connection-oriented**, **reliable**, and **byte-stream based protocol**.

---

## 🔗 TCP Connection Setup: The 3-Way Handshake

Before any message is sent, sender and receiver must shake hands to agree:
“I’m ready”, “I’m also ready”, “Let’s begin!”

### 🧠 What Happens:

1. **SYN**: Client → Server (`I'm ready to talk, sequence starts here`)
2. **SYN-ACK**: Server → Client (`Okay, I’m ready too`)
3. **ACK**: Client → Server (`Let’s go`)

📦 This ensures both sides know initial sequence numbers and that they can **start reliably exchanging data**.

> 🔍 If this handshake fails midway, connection never starts — avoiding half-baked communication.

---

## 📦 TCP Packet Structure (Header)

Each TCP packet contains:

* `Source Port`, `Destination Port`
* `Sequence Number`: Which **byte number** this packet starts with.
* `ACK Number`: What byte receiver expects **next**.
* `Flags`: Like SYN, ACK, FIN
* `Window Size`: How much data can be accepted without overflow
* `Checksum`: For **data integrity**

---

## 🧮 Checksum: Is The Data Damaged?

Checksum is like a **digital signature** to detect corruption in transit.

### 🛠 How It Works:

1. Sender:

   * Converts data to binary (e.g. ASCII of "Hello")
   * Sums it
   * Takes **1’s complement** of the sum → ✅ this becomes **checksum**
2. Receiver:

   * Repeats the same
   * Adds the received checksum
   * If result is all `1s` → ✅ data is valid
   * Else → ❌ data corrupted → **packet discarded**, **no ACK sent**

📌 **Important**:

* Checksum is done **per-packet**.
* If a packet is lost completely → checksum is not even **performed** — it’s just not received.

> ⚠️ Example: Sender sends “Hello World” split into 3 TCP packets. Packet 2 is lost. Checksum for 2 will **never be checked**, as it never arrives. Sender **waits for ACK**, doesn’t get it, so **resends packet 2**.

---

## 🧾 Sequence Number & Order Management

Each byte in TCP has a sequence number.

📦 If you're sending 5000 bytes, and your first packet sends 1000 bytes, it gets sequence number **1000**.
Next one gets **2000**, and so on.

### Why is this important?

It helps **receiver reorder** packets and detect **missing** ones.

> 🧠 Even if packet 2 is delayed and arrives after packet 3, the sequence numbers help receiver place data **in correct order**.

---

## 📤 Sliding Window: Send More Without Waiting

Instead of waiting for ACK after every packet, TCP allows **sending multiple packets** before needing ACK.

This is called the **sliding window** mechanism.

### Example:

* Sender has a window size of 4
* Sends packets 1, 2, 3, 4 in one go
* Gets ACK for 1 → window slides → sends packet 5

This allows **faster data transmission**, especially over high-latency networks.

---

## 🧊 Flow Control (Protecting the Receiver)

Even if sender is fast, receiver may be slow.

To prevent buffer overflow, receiver shares a value called **rwnd (receiver window)** which tells sender:

> “This is how much space I have, don’t send more than this.”

If buffer is full → rwnd = 0 → Sender pauses

---

## 🌐 Congestion Control (Protecting the Network)

Sometimes the **network itself is overloaded** (too many users).

TCP detects this using:

### 🔥 Packet Loss

Loss is a **signal of congestion**. So TCP slows down.

#### Process:

1. Starts with **slow start** (increases sending rate exponentially)
2. If **3 duplicate ACKs** or **timeout** happens → loss detected
3. Reduces sending rate (congestion avoidance)
4. Then **slowly increases** again → like probing for available bandwidth

> 📌 Example: TCP sends 1 → 2 → 4 → 8 → 16 packets.
> Suddenly packet lost → Now TCP cuts speed to 8 → 4 → then tries to climb again.

This ensures TCP **adapts to the environment** like a smart driver adjusting speed.

---

## 📡 What Happens When a Packet is Lost?

Let’s say:

* 10 packets sent
* Packet 4 lost

What happens:

* Receiver gets 1, 2, 3, then 5, 6…
* **ACKs 3 repeatedly** (called **duplicate ACKs**)
* Sender sees **3 duplicate ACKs for 3** → concludes packet 4 was lost
* Resends **only packet 4**

### ✅ Not all data is resent, only missing one.

---

## 🌀 Out-of-Order Packets? No Problem

If packet 6 arrives before 4 → receiver **buffers it**, waits for 4

But it still sends **duplicate ACKs** for 3 to tell sender:

> "Hey, I’m still waiting for 4!"

If 4 arrives late **after retransmission already occurred**, then:

* Receiver **drops** the delayed original (already got good one)
* Or **accepts and ignores** based on logic

---

## 🚫 What if Data Gets Corrupted?

If packet 7 arrives with wrong checksum:

* Discarded immediately
* No ACK sent
* Sender **times out** or gets duplicate ACKs
* Resends it

> ✅ Corruption ≠ Loss, but handled similarly (no ACK → resend)

---

## 🔀 TCP Multiplexing & Ports

Each TCP connection is identified by:

```
<Source IP, Source Port, Dest IP, Dest Port>
```

So you can open multiple tabs to same website — TCP knows which tab (socket) the data belongs to.

---

## 🔌 TCP in Practice — Socket Programming Basics

* Server:

  * `socket() → bind() → listen() → accept()`
* Client:

  * `socket() → connect()`

Once connected:

* Use `send()`, `recv()` to exchange data

TCP handles all reliability under the hood.

---

## ⚡ QUIC (Quick UDP Internet Connections) — Basic Idea

* Google’s protocol built over UDP
* Combines: TLS + Multiplexing + Connection
* Avoids TCP handshake → faster page loads
* Used in: **Google, YouTube, Facebook**

---

## ⚙️ BBR — Basic Idea

* A congestion control algorithm
* Uses **bottleneck bandwidth** and **RTT**
* Doesn’t wait for packet loss
* Tries to maintain **optimal steady rate**

Used by Google for **video streaming** for smoother performance.

---

## 🧠 Understanding the TCP Packet Header — Deep Dive with Real-World Flow

### 1. **Source Port & Destination Port**

These are like door numbers at both ends:

* **Source Port**: From which port the sender is sending data.
* **Destination Port**: Where the receiver is listening.

Example:

* Browser (Client) uses port `54321`
* Server (Website) listens on port `80` (HTTP)

Essential for connection setup and correct delivery.

---

### 2. **Sequence Number**

This field tells:

> “From which byte does the data in this packet start?”

Remember: TCP is **byte-oriented**, not packet-oriented.

Let’s say:

* You want to send 4000 bytes
* TCP breaks them into 4 packets of 1000 bytes each (assuming MSS = 1000)
* Starting sequence number = 1000

Then:

* Packet 1 → Sequence number = 1000 (bytes 1000–1999)
* Packet 2 → 2000 (bytes 2000–2999)
* Packet 3 → 3000
* Packet 4 → 4000

So, sequence number = "Where does this data start in the overall stream?"

---

### 3. **ACK Number — What Is It Really Saying?**

This number means:

> “I’ve received everything **up to byte X – 1**. Please send me the next byte starting from X.”

For example:

* ACK = `5000`
  Means: “I’ve received bytes up to 4999. I now expect byte 5000 onward.”

This is used to confirm successful receipt of data.

**Important**: This field is only valid when the `ACK` flag is set in the packet (which is almost always during data transfer).

---

### 4. **Flags — TCP’s Emotion Indicators 😄**

Each packet may have one or more flags set, like:

* `SYN`: "Let’s start a connection"
* `ACK`: "I’m acknowledging something"
* `FIN`: "I’m closing the connection"
* `RST`: "Reset — something went wrong"
* `PSH`: "Push this data immediately"
* `URG`: "Urgent data inside"

You asked: “If data isn’t over, why does ACK appear?”

Good question.

* In **connection setup**, server sends `SYN + ACK` (acknowledging client’s SYN).
* During **normal transmission**, each data packet is usually acknowledged, so `ACK` is set.
* When **connection ends**, client or server sends a `FIN`, and the receiver replies with `ACK` to that `FIN`.

---

### 5. **Window Size — The Receiver’s Buffer Limit**

This tells:

> “How many more bytes of data can I accept **right now**, without overflow?”

Although this value is carried in a sender’s packet, it **represents the receiver’s buffer capacity**.

Example:

* Receiver has 16000 bytes of free buffer → Tells sender: “You can send 16000 bytes without waiting.”

This is used for **flow control**.

---

### 6. **Checksum — Did the Data Get Corrupted?**

This is how TCP checks for data integrity:

* Sender creates a checksum (a kind of fingerprint of the packet’s contents).
* Receiver recalculates it upon arrival.
* If it **matches**, data is accepted.
* If not → **packet is discarded silently**, no ACK is sent.

TCP will **retransmit** that packet either on timeout or based on **duplicate ACKs** from receiver.

---

## 🚀 Real-World Scenario You Asked

You said:

> Suppose sender sends 5 TCP packets (each 1000 bytes) with sequence numbers:
> 1000, 2000, 3000, 4000, 5000

If **receiver gets all 5 successfully**, what ACK number will it send?

### ✅ Answer:

Receiver will send:

```
ACK = 6000
```

Why?
Because it means:

> “I have received bytes up to 5999. Please send from byte 6000 next.”

---

### ⚠️ What if a Packet Arrives Late?

Let’s say packet 3 (with sequence 3000) is delayed or lost.

Receiver gets:

* Packet 1 → OK
* Packet 2 → OK
* Packet 4 → Comes before packet 3
* Packet 5 → Also early

Receiver can't process packets out of order.

So it keeps sending:

```
ACK = 3000
```

This repeats (called **duplicate ACKs**), and after 3 duplicate ACKs:

> Sender **resends packet 3** quickly, without waiting for full timeout (this is called *fast retransmit*).

---

### 🧠 Final Insights (Why This Matters)

* TCP is all about **reliability** — in-order, error-free, byte-perfect delivery
* Sequence and ACK numbers ensure no data is skipped or duplicated
* Window size and ACKs help balance speed and network safety
* Checksum guarantees data wasn't changed or corrupted

---

---

# ✅ Wrap-Up

> TCP is not just about packets — it’s about **reliable, ordered, congestion-aware, stream-based communication**.
> It handles **delays, loss, reordering, corruption** — and still delivers your YouTube video or Gmail flawlessly.

---