# Python Functional Programming Concepts

Functional programming is a programming paradigm that treats computation as the evaluation of mathematical functions and avoids changing state and mutable data. While Python is not a purely functional language, it supports many functional programming concepts and provides tools that enable a functional style.

## Core Concepts

1.  **Pure Functions:**
    -   Given the same input, they always return the same output.
    -   They have no side effects (e.g., modifying global variables, printing to console, I/O operations).
    -   Benefits: Easier to test, debug, and reason about.

2.  **Immutability:**
    -   Data structures are not modified after creation. Instead, new data structures are created with the changes.
    -   In Python, tuples and strings are immutable. Lists and dictionaries are mutable.

3.  **First-Class Functions:**
    -   Functions can be treated like any other variable: assigned to variables, passed as arguments to other functions, and returned as values from other functions.

4.  **Higher-Order Functions:**
    -   Functions that take one or more functions as arguments or return a function as their result.

5.  **Referential Transparency:**
    -   An expression can be replaced with its value without changing the program's behavior. This is a direct consequence of pure functions.

## Python Tools for Functional Programming

### 1. `lambda` Functions (Anonymous Functions)

`lambda` functions are small, anonymous functions defined with the `lambda` keyword. They can have any number of arguments but only one expression.

```python
# Syntax: lambda arguments: expression

add = lambda x, y: x + y
print(add(5, 3)) # Output: 8

# Used often with higher-order functions
numbers = [(1, 2), (3, 1), (0, 5)]
numbers.sort(key=lambda item: item[1]) # Sort by the second element of each tuple
print(numbers) # Output: [(3, 1), (1, 2), (0, 5)]
```

### 2. `map()`

Applies a given function to each item of an iterable and returns an iterator of the results.

```python
numbers = [1, 2, 3, 4, 5]
squared_numbers = map(lambda x: x * x, numbers)
print(list(squared_numbers)) # Output: [1, 4, 9, 16, 25]

def to_upper(s):
    return s.upper()

words = ["hello", "world"]
uppercased_words = map(to_upper, words)
print(list(uppercased_words)) # Output: ['HELLO', 'WORLD']
```

### 3. `filter()`

Constructs an iterator from elements of an iterable for which a function returns true.

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
even_numbers = filter(lambda x: x % 2 == 0, numbers)
print(list(even_numbers)) # Output: [2, 4, 6, 8, 10]

words = ["apple", "banana", "cat", "dog"]
long_words = filter(lambda w: len(w) > 3, words)
print(list(long_words)) # Output: ['apple', 'banana']
```

### 4. `reduce()` (from `functools`)

Applies a function of two arguments cumulatively to the items of an iterable, from left to right, so as to reduce the iterable to a single value.

```python
from functools import reduce

numbers = [1, 2, 3, 4, 5]
sum_all = reduce(lambda x, y: x + y, numbers)
print(sum_all) # Output: 15 ((((1+2)+3)+4)+5)

product_all = reduce(lambda x, y: x * y, numbers)
print(product_all) # Output: 120 (1*2*3*4*5)

# Finding the maximum number
max_num = reduce(lambda a, b: a if a > b else b, numbers)
print(max_num) # Output: 5
```

### 5. `functools` Module

Provides higher-order functions and operations on callable objects.

-   **`@functools.wraps`:** Used in decorators to preserve metadata of the decorated function (already covered in Decorators notes).
-   **`@functools.lru_cache`:** A decorator that caches the results of a function call. Useful for memoization.

    ```python
    from functools import lru_cache

    @lru_cache(maxsize=None) # Cache all results
    def fibonacci(n):
        if n <= 1:
            return n
        return fibonacci(n-1) + fibonacci(n-2)

    print(fibonacci(10)) # Computes quickly due to caching
    print(fibonacci(5))
    ```

-   **`functools.partial`:** Allows you to create a new function with some arguments of an existing function pre-filled.

    ```python
    from functools import partial

    def power(base, exponent):
        return base ** exponent

    square = partial(power, exponent=2)
    cube = partial(power, exponent=3)

    print(square(5)) # Output: 25
    print(cube(5))   # Output: 125
    ```

### 6. List Comprehensions, Set Comprehensions, Dictionary Comprehensions

While not strictly functional, comprehensions provide a concise and often more readable way to create new collections based on existing ones, promoting immutability by creating new data structures rather than modifying existing ones in place.

```python
# List comprehension
squares = [x*x for x in range(10) if x % 2 == 0]
print(squares) # Output: [0, 4, 16, 36, 64]

# Dictionary comprehension
word_lengths = {word: len(word) for word in ["apple", "banana", "cat"]}
print(word_lengths) # Output: {'apple': 5, 'banana': 6, 'cat': 3}

# Set comprehension
unique_letters = {letter for word in ["hello", "world"] for letter in word}
print(unique_letters) # Output: {'o', 'r', 'd', 'e', 'h', 'l', 'w'}
```

## Benefits of Functional Programming Style

-   **Modularity:** Functions are independent and self-contained.
-   **Testability:** Pure functions are easy to test in isolation.
-   **Concurrency:** Easier to parallelize code without worrying about race conditions due to immutability and lack of side effects.
-   **Readability:** Can lead to more concise and expressive code, especially with comprehensions and higher-order functions.
-   **Debugging:** Easier to debug as state changes are minimized.

## Limitations in Python

Python is an object-oriented language at its core, and not purely functional. While it supports functional paradigms, some features (like mutable data structures by default) mean you need to be mindful when adopting a purely functional style.
