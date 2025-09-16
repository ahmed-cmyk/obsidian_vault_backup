# Python Error Handling

## `try`, `except`, `else`, `finally`

The `try` block lets you test a block of code for errors.

The `except` block lets you handle the error.

The `else` block lets you execute code when there is no error.

The `finally` block lets you execute code, regardless of the result of the try- and except blocks.

```python
try:
    print(x)
except NameError:
    print("Variable x is not defined")
except:
    print("Something else went wrong")
```

### Handling Specific Exceptions

You can specify which exception to catch.

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero!")
except TypeError:
    print("Type error occurred!")
except Exception as e:
    print(f"An unexpected error occurred: {e}")
```

### `else` Block

Code in the `else` block is executed if the `try` block does NOT raise an error.

```python
try:
    print("Hello")
except:
    print("Something went wrong")
else:
    print("Nothing went wrong")
```

### `finally` Block

The `finally` block will be executed regardless if the `try` block raises an error or not.

```python
try:
    f = open("demofile.txt")
    try:
        f.write("Lorum Ipsum")
    except:
        print("Something went wrong when writing to the file")
    finally:
        f.close()
except:
    print("Something went wrong when opening the file")
```

## Raising Exceptions (`raise`)

You can raise an exception using the `raise` keyword.

```python
x = -1

if x < 0:
    raise Exception("Sorry, no numbers below zero")
```

You can also define the type of error to raise.

```python
x = "hello"

if not type(x) is int:
    raise TypeError("Only integers are allowed")
```