# Promises

A promise is commonly defined as a proxy for a value that will eventually become available.

---
## How they work

Once a promise has been called, it will start in a `pending` state. This means that the calling funciton continues executing. while the promise is `pending` until it resolves, giving the calling function whatever data was being requested.

---
## Creation

The Promise API exposes a Promise constructor, which you initialize using new Promise().

Using `resolve()` and `reject()`, we can communicate back to the caller what the resulting Promise state was, and what to do with it.

---

#nodejs