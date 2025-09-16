# Handling events

The event handling mechanism of LynxJS is similar to the Web. However, unlike the Web system, Lynx's event response mechanism supports dual-threaded processing.

Developers can choose to handle the event in the main thread or the background thread.

- When timely event response is not required, you can choose to handle the event in the background thread, and the event processing of the background thread will not block the main thread.
- When timely event response is required, you can choose to handle the event in the main thread, which can avoid event delays caused by cross-threading, but it should be noted that excessive event processing may cause the main thread to be busy.

---
## Listen for user clicks

When a user clicks on a page, the system triggers a `tap` event.

```tsx
<view bindtap={handleTap} />
```

To handle `tap` events on the main thread you can use:

```tsx
<view main-thread:bindtap={handleTap} />
```

When an event is triggered, it calls the event handler function set by the event handler property. The function receives an event object which contains detailed information about the event.

When the event processing function is a main thread script, you need to add a `main thread` directive to the first line of the function body to indicate that the function runs on the **main thread**.

---
## **Main Thread vs Background Thread Event Processing**

### **Main Thread Event Processing**

**Key Difference**:  

  - **Main Thread**: Event objects are operable `EventObject`s.  
  - **Background Thread**: Event objects are pure JSON objects.

**Direct Node Manipulation**:  

When events are handled in the **main thread**, you can directly use `e.currentTarget` to operate on nodes.
  
**Example:** Change the node's background color using `setStyleProperty`.

#### **Example: Main Thread Event Handling**

```tsx
import { root } from "@lynx-js/react";
import type { MainThread } from "@lynx-js/types";

export default function App() {

  function handleTapInMTS(e: MainThread.TouchEvent) {
    "main thread";
    const rndCol = `rgb(${Math.floor(Math.random() * 256)}, ${Math.floor(Math.random() * 256)}, ${
      Math.floor(Math.random() * 256)
    })`;
    e.currentTarget.setStyleProperty("background-color", rndCol);
  }

  return (
    <view
      style={{ width: "100%", height: "100%", justifyContent: "center", alignItems: "center" }}
      main-thread:bindtap={handleTapInMTS}
    >
      <view
        style={{
          width: "100px",
          height: "50px",
          borderRadius: "5px",
          backgroundColor: "gray",
          justifyContent: "center",
          alignItems: "center",
        }}
      >
        <text style={{ color: "red" }}>click me</text>
      </view>
    </view>
  );
} 

root.render(<App />);
```

### **Background Thread Event Processing**

- **Limitation**:
    You **cannot** directly manipulate nodes using e.currentTarget.
- **Solution**:
    Use SelectorQuery and setNativeProps to modify node properties.

#### **Example: Background Thread Event Handling**

```tsx
import { root } from "@lynx-js/react";
import type { NodesRef, SelectorQuery, TouchEvent } from "@lynx-js/types";

export default function App() {
  function handleTap(e: TouchEvent) {
    const rndCol = `rgb(${Math.floor(Math.random() * 256)}, ${Math.floor(Math.random() * 256)}, ${
      Math.floor(Math.random() * 256)
    })`;
    (lynx
      .createSelectorQuery() as { selectUniqueID(uid: number): NodesRef } & SelectorQuery)
      .selectUniqueID(e.currentTarget.uid)
      .setNativeProps({
        "background-color": rndCol,
      })
      .exec();
  }

  return (
    <view
      style={{ width: "100%", height: "100%", justifyContent: "center", alignItems: "center" }}
      bindtap={handleTap}
    >
      <view
        style={{
          width: "100px",
          height: "50px",
          borderRadius: "5px",
          backgroundColor: "gray",
          justifyContent: "center",
          alignItems: "center",
        }}
      >
        <text style={{ color: "red" }}>click me</text>
      </view>
    </view>
  );
}

root.render(<App />);
```

---