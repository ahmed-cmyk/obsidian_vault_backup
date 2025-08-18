# UDP (User Datagram Protocol)

* Lightweight transport layer protocol

* Sits on top of IP (Internet Protocol)

* Connectionless and stateless

* Often called “fire-and-forget”

---

## **What it Does**

* Sends packets (called **datagrams**) without establishing a connection

* No delivery guarantees, no ordering, no retransmission

* No handshake — just send the data

---

## **Where It’s Used**

* **Real-time applications**: VoIP, video streaming, online gaming

* **DNS**: Fast queries that can tolerate occasional loss

* **Syslog**, **SNMP**, **TFTP**, **DHCP**

---

## **How it Works (packet-level)**

* No 3-way handshake like TCP

* Data is wrapped in a UDP header:

|  **Field**  |  **Size**  | 
|---|---|
|  Source Port  |  16 bits  |
|  Dest Port  |  16 bits  |
|  Length  |  16 bits  |
|  Checksum  |  16 bits  |

* Sent directly to IP layer, then to destination

* Receiver uses **demultiplexing** (via port numbers) to route to the right application

---

## **Pros**

* **Low latency** (no handshakes, no ACKs)

* **Low overhead**

* Works well in **broadcast/multicast**

* Good for **high-throughput** use cases

---

## **Cons**

* **No delivery guarantee**

* **No retransmission or ordering**

* Can result in **lost, duplicated, or out-of-order packets**

* You have to build reliability manually at the app level (if needed)

---

## **Real-world Analogy**

> Like dropping a bunch of flyers from a helicopter. It’s fast, but you don’t know who got one (or how many copies they got).

---

## **Debugging Tip**

* Use tools like **Wireshark** to inspect UDP traffic

* Be cautious of firewalls or NAT — UDP is easier to drop or block

* Great candidate when packet loss is acceptable but speed is essential

---

#backend