# PHP: Getting Started

PHP (Hypertext Preprocessor) is a widely-used open source general-purpose scripting language that is especially suited for web development and can be embedded into HTML.

## Installation

To get started with PHP, you typically need a web server (like Apache or Nginx) and a database (like MySQL). A common way to set this up is using a package like XAMPP, WAMP, or MAMP, which bundle Apache, MySQL, and PHP.

Alternatively, you can install PHP directly:

```bash
# On macOS using Homebrew
brew install php

# On Debian/Ubuntu
sudo apt update
sudo apt install php libapache2-mod-php php-mysql
```

## Your First PHP Script

PHP code is executed on the server. A basic PHP script looks like this:

```php
<!DOCTYPE html>
<html>
<head>
    <title>My First PHP Page</title>
</head>
<body>

    <h1>Hello from PHP!</h1>

    <?php
    // This is a single-line comment
    /*
     * This is a multi-line comment
     */
    echo "Welcome to PHP programming!"; // The echo statement outputs text
    ?>

</body>
</html>
```

Save this file as `index.php` in your web server's document root (e.g., `htdocs` for Apache). When you access it through your browser (e.g., `http://localhost/index.php`), the PHP code will be processed by the server, and the HTML output will be sent to your browser.

## Running PHP from the Command Line

PHP can also be used for command-line scripting:

```php
<?php
// cli_script.php
echo "This script runs from the command line.
";
echo "Current PHP version: " . phpversion() . "
";
?>
```

Run it using:

```bash
php cli_script.php
```
