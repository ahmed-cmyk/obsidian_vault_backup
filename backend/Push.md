# Push

Request - response isn’t always ideal like when a client wants realtime notification from backend.

- Client connects to server
- Server sends data to client
- Client doesn’t have to request anything
- Protocol must be bidirectional
- Used by RabbitMQ

Pros

- Realtime

Cons

- Clients must be online
- Clients might not be able to handle the load
- Requires a bidirectional protocol
- Polling is preferred for light clients

---

#backend