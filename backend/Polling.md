# Polling
## Short Polling

- Client sends a request
- Server responds immediately with a handle
- Server continues to process the request
- Client uses the handle to check for status
- Multiple “short” request response as polls

### pros & cons
#### pros

- Simple
- Good for long running process
- Client can disconnect

#### cons

- Too chatty
- Network bandwidth
- Wasted backend resources

---
## Long Polling

- Client sends a request
- Server responds immediately with a handle
- Server continues to process the request
- Client uses that handle to check for status
- Server does not reply until it has a response
- So we got a handle, we can disconnect and we are less chatty

> Long polling is used by kafka.

### pros & cons
#### pros 

- Less chatty and backend friendly
- Client can still disconnect

#### cons

- Not real time

---

#backend