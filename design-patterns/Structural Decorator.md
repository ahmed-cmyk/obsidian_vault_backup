# Structural: Decorator

**Decorator** is a structural design pattern that allows behavior to be added to individual objects, dynamically, without affecting the behavior of other objects from the same class.

---
## Structure

1. **Component Interface**: Defines the common interface for both concrete components and decorators.
2. **Concrete Component**: The core object to which new behavior will be added.
3. **Base Decorator**: Implements the component interface and stores a reference to a wrapped component.
4. **Concrete Decorators**: Extend the base decorator to add or override behavior before or after delegating to the wrapped component.
5. **Client**: Works only with the component interface, unaware of how many layers of decoration are applied.

⠀
> 🎯 The decorator pattern promotes **composition over inheritance** for extending behavior.

---
## Applicability

Use the Decorator pattern when:

* You want to add responsibilities to individual objects dynamically, without affecting others.
* Extending functionality via inheritance is impractical or leads to class explosion.
* You need a flexible alternative to subclassing for extending behavior.

---
## How to Implement

1. Define a **component interface** that declares the common operations.
2. Create a **concrete component** that implements the interface.
3. Create an **abstract base decorator** that also implements the interface and has a reference to a component.
4. Create **concrete decorators** that extend the base decorator and add extra behavior.
5. Wrap the component in one or more decorators to combine behaviors.

⠀
---
## Pros and Cons

| Pros | Cons |
|---|---|
| Adds behavior without modifying existing code (Open/Closed Principle). | Can result in many small classes with similar logic. |
| Allows dynamic composition of behavior at runtime. | Wrapping multiple decorators can be harder to debug. |
| More flexible than subclassing for extending behavior. | Order of decorators can affect output, leading to subtle bugs. |

---
## Example Use Cases

* Adding scrollbars or borders to a window in a GUI framework.
* Extending logging, buffering, or encryption to data streams.
* Wrapping request/response objects in middleware (e.g., Express.js).
* Formatting text with multiple styles (bold, italic, underline).

---
## Example

### Scenario:

You’re designing a notification system that can be extended with SMS and Slack messages in addition to basic email.

#### Step 1: The `Component Interface`

```ts
tsCopyEditinterface Notifier {
  send(message: string): void;
}
```

#### Step 2: The `Concrete Component`

```ts
tsCopyEditclass EmailNotifier implements Notifier {
  send(message: string): void {
    console.log(`Sending Email: ${message}`);
  }
}
```

#### Step 3: The `Base Decorator`

```ts
tsCopyEditclass NotifierDecorator implements Notifier {
  protected wrappee: Notifier;

  constructor(notifier: Notifier) {
    this.wrappee = notifier;
  }

  send(message: string): void {
    this.wrappee.send(message);
  }
}
```

#### Step 4: `Concrete Decorators`

```ts
tsCopyEditclass SMSDecorator extends NotifierDecorator {
  send(message: string): void {
    super.send(message);
    console.log(`Sending SMS: ${message}`);
  }
}

class SlackDecorator extends NotifierDecorator {
  send(message: string): void {
    super.send(message);
    console.log(`Sending Slack: ${message}`);
  }
}
```

#### Step 5: `Client` Code

```ts
tsCopyEditconst email = new EmailNotifier();

const emailWithSMS = new SMSDecorator(email);
const emailWithSMSAndSlack = new SlackDecorator(emailWithSMS);

emailWithSMSAndSlack.send("System Alert!");
```

#### Output:

```
sqlCopyEditSending Email: System Alert!
Sending SMS: System Alert!
Sending Slack: System Alert!
```

---

#design-patterns