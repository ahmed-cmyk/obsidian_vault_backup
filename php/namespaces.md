# PHP: Namespaces

Namespaces are a way of encapsulating items. They provide a way to group related classes, interfaces, functions, and constants, preventing name collisions between different libraries or parts of an application. Think of them like directories in a file system, where files with the same name can exist in different directories without conflict.

## Why Use Namespaces?

*   **Name Collision Avoidance**: Prevents conflicts when using multiple libraries or frameworks that might define classes or functions with the same name.
*   **Code Organization**: Helps in organizing code into logical groups, making it easier to navigate and understand large codebases.
*   **Readability**: Improves code readability by providing clear context for classes and functions.

## Declaring Namespaces

A namespace is declared at the top of a PHP file using the `namespace` keyword.

```php
<?php
// File: App/Models/User.php
namespace App\Models;

class User {
    public function getName() {
        return "John Doe from App\\Models";
    }
}
?>
```

```php
<?php
// File: Library/Auth/User.php
namespace Library\Auth;

class User {
    public function authenticate() {
        return "User authenticated by Library\\Auth";
    }
}
?>
```

## Using Namespaced Elements

To use elements from a namespace, you have a few options:

### 1. Fully Qualified Name

You can refer to a class, function, or constant using its full namespace path.

```php
<?php
// index.php

// Using fully qualified names
$appUser = new \App\Models\User();
echo $appUser->getName(); // Outputs: John Doe from App\Models

$authLibUser = new \Library\Auth\User();
echo $authLibUser->authenticate(); // Outputs: User authenticated by Library\Auth
?>
```

### 2. `use` Keyword (Importing)

The `use` keyword allows you to import a namespace or a specific class, function, or constant into the current file, making it easier to refer to them without their full qualified name.

```php
<?php
// index.php

use App\Models\User;
use Library\Auth\User as AuthUser; // Alias for Library\Auth\User

$appUser = new User();
echo $appUser->getName(); // Outputs: John Doe from App\Models

$authLibUser = new AuthUser();
echo $authLibUser->authenticate(); // Outputs: User authenticated by Library\Auth

// You can also import functions and constants (PHP 5.6+)
use function MyNamespace\myFunction;
use const MyNamespace\MY_CONSTANT;
?>
```

### 3. Relative Namespaces

If you are already within a namespace, you can refer to other elements within the same namespace or sub-namespaces relatively.

```php
<?php
// File: App/Services/UserService.php
namespace App\Services;

use App\Models\User; // This is an absolute import from the root namespace

class UserService {
    public function getUserData() {
        $user = new User(); // Refers to App\Models\User
        return $user->getName();
    }
}

// In another file, using UserService
// use App\Services\UserService;
// $service = new UserService();
// echo $service->getUserData();
?>
```

## Global Space

Code without a namespace declaration is in the global space. To refer to global classes, functions, or constants from within a namespace, you must prefix them with a backslash (`\`).

```php
<?php
namespace MyProject;

// Accessing a global function
echo \strlen("hello"); // Outputs: 5

// Accessing a global class (e.g., DateTime)
$date = new \DateTime();
echo $date->format('Y-m-d');
?>
```

## Multiple Namespaces in a Single File (Discouraged)

While technically possible, it's generally discouraged to define multiple namespaces in a single file for readability and maintainability.

```php
<?php
namespace MyNamespace1 {
    class MyClass {}
}

namespace MyNamespace2 {
    class MyClass {}
}
?>
```

## Autoloading and Namespaces

Namespaces work hand-in-hand with autoloading (e.g., PSR-4 standard) to automatically load class files when they are needed, without requiring explicit `require` or `include` statements. This is typically managed by tools like Composer.

For example, if you have a class `App\Models\User`, an autoloader configured for PSR-4 would expect to find this class in a file named `User.php` within the `App/Models/` directory (relative to a defined base directory).

Namespaces are essential for modern PHP development, especially in larger applications and when working with frameworks and libraries.