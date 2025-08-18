# **WebSockets**

WebSockets enable **full-duplex** communication over a single, long-lived TCP connection. Unlike HTTP (which is request/response), WebSockets allow real-time two-way data flow.

---

## **What it does**

* Enables **real-time**, bidirectional communication between **client and server**

* Reduces overhead vs HTTP by keeping a single connection open

* Ideal for chat, collaborative apps, live updates, gaming, etc.

---

## **Where it’s used**

* Chat systems (WhatsApp Web, Slack)

* Collaborative editors (Google Docs)

* Multiplayer games

* Real-time dashboards or stock tickers

* IoT device communication

---

## **How it works (Handshake + Packets)**

* Starts as a **normal HTTP request**:

```
GET /chat HTTP/1.1  
Host: example.com  
Upgrade: websocket  
Connection: Upgrade
```

* Server responds:

```
HTTP/1.1 101 Switching Protocols  
Upgrade: websocket  
Connection: Upgrade
```

* After handshake:

  * TCP socket stays open

  * Both sides can send messages any time (no need for a request)

**Frame structure (simplified)**:

* Control bits (like FIN, opcode)

* Payload length

* Masking key (used by client → server)

* Payload data

---

## **Pros**

* **Low latency**, no need for polling or request overhead

* Uses fewer resources (single TCP connection)

* Great for **event-driven** systems

---

## **Cons**

* Doesn’t scale well over HTTP/1.1 due to **connection limits**

* Connection must stay open → more server memory usage

* Load balancers or proxies may interfere (needs sticky sessions or WebSocket-aware proxies)

* No built-in reconnection logic (you have to implement it)

---

## **Real-world analogy**

Imagine HTTP like sending a letter and waiting for a reply every time.

WebSockets are like opening a **walkie-talkie** channel: you press to talk any time, and the other side listens and responds whenever needed — **no need to re-open the connection**.

---

## **Debugging tips**

* Check browser dev tools: ws:// or wss:// in the Network tab

* Watch for 101 status → that means upgrade succeeded

* Ensure proxies/load balancers allow WebSocket traffic

* Use heartbeat/ping frames to detect dead connections

---

## Bonus: ws:// vs wss://

* ws:// = unencrypted WebSocket over plain TCP

* wss:// = encrypted WebSocket over TLS (like HTTPS)

>Use wss:// in production.

---

#backend