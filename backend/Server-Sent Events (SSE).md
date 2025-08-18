# Server-Sent Events (SSE)

SSE enables the **server to push updates** to the client over a **single HTTP connection**, using a simple **text-based stream** of events.

* A response has a start and an end
* The client sends a standard HTTP request
* The server sends **logical events** (e.g., messages, updates) within the response body
* The **response never ends** — the server holds it open to stream data
* It remains **compatible with HTTP request/response semantics**, unlike WebSockets

---

## **How SSE Works**

### **Backend**

You must return a response with the appropriate header to indicate an **event stream**:

```js
res.setHeader("Content-Type", "text/event-stream");
res.setHeader("Cache-Control", "no-cache");
res.setHeader("Connection", "keep-alive");
```

Then stream data like:

```js
res.write("data: Hello world\n\n");
```

> Each event is sent in a simple format:

```http
data: message content
```

> An empty line indicates the end of an event.

---
### **Frontend**

You consume SSE with the built-in EventSource API:

```js
const evtSource = new EventSource("/sse");
const eventList = document.querySelector("ul");

evtSource.onmessage = (e) => {
  const newElement = document.createElement("li");
  newElement.textContent = `message: ${e.data}`;
  eventList.appendChild(newElement);
};
```

You can also listen for specific event types, handle connection errors, and automatically reconnect.

---
## **Pros & Cons**

### **✅ Pros**

* **Real-time**: Enables server-to-client push without polling
* **Simple API**: Native support via EventSource — no need for third-party libraries
* **Efficient**: Built on top of **HTTP/1.1 keep-alive**; lower overhead than polling
* **Automatic reconnect**: Built-in retry/reconnect support if the connection drops
* **Text-based**: Easy to debug and stream plain data or JSON

### **❌ Cons**

* **Clients must stay connected**: No built-in queuing for offline clients
* **One-way only**: Server → client only (no client → server via the same channel)
* **Less powerful than WebSockets** for bi-directional communication
* **Not supported in all environments** (e.g., older versions of IE, some mobile webviews)
* **Firewalls or proxies** may kill long-lived HTTP connections
* **Limited to HTTP/1.1 (or 2.0 with caveats)**

---
## **HTTP/1.1 Problem — Six Connections Limit**

### **The Issue:**

* HTTP/1.1 **limits browsers to 6 concurrent connections per origin** (domain)
* (This number varies slightly by browser but 6 is standard in Chrome/Firefox)
* SSE **holds open one of those connections indefinitely**

* If you’re making many simultaneous requests (e.g., images, APIs, etc.), you risk:

  * **Connection starvation**
  * **Stalled requests**
  * Slower page load or broken assets on complex pages

> **Example**: If a page uses SSE and loads many resources, and each uses a separate connection, the SSE stream might block or get deprioritized. Or, the SSE stream might block other essential requests.

### **Workarounds:**

* **Use HTTP/2** or **HTTP/3**: These protocols multiplex streams over a single connection — no more 6-connection limit
* **Switch to WebSockets** if bi-directional communication is required and you’re hitting connection limits
* **Offload SSE to a dedicated subdomain** (e.g., sse.example.com) to isolate the open connection

---
## **Summary Table**

|  **Feature**  |  **SSE**  | 
|---|---|
|  Direction  |  One-way (server → client)  |
|  Protocol  |  HTTP/1.1  |
|  API  |  EventSource  |
|  Connection Limit  |  Affected by HTTP/1.1’s 6-per-origin limit  |
|  Reconnect  |  Built-in  |
|  Streaming Support  |  Yes  |
|  Binary Support  |  No (text-only)  |
|  Compatibility  |  Widely supported (except IE < 10)  |
|  Use case  |  Real-time notifications, live feeds, logs, dashboards  |

---

#backend