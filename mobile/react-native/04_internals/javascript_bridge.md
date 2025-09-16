# JavaScript Bridge

This note explains the role of the JavaScript Bridge in the old React Native architecture, detailing how communication occurs between the JavaScript thread and the native UI thread.

## What is the JavaScript Bridge?

- **Communication Layer:** The JavaScript Bridge is a serialization layer that facilitates asynchronous, batched, and serializable communication between the JavaScript thread and the native threads (UI thread, shadow thread).
- **Asynchronous:** All communication across the bridge is asynchronous to prevent blocking the UI thread.
- **Batched:** Messages are batched together to reduce overhead and improve performance.
- **Serializable:** Data passed over the bridge must be JSON-serializable.

## How it Works

1.  **JavaScript to Native:** When JavaScript needs to interact with a native module (e.g., access camera, GPS), it sends a message to the bridge.
2.  **Serialization:** The message (function call, arguments) is serialized into a JSON string.
3.  **Native Execution:** The native side deserializes the message and executes the corresponding native code.
4.  **Native to JavaScript:** If the native operation has a result or callback, it sends a message back to the bridge.
5.  **Deserialization:** The message is deserialized on the JavaScript side, and the callback is invoked.

## Limitations

- **Asynchronous Nature:** Can lead to complex code when dealing with synchronous-like operations.
- **Serialization Overhead:** The process of serializing and deserializing data can introduce performance bottlenecks, especially for large amounts of data.
- **Single-threaded JavaScript:** The JavaScript thread can become a bottleneck if heavy computations are performed on it.

## Bridge in the Old Architecture

In the old architecture, the bridge was the sole communication channel. This design, while effective, had inherent limitations that the New Architecture aims to address.