# Event Propagation

When an event is triggered, it will propagate along the *event response chain*. If the corresponding type of event handler property is set on the node, the node can listen to the corresponding event or even intercept it during the event propagation process.

> **NOTE:** LynxJS also provides cross-component event monitoring, event aspect interface, and `GlobalEventEmitter` to implement special event propagation.

---
## **Event Handler Property**

By setting the event handler properties, developers can decide at which stage (or across components) of event propagation to listen or intercept the event, and specify the processing function to be called when the event is triggered.

The names of these event handler properties are usually composed of the bound event type and event name.

| Event Type      | Description                                                                         |
| --------------- | ----------------------------------------------------------------------------------- |
| `bind`          | Listen to events in the bubbling stage, and do not intercept event bubbling.        |
| `catch`         | Listen to events in the bubbling stage and intercept event bubbling.                |
| `capture-bind`  | Listen to events in the capture phase, do not intercept event capture and bubbling. |
| `capture-catch` | Listen to events in the capture phase, intercept event capture and bubbling.        |
| `global-bind`   | Listen to events across components.                                                 |

> **NOTE:** When the event handler is a main thread script, you need to add the `main-thread:` prefix before the event handler property name to ensure that the handler is executed in the main thread.

---
## **Event Response Chain**

The event response chain refers to a linked list of nodes that can respond to events. Generally speaking, the event response chain consists of the path from the root node of the page to the node where the action is actually triggered.

```tsx
import { root, useState } from "@lynx-js/react";
import type { TouchEvent } from "@lynx-js/types";

export default function App() {
  const [tap, setTap] = useState<boolean>(false);
  const [tap1, setTap1] = useState<boolean>(false);
  const [tap11, setTap11] = useState<boolean>(false);
  const [tap2, setTap2] = useState<boolean>(false);
  const [tap22, setTap22] = useState<boolean>(false);

  function handleTap(e: TouchEvent) {
    if (e.currentTarget.id === "tap") {
      setTap(!tap);
    }
    if (e.currentTarget.id === "tap1") {
      setTap1(!tap1);
    }
    if (e.currentTarget.id === "tap11") {
      setTap11(!tap11);
    }
    if (e.currentTarget.id === "tap2") {
      setTap2(!tap2);
    }
    if (e.currentTarget.id === "tap22") {
      setTap22(!tap22);
    }
  }

  function handletouchstart(e: TouchEvent) {
    setTap(false);
    setTap1(false);
    setTap11(false);
    setTap2(false);
    setTap22(false);
  }

  return (
    <view
      id="tap"
      style={{
        width: "calc(100% - 40px)",
        height: "90%",
        margin: "20px",
        borderRadius: "10px",
        boxShadow: "0 0 5px 5px #ccc",
        backgroundColor: tap ? "rgb(255, 179, 0)" : "white",
        justifyContent: "center",
        alignItems: "center",
      }}
      bindtap={handleTap}
      capture-bindtouchstart={handletouchstart}
    >
      <view
        id="tap1"
        style={{
          width: "70%",
          height: "40%",
          marginBottom: "25px",
          borderRadius: "10px",
          boxShadow: "0 0 5px 5px #ccc",
          backgroundColor: tap1 ? "rgb(255, 179, 0)" : "white",
          justifyContent: "center",
          alignItems: "center",
        }}
        bindtap={handleTap}
      >
        <view
          id="tap11"
          style={{
            width: "60%",
            height: "45%",
            borderRadius: "10px",
            boxShadow: "0 0 5px 5px #ccc",
            backgroundColor: tap11 ? "rgb(255, 179, 0)" : "white",
            justifyContent: "center",
            alignItems: "center",
          }}
          bindtap={handleTap}
        >
          <text user-interaction-enabled={false} style={{ color: "green" }}>click me 1</text>
        </view>
      </view>
      <view
        id="tap2"
        style={{
          width: "70%",
          height: "40%",
          marginTop: "25px",
          borderRadius: "10px",
          boxShadow: "0 0 5px 5px #ccc",
          backgroundColor: tap2 ? "rgb(255, 179, 0)" : "white",
          justifyContent: "center",
          alignItems: "center",
        }}
        bindtap={handleTap}
      >
        <view
          id="tap22"
          style={{
            width: "60%",
            height: "45%",
            borderRadius: "10px",
            boxShadow: "0 0 5px 5px #ccc",
            backgroundColor: tap22 ? "rgb(255, 179, 0)" : "white",
            justifyContent: "center",
            alignItems: "center",
          }}
          bindtap={handleTap}
        >
          <text user-interaction-enabled={false} style={{ color: "red" }}>click me 2</text>
        </view>
      </view>
    </view>
  );
}

root.render(<App />);

if (import.meta.webpackHot) {
  import.meta.webpackHot.accept();
}
```

---
## **Event Capture**

The event will go through two stages in the event response chain:

1. Event capture
2. Event bubbling

In the event capture stage, the event will start from the root node of the page and propagate down along the event response chain until the node where the action is actually triggered.

In the event capture stage, nodes with the event handler property of the `capture-bind` type set can listen to the corresponding event.

```tsx
import { root, useState } from "@lynx-js/react";
import type { TouchEvent } from "@lynx-js/types";

export default function App() {
  const [cnt, setCnt] = useState<number>(0);

  function handleTap(e: TouchEvent) {
    setCnt(cnt + 1);
  }

  return (
    <view
      style={{ width: "100%", height: "90%" }}
      capture-bindtap={handleTap}
    >
      <view
        style={{
          width: "calc(100% - 10px)",
          height: "150px",
          margin: "5px",
          borderWidth: "2px",
          justifyContent: "center",
          alignItems: "center",
        }}
      >
        <text>
          Counts the number of clicks on a page: <text style={{ color: "red" }}>{cnt}</text>
        </text>
      </view>
      <scroll-view
        scroll-orientation="vertical"
        style={{ width: "100%", height: "calc(100% - 150px)" }}
      >
        {[0, 1, 2, 3, 4, 5, 6].map((item) => {
          return (
            <view
              style={{
                width: "calc(100% - 10px)",
                height: "150px",
                margin: "5px",
                backgroundColor: `rgb(${Math.floor(Math.random() * 256)}, ${Math.floor(Math.random() * 256)}, ${
                  Math.floor(Math.random() * 256)
                })`,
                borderRadius: "5px",
                justifyContent: "center",
                alignItems: "center",
              }}
            >
              <text>item-{item + 1}</text>
            </view>
          );
        })}
      </scroll-view>
    </view>
  );
}

root.render(<App />);

if (import.meta.webpackHot) {
  import.meta.webpackHot.accept();
}
```

---
## **Event Interception**

During the process of event propagation, the event can be intercepted midway to prevent the event from continuing to propagate.

```tsx
import { root, useState } from "@lynx-js/react";
import type { TouchEvent } from "@lynx-js/types";

export default function App() {
  const [bgColor, setBgColor] = useState<string>("white");

  function handleTap(e: TouchEvent) {
    const rndCol = `rgb(${Math.floor(Math.random() * 256)}, ${Math.floor(Math.random() * 256)}, ${
      Math.floor(Math.random() * 256)
    })`;
    setBgColor(rndCol);
  }

  return (
    <view
      style={{
        width: "100%",
        height: "100%",
        backgroundColor: bgColor,
        justifyContent: "center",
        alignItems: "center",
      }}
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
        catchtap={(e: TouchEvent) => {}}
      >
        <text style={{ color: "red" }}>click me</text>
      </view>
    </view>
  );
}

root.render(<App />);

if (import.meta.webpackHot) {
  import.meta.webpackHot.accept();
}
```

---
## **Cross-Component Event Listening**

Normally, only nodes on the event response chain can listen to events. Lynx allows developers to listen to events globally or across components.

### `global-bind` Event Handlers

- **Purpose:** Attach event listeners that work across the entire page, regardless of the node’s position in the event chain.
- **Example:** `global-bindtap`, `global-bindscroll`
- **Use Case:** Count all taps or detect scroll events across multiple components.

```tsx
<view
  style={{ width: "100%", height: "100%" }}
  global-bindtap={handleTap}
  global-bindscroll={handleScroll}
>
  ...
</view>
```

- **Behavior:**
    - Listens to events globally, not just on descendants.
    - Great for analytics (page-wide click counters) or unified scroll detection.

### `global-target`

- **Purpose:** Filter global events to only listen to events from specific node IDs.
- **Example:**

```tsx
<view global-bindtap={handleTap} global-target="scroll-1,scroll-2" />
```

- **Result:** `handleTap` only fires when taps originate from nodes with IDs `scroll-1` or `scroll-2`.

---
## **Event Aspect Interface (`beforePublishEvent`)**

- **Purpose:** Intercept events before they are published in the event system.
- **Scope:** Works only in the **background thread (BTS)**.
- **Usage:** Good for instrumentation, analytics, or logging.

### Example

```tsx
useMemo(() => {
  lynx.beforePublishEvent.add("tap", () => {
    setCnt((cnt) => cnt + 1);
  });
}, []);
```

- **Behavior:** Fires for _every_ `tap` event before normal propagation.
- **Use Case:** Centralized event logging without attaching multiple listeners.

---
## `GlobalEventEmitter`

- **Purpose:** Pass events across components or between client ↔ front end, independent of DOM tree.
- **Available in:** Background threads (BTS context only).

### API

|Method|Description|
|---|---|
|`addListener(eventName, listener, context?)`|Subscribe to a global event|
|`removeListener(eventName, listener)`|Remove specific listener|
|`removeAllListeners(eventName)`|Remove all listeners for an event|
|`toggle(eventName, ...data)`|Broadcast event with data (multiple args supported)|
|`trigger(eventName, params)`|Broadcast event with one parameter|

### **Example: Broadcasting Events Between Components**

```tsx
function ComponentB() {
  function handleTap(e: TouchEvent) {
    lynx.getJSModule("GlobalEventEmitter").toggle("tapitem", e);
  }
}

function ComponentA() {
  useLynxGlobalEventListener("tapitem", (e) => {
    setEventlog((e as TouchEvent).target.dataset.item);
  });
}
```

- **Result:** ComponentA responds to taps happening inside ComponentB, even though they are not in the same component hierarchy.

---
## **Event Subscription From Lynx (Front-End ↔ Client)**

- **Use Case:** Listen to events triggered by the client, e.g., `onWindowResize`.

```tsx
useLynxGlobalEventListener("onWindowResize", (e) => {
  setEventLog("" + e);
});
```

- **Behavior:** Reacts whenever Lynx notifies the page of size changes.

---
## **Summary**

|Mechanism|Scope|Typical Use|
|---|---|---|
|`global-bind`|Global (all nodes)|Count taps, monitor global scroll|
|`global-target`|Filtered global|Listen to specific node IDs|
|`beforePublishEvent`|BTS only|Analytics/logging before propagation|
|`GlobalEventEmitter`|Global (BTS)|Cross-component communication|
|`useLynxGlobalEventListener`|Global listener hook|Client → front-end events (resize, custom events)|

**Key Takeaway:** These tools let you build **analytics, cross-component communication, and global event handling**without wiring every node manually.

---