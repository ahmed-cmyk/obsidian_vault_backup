# Event Propagation

When an event is triggered, it will propagate along the *event response chain*. If the corresponding type of event handler property is set on the node, the node can listen to the corresponding event or even intercept it during the event propagation process.

> **NOTE:** LynxJS also provides cross-component event monitoring, event aspect interface, and `GlobalEventEmitter` to implement special event propagation.

---
## **Event Handler Property**

By setting the event handler properties, developers can decide at which stage (or across components) of event propagation to listen or intercept the event, and specify the processing function to be called when the event is triggered.

The names of these event handler properties are usually composed of the bound event type and event name.

---