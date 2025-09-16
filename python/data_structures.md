# Python Data Structures

Python has several built-in data structures that are highly versatile and commonly used.

## 1. Lists

Lists are ordered, mutable (changeable) sequences of items. They are defined by enclosing elements in square brackets `[]`, with elements separated by commas.

### Characteristics:
-   **Ordered:** Elements have a defined order, and that order will not change.
-   **Mutable:** You can change, add, or remove items after the list has been created.
-   **Allows Duplicates:** Lists can contain duplicate items.
-   **Heterogeneous:** Can contain items of different data types.

### Examples:
```python
# Creating a list
my_list = [1, 2, 3, "apple", "banana"]

# Accessing elements
print(my_list[0])  # Output: 1
print(my_list[-1]) # Output: banana (last element)

# Slicing
print(my_list[1:4]) # Output: [2, 3, 'apple']

# Modifying elements
my_list[0] = 10
print(my_list) # Output: [10, 2, 3, 'apple', 'banana']

# Adding elements
my_list.append("orange") # Adds to the end
my_list.insert(1, "grape") # Inserts at a specific index
print(my_list)

# Removing elements
my_list.remove("apple") # Removes the first occurrence of the value
my_list.pop(0) # Removes and returns the element at the specified index (default last)
del my_list[1] # Deletes element at index
print(my_list)
```

## 2. Tuples

Tuples are ordered, immutable (unchangeable) sequences of items. They are defined by enclosing elements in parentheses `()`, with elements separated by commas.

### Characteristics:
-   **Ordered:** Elements have a defined order.
-   **Immutable:** Once created, you cannot change, add, or remove items.
-   **Allows Duplicates:** Tuples can contain duplicate items.
-   **Heterogeneous:** Can contain items of different data types.

### Examples:
```python
# Creating a tuple
my_tuple = (1, 2, "apple", "banana")

# Accessing elements
print(my_tuple[0])  # Output: 1

# Slicing
print(my_tuple[1:3]) # Output: (2, 'apple')

# Attempting to modify (will raise an error)
# my_tuple[0] = 10 # TypeError: 'tuple' object does not support item assignment
```

## 3. Dictionaries

Dictionaries are unordered (in Python versions before 3.7, ordered from 3.7+), mutable collections of key-value pairs. Each key must be unique. They are defined by enclosing elements in curly braces `{}`, with key-value pairs separated by colons `:`.

### Characteristics:
-   **Key-Value Pairs:** Stores data in `key: value` format.
-   **Ordered (Python 3.7+):** Insertion order is preserved.
-   **Mutable:** You can change, add, or remove key-value pairs.
-   **Unique Keys:** Keys must be unique. Values can be duplicated.
-   **Keys are Immutable:** Keys must be of an immutable type (e.g., strings, numbers, tuples).

### Examples:
```python
# Creating a dictionary
my_dict = {
    "name": "Alice",
    "age": 30,
    "city": "New York"
}

# Accessing values
print(my_dict["name"])  # Output: Alice
print(my_dict.get("age")) # Output: 30 (safer way to access, returns None if key not found)

# Modifying values
my_dict["age"] = 31
print(my_dict) # Output: {'name': 'Alice', 'age': 31, 'city': 'New York'}

# Adding new key-value pairs
my_dict["occupation"] = "Engineer"
print(my_dict)

# Removing key-value pairs
my_dict.pop("city") # Removes and returns the value for the specified key
del my_dict["age"] # Deletes the key-value pair
print(my_dict)

# Iterating through a dictionary
for key, value in my_dict.items():
    print(f"{key}: {value}")
```

## 4. Sets

Sets are unordered collections of unique items. They are defined by enclosing elements in curly braces `{}` (except for an empty set, which is `set()`).

### Characteristics:
-   **Unordered:** Elements do not have a defined order.
-   **Mutable:** You can add or remove items.
-   **Unique Elements:** Sets do not allow duplicate items.
-   **Unindexed:** You cannot access elements by index.

### Examples:
```python
# Creating a set
my_set = {1, 2, 3, 3, 4, 5}
print(my_set) # Output: {1, 2, 3, 4, 5} (duplicates are automatically removed)

# Creating an empty set
empty_set = set()

# Adding elements
my_set.add(6)
print(my_set)

# Removing elements
my_set.remove(1) # Raises KeyError if item not found
my_set.discard(10) # Does nothing if item not found
print(my_set)

# Set operations
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}

print(set1.union(set2))        # Output: {1, 2, 3, 4, 5, 6}
print(set1.intersection(set2)) # Output: {3, 4}
print(set1.difference(set2))   # Output: {1, 2}
print(set1.symmetric_difference(set2)) # Output: {1, 2, 5, 6}
```
