# Synchronous vs Asynchronous

Asynchronous means that two things — the client and the server — are not in sync. In backend, you’d find that we don’t actually want them to be in sync, we want them to be doing different things.

In synchronous, applications block while they’re processing a particular request and during that time you cannot do anything else. Once the receiver responds the caller unblocks.

## Example (Synchronous)

- Program asks OS to read from disk
- Program main thread is taken off of the CPU
- Read completes, program can continue execution. This is referred to as context switching.

> Doing too much context switching is expensive and should be done as little as possible.

---
## Example (Asynchronous)

- Caller sends a request
- Caller can work until it gets a response
- Caller either:
  - Checks if the response is read (epoll)
  - Receiver calls back when it’s done (io_uring)
  - Spins up a new thread that blocks. The new thread calls back to the main thread once it’s done (NodeJS uses this trick to pretend to be asynchronous)
- Caller and receiver are not necessarily in sync

---

When you write  a file to disk it goes first to the OS file system cache where writes go to in the form of pages and then asynchronously the OS will wait for a lot of writes in the memory and then flush them all together. Without this all writes will go to the disk and cause fragmentation.

> DB people hate this. You can skip this using an fsync property.

---

#backend