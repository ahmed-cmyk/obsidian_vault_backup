# process.nextTick

Every time the event loop takes a full trip, we call it a tick. When we pass a function to `process.nextTick()`, we instruct the engine to invoke this function at the end of the current operation before the next event loop tick starts.

---

#nodejs