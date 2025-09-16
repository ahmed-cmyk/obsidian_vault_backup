# PHP: Strings

A string is a sequence of characters, like "Hello world!". PHP has excellent support for manipulating strings.

## Creating Strings

Strings in PHP can be enclosed in single quotes (`'`) or double quotes (`"`).

```php
<?php
$single_quoted = 'This is a single-quoted string.';
$double_quoted = "This is a double-quoted string.";

echo $single_quoted; // Outputs: This is a single-quoted string.
echo $double_quoted; // Outputs: This is a double-quoted string.
?>
```

### Differences between Single and Double Quotes

*   **Double quotes** process escape sequences (like `\n` for newline, `\t` for tab) and parse variables.
*   **Single quotes** treat almost everything literally. Variables and most escape sequences will not be parsed.

```php
<?php
$name = "John";

echo "Hello, $name!\n"; // Outputs: Hello, John! (and a newline)
echo 'Hello, $name!\n'; // Outputs: Hello, $name!\n (literal output)
?>
```

## String Concatenation

To concatenate (join) two or more strings, use the `.` operator.

```php
<?php
$firstName = "John";
$lastName = "Doe";
$fullName = $firstName . " " . $lastName;
echo $fullName; // Outputs: John Doe

$message = "Hello";
$message .= " World!"; // Equivalent to $message = $message . " World!";
echo $message; // Outputs: Hello World!
?>
```

## String Length

To get the length of a string, use the `strlen()` function.

```php
<?php
$text = "Hello, PHP!";
echo strlen($text); // Outputs: 11
?>
```

## Word Count

To count the number of words in a string, use the `str_word_count()` function.

```php
<?php
$text = "Hello world, how are you?";
echo str_word_count($text); // Outputs: 5
?>
```

## Searching for Text within a String

To search for a specific text within a string, use the `strpos()` function. It returns the character position of the first match. If no match is found, it returns `false`.

```php
<?php
$text = "Hello world!";
echo strpos($text, "world"); // Outputs: 6 (position of 'w')

if (strpos($text, "PHP") === false) {
    echo "'PHP' not found in the string.";
}
?>
```

## Replacing Text within a String

To replace some characters with some other characters in a string, use the `str_replace()` function.

```php
<?php
$text = "Hello world!";
echo str_replace("world", "PHP", $text); // Outputs: Hello PHP!
?>
```

## Substrings

To extract a part of a string, use the `substr()` function.

```php
<?php
$text = "Hello world!";
echo substr($text, 6);    // Outputs: world! (from index 6 to end)
echo substr($text, 0, 5); // Outputs: Hello (from index 0, 5 characters long)
echo substr($text, -6);   // Outputs: world! (last 6 characters)
?>
```

## Case Conversion

*   `strtolower()`: Converts a string to lowercase.
*   `strtoupper()`: Converts a string to uppercase.
*   `ucfirst()`: Converts the first character of a string to uppercase.
*   `ucwords()`: Converts the first character of each word in a string to uppercase.

```php
<?php
$text = "Hello World";
echo strtolower($text); // Outputs: hello world
echo strtoupper($text); // Outputs: HELLO WORLD
echo ucfirst("hello world"); // Outputs: Hello world
echo ucwords("hello world"); // Outputs: Hello World
?>
```

## Trimming Whitespace

*   `trim()`: Removes whitespace or other predefined characters from both sides of a string.
*   `ltrim()`: Removes whitespace or other predefined characters from the left side of a string.
*   `rtrim()`: Removes whitespace or other predefined characters from the right side of a string.

```php
<?php
$text = "   Hello World   ";
echo trim($text); // Outputs: Hello World
?>
```

## Other Useful String Functions

*   `explode()`: Breaks a string into an array.
*   `implode()`: Joins array elements with a string.
*   `htmlentities()` / `htmlspecialchars()`: Converts characters to HTML entities.
*   `strip_tags()`: Strips HTML and PHP tags from a string.
*   `addslashes()` / `stripslashes()`: Adds/removes backslashes.

```php
<?php
$csv = "apple,banana,cherry";
$fruits_array = explode(",", $csv);
print_r($fruits_array); // Outputs: Array ( [0] => apple [1] => banana [2] => cherry )

$words = ["Hello", "World"];
$sentence = implode(" ", $words);
echo $sentence; // Outputs: Hello World
?>
```

```