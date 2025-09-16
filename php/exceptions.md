# PHP: Exceptions

Exceptions provide a structured, object-oriented way to handle unexpected errors or exceptional conditions in your code. Unlike traditional PHP errors, exceptions allow you to gracefully manage problems without halting script execution immediately, giving you more control over the error flow.

## What is an Exception?

An exception is an object that encapsulates an error or an unusual event that occurs during the execution of a program. When such an event occurs, an exception is "thrown". The normal flow of the program is interrupted, and PHP looks for a "catch" block to handle the exception.

## `try...catch` Block

Exceptions are handled using `try...catch` blocks:

*   The `try` block contains the code that might throw an exception.
*   The `catch` block catches the exception if one is thrown in the `try` block.

```php
<?php
function divide($numerator, $denominator) {
    if ($denominator === 0) {
        throw new Exception("Division by zero is not allowed.");
    }
    return $numerator / $denominator;
}

// Example 1: Successful execution
try {
    echo divide(10, 2); // Outputs: 5
} catch (Exception $e) {
    echo "Caught exception: " . $e->getMessage();
}

echo "\n";

// Example 2: Exception thrown
try {
    echo divide(10, 0);
} catch (Exception $e) {
    echo "Caught exception: " . $e->getMessage(); // Outputs: Caught exception: Division by zero is not allowed.
}

echo "\nScript continues after exception handling.";
?>
```

## The `finally` Block (PHP 5.5+)

The `finally` block is optional and executes regardless of whether an exception was thrown or caught. It's useful for cleanup code (e.g., closing database connections, file handles).

```php
<?php
function processFile($filename) {
    $fileHandle = null;
    try {
        $fileHandle = fopen($filename, 'r');
        if (!$fileHandle) {
            throw new Exception("Could not open file: " . $filename);
        }
        // Simulate reading file content
        $content = fread($fileHandle, 1024);
        echo "File content: " . $content . "\n";
    } catch (Exception $e) {
        echo "Error: " . $e->getMessage() . "\n";
    } finally {
        if ($fileHandle) {
            fclose($fileHandle);
            echo "File handle closed.\n";
        }
    }
}

// Test with a valid file (create a dummy_file.txt first)
// file_put_contents('dummy_file.txt', 'Hello from dummy file!');
// processFile('dummy_file.txt');

// Test with a non-existent file
processFile('non_existent_file.txt');
?>
```

## Multiple `catch` Blocks

You can have multiple `catch` blocks to handle different types of exceptions. PHP will try to catch the exception with the first matching `catch` block.

```php
<?php
class CustomException extends Exception {}
class AnotherCustomException extends Exception {}

function testException($type) {
    if ($type === 'custom') {
        throw new CustomException("This is a custom exception.");
    } elseif ($type === 'another') {
        throw new AnotherCustomException("This is another custom exception.");
    } else {
        throw new Exception("This is a generic exception.");
    }
}

try {
    testException('custom');
    // testException('another');
    // testException('generic');
} catch (CustomException $e) {
    echo "Caught CustomException: " . $e->getMessage() . "\n";
} catch (AnotherCustomException $e) {
    echo "Caught AnotherCustomException: " . $e->getMessage() . "\n";
} catch (Exception $e) {
    echo "Caught Generic Exception: " . $e->getMessage() . "\n";
}
?>
```

## The `Exception` Class and Its Methods

All exceptions in PHP are instances of the `Exception` class or its subclasses. The `Exception` class provides several useful methods:

*   `getMessage()`: Returns the exception message.
*   `getCode()`: Returns the exception code (an integer).
*   `getFile()`: Returns the name of the file where the exception was thrown.
*   `getLine()`: Returns the line number where the exception was thrown.
*   `getTrace()`: Returns an array of the backtrace.
*   `getTraceAsString()`: Returns the backtrace as a string.

```php
<?php
try {
    throw new Exception("Something went wrong!", 1001);
} catch (Exception $e) {
    echo "Message: " . $e->getMessage() . "\n";
    echo "Code: " . $e->getCode() . "\n";
    echo "File: " . $e->getFile() . "\n";
    echo "Line: " . $e->getLine() . "\n";
    echo "Trace: " . $e->getTraceAsString() . "\n";
}
?>
```

## Custom Exceptions

You can create your own custom exception classes by extending the built-in `Exception` class. This allows you to define specific types of errors for your application.

```php
<?php
class DatabaseConnectionException extends Exception {
    public function __construct($message, $code = 0, Throwable $previous = null) {
        parent::__construct($message, $code, $previous);
    }

    public function __toString() {
        return __CLASS__ . ": [{$this->code}]: {$this->message} in {$this->file} on line {$this->line}\n";
    }

    public function customErrorMethod() {
        return "A database connection error occurred.";
    }
}

function connectToDatabase() {
    // Simulate a connection error
    $connected = false;
    if (!$connected) {
        throw new DatabaseConnectionException("Failed to connect to MySQL server.", 500);
    }
    return "Connected to database.";
}

try {
    echo connectToDatabase();
} catch (DatabaseConnectionException $e) {
    echo $e->customErrorMethod() . "\n";
    echo $e; // Uses __toString() method
} catch (Exception $e) {
    echo "Caught generic exception: " . $e->getMessage() . "\n";
}
?>
```

## Error vs. Exception

Historically, PHP had a distinction between errors (which were handled by error handlers) and exceptions (which were caught by `try...catch`). With PHP 7, many fatal errors were converted into `Error` exceptions (which implement the `Throwable` interface). This means you can now catch many more types of critical issues using `try...catch`.

It's generally recommended to use exceptions for handling expected but unusual conditions (e.g., invalid user input, file not found) and to let PHP's error handling system (or a custom error handler) deal with programming errors (e.g., syntax errors, undefined variables) that indicate a bug in the code.
