# PHP: Forms and Input Handling

Handling HTML forms is a core part of web development with PHP. This involves processing user input, validating it, and ensuring security.

## 1. HTML Form Basics

A basic HTML form uses the `<form>` tag with `action` and `method` attributes.

*   `action`: Specifies where to send the form data when the form is submitted. If empty, data is sent to the current page.
*   `method`: Specifies the HTTP method to use (`GET` or `POST`).

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
    <title>User Registration</title>
</head>
<body>

    <h2>Register Here</h2>
    <form action="process_form.php" method="post">
        <label for="name">Name:</label><br>
        <input type="text" id="name" name="username" required><br><br>

        <label for="email">Email:</label><br>
        <input type="email" id="email" name="useremail" required><br><br>

        <label for="password">Password:</label><br>
        <input type="password" id="password" name="userpassword" required><br><br>

        <input type="submit" value="Register">
    </form>

</body>
</html>
```

## 2. Retrieving Form Data in PHP

PHP provides superglobal arrays to access form data:

*   `$_POST`: Used for data sent with `method="post"`.
*   `$_GET`: Used for data sent with `method="get"` (data appears in the URL).
*   `$_REQUEST`: Contains data from both `$_GET`, `$_POST`, and `$_COOKIE` (less specific, generally prefer `$_POST` or `$_GET`).

```php
<?php
// process_form.php

// Check if the form was submitted using POST method
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    // Retrieve data from $_POST superglobal array
    $username = $_POST['username'];
    $useremail = $_POST['useremail'];
    $userpassword = $_POST['userpassword'];

    echo "<h2>Registration Details:</h2>";
    echo "Name: " . htmlspecialchars($username) . "<br>";
    echo "Email: " . htmlspecialchars($useremail) . "<br>";
    echo "Password (hashed in real app): " . htmlspecialchars($userpassword) . "<br>";

} else {
    echo "Form not submitted via POST method.";
}
?>
```

**Important**: Always use `htmlspecialchars()` or `htmlentities()` when displaying user-submitted data to prevent XSS (Cross-Site Scripting) attacks.

## 3. Input Validation

Validation is crucial to ensure that user input is in the correct format and meets application requirements. This helps prevent security vulnerabilities and ensures data integrity.

### Common Validation Checks:

*   **Required fields**: Check if fields are empty.
*   **Data type**: Ensure input is numeric, string, email, etc.
*   **Length**: Check minimum/maximum length.
*   **Format**: Validate against regular expressions (e.g., phone numbers, postal codes).

```php
<?php
// process_form.php with validation

$nameErr = $emailErr = $passwordErr = "";
$username = $useremail = $userpassword = "";

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    // Validate Name
    if (empty($_POST["username"])) {
        $nameErr = "Name is required";
    } else {
        $username = test_input($_POST["username"]);
        // Check if name contains only letters and whitespace
        if (!preg_match("/^[a-zA-Z ]*$/", $username)) {
            $nameErr = "Only letters and white space allowed";
        }
    }

    // Validate Email
    if (empty($_POST["useremail"])) {
        $emailErr = "Email is required";
    } else {
        $useremail = test_input($_POST["useremail"]);
        // Check if email address is well-formed
        if (!filter_var($useremail, FILTER_VALIDATE_EMAIL)) {
            $emailErr = "Invalid email format";
        }
    }

    // Validate Password
    if (empty($_POST["userpassword"])) {
        $passwordErr = "Password is required";
    } else {
        $userpassword = test_input($_POST["userpassword"]);
        // Example: Password must be at least 8 characters long
        if (strlen($userpassword) < 8) {
            $passwordErr = "Password must be at least 8 characters long";
        }
        // In a real application, you would hash the password here
        // $hashedPassword = password_hash($userpassword, PASSWORD_DEFAULT);
    }

    // If no errors, process data (e.g., save to database)
    if (empty($nameErr) && empty($emailErr) && empty($passwordErr)) {
        echo "<h2>Registration Successful!</h2>";
        echo "Name: " . htmlspecialchars($username) . "<br>";
        echo "Email: " . htmlspecialchars($useremail) . "<br>";
        // echo "Hashed Password: " . htmlspecialchars($hashedPassword) . "<br>";
    } else {
        echo "<h2>Registration Failed!</h2>";
        echo "Name Error: " . $nameErr . "<br>";
        echo "Email Error: " . $emailErr . "<br>";
        echo "Password Error: " . $passwordErr . "<br>";
    }
}

function test_input($data) {
    $data = trim($data); // Remove whitespace from beginning and end
    $data = stripslashes($data); // Remove backslashes
    $data = htmlspecialchars($data); // Convert special characters to HTML entities
    return $data;
}
?>
```

## 4. Input Sanitization

Sanitization is the process of cleaning user input to remove any potentially harmful characters or code. This is crucial for security.

*   `trim()`: Removes whitespace from both ends of a string.
*   `stripslashes()`: Removes backslashes added by `addslashes()`.
*   `htmlspecialchars()`: Converts special characters to HTML entities to prevent XSS.
*   `filter_var()`: A powerful function for both validating and sanitizing data.

```php
<?php
$unsafe_input = "  <script>alert('XSS');</script>  ";

// Sanitization using custom function
$safe_input = test_input($unsafe_input);
echo "Sanitized: " . $safe_input . "\n"; // Outputs: Sanitized: &lt;script&gt;alert(&#039;XSS&#039;);&lt;/script&gt;

// Sanitization using filter_var
$email = "test@example.com<script>alert('XSS');</script>";
$sanitized_email = filter_var($email, FILTER_SANITIZE_EMAIL);
echo "Sanitized Email: " . $sanitized_email . "\n"; // Outputs: Sanitized Email: test@example.comalert('XSS');

$url = "http://example.com/path?query=string";
$sanitized_url = filter_var($url, FILTER_SANITIZE_URL);
echo "Sanitized URL: " . $sanitized_url . "\n";

$string = "Hello World! 123 <br>";
$sanitized_string = filter_var($string, FILTER_SANITIZE_STRING); // Deprecated in PHP 8.1
echo "Sanitized String: " . $sanitized_string . "\n";

// For general string sanitization, consider using htmlspecialchars or custom regex
$sanitized_string_alt = htmlspecialchars(strip_tags($string));
echo "Sanitized String (alt): " . $sanitized_string_alt . "\n";
?>
```

## 5. Displaying Validation Errors on the Form

It's good practice to redisplay the form with error messages and pre-fill valid input fields so the user doesn't have to re-enter everything.

```php
<?php
// index.php (combining form and processing)

$nameErr = $emailErr = "";
$username = $useremail = "";

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    if (empty($_POST["username"])) {
        $nameErr = "Name is required";
    } else {
        $username = test_input($_POST["username"]);
    }

    if (empty($_POST["useremail"])) {
        $emailErr = "Email is required";
    } else {
        $useremail = test_input($_POST["useremail"]);
        if (!filter_var($useremail, FILTER_VALIDATE_EMAIL)) {
            $emailErr = "Invalid email format";
        }
    }
}

function test_input($data) {
    $data = trim($data);
    $data = stripslashes($data);
    $data = htmlspecialchars($data);
    return $data;
}
?>

<!DOCTYPE html>
<html>
<head>
    <title>User Registration</title>
    <style>.error {color: #FF0000;}</style>
</head>
<body>

    <h2>Register Here</h2>
    <form action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>" method="post">
        <label for="name">Name:</label><br>
        <input type="text" id="name" name="username" value="<?php echo htmlspecialchars($username);?>"><span class="error">* <?php echo $nameErr;?></span><br><br>

        <label for="email">Email:</label><br>
        <input type="email" id="email" name="useremail" value="<?php echo htmlspecialchars($useremail);?>"><span class="error">* <?php echo $emailErr;?></span><br><br>

        <label for="password">Password:</label><br>
        <input type="password" id="password" name="userpassword" required><span class="error">*</span><br><br>

        <input type="submit" value="Register">
    </form>

</body>
</html>
```
Properly handling forms and validating/sanitizing input is critical for building secure and user-friendly PHP web applications.
