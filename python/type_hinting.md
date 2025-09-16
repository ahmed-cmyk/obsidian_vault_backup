# Python Type Hinting

Type hinting (or type annotations) is a feature introduced in Python 3.5 (PEP 484) that allows you to indicate the expected types of variables, function parameters, and return values. While Python remains a dynamically typed language (types are checked at runtime), type hints provide a way to add static type information to your code. This information can then be used by external tools (like static type checkers, IDEs, and linters) to help you write more robust and maintainable code.

## Why Use Type Hinting?

1.  **Improved Readability and Maintainability:** Type hints make code easier to understand by explicitly stating the expected types, acting as living documentation.
2.  **Better Tooling Support:** IDEs can provide better autocompletion, refactoring, and error checking based on type information.
3.  **Static Analysis:** Tools like MyPy can analyze your code *before* it runs to catch potential type-related bugs, reducing runtime errors.
4.  **Easier Refactoring:** When types are clear, refactoring becomes less risky as tools can help identify where changes might break type compatibility.
5.  **Collaboration:** Helps teams understand each other's code more quickly and reduces miscommunication about expected data types.

## Basic Syntax

Type hints are added using a colon (`:`) after the variable or parameter name, followed by the type. For return values, an arrow (`->`) is used before the colon.

```python
def greet(name: str) -> str:
    return f"Hello, {name}"

message = greet("Alice")
print(message)

# Variable annotation
count: int = 0
price: float = 19.99
is_active: bool = True
```

## Common Types from the `typing` Module

For more complex type hints, you'll often need to import types from the built-in `typing` module.

### 1. Collections

-   **`List[T]`:** A list where all elements are of type `T`.
-   **`Tuple[T1, T2, ...]` or `Tuple[T, ...]`:** A tuple with specific types at each position, or a tuple with an arbitrary number of elements of type `T`.
-   **`Dict[K, V]`:** A dictionary with keys of type `K` and values of type `V`.
-   **`Set[T]`:** A set where all elements are of type `T`.

```python
from typing import List, Tuple, Dict, Set

def process_numbers(numbers: List[int]) -> List[float]:
    return [float(n) * 2 for n in numbers]

def get_user_info(user_id: int) -> Dict[str, str]:
    return {"name": "Bob", "email": "bob@example.com"}

def get_coordinates() -> Tuple[float, float]:
    return (10.5, 20.3)

def get_unique_ids(ids: Set[int]) -> Set[int]:
    return {id for id in ids if id > 0}
```

### 2. Optional and Union

-   **`Optional[T]`:** Indicates that a variable can be either of type `T` or `None`. It's equivalent to `Union[T, None]`. (Note: In Python 3.10+, `T | None` is preferred).
-   **`Union[T1, T2, ...]`:** Indicates that a variable can be one of several specified types.

```python
from typing import Optional, Union

def find_item(item_id: int) -> Optional[str]:
    if item_id == 1:
        return "Found Item"
    return None

def process_input(value: Union[str, int]) -> str:
    if isinstance(value, int):
        return f"Received integer: {value}"
    return f"Received string: {value.upper()}"

# Python 3.10+ syntax for Union
def process_input_py310(value: str | int) -> str:
    if isinstance(value, int):
        return f"Received integer: {value}"
    return f"Received string: {value.upper()}"
```

### 3. Any

-   **`Any`:** Indicates that a variable can be of any type. Use sparingly, as it defeats the purpose of type checking.

```python
from typing import Any

def process_anything(data: Any) -> Any:
    print(f"Processing: {data}")
    return data
```

### 4. Callable

-   **`Callable[[Arg1Type, Arg2Type, ...], ReturnType]`:** Used to hint functions or other callable objects.

```python
from typing import Callable

def apply_function(func: Callable[[int, int], int], a: int, b: int) -> int:
    return func(a, b)

def add(x, y):
    return x + y

result = apply_function(add, 5, 3)
print(result) # Output: 8
```

### 5. TypeVar (Generic Types)

-   **`TypeVar`:** Used to define generic types, allowing you to write functions or classes that work with multiple types while maintaining type safety.

```python
from typing import TypeVar, List

T = TypeVar('T') # Define a generic type variable

def first_element(items: List[T]) -> T:
    return items[0]

print(first_element([1, 2, 3]))      # T is inferred as int
print(first_element(["a", "b", "c"])) # T is inferred as str
```

### 6. TypedDict

-   **`TypedDict`:** Used to define dictionaries with a specific set of keys and their corresponding types. Useful for defining the structure of JSON-like data.

```python
from typing import TypedDict

class User(TypedDict):
    name: str
    age: int
    email: Optional[str]

def create_user(name: str, age: int, email: Optional[str] = None) -> User:
    return {"name": name, "age": age, "email": email}

user_data: User = create_user("Charlie", 25)
print(user_data)

# Type checker would warn if 'age' was missing or had wrong type
# invalid_user: User = {"name": "David"}
```

## Running a Static Type Checker (MyPy)

MyPy is a popular static type checker for Python. It can be installed via pip:

```bash
pip install mypy
```

To check a file (e.g., `my_module.py`):

```bash
mypy my_module.py
```

Example `my_module.py`:

```python
# my_module.py
def add(a: int, b: int) -> int:
    return a + b

result = add(1, 2) # OK
# result = add("1", "2") # MyPy will flag this as an error
```

Running `mypy my_module.py` would produce an error for the commented line.

## Type Comments (for older Python versions)

For Python versions older than 3.5, you can use type comments:

```python
def greet(name): # type: (str) -> str
    return f"Hello, {name}"

# type: (int) -> None
def process_id(user_id):
    pass
```

However, it's highly recommended to use the modern annotation syntax with Python 3.5+.

## Best Practices

-   **Start Simple:** Begin by adding hints to function signatures and gradually expand.
-   **Be Specific:** Use the most specific type possible (e.g., `List[str]` instead of `List[Any]`).
-   **Use `Optional` for `None`:** Explicitly use `Optional[T]` when a value can be `None`.
-   **Consistency:** Apply type hints consistently across your codebase.
-   **Run a Type Checker:** Integrate MyPy or another type checker into your development workflow.
-   **Don't Over-engineer:** Don't add type hints where they don't add clarity or value.
