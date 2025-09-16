# Python Decorators

Decorators are a powerful and elegant way to modify or enhance the behavior of functions or methods without permanently altering their source code. They are essentially functions that take another function as an argument, add some functionality, and return a new function.

## What is a Decorator?

In Python, functions are first-class objects, meaning they can be passed as arguments, returned from other functions, and assigned to variables. Decorators leverage this concept.

A decorator is a callable that takes a function as an argument and returns a new function. The new function usually incorporates the original function's behavior along with some added functionality.

## Basic Decorator Syntax

Python provides a special syntax for applying decorators using the `@` symbol, which is syntactic sugar for a function call.

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("Something is happening before the function is called.")
        result = func(*args, **kwargs)
        print("Something is happening after the function is called.")
        return result
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()
# Output:
# Something is happening before the function is called.
# Hello!
# Something is happening after the function is called.
```

In the example above:
1.  `my_decorator` is the decorator function.
2.  `say_hello` is the function being decorated.
3.  `@my_decorator` is equivalent to `say_hello = my_decorator(say_hello)`.
4.  The `wrapper` function is what actually replaces the original `say_hello` function.

## Decorators with Arguments

If you need to pass arguments to your decorator, you need an extra layer of nesting.

```python
def repeat(num_times):
    def decorator_repeat(func):
        def wrapper(*args, **kwargs):
            for _ in range(num_times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator_repeat

@repeat(num_times=3)
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")
# Output:
# Hello, Alice!
# Hello, Alice!
# Hello, Alice!
```

Here, `repeat(num_times=3)` is called first, which returns `decorator_repeat`. Then `decorator_repeat` is applied to `greet`.

## Use Cases for Decorators

Decorators are commonly used for:

1.  **Logging:** Logging function calls, arguments, and return values.
2.  **Timing:** Measuring the execution time of functions.
3.  **Authentication/Authorization:** Checking user permissions before executing a function.
4.  **Caching:** Storing results of expensive function calls.
5.  **Input Validation:** Validating arguments passed to a function.
6.  **Rate Limiting:** Controlling how often a function can be called.
7.  **Retries:** Retrying a function call if it fails.

## Preserving Function Metadata: `functools.wraps`

When you use a decorator, the `wrapper` function replaces the original function. This means that metadata like the function's name (`__name__`), docstring (`__doc__`), and module (`__module__`) are lost. This can make debugging and introspection difficult.

To preserve this metadata, use the `functools.wraps` decorator from the `functools` module.

```python
import functools

def my_decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print("Something is happening before the function is called.")
        result = func(*args, **kwargs)
        print("Something is happening after the function is called.")
        return result
    return wrapper

@my_decorator
def say_hello():
    """This function says hello."""
    print("Hello!")

say_hello()
print(say_hello.__name__) # Output: say_hello (preserved)
print(say_hello.__doc__)  # Output: This function says hello. (preserved)
```

## Class-based Decorators

Decorators can also be implemented as classes. For a class to be used as a decorator, it needs to be callable, which means it must implement the `__call__` method.

```python
class MyClassDecorator:
    def __init__(self, func):
        self.func = func

    def __call__(self, *args, **kwargs):
        print("Class decorator: Before function call.")
        result = self.func(*args, **kwargs)
        print("Class decorator: After function call.")
        return result

@MyClassDecorator
def greet_class(name):
    print(f"Hello from class decorator, {name}!")

greet_class("Bob")
# Output:
# Class decorator: Before function call.
# Hello from class decorator, Bob!
# Class decorator: After function call.
```

Class-based decorators are useful when you need to maintain state across multiple function calls or when you want to provide more complex configuration options.
