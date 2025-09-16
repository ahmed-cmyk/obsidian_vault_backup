# PHP: Sessions and Cookies

HTTP is a stateless protocol, meaning it doesn't remember anything about previous requests. To maintain state and track user information across multiple page requests, PHP provides mechanisms like Sessions and Cookies.

## 1. Cookies

Cookies are small pieces of data (text files) that are stored on the client's (user's) computer by their web browser. They are typically used to remember information about the user, such as login status, shopping cart contents, or user preferences.

### Setting a Cookie

Cookies are set using the `setcookie()` function. This function must be called *before* any actual output is sent to the browser.

```php
<?php
// setcookie(name, value, expire, path, domain, secure, httponly);

// Set a cookie named "user" with value "John Doe" that expires in 30 days
setcookie("user", "John Doe", time() + (86400 * 30), "/"); // 86400 = 1 day

// Set a cookie named "last_visit" with current timestamp, expires in 1 hour
setcookie("last_visit", time(), time() + 3600, "/");

echo "Cookies have been set.";
?>
```

**Parameters of `setcookie()`:**

*   `name` (required): The name of the cookie.
*   `value` (optional): The value of the cookie.
*   ``expire` (optional): The time the cookie expires. `time() + seconds`.
*   `path` (optional): The path on the server where the cookie will be available. `/` means the cookie will be available throughout the entire domain.
*   `domain` (optional): The domain that the cookie is available to.
*   `secure` (optional): If `true`, the cookie will only be sent over secure HTTPS connections.
*   `httponly` (optional): If `true`, the cookie will be made accessible only through the HTTP protocol. This helps prevent client-side scripts (like JavaScript) from accessing the cookie, reducing XSS risks.

### Retrieving a Cookie

Cookie values are retrieved using the `$_COOKIE` superglobal array.

```php
<?php
if (isset($_COOKIE["user"])) {
    echo "Welcome back, " . $_COOKIE["user"] . "!";
} else {
    echo "Welcome, Guest!";
}

if (isset($_COOKIE["last_visit"])) {
    echo "<br>Your last visit was on: " . date("Y-m-d H:i:s", $_COOKIE["last_visit"]);
}
?>
```

### Deleting a Cookie

To delete a cookie, you set its expiration time to a past value.

```php
<?php
// Set the expiration date to one hour ago
setcookie("user", "", time() - 3600);
echo "Cookie 'user' is deleted.";
?>
```

## 2. Sessions

Sessions provide a way to store information (in variables) to be used across multiple pages. Unlike cookies, session data is stored on the server, and only a unique session ID (usually stored in a cookie) is sent to the client.

### Starting a Session

To start a session, you must call `session_start()` at the very beginning of your script, before any output.

```php
<?php
session_start();

// Set session variables
$_SESSION["favcolor"] = "green";
$_SESSION["favanimal"] = "cat";
$_SESSION["username"] = "Alice";

echo "Session variables are set.";
?>
```

### Retrieving Session Data

Session data is accessed via the `$_SESSION` superglobal array.

```php
<?php
session_start(); // Always start the session to access session variables

if (isset($_SESSION["username"])) {
    echo "Welcome, " . $_SESSION["username"] . "!<br>";
    echo "Your favorite color is " . $_SESSION["favcolor"] . ".<br>";
    echo "Your favorite animal is " . $_SESSION["favanimal"] . ".<br>";
} else {
    echo "No active session.<br>";
}
?>
```

### Modifying Session Data

Simply reassign the value in the `$_SESSION` array.

```php
<?php
session_start();
$_SESSION["favcolor"] = "yellow"; // Change favorite color
echo "Favorite color is now " . $_SESSION["favcolor"] . ".";
?>
```

### Destroying a Session

To remove all session variables, use `session_unset()`. To destroy the entire session (including the session cookie), use `session_destroy()`.

```php
<?php
session_start();

// Unset all of the session variables
session_unset();

// Destroy the session
session_destroy();

echo "All session variables are removed, and the session is destroyed.";
?>
```

## Cookies vs. Sessions

| Feature         | Cookies                                     | Sessions                                    |
| :-------------- | :------------------------------------------ | :------------------------------------------ |
| **Storage**     | Client-side (browser)                       | Server-side                                 |
| **Data Limit**  | Small (typically 4KB per cookie)            | Limited only by server memory/configuration |
| **Security**    | Less secure (can be manipulated by client)  | More secure (data not directly exposed to client) |
| **Expiration**  | Can be set to expire (persistent) or expire when browser closes (session cookie) | Typically expire when browser closes or after a period of inactivity |
| **Purpose**     | Remembering user preferences, tracking, small data | Managing user login, shopping carts, sensitive data |

## Best Practices

*   **Always start `session_start()`**: Call it at the very beginning of any script that uses sessions.
*   **Sanitize and validate cookie data**: Treat cookie data as untrusted user input.
*   **Use `httponly` for cookies**: Prevents JavaScript access, reducing XSS risk.
*   **Use `secure` for cookies**: Ensures cookies are only sent over HTTPS.
*   **Don't store sensitive data in cookies**: Passwords, credit card numbers, etc., should never be stored in cookies.
*   **Regenerate session ID**: Use `session_regenerate_id(true)` after login to prevent session fixation attacks.

Both sessions and cookies are essential tools for building dynamic web applications that need to maintain state and personalize user experiences.