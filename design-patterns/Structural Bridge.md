# Structural: Bridge

**Bridge** is a structural design pattern that decouples an abstraction from its implementation so that the two can vary independently.

---
## Structure

1. **Abstraction**: Defines the high-level control logic and maintains a reference to an implementation.
2. **Refined Abstraction**: A subclass of the abstraction that may provide extended or specialized behavior.
3. **Implementation Interface**: Declares the interface for low-level operations.
4. **Concrete Implementations**: Provide platform-specific or variant-specific logic.
5. **Bridge**: The abstraction uses composition (not inheritance) to delegate actual work to the implementation.

This pattern is all about **composition over inheritance** and separates concerns between **what is done** (abstraction) and **how it’s done** (implementation).

---
## Applicability

Use the Bridge pattern when:

* You want to avoid a **combinatorial explosion** of subclasses for every feature + platform/config variant.
* You want to separate abstraction and implementation for independent evolution.
* You want to switch implementations at runtime.
* You’re building a framework or plugin-based system with multiple interchangeable components.

---
## How to Implement

1. Identify parts of your class hierarchy that are prone to change.
2. Extract these behaviors into a separate **implementation interface**.
3. Create one or more **concrete implementations** for different variants.
4. Create the **abstraction** class that references the implementation interface.
5. Use **composition** inside the abstraction to delegate implementation details.

---
## Pros and Cons

| Pros | Cons |
|---|---|
| Eliminates tight coupling between abstraction and implementation. | Adds extra layers of abstraction and indirection. |
| Promotes code reusability and composition. | Can be overkill if you don’t need multiple variants. |
| Enables independent evolution of interfaces and implementations. | Slightly more complex to trace behavior flow. |
| Supports runtime flexibility for switching implementations. | Refactoring a hierarchy into Bridge can be effort-intensive. |

---
## Example Use Cases

* Rendering UI components differently on different platforms (e.g., Windows vs Linux).
* Exporting documents in multiple formats (e.g., PDF, DOCX, HTML).
* Supporting multiple persistence backends (e.g., MySQL, MongoDB).
* Sending notifications via multiple channels (e.g., Email, SMS, Push).

---
## Example

### Scenario:

You’re building a **Message** system that should support multiple **delivery mechanisms** (e.g., Email, SMS).

#### Step 1: The `Implementation Interface`

```ts
// Platform-specific or variant-specific logic
interface MessageSender {
  sendMessage(message: string): void;
}
```

#### Step 2: `Concrete Implementations`

```ts
class EmailSender implements MessageSender {
  sendMessage(message: string): void {
    console.log(`Sending EMAIL: ${message}`);
  }
}

class SmsSender implements MessageSender {
  sendMessage(message: string): void {
    console.log(`Sending SMS: ${message}`);
  }
}
```

#### Step 3: The `Abstraction`

```ts
// High-level logic that delegates to a MessageSender
class Message {
  protected sender: MessageSender;

  constructor(sender: MessageSender) {
    this.sender = sender;
  }

  send(content: string): void {
    this.sender.sendMessage(content);
  }
}
```

#### Step 4: The `Refined Abstraction`

```ts
class UrgentMessage extends Message {
  send(content: string): void {
    console.log("URGENT:");
    this.sender.sendMessage(`🚨 ${content}`);
  }
}
```

#### Step 5: `Client` code

```ts
const email = new EmailSender();
const sms = new SmsSender();

const normalMessage = new Message(email);
normalMessage.send("Meeting at 10 AM"); // Sending EMAIL: Meeting at 10 AM

const urgent = new UrgentMessage(sms);
urgent.send("Server is down!"); // URGENT: Sending SMS: 🚨 Server is down!
```

---

#design-patterns
