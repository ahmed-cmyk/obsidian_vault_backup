# Creational: Singleton

**Singleton** is a creational design pattern that ensures a class has only one instance and provides a global access point to that instance.

---
## Structure

### `Singleton Class`

The `Singleton Class` declares a static method that returns the same instance of its own class. The constructor is kept private to prevent direct instantiation from other classes.

The class holds a static reference to its single instance and ensures it's created lazily (on first access) or eagerly (at class loading), depending on the implementation.

---
## Applicability

* Use the Singleton pattern when:
  * There should be exactly one instance of a class, accessible from anywhere in the code.
  * You want stricter control over global variables.
  * You need a centralized, shared resource like a config manager, logger, cache, or connection pool.

---
## How to Implement

1. Declare the constructor of the class as **private** to prevent direct instantiation.
2. Create a **static private field** in the class to store the single instance.
3. Add a **public static method** (often called `getInstance()`) that:
   * Creates the instance if it doesn’t exist yet.
   * Returns the existing instance if it does.
4. (Optional) Handle **multithreading** to avoid creating multiple instances in multi-threaded environments (e.g., using locks or synchronized blocks).
5. (Optional) If needed, make the singleton class **lazy** or **eager**, depending on performance and resource concerns.

---
## Use Cases

The **Singleton Pattern** ensures a class has **only one instance**, and provides a global point of access to that instance. It’s particularly useful when:

### 1. **Database Connection Managers**

You don’t want multiple connections fighting over resources. Instead, you instantiate a single DB connection manager and reuse it:

```
tsCopyEditconst db = Database.getInstance();
db.query("SELECT * FROM users");
```

✅ Prevents connection overload and ensures consistent config.

### 2. **Configuration Settings / Environment Managers**

Apps often need access to shared config values (e.g. API keys, feature flags, environments). Instead of passing them around or creating duplicates:

```
tsCopyEditconst config = Config.getInstance();
console.log(config.apiBaseUrl);
```

✅ Centralizes control of environment settings.

### 3. **Logging Service**

You typically want a single logging service writing to a file or console in a consistent format:

```
tsCopyEditLogger.getInstance().log("New user created");
```

✅ Keeps logs centralized, avoids duplicate handlers or conflicts.

### 4. **Caching or In-Memory Stores**

Used to cache expensive computations or frequently accessed data:

```
tsCopyEditconst cache = Cache.getInstance();
cache.set("user_1", userObj);
```

✅ Ensures the cache state is shared across components.

### 5. **UI Managers (Modals, Notifications, etc.)**

For UI elements like modals or toasts, you usually want a single controller to manage their display state:

```
tsCopyEditNotificationManager.getInstance().show("Item added to cart");
```

✅ Prevents stacking multiple instances or conflicting behaviors.

---
## TypeScript Implementation

```ts
tsCopyEditclass Singleton {
  private static instance: Singleton;

  // Private constructor prevents direct instantiation
  private constructor() {
    console.log("Initializing Singleton...");
  }

  // Global access point
  public static getInstance(): Singleton {
    if (!Singleton.instance) {
      Singleton.instance = new Singleton();
    }
    return Singleton.instance;
  }

  public sayHello() {
    console.log("Hello from the Singleton!");
  }
}

// Usage
const a = Singleton.getInstance();
const b = Singleton.getInstance();

console.log(a === b); // true
```

✅ Ensures only one instance ever exists — even across multiple calls.

---
## Pros and Cons

| Pros | Cons |
|---|---|
| Ensures a single instance throughout the application. | Often considered a **global state**, which can hide dependencies. | 
| Provides a controlled access point to the instance. | Makes unit testing harder due to tight coupling. |
| Can be implemented lazily to save resources. | May introduce hidden dependencies across code. |
| Useful for shared resources (e.g., loggers, configs). | Can be violated in multi-threaded environments if not implemented carefully. |

---

#design-patterns