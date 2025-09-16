# PHP: Superglobals

PHP Superglobals are built-in variables that are always available in all scopes throughout a script. This means you can access them from any function, class, or file without having to do anything special. They are used to store various types of data, such as user input, server information, and session data.

There are nine PHP superglobal variables:

1.  `$GLOBALS`
2.  `$_SERVER`
3.  `$_REQUEST`
4.  `$_POST`
5.  `$_GET`
6.  `$_FILES`
7.  `$_ENV`
8.  `$_COOKIE`
9.  `$_SESSION`

## 1. `$GLOBALS`

`$GLOBALS` is a PHP superglobal variable which is used to access global variables from anywhere in the PHP script (also from within functions or methods).

```php
<?php
$x = 75;
$y = 25;

function addition() {
    $GLOBALS['z'] = $GLOBALS['x'] + $GLOBALS['y'];
}

addition();
echo $GLOBALS['z']; // Outputs: 100
?>
```

## 2. `$_SERVER`

`$_SERVER` is a PHP superglobal variable which holds information about headers, paths, and script locations. It's an array with entries created by the web server.

Some common `$_SERVER` elements:

*   `$_SERVER['PHP_SELF']`: Returns the filename of the currently executing script.
*   `$_SERVER['SERVER_NAME']`: Returns the name of the host server.
*   `$_SERVER['HTTP_HOST']`: Returns the Host header from the current request.
*   `$_SERVER['HTTP_USER_AGENT']`: Returns the User-Agent string from the current request.
*   `$_SERVER['REQUEST_METHOD']`: Returns the request method used to access the page (e.g., `GET`, `POST`).
*   `$_SERVER['REMOTE_ADDR']`: Returns the IP address from which the user is viewing the current page.
*   `$_SERVER['QUERY_STRING']`: Returns the query string if the page is accessed via a query string.

```php
<?php
echo $_SERVER['PHP_SELF'];        // e.g., /index.php
echo "<br>";
echo $_SERVER['SERVER_NAME'];     // e.g., localhost
echo "<br>";
echo $_SERVER['HTTP_HOST'];       // e.g., localhost
echo "<br>";
echo $_SERVER['HTTP_USER_AGENT']; // e.g., Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/...
echo "<br>";
echo $_SERVER['REQUEST_METHOD'];  // e.g., GET or POST
?>
```

## 3. `$_REQUEST`

`$_REQUEST` is a PHP superglobal variable which is used to collect data after submitting an HTML form. It contains the contents of `$_GET`, `$_POST`, and `$_COOKIE`.

```php
<!-- form.html -->
<form method="post" action="process.php">
  Name: <input type="text" name="fname">
  <input type="submit">
</form>

<!-- process.php -->
<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $name = $_REQUEST['fname'];
    if (empty($name)) {
        echo "Name is empty";
    } else {
        echo $name;
    }
}
?>
```

## 4. `$_POST`

`$_POST` is a PHP superglobal variable which is used to collect form data after submitting an HTML form with `method="post"`. `$_POST` is widely used to pass variables.

```php
<!-- form.html -->
<form method="post" action="process.php">
  Name: <input type="text" name="fname">
  <input type="submit">
</form>

<!-- process.php -->
<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $name = $_POST['fname'];
    if (empty($name)) {
        echo "Name is empty";
    } else {
        echo $name;
    }
}
?>
```

## 5. `$_GET`

`$_GET` is a PHP superglobal variable which is used to collect form data after submitting an HTML form with `method="get"`. It can also collect data sent in the URL query string.

```php
<!-- form.html -->
<form method="get" action="process.php">
  Name: <input type="text" name="fname">
  <input type="submit">
</form>

<!-- process.php -->
<?php
if (isset($_GET['fname'])) {
    $name = $_GET['fname'];
    if (empty($name)) {
        echo "Name is empty";
    } else {
        echo $name;
    }
}

// Example with URL: process.php?city=London&country=UK
if (isset($_GET['city'])) {
    echo "<br>City: " . $_GET['city'];
}
?>
```

## 6. `$_FILES`

`$_FILES` is a PHP superglobal variable which holds information about files uploaded to the script via the HTTP POST method.

```php
<!-- upload_form.html -->
<form action="upload.php" method="post" enctype="multipart/form-data">
  Select image to upload:
  <input type="file" name="myFile" id="myFile">
  <input type="submit" value="Upload File" name="submit">
</form>

<!-- upload.php -->
<?php
if (isset($_FILES['myFile'])) {
    $file = $_FILES['myFile'];
    echo "File Name: " . $file['name'] . "<br>";
    echo "File Type: " . $file['type'] . "<br>";
    echo "File Size: " . $file['size'] . " bytes<br>";
    echo "Temp Name: " . $file['tmp_name'] . "<br>";
    echo "Error: " . $file['error'] . "<br>";

    // To move the uploaded file to a permanent location:
    // move_uploaded_file($file['tmp_name'], "uploads/" . $file['name']);
}
?>
```

## 7. `$_ENV`

`$_ENV` is a PHP superglobal variable which contains environment variables passed to the current script. These variables are imported into PHP's global namespace from the environment under which the PHP parser is running.

```php
<?php
echo $_ENV['PATH']; // Outputs the system's PATH environment variable

// Note: $_ENV might not be populated on all server setups. 
// getenv() function is a more reliable way to access environment variables.
echo getenv('HOME');
?>
```

## 8. `$_COOKIE`

`$_COOKIE` is a PHP superglobal variable which is used to retrieve cookie values sent by the client.

```php
<?php
// Setting a cookie (must be called before any HTML output)
setcookie("user_name", "Alice", time() + (86400 * 30), "/"); // 86400 = 1 day

// Retrieving a cookie
if (isset($_COOKIE['user_name'])) {
    echo "Welcome back, " . $_COOKIE['user_name'] . "!";
} else {
    echo "Welcome, Guest!";
}
?>
```

## 9. `$_SESSION`

`$_SESSION` is a PHP superglobal variable which is used to store session variables. Session variables store user information to be used across multiple pages (e.g., user login status).

```php
<?php
session_start(); // Must be called at the beginning of the script

// Set session variables
$_SESSION["favcolor"] = "green";
$_SESSION["favanimal"] = "cat";

echo "Session variables are set.<br>";

// Access session variables on another page
// echo "Favorite color is " . $_SESSION["favcolor"] . ".<br>";
// echo "Favorite animal is " . $_SESSION["favanimal"] . ".<br>";

// To destroy a session:
// session_unset();   // unset all session variables
// session_destroy(); // destroy the session
?>
```

Superglobals are essential for interacting with the web server, user input, and maintaining state across requests in PHP applications.