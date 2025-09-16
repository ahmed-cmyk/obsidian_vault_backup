# Python File I/O

File Input/Output (I/O) operations allow Python programs to interact with files on the computer's filesystem. This includes reading data from files and writing data to files.

## Opening Files: `open()` function

The `open()` function is used to open a file. It returns a file object, which has methods for reading, writing, and manipulating the file.

```python
file_object = open(file, mode, encoding)
```

-   `file`: The path to the file (can be relative or absolute).
-   `mode`: A string indicating the mode in which the file is opened. Common modes include:
    -   `'r'` (read): Default mode. Opens for reading. Raises `FileNotFoundError` if the file doesn't exist.
    -   `'w'` (write): Opens for writing. Creates a new file if it doesn't exist or truncates (empties) the file if it does exist.
    -   `'a'` (append): Opens for appending. Creates a new file if it doesn't exist. Data is written to the end of the file.
    -   `'x'` (exclusive creation): Opens for exclusive creation. Creates a new file, but raises `FileExistsError` if the file already exists.
    -   `'b'` (binary): Used with other modes (e.g., `'rb'`, `'wb'`) for binary files (images, executables).
    -   `'t'` (text): Default mode. Used with other modes (e.g., `'rt'`, `'wt'`) for text files.
    -   `'+'` (update): Opens a file for updating (reading and writing). Can be combined with other modes (e.g., `'r+'`, `'w+'`, `'a+'`).
-   `encoding`: (Optional) The encoding of the file (e.g., `'utf-8'`, `'latin-1'`). Important for text files.

## Closing Files: `close()` method

It's crucial to close a file after you're done with it to free up system resources and ensure all changes are saved. The `close()` method is used for this.

```python
file_object.close()
```

## Best Practice: `with` statement (Context Manager)

The `with` statement is the recommended way to handle file operations because it ensures that the file is automatically closed, even if errors occur.

```python
with open('example.txt', 'r') as file:
    # file operations here
# File is automatically closed outside the 'with' block
```

## Reading from Files

### `read()`
Reads the entire content of the file as a single string.

```python
with open('my_file.txt', 'r') as file:
    content = file.read()
    print(content)
```

### `readline()`
Reads a single line from the file. Returns an empty string when the end of the file is reached.

```python
with open('my_file.txt', 'r') as file:
    line1 = file.readline()
    line2 = file.readline()
    print(line1)
    print(line2)
```

### `readlines()`
Reads all lines from the file and returns them as a list of strings, where each string is a line.

```python
with open('my_file.txt', 'r') as file:
    lines = file.readlines()
    for line in lines:
        print(line.strip()) # .strip() removes newline characters
```

### Iterating through a file object
This is the most memory-efficient way to read large files line by line.

```python
with open('my_file.txt', 'r') as file:
    for line in file:
        print(line.strip())
```

## Writing to Files

### `write()`
Writes a string to the file. It does not add a newline character automatically.

```python
with open('output.txt', 'w') as file:
    file.write("Hello, Python!\n")
    file.write("This is a new line.\n")
```

### `writelines()`
Writes a list of strings to the file. Each string in the list is written as is, so you need to include newline characters (`\n`) if you want them on separate lines.

```python
lines_to_write = [
    "First line.\n",
    "Second line.\n",
    "Third line.\n"
]

with open('output_list.txt', 'w') as file:
    file.writelines(lines_to_write)
```

## Example: Appending to a file

```python
with open('log.txt', 'a') as file:
    file.write("New log entry at " + str(datetime.now()) + "\n")
```

## Example: Reading and Writing (r+ mode)

```python
# Create a file first
with open('update_me.txt', 'w') as f:
    f.write("Line 1\nLine 2\nLine 3\n")

# Open in r+ mode (read and write, cursor at beginning)
with open('update_me.txt', 'r+') as file:
    content = file.read() # Reads all content
    print("Original content:\n", content)

    file.seek(0) # Move cursor to the beginning of the file
    file.write("UPDATED Line 1\n") # Overwrites the beginning
    file.truncate() # Truncate the rest of the file from current position

# Verify content
with open('update_me.txt', 'r') as file:
    print("\nUpdated content:\n", file.read())
```

## File Pointers: `seek()` and `tell()`

-   `tell()`: Returns the current position of the file pointer (in bytes).
-   `seek(offset, whence)`: Changes the position of the file pointer.
    -   `offset`: Number of bytes to move.
    -   `whence`: (Optional) Reference point:
        -   `0` (default): Beginning of the file.
        -   `1`: Current position.
        -   `2`: End of the file.

```python
with open('sample.txt', 'w+') as file:
    file.write("Hello World")
    print("Current position:", file.tell()) # Output: 11
    file.seek(0) # Move to the beginning
    print("Current position after seek(0):", file.tell()) # Output: 0
    content = file.read()
    print("Content after seek(0) and read:", content)
```