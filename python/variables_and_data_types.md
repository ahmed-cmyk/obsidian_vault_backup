# Python Variables and Data Types

## Variables

Variables are used to store data values. Python has no command for declaring a variable. A variable is created the moment you first assign a value to it.

```python
x = 5         # x is of type int
y = "Hello"   # y is of type str
```

## Data Types

Python has various built-in data types.

### Numeric Types

- **`int`**: Integers (whole numbers, positive or negative).
- **`float`**: Floating point numbers (numbers with a decimal point).
- **`complex`**: Complex numbers.

```python
a = 10
b = 3.14
c = 1j
```

### Text Type

- **`str`**: Strings (sequences of characters).

```python
message = "Python is fun!"
```

### Sequence Types

- **`list`**: Ordered, changeable, and allows duplicate members.
- **`tuple`**: Ordered, unchangeable, and allows duplicate members.
- **`range`**: A sequence of numbers.

```python
my_list = [1, 2, 3, "apple"]
my_tuple = (1, "banana", 3)
my_range = range(6)
```

### Mapping Type

- **`dict`**: Unordered, changeable, and does not allow duplicate keys.

```python
my_dict = {"name": "Alice", "age": 30}
```

### Set Types

- **`set`**: Unordered, unchangeable*, and unindexed. No duplicate members.
- **`frozenset`**: Immutable version of a set.

```python
my_set = {"apple", "banana", "cherry"}
```

### Boolean Type

- **`bool`**: Represents one of two values: `True` or `False`.

```python
is_active = True
```