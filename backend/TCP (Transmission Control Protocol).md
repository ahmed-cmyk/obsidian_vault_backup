# **TCP (Transmission Control Protocol)**

* Heavyweight transport-layer protocol built on top of IP

* **Reliable**, **connection-oriented**, and **stateful**

* Guarantees **ordered**, **lossless** delivery of bytes

---

## **What it Does**

* Establishes a connection between two endpoints

* Ensures all data is received in order and without duplication

* Retransmits lost packets

* Provides **flow control**, **congestion control**, and **error checking**

---

## **Where It’s Used**

* **Web traffic** (HTTP/HTTPS)

* **Email** (SMTP, IMAP, POP3)

* **SSH**, **FTP**

* Any application where **reliability** is more important than raw speed

---

## **How it Works (packet-level)**

### **1.**

### **3-Way Handshake (Connection Setup)**

* **SYN →** Client initiates connection

* **SYN-ACK ←** Server acknowledges

* **ACK →** Client confirms

### **2.**

### **Data Transfer**

* Data sent in **segments**

* Each segment has a **sequence number**

* Receiver sends back **ACKs** (acknowledgements)

### **3.**

### **Connection Teardown**

* **FIN → FIN-ACK ← ACK** (graceful close)

* Or reset via **RST** if something goes wrong

---

## **TCP Header Fields (key ones)**

|  **Field**  |  **Description**  | 
|---|---|
|  Source Port  |  Where it came from  |
|  Dest Port  |  Where it’s going  |
|  Sequence Num  |  Byte offset for this packet  |
|  ACK Num  |  Next byte expected  |
|  Flags  |  SYN, ACK, FIN, RST, etc.  |
|  Window Size  |  Used for flow control  |
---

## **Pros**

* **Reliable** delivery

* **Ordered** byte stream

* Built-in **error correction**

* Handles **network congestion** gracefully

---

## **Cons**

* **Slower** than UDP due to overhead

* **Higher latency** (especially with TLS/HTTPS on top)

* **Head-of-line blocking** (one delayed packet stalls the stream)

* Needs connection setup and teardown

---

## **Real-world Analogy**

> Like making a phone call: you dial, they pick up, you talk in order, and then hang up — everything is confirmed.

---

## **Debugging Tip**

* **SYN flood** = too many half-open connections (common in DDoS)

* Use netstat, tcpdump, Wireshark to monitor state transitions

* Check for retransmissions, duplicate ACKs, or window size issues

---

## **Bonus: Useful Socket States**

* **LISTEN** — waiting for a connection

* **SYN_SENT** / **SYN_RECEIVED** — handshake in progress

* **ESTABLISHED** — ready to send/receive

* **TIME_WAIT** — closed but waiting in case late packets arrive

---

#backend