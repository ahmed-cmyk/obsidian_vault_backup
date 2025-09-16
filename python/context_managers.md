# Python Context Managers

Context managers in Python are objects that define a runtime context to be established before and torn down after the execution of a block of code. They are primarily used to manage resources (like files, network connections, or locks) to ensure that they are properly acquired and released, even if errors occur.

The most common way to use context managers is with the `with` statement.

## The `with` Statement

The `with` statement simplifies resource management by ensuring that a resource is properly cleaned up after its use. It works by calling specific methods on the context manager object:

-   `__enter__()`: This method is called when the `with` statement is entered. It should return the resource that will be used within the `with` block (or `None` if no specific resource needs to be returned).
-   `__exit__(exc_type, exc_val, exc_tb)`: This method is called when the `with` block is exited, regardless of whether an exception occurred or not. It's responsible for cleaning up the resource. If an exception occurred within the `with` block, the exception details are passed to these parameters; otherwise, they are `None`.

### Basic Example: File Handling

The `with` statement is most commonly seen with file operations:

```python
# Without 'with' (requires manual closing)
file = open('my_file.txt', 'w')
try:
    file.write("Hello, world!")
finally:
    file.close()

# With 'with' (automatic closing)
with open('my_file.txt', 'w') as file:
    file.write("Hello, world!")
# File is automatically closed here, even if an error occurs inside the 'with' block.
```

In this example, `open()` returns a file object, which is a context manager. When the `with` block is entered, `file.__enter__()` is called, and the returned file object is assigned to the `file` variable. When the block is exited (either normally or due to an exception), `file.__exit__()` is called to ensure the file is closed.

## Creating Custom Context Managers

You can create your own context managers in two primary ways:

### 1. Using Classes

Define a class that implements the `__enter__` and `__exit__` methods.

```python
class MyContextManager:
    def __init__(self, resource_name):
        self.resource_name = resource_name

    def __enter__(self):
        print(f"Acquiring resource: {self.resource_name}")
        # Simulate acquiring a resource
        self.resource = f"<{self.resource_name}_resource>"
        return self.resource

    def __exit__(self, exc_type, exc_val, exc_tb):
        if exc_type:
            print(f"An exception of type {exc_type.__name__} occurred: {exc_val}")
        print(f"Releasing resource: {self.resource_name}")
        # Simulate releasing the resource
        self.resource = None
        # Return True if you want to suppress the exception, False otherwise
        return False

# Usage:
with MyContextManager("Database Connection") as db_conn:
    print(f"Using resource: {db_conn}")
    # Simulate an error
    # raise ValueError("Something went wrong in the block!")

print("Outside the with block.")
```

### 2. Using the `contextlib` Module (Generator-based Context Managers)

The `contextlib` module provides utilities for creating context managers, especially the `@contextlib.contextmanager` decorator, which allows you to write a context manager as a simple generator function.

```python
import contextlib

@contextlib.contextmanager
def my_generator_context_manager(resource_name):
    print(f"Acquiring resource: {resource_name}")
    resource = f"<{resource_name}_resource>"
    try:
        yield resource # This is where the 'with' block code executes
    except Exception as e:
        print(f"An exception occurred: {e}")
        # You can handle the exception here or re-raise it
        raise # Re-raise the exception if not handled
    finally:
        print(f"Releasing resource: {resource_name}")

# Usage:
with my_generator_context_manager("Network Socket") as sock:
    print(f"Using resource: {sock}")
    # raise ConnectionError("Network issue!")

print("Outside the generator context manager block.")
```

In a generator-based context manager:
-   Code *before* the `yield` statement is executed by `__enter__()`.
-   The `yield` statement itself returns the value to the `as` variable in the `with` statement.
-   Code *after* the `yield` statement (in the `try...finally` or `try...except` block) is executed by `__exit__()`.

## Why use Context Managers?

-   **Resource Management:** Ensures that resources are properly acquired and released, preventing leaks and improving reliability.
-   **Error Handling:** Guarantees cleanup even if exceptions occur within the `with` block.
-   **Code Readability:** Makes code cleaner and easier to understand by abstracting away setup and teardown logic.
-   **Reduced Boilerplate:** Avoids repetitive `try...finally` blocks for resource management.

## Common Use Cases

-   **File I/O:** `open()`
-   **Locking:** `threading.Lock()` or `multiprocessing.Lock()`
-   **Database Connections:** Ensuring connections are closed.
-   **Network Sockets:** Ensuring sockets are closed.
-   **Temporary Directories:** Creating and cleaning up temporary files/directories.
-   **Changing Current Directory:** Temporarily changing the working directory.

## `contextlib.suppress`

This context manager suppresses specified exceptions.

```python
from contextlib import suppress

with suppress(FileNotFoundError):
    with open('non_existent_file.txt', 'r') as f:
        content = f.read()
        print(content)
print("File not found error was suppressed.")
```

## `contextlib.redirect_stdout` and `redirect_stderr`

These context managers temporarily redirect `sys.stdout` or `sys.stderr` to another file-like object.

```python
import io
from contextlib import redirect_stdout

f = io.StringIO()
with redirect_stdout(f):
    print("Hello from stdout")
    print("Another line")

s = f.getvalue()
print("Captured output:\n", s)
```