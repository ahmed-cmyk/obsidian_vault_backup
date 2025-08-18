# System Design: Availability Patterns

There are two complimentary patterns to support high-availability:

1. Fail-over
2. Replication

---
## Fail-over
### Types
#### Active-passive

With active-passive fail-over, heartbeats are sent between the active and passive server on standby. If the heartbeat is interrupted, the passive server takes over the active’s IP address and resumes service.

The length of the downtime is determined by whether the passive server is already running in "hot" standby or whether it needs to startup from “cold” standby. Only the active server handles traffic.

> Active-passive fail-over can also be referred to as master-slave fail-over.

#### Active-active

In active-active, both servers are managing traffic, spreading the load between them.

- If the servers are public-facing, the DNS would need to know about the public IPs of both servers.
- If the servers are internal facing, application logic would need to know about both servers.

> Active-active fail-over can be referred to as master-master fail-over.

### Disadvantages

- Fail-over adds more hardware and additional complexity.
- There is a potential for loss of data if the active system fails before any newly written data can be replicated to the passive.

---
## Replication

Master-slave and master-master

**—————— TODO ——————**

---

#backend