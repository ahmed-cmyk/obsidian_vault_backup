# Error Handling

Robust error handling is essential for building reliable JavaScript applications. It allows your program to gracefully manage unexpected situations and prevent crashes.

---
## `try...catch...finally` pattern

The `try...catch...finally` statement provides a structured way to handle potential errors (exceptions) that might occur during the execution of a block of code.

### `try` block

This block contains the code that you want to monitor for errors. If an error occurs anywhere within the `try` block, execution immediately stops, and control is passed to the `catch` block.

```js
try {
    // Code that might throw an error
    console.log("Starting try block...");
    nonExistentFunction(); // This will throw a ReferenceError
    console.log("This line will not be reached.");
}
```

### `catch` block

This block is executed if an error occurs in the `try` block. It receives the error object as an argument, which contains information about the error (e.g., `name`, `message`, `stack`). The `catch` block allows you to handle the error gracefully, log it, display a user-friendly message, or recover from the error.

```js
try {
    console.log("Starting try block...");
    nonExistentFunction();
} catch (error) { // 'error' is the error object
    console.error("An error occurred:", error.name);
    console.error("Error message:", error.message);
    // You might log the error to a server, show a message to the user, etc.
}
```

### `finally` block

This optional block is executed *after* the `try` and `catch` blocks, regardless of whether an error occurred or not. It's guaranteed to run, making it ideal for cleanup operations, such as closing file connections, releasing resources, or hiding loading indicators.

```js
try {
    console.log("In try block.");
    // throw new Error("Simulated error");
} catch (error) {
    console.error("In catch block:", error.message);
} finally {
    console.log("In finally block (always executes).");
}
// Output if no error:
// In try block.
// In finally block (always executes).

// Output if error:
// In try block.
// In catch block: Simulated error
// In finally block (always executes).
```

---
## Throwing & catching custom errors

JavaScript allows you to throw your own custom errors using the `throw` statement. This is useful for signaling specific error conditions that your application logic encounters, making your code more expressive and easier to debug. You can throw any value, but it's best practice to throw an `Error` object or an instance of a more specific error type (like `TypeError`, `RangeError`, or a custom error class).

### Throwing Custom Errors

You can create instances of built-in Error types or define your own custom error classes that extend the base Error class. Extending Error ensures your custom error objects have the standard name and message properties, and a proper stack trace.

```js
// Throwing a built-in Error type
function validateAge(age) {
    if (age < 0 || age > 120) {
        throw new RangeError("Age must be between 0 and 120.");
    }
    console.log("Age is valid.");
}

// Defining a custom error class
class CustomValidationError extends Error {
    constructor(message, field) {
        super(message); // Call the parent Error constructor
        this.name = "CustomValidationError"; // Set the error name
        this.field = field; // Add custom property
    }
}

function processUserData(data) {
    if (!data || !data.username) {
        throw new CustomValidationError("Username is required.", "username");
    }
    // ... further processing
    console.log("User data processed for:", data.username);
}
```

### Catching Custom Errors

Custom errors are caught using the same try...catch block. Inside the catch block, you can inspect the error object's name property or use instanceof to determine the type of error and handle it accordingly.

```js
try {
    validateAge(150); // This will throw a RangeError
} catch (error) {
    if (error instanceof RangeError) {
        console.error("Range Error caught:", error.message);
    } else {
        console.error("Unknown error caught:", error.message);
    }
}

try {
    processUserData({}); // This will throw a CustomValidationError
} catch (error) {
    if (error instanceof CustomValidationError) {
        console.error(`Custom Validation Error: ${error.message} (Field: ${error.field})`);
    } else if (error instanceof Error) {
        console.error("Generic Error caught:", error.message);
    } else {
        console.error("Non-Error value thrown:", error);
    }
}
```

Throwing and catching custom errors promotes cleaner code by separating error-handling logic from normal program flow, and provides more specific context about what went wrong.

---
#javascript