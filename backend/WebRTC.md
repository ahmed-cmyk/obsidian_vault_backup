# **WebRTC**

**WebRTC (Web Real-Time Communication)** is a free, open-source project that enables **real-time communication (RTC)** between peers (usually browsers or apps) using simple APIs.

It’s designed for **audio**, **video**, and **data** sharing directly between devices — without a server acting as a middleman for the media.

---

## **What it does**

* Enables **P2P (peer-to-peer)** audio, video, and data transfer

* Allows browsers and mobile apps to communicate in real-time

* No plugins or external software needed

* Handles complex networking: NAT traversal, ICE candidates, STUN/TURN, etc.

---

## **Where it’s used**

* **Video conferencing** tools (e.g., Google Meet, Zoom, Discord)

* **Voice calls** in web/mobile apps

* **File sharing** between users (P2P)

* **Gaming** (low-latency peer-to-peer communication)

* **IoT** and **remote desktop** tools

---

## **Pros**

* **Low latency** (direct P2P communication)

* Built into browsers (Chrome, Firefox, Safari, etc.)

* Supports **audio, video, and arbitrary data**

* **Secure by default** (DTLS + SRTP encryption)

* Cross-platform (JS, Android, iOS, C++)

---

## **Cons**

* **Complicated signaling setup** (requires custom server)

* NAT/firewall traversal is tricky (relies on STUN/TURN servers)

* Difficult to scale to large groups (mesh network issues)

* Poor debugging tools

* No built-in user authentication — must implement separately

---

## **Components of WebRTC**

```
[1] Signaling Server — for initial handshake
[2] STUN — helps discover public IPs (NAT traversal)
[3] TURN — relays media if direct P2P fails
[4] ICE — handles connection candidates
[5] SDP — session description (negotiation info)
[6] MediaStream / RTCDataChannel — handles actual streams
```

---

## **How WebRTC works (simplified flow)**

1. **Signaling**: Peers exchange info via a signaling server (e.g., over WebSocket)

2. **SDP Offer/Answer**: Session description protocol for negotiating media settings

3. **ICE Candidates**: Each peer gathers potential connection paths (IP/port)

4. **Connection Established**: WebRTC picks the best path (P2P if possible)

5. **Media/Data Streams** flow directly between peers

⠀
---

## **Real-world analogy**

Like calling a friend directly without going through a phone company. You need a way to say “Hey, I want to talk” (signaling), then you connect and start talking (P2P stream).

---

## **Architecture comparison**

|  **Feature**  |  **WebRTC**  |  **gRPC**  | 
|---|---|---|
|  Transport  |  UDP, TCP (via ICE/STUN/TURN)  |  HTTP/2  |
|  Use Case  |  Real-time media/data (P2P)  |  Backend API communication  |
|  Server required?  |  Only for signaling/STUN/TURN  |  Yes (client ↔ server)  |
|  Latency  |  Ultra-low  |  Low  |
|  Data type  |  Audio, video, binary  |  Binary via Protobuf  |
|  Browser compatible  |  Yes  |  No (unless using grpc-web)  |
---

## **When to use WebRTC**

Use WebRTC when:

* You need **real-time audio/video** communication

* You want to transfer data **directly between clients**

* You are building chat/video/file-sharing apps

* Latency is critical

---

## **When NOT to use WebRTC**

Avoid WebRTC when:

* You need **server-side processing or storage** of media

* You want **easy scalability** to many users (consider SFU/MCU)

* Your app doesn’t need real-time or P2P features

---

## **Tools & Libraries**

* simple-peer: Easiest wrapper for Node.js/WebRTC

* PeerJS: Abstraction for peer connection

* mediasoup, Janus, Jitsi: SFU media servers

* coturn: TURN/STUN server

* WebSocket/Socket.IO: Used for signaling

---

#backend