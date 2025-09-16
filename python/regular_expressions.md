# Python Regular Expressions (re module)

Regular expressions (often shortened to regex or regexp) are powerful tools for pattern matching and manipulation of strings. Python's built-in `re` module provides full support for regular expressions.

## Basic Concepts

-   **Pattern:** A sequence of characters that defines a search pattern.
-   **String:** The text in which you search for the pattern.

## The `re` Module Functions

### 1. `re.match(pattern, string, flags=0)`

Attempts to match the `pattern` only at the *beginning* of the `string`. Returns a match object if successful, `None` otherwise.

```python
import re

result = re.match(r"Python", "Python Programming")
print(result) # <re.Match object; span=(0, 6), match='Python'>
print(result.group(0)) # Python

result = re.match(r"Python", "Learning Python")
print(result) # None
```

### 2. `re.search(pattern, string, flags=0)`

Scans through the `string` looking for the first location where the `pattern` produces a match. Returns a match object if successful, `None` otherwise.

```python
import re

result = re.search(r"Python", "Learning Python Programming")
print(result) # <re.Match object; span=(9, 15), match='Python'>
print(result.group(0)) # Python

result = re.search(r"Java", "Learning Python Programming")
print(result) # None
```

### 3. `re.findall(pattern, string, flags=0)`

Returns a list of all non-overlapping matches of `pattern` in `string`. If groups are present, it returns a list of tuples.

```python
import re

text = "Emails: user1@example.com, user2@domain.org, another@test.net"
emails = re.findall(r"\w+@\w+\.\w+", text)
print(emails) # ['user1@example.com', 'user2@domain.org', 'another@test.net']

numbers = re.findall(r"\d+", "The year is 2023 and the count is 123.")
print(numbers) # ['2023', '123']
```

### 4. `re.finditer(pattern, string, flags=0)`

Returns an iterator yielding match objects for all non-overlapping matches of `pattern` in `string`.

```python
import re

text = "Colors: red, blue, green, yellow"
for match in re.finditer(r"\b\w{3,5}\b", text):
    print(f"Found '{match.group(0)}' at {match.span()}")
# Output:
# Found 'red' at (8, 11)
# Found 'blue' at (13, 17)
# Found 'green' at (19, 24)
```

### 5. `re.sub(pattern, repl, string, count=0, flags=0)`

Replaces occurrences of `pattern` in `string` with `repl`. `repl` can be a string or a function. `count` limits the number of replacements.

```python
import re

text = "The quick brown fox jumps over the lazy dog."
new_text = re.sub(r"fox", "cat", text)
print(new_text) # The quick brown cat jumps over the lazy dog.

# Replace all digits with '#'
text_with_digits = "Phone: 123-456-7890, Zip: 98765"
masked_text = re.sub(r"\d", "#", text_with_digits)
print(masked_text) # Phone: ###-###-####, Zip: #####

# Using a function as repl
def double_number(match):
    return str(int(match.group(0)) * 2)

calc_text = re.sub(r"\d+", double_number, "Numbers: 5, 10, 15")
print(calc_text) # Numbers: 10, 20, 30
```

### 6. `re.split(pattern, string, maxsplit=0, flags=0)`

Splits the `string` by occurrences of `pattern`. `maxsplit` limits the number of splits.

```python
import re

text = "apple,banana;orange grapes"
parts = re.split(r"[,;\s]+", text)
print(parts) # ['apple', 'banana', 'orange', 'grapes']

parts_limited = re.split(r"[,;\s]+", text, maxsplit=1)
print(parts_limited) # ['apple', 'banana;orange grapes']
```

### 7. `re.compile(pattern, flags=0)`

Compiles a regular expression pattern into a regular expression object, which can be used for more efficient pattern matching when the same pattern is used multiple times.

```python
import re

email_pattern = re.compile(r"\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b")

text1 = "Contact us at info@example.com"
text2 = "Support at help@domain.org is available."

match1 = email_pattern.search(text1)
match2 = email_pattern.search(text2)

print(match1.group(0) if match1 else "No match")
print(match2.group(0) if match2 else "No match")
```

## Regular Expression Syntax (Metacharacters and Special Sequences)

| Character(s) | Description                                                              | Example                               | Matches                               |
| :------------- | :----------------------------------------------------------------------- | :------------------------------------ | :------------------------------------ |
| `.`            | Any character (except newline)                                           | `a.b`                                 | `acb`, `a#b`, `a3b`                   |
| `^`            | Start of the string                                                      | `^Hello`                              | `Hello World`                         |
| `$`            | End of the string                                                        | `World$`                              | `Hello World`                         |
| `*`            | Zero or more occurrences of the preceding character/group                | `a*b`                                 | `b`, `ab`, `aaab`                     |
| `+`            | One or more occurrences of the preceding character/group                 | `a+b`                                 | `ab`, `aaab` (not `b`)                |
| `?`            | Zero or one occurrence of the preceding character/group (optional)       | `colou?r`                             | `color`, `colour`                     |
| `{n}`          | Exactly `n` occurrences                                                  | `a{3}`                                | `aaa`                                 |
| `{n,m}`        | Between `n` and `m` occurrences (inclusive)                              | `a{2,4}`                              | `aa`, `aaa`, `aaaa`                   |
| `[]`           | Character set: Matches any one of the characters inside                  | `[abc]`                               | `a`, `b`, or `c`                      |
| `[^]`          | Negated character set: Matches any character NOT inside                  | `[^abc]`                              | Any character except `a`, `b`, or `c` |
| `|`            | OR: Matches either the expression before or after                        | `cat|dog`                             | `cat` or `dog`                        |
| `()`           | Grouping: Groups expressions and captures matches                        | `(ab)+`                               | `ab`, `abab`, `ababab`                |
| `\`            | Escape special characters or introduce special sequences                 | `\.`                                 | A literal dot `.`                     |

### Special Sequences

| Sequence | Description                                                              | Example                               | Matches                               |
| :------- | :----------------------------------------------------------------------- | :------------------------------------ | :------------------------------------ |
| `\d`     | Any digit (0-9)                                                          | `\d+`                                | `123`, `45`                           |
| `\D`     | Any non-digit character                                                  | `\D+`                                | `abc`, `!@#`                          |
| `\w`     | Any word character (alphanumeric + underscore)                           | `\w+`                                | `hello_world`, `num1`                 |
| `\W`     | Any non-word character                                                   | `\W+`                                | `!@#`, ` `                            |
| `\s`     | Any whitespace character (space, tab, newline, etc.)                     | `\s+`                                | ` `, `\t\n`                          |
| `\S`     | Any non-whitespace character                                             | `\S+`                                | `abc`, `123`                          |
| `\b`     | Word boundary (start or end of a word)                                   | `\bcat\b`                             | `cat` in `The cat sat`                |
| `\B`     | Non-word boundary                                                        | `\Bcat\B`                             | `cat` in `category`                   |

## Flags

Flags modify the behavior of the regex engine. They can be passed as the `flags` argument to `re` functions or embedded in the pattern using `(?iLmsux)`.

-   `re.IGNORECASE` or `re.I`: Performs case-insensitive matching.
-   `re.MULTILINE` or `re.M`: Makes `^` and `$` match the start/end of each line, not just the start/end of the string.
-   `re.DOTALL` or `re.S`: Makes `.` match any character, including newline characters.
-   `re.VERBOSE` or `re.X`: Allows you to write more readable regex by ignoring whitespace and allowing comments within the pattern.

```python
import re

text = "Hello\nworld"

# Without DOTALL, . doesn't match newline
match = re.search(r".+", text)
print(match.group(0)) # Hello

# With DOTALL, . matches newline
match_s = re.search(r".+", text, re.S)
print(match_s.group(0)) # Hello\nworld

# Case-insensitive search
match_i = re.search(r"apple", "Apple pie", re.I)
print(match_i.group(0)) # Apple

# Verbose regex
pattern = re.compile(r"""
    ^\d{3}      # Match 3 digits at the start
    -?          # Optional hyphen
    \d{3}      # Match 3 digits
    -?          # Optional hyphen
    \d{4}$      # Match 4 digits at the end
""", re.VERBOSE)

phone_number = "123-456-7890"
match_phone = pattern.match(phone_number)
print(match_phone.group(0)) # 123-456-7890
```

## Groups

Parentheses `()` are used to create capturing groups. You can extract the content of these groups from a match object.

```python
import re

text = "Name: John Doe, Age: 30"
match = re.search(r"Name: (\w+ \w+), Age: (\d+)", text)

if match:
    print(match.group(0)) # Full match: Name: John Doe, Age: 30
    print(match.group(1)) # First group: John Doe
    print(match.group(2)) # Second group: 30
    print(match.groups()) # ('John Doe', '30')

# Named groups using ?P<name>
match_named = re.search(r"Name: (?P<name>\w+ \w+), Age: (?P<age>\d+)", text)
if match_named:
    print(match_named.group("name")) # John Doe
    print(match_named.group("age"))  # 30
    print(match_named.groupdict()) # {'name': 'John Doe', 'age': '30'}
```

## Raw Strings (`r""`)

It is highly recommended to use raw strings (prefixed with `r`) for regular expression patterns. This prevents backslashes from being interpreted as escape sequences by Python, ensuring that they are passed directly to the regex engine.

```python
# Without raw string (needs double backslash)
pattern = "\\d+"
print(re.findall(pattern, "123 abc")) # ['123']

# With raw string (single backslash is enough)
raw_pattern = r"\d+"
print(re.findall(raw_pattern, "123 abc")) # ['123']
```