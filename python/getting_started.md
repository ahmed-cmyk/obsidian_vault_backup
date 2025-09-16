# Getting Started with Python

## Installation

Python is often pre-installed on many operating systems, but it's good practice to use a version manager or download the latest stable release.

- **Check Installation**: Open your terminal or command prompt and type:
  ```bash
  python3 --version
  ```
  This should display the installed Python version.

- **Using `pyenv` (Recommended for macOS/Linux)**:
  ```bash
  curl https://pyenv.run | bash
  ```
  Follow the instructions to set up `pyenv` in your shell. Then you can install specific Python versions:
  ```bash
  pyenv install 3.9.10
  pyenv global 3.9.10
  ```

- **Windows**: Download the official installer from [python.org](https://www.python.org/downloads/). Make sure to check the option "Add Python to PATH" during installation.

## Hello, World!

Let's write your first Python program.

1.  **Create a new file**: Create a file named `hello.py` with the following content:
    ```python
    print("Hello, world!")
    ```
    - `print()`: A built-in function that outputs text to the console.

2.  **Run the program**:
    ```bash
    python3 hello.py
    ```
    This command executes the `hello.py` script.

## Basic Concepts

### Variables and Data Types

Python is dynamically typed, meaning you don't need to declare the type of a variable.

```python
name = "Alice"
age = 30
is_student = True

print(f"Name: {name}, Age: {age}, Student: {is_student}")
```

### Control Flow

Python uses indentation to define code blocks.

```python
score = 85

if score >= 90:
    print("Grade: A")
elif score >= 80:
    print("Grade: B")
else:
    print("Grade: C")

for i in range(5):
    print(i)

count = 0
while count < 3:
    print(f"Count: {count}")
    count += 1
```