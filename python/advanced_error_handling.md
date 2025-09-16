# Python Advanced Error Handling

Building upon basic `try-except` blocks, Python offers more sophisticated mechanisms for managing errors and exceptions, including custom exceptions, the `finally` block, and exception chaining.

## 1. The `finally` Block

The `finally` block is executed regardless of whether an exception occurred in the `try` block or not. It's typically used for cleanup operations that *must* happen, such as closing files, releasing locks, or closing network connections.

```python
try:
    file = open("my_file.txt", "r")
    content = file.read()
    print(content)
except FileNotFoundError:
    print("Error: The file was not found.")
except Exception as e:
    print(f"An unexpected error occurred: {e}")
finally:
    # This block will always execute
    if 'file' in locals() and not file.closed:
        file.close()
        print("File closed in finally block.")
    else:
        print("File was not opened or already closed.")

print("Program continues after try-except-finally.")
```

**Note:** While `finally` is useful, the `with` statement (context managers) is generally preferred for resource management as it's cleaner and less error-prone.

## 2. Custom Exceptions

Python allows you to define your own custom exception classes. This is useful for creating more specific and meaningful error types that reflect the domain logic of your application. Custom exceptions should typically inherit from the built-in `Exception` class or one of its subclasses.

```python
class InvalidInputError(Exception):
    """Custom exception for invalid input values."""
    def __init__(self, message="Invalid input provided.", value=None):
        self.message = message
        self.value = value
        super().__init__(self.message)

class NetworkError(Exception):
    """Custom exception for network-related issues."""
    pass # Can be empty if no custom attributes are needed

def process_data(data):
    if not isinstance(data, (int, float)):
        raise InvalidInputError("Input must be a number.", value=data)
    if data < 0:
        raise InvalidInputError("Input cannot be negative.", value=data)
    print(f"Processing data: {data}")

def fetch_from_api(url):
    # Simulate a network error
    if "fail" in url:
        raise NetworkError(f"Failed to connect to {url}")
    return f"Data from {url}"

# Usage of custom exceptions
try:
    process_data("hello")
except InvalidInputError as e:
    print(f"Caught custom error: {e.message} (Value: {e.value})")

try:
    process_data(-5)
except InvalidInputError as e:
    print(f"Caught custom error: {e.message} (Value: {e.value})")

try:
    fetch_from_api("http://api.example.com/fail")
except NetworkError as e:
    print(f"Caught network error: {e}")
```

## 3. Exception Chaining (`raise ... from ...`)

When an exception occurs inside an `except` or `finally` block, or when you catch an exception and then raise a new one, Python automatically chains the new exception to the original one. This is useful for debugging, as it preserves the context of the original error.

You can explicitly chain exceptions using the `raise ... from ...` syntax.

```python
class DatabaseError(Exception):
    pass

class DataProcessingError(Exception):
    pass

def read_from_db():
    try:
        # Simulate a database connection error
        raise ConnectionError("Could not connect to database.")
    except ConnectionError as e:
        # Chain the new exception to the original one
        raise DatabaseError("Failed to read data from database.") from e

def process_report():
    try:
        data = read_from_db()
        # Further processing...
    except DatabaseError as e:
        # Catch the chained exception and raise another, preserving the chain
        raise DataProcessingError("Report generation failed due to database issue.") from e

try:
    process_report()
except DataProcessingError as e:
    print(f"Caught: {e}")
    # Access the original exception that caused this one
    if e.__cause__:
        print(f"Caused by: {e.__cause__}")
        if e.__cause__.__cause__:
            print(f"Further caused by: {e.__cause__.__cause__}")

# Output will show a traceback with "The above exception was the direct cause of the following exception:"
# and "During handling of the above exception, another exception occurred:"
```

**Benefits of Exception Chaining:**
-   **Clearer Debugging:** Provides a full history of how an exception propagated, making it easier to pinpoint the root cause.
-   **Context Preservation:** Allows you to raise a more specific or higher-level exception while still retaining information about the underlying problem.

## 4. `else` Block in `try-except`

The `else` block in a `try-except` statement is executed only if no exception occurs in the `try` block. It's a good place for code that should run only when the `try` block succeeds.

```python
try:
    result = 10 / 2
except ZeroDivisionError:
    print("Cannot divide by zero!")
else:
    print(f"Division successful. Result: {result}")
finally:
    print("Execution of try-except-else-finally block finished.")

print("\n---")

try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero!")
else:
    print(f"Division successful. Result: {result}") # This will not execute
finally:
    print("Execution of try-except-else-finally block finished.")
```

## 5. Assertions (`assert` statement)

Assertions are used to check if a condition is true. If the condition is false, an `AssertionError` is raised. Assertions are primarily used for debugging and internal consistency checks, not for handling expected errors from user input or external systems.

```python
def divide(a, b):
    assert b != 0, "Cannot divide by zero! Denominator is 0."
    return a / b

try:
    print(divide(10, 2)) # Output: 5.0
    print(divide(10, 0)) # Raises AssertionError
except AssertionError as e:
    print(f"Assertion failed: {e}")
```

**When to use `assert` vs. `raise`:**
-   Use `assert` for conditions that *should never happen* in a correct program. They are often removed in optimized Python code.
-   Use `raise` for expected error conditions that your program should handle gracefully (e.g., invalid user input, file not found, network issues).

## 6. Logging Exceptions

Instead of just printing error messages, it's often better to use Python's `logging` module to record exceptions. This provides more control over where and how error messages are stored (e.g., to a file, console, or remote service).

```python
import logging

logging.basicConfig(level=logging.ERROR, format='%(asctime)s - %(levelname)s - %(message)s')

def risky_operation():
    try:
        value = int("abc")
    except ValueError as e:
        logging.error("Failed to convert value to integer.", exc_info=True) # exc_info=True adds traceback

risky_operation()
```