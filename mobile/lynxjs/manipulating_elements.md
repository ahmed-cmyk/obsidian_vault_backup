# **LynxJS: Manipulating Elements & Using References**

Modern front-end frameworks usually handle **element tree updates** and **node property changes** automatically.

But there are times you need **direct element manipulation**, such as:

- Controlling media players
- Programmatically scrolling views
- Getting element position/dimensions
- Dynamically modifying styles

In LynxJS, this is done by **getting a reference** to the element and calling methods on it.

---
## **1. Obtaining a Reference to an Element**

Lynx provides a React-like useRef hook, but its type is NodesRef:

```tsx
const nodeRef = useRef<NodesRef>(null);
// ...
<text ref={nodeRef} />;
```

- **NodesRef** is different from React’s RefObject.
- It provides special methods (invoke, exec, etc.) to talk to the client-side component implementation.

---
## **2. Manipulating Elements via Reference**

Once you have the reference, you can call methods on it using .invoke().

**Example:** Get element bounding box

```tsx
ref
  .invoke({
    method: "boundingClientRect",
    params: { relativeTo: "screen" },
    success: (res) => {
      const { left, top, width, height } = res;
    },
    fail: (err) => {
      console.error("Failed to get element position:", err);
    },
  })
  .exec();
```

- params → Arguments passed to the method
- success / fail → Callbacks
- exec() → Submits the operation for execution

---
## **3. Example: Auto-Scroll in Background Thread**

```tsx
const scrollRef = useRef<NodesRef>(null);

function handleTap() {
  scrollRef.current
    ?.invoke({
      method: "autoScroll",
      params: { rate: 120, start: true },
    })
    .exec();
}

<scroll-view ref={scrollRef}> ... </scroll-view>
```

**Steps:**

1. Create a reference with `useRef`
2. Attach with `ref={scrollRef}`
3. Call `.invoke()` in event handler
4. Finish with `.exec()`

---
## **4. Manipulating Elements in the Main Thread**

Main thread manipulation has **lower latency** and **faster UI response**.

Use `useMainThreadRef` and `main-thread:ref`:

```tsx
const scrollRef = useMainThreadRef<MainThread.Element>(null);

function handleTap() {
  "main thread";
  scrollRef.current?.invoke("autoScroll", { rate: 120, start: true });
}

<scroll-view main-thread:ref={scrollRef}> ... </scroll-view>
```

**Key differences:**

- `MainThread.Element` replaces `NodesRef`
- No need for `.exec()`
- API calls feel more “synchronous” like the browser DOM

---
## **5. Obtaining Element References via Selectors**

### **Background Thread**

Use `SelectorQuery`:

```tsx
lynx
  .createSelectorQuery()
  .select("scroll-view")
  .invoke({ method: "autoScroll", params: { rate: 120, start: true } })
  .exec();
```

### **Main Thread**

Much simpler — use querySelector:

```tsx
"main thread";
lynx.querySelector("scroll-view")
  ?.invoke("autoScroll", { rate: 120, start: true });
```

---
## **6. Accessing the Event Target Element**

In the main thread, event objects have target and currentTarget, just like the browser DOM.

```tsx
function handleTap(e: MainThread.TouchEvent) {
  "main thread";
  e.currentTarget.setStyleProperty(
    "color",
    "linear-gradient(to right, rgb(255,53,26), rgb(0,235,235))"
  );
}
```

This lets you directly manipulate the element that triggered the event.

---
## **7. Using getElementById**

- Best choice for animations and CSS variable manipulation.
- Used when you need **imperative control** over an element (similar to vanilla JS).
- Lynx is working on more modern alternatives.

---
### **Key Takeaways**

- **useRef / useMainThreadRef** → get references to elements.
- **invoke() + exec()** (background thread) vs direct invoke() (main thread).
- **Selectors** (createSelectorQuery, querySelector) are useful for batch/dynamic operations.
- Main thread references feel more like the browser DOM and are more performant.

---