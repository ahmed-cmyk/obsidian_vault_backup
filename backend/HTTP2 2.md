# **HTTP/2**

HTTP/2 is a major upgrade over HTTP/1.1, designed to solve performance bottlenecks — **especially latency** and **parallelism** — without changing the core web semantics (still request/response).

---

## **What it does**

* Improves performance by allowing multiple **concurrent streams** over a single TCP connection

* **Binary protocol**, not text-based like HTTP/1.1

* Adds **header compression**

* Supports **server push**

---

## **Where it’s used**

* Supported by all major browsers and CDNs (Cloudflare, Fastly)

* Used to speed up API calls, websites, and microservices that need **many concurrent requests**

* Backend systems with many small HTTP calls between services benefit from it

---

## **Pros**

* **Multiplexing**: multiple requests/responses at once → no head-of-line blocking at the app layer

* **Binary format**: faster parsing, less error-prone than text

* **Header compression (HPACK)**: reduces bandwidth use for repetitive headers (e.g. Authorization, User-Agent)

* **Server push**: server can push resources before the client asks (e.g. CSS with HTML)

* Still uses **TCP** (same ports: 80/443)

---

## **Cons**

* **Still suffers from TCP-level head-of-line blocking** (unlike HTTP/3)

* **Server push** often adds complexity with minimal gains → mostly unused in practice

* More complex to debug at low level due to binary framing

* Needs **ALPN** during TLS handshake to negotiate protocol upgrade

---

## **How it works (handshake + packet level)**

1. If using HTTPS (which is required by most browsers), client uses **ALPN** in the TLS handshake to request HTTP/2

2. Server responds with support → connection is upgraded

3. Messages are split into **frames**:

   * HEADERS frame for metadata (compressed)

   * DATA frames for body

   * SETTINGS, PING, PRIORITY, etc.

⠀
All messages share the same TCP connection but are isolated via **stream IDs** 

---

## **Real-world analogy**

HTTP/1.1: One-lane road → cars queue up and block each other

HTTP/2: Multi-lane highway → multiple cars can drive in parallel, no bottlenecks

---

## **Debugging tips**

* Use curl --http2 or Chrome DevTools to test support

* Wireshark can show binary frame types and stream IDs

* Watch for dropped or stalled frames if a proxy doesn’t support HTTP/2

* **ALPN** misconfigurations in TLS setups are a common cause of fallback to HTTP/1.1

---

## **Notes for deployment**

* Requires TLS in most browsers → h2 negotiated during TLS via ALPN

* Use HTTP/2-aware reverse proxies/load balancers (e.g. NGINX with http2 directive)

* Beware of middleboxes or old proxies that don’t understand binary framing

---

#backend