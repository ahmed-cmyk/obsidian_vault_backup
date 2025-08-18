# Request - Response

In TCP, the data you send is a continuous stream which the server needs to parse. Then it needs to process the request. The server then sends a response and the client receives and consumes the response.

Parsing means this is where it starts and this is where it ends.

It is used:
- Web, HTTP, DNS, SSH
- Remote Procedure Call (RPC)
- SQL and Database protocols
- APIs (REST/SOAP/GraphQL)

---
## Anatomy

- A request is structured by both the client and server.
- Request has a boundary
- Defined by a protocol and message format
- Same for the response

> There is a cost to certain parsers. The operation is not cheap. For example, parsing JSON in C++ isn’t cheap.

```http
GET / HTTP/1.1
Headers
<CSRF>
Body
```

---
## Building an image upload service with request response

- Send large request with image (old)
- Chunk image and send a request per chunk (resumable)

---
## Doesn’t work everywhere

- Notification service
- Chatting application
- Very long requests
- What if client disconnects?

---

#backend