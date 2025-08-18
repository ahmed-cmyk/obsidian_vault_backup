# Publish-Subscribe (Pub/Sub)

**Pub/Sub** is a **messaging pattern** where **senders (publishers)** do not send messages directly to specific receivers. Instead, messages are sent to a **topic/channel**, and **receivers (subscribers)** listen to those topics.

* **Decouples** the sender from the receiver
* Enables **asynchronous**, **event-driven** communication
* Commonly used in **event-based systems**, **message queues**, **real-time apps**, and **microservices**

---
## **How It Works**

1. **Publishers** emit messages to a **topic/channel**
2. **Subscribers** register interest in specific topics
3. A **broker** (message server) handles delivery of messages from publishers to subscribers

⠀
---

### **Message Flow (Conceptual)**

```
Publisher → Topic → Broker → Subscribers
```

> Publishers don’t know who the subscribers are.

> Subscribers don’t know who the publisher is.

> The **broker handles all routing.** 

---
## **Key Components**

|  **Component**  |  **Role**  | 
|---|---|
|  **Publisher**  |  Sends messages to a topic  | 
|  **Subscriber**  |  Receives messages for a topic  | 
|  **Broker**  |  Mediates between publisher and subscriber, routes messages  | 
---

## **Implementation Types**

### 1. Message Brokers (Backend Systems)

* **Redis Pub/Sub**
* **RabbitMQ / Kafka / NATS**
* **Google Cloud Pub/Sub**
* **AWS SNS (Simple Notification Service)**

### 2. Frontend / Web Implementations

* Event buses in frameworks (e.g., EventEmitter, RxJS)
* Custom event systems (window.addEventListener)
* WebSockets or SSE can be used as transport for pub/sub in real-time systems

---
## **Example (Pseudo-code)**

```js
// Publisher
pubsub.publish("chat", "new message");

// Subscriber
pubsub.subscribe("chat", (msg) => {
  console.log("Received:", msg);
});
```

---
## **Pros & Cons**

### **✅ Pros**

* **Loose coupling** between components
* **Scalable**: Easy to add more publishers/subscribers
* **Asynchronous**: Improves performance, responsiveness

* Useful for:

  * **Microservice communication**
  * **Real-time updates**
  * **Log/event streaming**
  * **Notifications, chat apps, analytics**

### **❌ Cons**

* **Delivery is not always guaranteed** (depends on broker and config)
* **Debugging is harder** due to indirection
* **Order of delivery** may not be guaranteed
* Requires **careful topic naming** and **subscription management**
* Risk of **message loss** if subscriber is offline (unless using persistent systems like Kafka)

---
## **Delivery Models**

|  **Model**  |  **Description**  | 
|---|---|
|  **At-most-once**  |  Fastest, but may drop messages  | 
|  **At-least-once**  |  Retries until delivered, can cause duplicates  | 
|  **Exactly-once**  |  Most reliable, but complex and slower (e.g., Kafka with idempotence)  | 

---
## **Broker vs Brokerless**

|  **Style**  |  **Description**  | 
|---|---|
|  **Broker-based**  |  Uses a centralized server (e.g., Redis, Kafka)  | 
|  **Brokerless / Local**  |  In-process pub/sub (e.g., event bus in a JS app)  | 

---
## **Common Use Cases**

* **Chat applications**
* **Real-time dashboards**
* **Notifications / Alerts**
* **IoT data collection**
* **Audit / Logging systems**
* **Event-driven microservices**

---
## **Comparison with Other Models**

|  **Feature**  |  **Pub/Sub**  |  **SSE**  |  **WebSockets**  | 
|---|---|---|---|
|  Direction  |  Bi-directional (via brokers)  |  One-way (server → client)  |  Bi-directional  |
|  Coupling  |  Loose  |  Tighter  |  Tighter  |
|  Broker Required  |  Usually  |  No  |  No  |
|  Message Durability  |  Yes (if broker supports it)  |  No  |  No  |
|  Scalability  |  High  |  Moderate  |  High  |
|  Use Case  |  Event streams, distributed systems  |  Notifications, logs  |  Games, chat, collaborative apps  |

---

#backend