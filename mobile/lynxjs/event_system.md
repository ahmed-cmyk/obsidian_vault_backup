# **LynxJS Event System**

## **1. What is a Node?**

A **node** in LynxJS is a unit in the UI tree that:

- Holds data (attributes, state)
- Renders something on screen
- Can have event handler properties (e.g., onClick)
- Can listen for or intercept events during propagation

> **NOTE:** Think of nodes like DOM elements in the browser — but in LynxJS’ own virtual tree.

---
## **2. Event Response Chain**

When an event is triggered, LynxJS builds an **event path** — a list of nodes from the root down to the target node.

### **Phases of Propagation**

LynxJS uses two traversal phases (the **response chain**):

1. **Capture Phase (downward)**
    - Event travels from **root → down the tree** until it reaches the target.
    - Nodes can handle events early or even intercept them.
2. **Bubble Phase (upward)**
    - After reaching the target, event travels **back up the tree**.
    - Nodes higher up can handle the event after the target has processed it.

> **NOTE:** The **target phase** (when the event hits the exact node that triggered it) is not considered part of the “response chain” because it doesn’t involve traversal — it’s just a single-node step.

---
## **3. Full Event Lifecycle (Step-by-Step)**

1. **Native OS Event Occurs**
    - The OS (iOS/Android/Web) emits a touch/mouse/keyboard event.
    - LynxJS has one **global listener** attached at the root.
2. **Hit Testing**
    - LynxJS determines which node was under the pointer/touch.
    - Builds an **event path**: Root → Container → TargetNode.
3. **Synthetic Event Creation**
    - LynxJS creates a normalized event object.
    - Adds methods like stopPropagation() and preventDefault().
    - Attaches the event path.
4. **Capture Phase**
    - Iterates downward through the path.
    - Calls all capture listeners.
    - Stops if stopPropagation() is called.
5. **Target Phase**
    - Calls the target node’s own event handler.
    - Can still stop propagation to prevent bubbling.
6. **Bubble Phase**
    - Iterates upward through the path.
    - Calls all bubble listeners.
    - Stops if propagation was stopped.
7. **Cleanup**
    - Event object may be recycled for performance.

---
## **4. Why Events Originate from the Root**

  
Even though you clicked the target node, the event dispatch **starts at the root** because:

- **Performance:** Only one native listener is needed, rather than one per node.
- **Consistency:** Guarantees capture → target → bubble order.
- **Control:** Allows LynxJS to support interception, propagation control, and synthetic events.
- **Cross-Platform:** Normalizes events across iOS/Android/Web before delivering them to nodes.

> **NOTE:** Think of it like a central post office: the button “reports” the event, and the root dispatch system delivers it to everyone in the correct order.

---
## **5. Example Log Output**

For a tree:

```
Root
 └─ Container
      └─ Button
```

And handlers:

```
Root.onClickCapture = () => console.log("Root capture");
Container.onClickCapture = () => console.log("Container capture");
Button.onClickCapture = () => console.log("Button capture");

Button.onClick = () => console.log("Button target");

Button.onClickBubble = () => console.log("Button bubble");
Container.onClickBubble = () => console.log("Container bubble");
Root.onClickBubble = () => console.log("Root bubble");
```

Clicking the button logs:

```
Root capture
Container capture
Button capture
Button target
Button bubble
Container bubble
Root bubble
```

---
## **6. Key Takeaways**

- **Capture = down, Bubble = up** — root is at the top of the tree.
- Target phase is a single step, not part of the traversal chain.
- Centralized event dispatch at the root provides performance, control, and consistency.
- You can intercept events at any point using stopPropagation().
- Understanding the full lifecycle is critical for debugging complex UI interactions.

---