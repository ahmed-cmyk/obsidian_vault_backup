# Python Control Flow

## Conditional Statements (if/elif/else)

Used to execute different blocks of code based on conditions.

```python
age = 20

if age < 18:
    print("You are a minor.")
elif age >= 18 and age < 65:
    print("You are an adult.")
else:
    print("You are a senior citizen.")
```

## Loops

Used to iterate over a sequence (like a list, tuple, dictionary, set, or string) or to execute a block of code repeatedly.

### `for` Loop

Iterates over a sequence.

```python
# Iterating through a list
fruits = ["apple", "banana", "cherry"]
for x in fruits:
    print(x)

# Iterating through a string
for char in "Python":
    print(char)

# Using range()
for i in range(5):
    print(i) # Prints 0, 1, 2, 3, 4

for i in range(2, 6): # From 2 (included) to 6 (excluded)
    print(i) # Prints 2, 3, 4, 5

for i in range(0, 10, 2): # From 0 to 10 (excluded), step 2
    print(i) # Prints 0, 2, 4, 6, 8
```

### `while` Loop

Executes a set of statements as long as a condition is true.

```python
i = 1
while i < 6:
    print(i)
    i += 1
```

### Loop Control Statements

- **`break`**: Stops the loop.
- **`continue`**: Skips the rest of the current iteration and moves to the next.
- **`pass`**: A null operation; nothing happens when it executes. Useful as a placeholder.

```python
# break example
for i in range(10):
    if i == 5:
        break
    print(i)

# continue example
for i in range(10):
    if i % 2 == 0:
        continue
    print(i) # Prints odd numbers

# pass example
def my_function():
    pass # TODO: Implement this function later
```