# PHP: Error Handling

Error handling is a crucial part of robust application development. In PHP, errors can range from minor notices to fatal errors that halt script execution. Understanding how to handle these errors is essential for creating stable and user-friendly applications.

## Types of Errors in PHP

PHP errors are generally categorized into:

*   **Notices (E_NOTICE)**: Minor, non-critical errors (e.g., accessing an undefined variable). Script continues.
*   **Warnings (E_WARNING)**: More serious than notices, but script execution continues (e.g., including a non-existent file).
*   **Errors (E_ERROR)**: Fatal errors that cause the script to terminate immediately (e.g., calling an undefined function).
*   **Parse Errors (E_PARSE)**: Syntax errors that prevent the script from running at all.
*   **Deprecated (E_DEPRECATED)**: Indicates that a feature is deprecated and will be removed in future PHP versions.

## PHP Error Reporting Configuration

PHP's behavior regarding errors is controlled by directives in `php.ini` or at runtime using `ini_set()`.

*   `display_errors`: Controls whether errors are displayed to the user. Should be `Off` in production for security.
*   `log_errors`: Controls whether errors are logged to a file. Should be `On` in production.
*   `error_log`: Specifies the file where errors should be logged.
*   `error_reporting`: Sets the level of errors to be reported.

```php
<?php
// In development, you might set these:
ini_set('display_errors', 1);
ini_set('display_startup_errors', 1);
error_reporting(E_ALL); // Report all errors

// In production, you would typically set these:
// ini_set('display_errors', 0);
// ini_set('log_errors', 1);
// ini_set('error_log', '/path/to/php-error.log');
// error_reporting(E_ALL & ~E_NOTICE & ~E_DEPRECATED);
?>
```

## Custom Error Handler

You can define your own function to handle PHP errors using `set_error_handler()`. This allows you to log errors, display custom messages, or perform other actions.

```php
<?php
// Set error reporting to catch all errors
error_reporting(E_ALL);
ini_set('display_errors', 0); // Don't display errors to the user
ini_set('log_errors', 1);    // Log errors
ini_set('error_log', __DIR__ . '/php_error.log'); // Log to a file in the current directory

function customErrorHandler($errno, $errstr, $errfile, $errline) {
    // $errno: error level
    // $errstr: error message
    // $errfile: filename where error occurred
    // $errline: line number where error occurred

    $logMessage = "[Error $errno] $errstr in $errfile on line $errline\n";

    // Log the error
    error_log($logMessage);

    // For critical errors, you might want to stop execution or show a generic message
    if ($errno == E_USER_ERROR || $errno == E_ERROR) {
        echo "<div style=\"color: red;\">A critical error occurred. Please try again later.</div>";
        die(); // Stop script execution
    }

    // For warnings and notices, just log and continue
    return true; // Return true to prevent PHP's default error handler from running
}

// Set the custom error handler
set_error_handler("customErrorHandler");

// Trigger some errors
echo $undefinedVar; // E_NOTICE

include("non_existent_file.php"); // E_WARNING

// Trigger a custom error
trigger_error("This is a custom user error!", E_USER_WARNING);

// This will not be reached if a fatal error occurs
echo "Script finished successfully (or handled errors).";
?>
```

## Handling Fatal Errors (Shutdown Function)

`set_error_handler()` does not catch fatal errors (like `E_ERROR`, `E_PARSE`, `E_COMPILE_ERROR`). To catch these, you can register a shutdown function using `register_shutdown_function()`.

```php
<?php
error_reporting(E_ALL);
ini_set('display_errors', 0);
ini_set('log_errors', 1);
ini_set('error_log', __DIR__ . '/php_fatal_error.log');

function fatalErrorShutdownHandler() {
    $last_error = error_get_last();
    if ($last_error && ($last_error['type'] === E_ERROR || $last_error['type'] === E_PARSE || $last_error['type'] === E_COMPILE_ERROR)) {
        $logMessage = "[FATAL ERROR] " . $last_error['message'] . " in " . $last_error['file'] . " on line " . $last_error['line'] . "\n";
        error_log($logMessage);
        // You might redirect to a generic error page or display a message
        echo "<div style=\"color: red;\">A critical system error occurred. We are working to fix it.</div>";
    }
}

register_shutdown_function("fatalErrorShutdownHandler");

// This will cause a fatal error (calling an undefined function)
// undefined_function_call();

echo "This line will not be reached if fatal error occurs.";
?>
```

## Best Practices for Error Handling

*   **Never display errors on production servers**: This can expose sensitive information and make your application vulnerable. Always log errors instead.
*   **Use a custom error handler**: For consistent error logging and user-friendly error messages.
*   **Distinguish between development and production environments**: Configure error reporting differently for each.
*   **Use `try...catch` for exceptions**: For logical errors and expected exceptional conditions (covered in `exceptions.md`).
*   **Log everything**: Ensure all errors, warnings, and notices are logged for debugging and monitoring.
*   **Monitor error logs**: Regularly check your error logs to identify and fix issues proactively.

Proper error handling makes your PHP applications more robust, secure, and maintainable.
