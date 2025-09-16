# PHP: Database Interaction with PDO

PHP Data Objects (PDO) is a database access layer that provides a lightweight, consistent interface for accessing databases in PHP. It supports various databases like MySQL, PostgreSQL, SQLite, SQL Server, Oracle, etc. PDO is the recommended way to interact with databases in modern PHP applications due to its security features (like prepared statements) and flexibility.

## Why PDO?

*   **Security**: Prepared statements help prevent SQL injection attacks.
*   **Consistency**: Provides a uniform API for different database systems.
*   **Performance**: Can be faster than traditional database extensions for some operations.
*   **Error Handling**: Offers robust error handling mechanisms.

## 1. Connecting to a Database

To connect to a database using PDO, you create a new `PDO` object. The constructor takes a DSN (Data Source Name) string, username, and password.

```php
<?php
$host = 'localhost';
$db   = 'my_database';
$user = 'my_user';
$pass = 'my_password';
$charset = 'utf8mb4';

$dsn = "mysql:host=$host;dbname=$db;charset=$charset";
$options = [
    PDO::ATTR_ERRMODE            => PDO::ERRMODE_EXCEPTION, // Throw exceptions on errors
    PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,       // Fetch results as associative arrays
    PDO::ATTR_EMULATE_PREPARES   => false,                  // Disable emulation for better security and performance
];

try {
    $pdo = new PDO($dsn, $user, $pass, $options);
    echo "Connected to database successfully!\n";
} catch (PDOException $e) {
    // Handle connection errors
    throw new PDOException($e->getMessage(), (int)$e->getCode());
}
?>
```

## 2. Executing SQL Queries

PDO provides methods for executing SQL queries:

*   `query()`: For simple, non-parameterized queries (e.g., `SELECT` statements without user input).
*   `prepare()` and `execute()`: For parameterized queries (recommended for `INSERT`, `UPDATE`, `DELETE`, and `SELECT` with user input) to prevent SQL injection.

### `query()` for Simple SELECTs

```php
<?php
// Assuming $pdo is already connected
try {
    $stmt = $pdo->query('SELECT id, name FROM users');

    while ($row = $stmt->fetch()) {
        echo $row['name'] . "\n";
    }
} catch (PDOException $e) {
    echo "Query failed: " . $e->getMessage();
}
?>
```

### `prepare()` and `execute()` for Prepared Statements (Recommended)

Prepared statements are crucial for security. They separate the SQL query structure from the data, preventing malicious input from altering the query logic.

#### INSERT Data

```php
<?php
// Assuming $pdo is already connected
$name = "Jane Doe";
$email = "jane.doe@example.com";

try {
    $stmt = $pdo->prepare("INSERT INTO users (name, email) VALUES (?, ?)");
    $stmt->execute([$name, $email]);
    echo "New record created successfully!\n";

    // Using named placeholders
    $stmt = $pdo->prepare("INSERT INTO users (name, email) VALUES (:name, :email)");
    $stmt->execute([':name' => "John Smith", ':email' => "john.smith@example.com"]);
    echo "Another record created successfully!\n";

} catch (PDOException $e) {
    echo "Insert failed: " . $e->getMessage();
}
?>
```

#### SELECT Data with Parameters

```php
<?php
// Assuming $pdo is already connected
$userId = 1;

try {
    $stmt = $pdo->prepare('SELECT id, name, email FROM users WHERE id = ?');
    $stmt->execute([$userId]);

    $user = $stmt->fetch();

    if ($user) {
        echo "User Name: " . $user['name'] . "\n";
        echo "User Email: " . $user['email'] . "\n";
    } else {
        echo "User not found.\n";
    }

    // Using named placeholders
    $userName = "Jane Doe";
    $stmt = $pdo->prepare('SELECT id, name, email FROM users WHERE name = :name');
    $stmt->execute([':name' => $userName]);
    $users = $stmt->fetchAll(); // Fetch all matching rows

    foreach ($users as $user) {
        echo "Found user: " . $user['name'] . " (ID: " . $user['id'] . ")\n";
    }

} catch (PDOException $e) {
    echo "Select failed: " . $e->getMessage();
}
?>
```

#### UPDATE Data

```php
<?php
// Assuming $pdo is already connected
$newEmail = "updated.email@example.com";
$userId = 1;

try {
    $stmt = $pdo->prepare("UPDATE users SET email = ? WHERE id = ?");
    $stmt->execute([$newEmail, $userId]);
    echo "Record updated successfully! Rows affected: " . $stmt->rowCount() . "\n";
} catch (PDOException $e) {
    echo "Update failed: " . $e->getMessage();
}
?>
```

#### DELETE Data

```php
<?php
// Assuming $pdo is already connected
$userIdToDelete = 2;

try {
    $stmt = $pdo->prepare("DELETE FROM users WHERE id = ?");
    $stmt->execute([$userIdToDelete]);
    echo "Record deleted successfully! Rows affected: " . $stmt->rowCount() . "\n";
} catch (PDOException $e) {
    echo "Delete failed: " . $e->getMessage();
}
?>
```

## 3. Fetch Modes

PDO allows you to specify how results are returned using fetch modes. Common modes include:

*   `PDO::FETCH_ASSOC`: Returns an associative array (column name as key).
*   `PDO::FETCH_NUM`: Returns a numeric array (column index as key).
*   `PDO::FETCH_BOTH`: Returns both associative and numeric arrays.
*   `PDO::FETCH_OBJ`: Returns an anonymous object with property names that correspond to the column names.

```php
<?php
// Assuming $pdo is already connected
try {
    $stmt = $pdo->query('SELECT id, name FROM users');

    // Fetch as associative array (default if set in options)
    $rowAssoc = $stmt->fetch(PDO::FETCH_ASSOC);
    print_r($rowAssoc);

    // Fetch as numeric array
    $stmt->execute(); // Re-execute to reset pointer
    $rowNum = $stmt->fetch(PDO::FETCH_NUM);
    print_r($rowNum);

    // Fetch as object
    $stmt->execute(); // Re-execute to reset pointer
    $rowObj = $stmt->fetch(PDO::FETCH_OBJ);
    echo $rowObj->name . "\n";

} catch (PDOException $e) {
    echo "Fetch mode example failed: " . $e->getMessage();
}
?>
```

## 4. Transactions

Transactions allow you to group multiple database operations into a single, atomic unit of work. Either all operations succeed, or none of them do. This is crucial for maintaining data integrity.

```php
<?php
// Assuming $pdo is already connected
try {
    $pdo->beginTransaction();

    // Operation 1: Deduct from account A
    $stmt1 = $pdo->prepare("UPDATE accounts SET balance = balance - ? WHERE id = ?");
    $stmt1->execute([100, 1]);

    // Simulate an error (e.g., insufficient funds, or a network issue)
    // throw new Exception("Simulated error during transfer.");

    // Operation 2: Add to account B
    $stmt2 = $pdo->prepare("UPDATE accounts SET balance = balance + ? WHERE id = ?");
    $stmt2->execute([100, 2]);

    $pdo->commit();
    echo "Transaction committed successfully!\n";

} catch (Exception $e) {
    $pdo->rollBack();
    echo "Transaction failed: " . $e->getMessage() . "\n";
    echo "Transaction rolled back.\n";
}
?>
```

## 5. Error Handling in PDO

As configured in the connection options (`PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION`), PDO will throw `PDOException` objects on errors. This allows you to use standard `try...catch` blocks for database error handling.

PDO is a powerful and flexible way to interact with databases in PHP, providing both security and ease of use for various database systems.
