# **gRPC**

**gRPC** (Google Remote Procedure Call) is a high-performance, open-source RPC framework that uses HTTP/2 as transport and Protocol Buffers (Protobuf) for data serialization.

---

## **What it does**

* Allows communication between **distributed services** using RPC

* Client calls methods on a remote server as if it were a local object

* Supports **streaming**, **authentication**, **deadlines**, **cancellation**, etc.

* Cross-language: Works with Go, Python, Java, C++, Node.js, etc.

---

## **Where it’s used**

* **Microservices communication** (especially in large backend systems)

* High-performance backend APIs (low latency, high throughput)

* Internal APIs where both client and server are under your control

* Common in systems built by Google, Netflix, Square, Dropbox, etc.

---

## **Pros**

* **Efficient binary format** (Protobuf) → smaller payloads, faster parsing

* Built on **HTTP/2** → supports **multiplexing**, **compression**, **streams**

* Supports **bi-directional streaming**: client ↔ server communication over a single connection

* Auto-generates client/server code from .proto files

* Built-in **auth**, **timeouts**, and **metadata**

* **Language-agnostic**

---

## **Cons**

* **Steep learning curve** for Protobuf and tooling

* Debugging is harder (binary format)

* Poor native support in browsers (requires gRPC-Web or proxies)

* Not REST/HTTP1.1 compatible (unless using gRPC Gateway)

* Requires tightly-coupled client-server development (both must share .proto files)

---

## **Basic structure**

```
// service definition (.proto)
syntax = "proto3";

service Greeter {
  rpc SayHello (HelloRequest) returns (HelloReply);
}

message HelloRequest {
  string name = 1;
}

message HelloReply {
  string message = 1;
}
```

---

## **Types of RPCs**

* **Unary**: One request → one response

* **Server streaming**: One request → stream of responses

* **Client streaming**: Stream of requests → one response

* **Bi-directional streaming**: Stream of requests ↔ stream of responses

---

## **Real-world analogy**

Calling a remote method using gRPC is like invoking a method on a local class — except it happens across the network, and the request/response is serialized using Protobuf.

---

## **Protobuf vs JSON**

|  **Feature**  |  **Protobuf**  |  **JSON**  | 
|---|---|---|
|  Size  |  Small (binary)  |  Large (text)  |
|  Speed  |  Fast parsing  |  Slower  |
|  Schema  |  Required  |  Optional  |
|  Human-readable  |  No  |  Yes  |
|  Typing  |  Strong  |  Loose  |
---

## **When to use gRPC**

Use gRPC when:

* You need **performance** and **type safety**

* You control both client and server

* You need **streaming**

* You work in **microservice architectures**

* You can afford the complexity of managing .proto definitions

---

## **When NOT to use gRPC**

Avoid gRPC when:

* Your clients are **web browsers** (unless using grpc-web)

* You need **simple REST APIs** for public exposure

* You want **human-readable logs or requests**

---

## **Tools & Ecosystem**

* protoc: Protocol buffer compiler

* grpcurl: CLI to call gRPC methods (like curl for gRPC)

* grpc-web: Enables browser support

* grpc-gateway: Converts gRPC to REST + JSON

* evans: Interactive REPL for gRPC services

---

#backend