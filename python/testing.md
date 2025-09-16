# Python Testing

Testing is a crucial part of software development, ensuring that code behaves as expected and remains functional as changes are made. Python provides built-in tools and popular third-party frameworks for writing various types of tests.

## Types of Tests

1.  **Unit Tests:** Test individual components (functions, methods, classes) in isolation. They are small, fast, and focus on the smallest testable parts of an application.
2.  **Integration Tests:** Test the interaction between multiple components or modules to ensure they work together correctly.
3.  **Functional/Acceptance Tests:** Test the system from the user's perspective, verifying that the application meets its requirements.
4.  **End-to-End (E2E) Tests:** Test the entire application flow, often involving a UI and database.

## 1. `unittest` (Built-in Testing Framework)

Python's `unittest` module is inspired by JUnit and provides a rich set of tools for constructing and running tests. It follows an xUnit style of testing.

### Basic Structure

-   Tests are organized into classes that inherit from `unittest.TestCase`.
-   Each test method within the class must start with `test_`.
-   Assertions (e.g., `assertEqual`, `assertTrue`) are used to check for expected outcomes.
-   `setUp()` and `tearDown()` methods can be used to prepare and clean up resources before and after each test method.

### Example:

Let's say we have a simple function to test in `my_math.py`:

```python
# my_math.py
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

def multiply(a, b):
    return a * b
```

And our test file `test_my_math.py`:

```python
# test_my_math.py
import unittest
from my_math import add, subtract, multiply

class TestMyMath(unittest.TestCase):

    def setUp(self):
        # This method is called before each test method
        print("\nSetting up test...")
        self.x = 10
        self.y = 5

    def tearDown(self):
        # This method is called after each test method
        print("Tearing down test...")

    def test_add(self):
        print("Running test_add")
        self.assertEqual(add(self.x, self.y), 15)
        self.assertEqual(add(-1, 1), 0)
        self.assertEqual(add(-1, -1), -2)

    def test_subtract(self):
        print("Running test_subtract")
        self.assertEqual(subtract(self.x, self.y), 5)
        self.assertEqual(subtract(5, 10), -5)

    def test_multiply(self):
        print("Running test_multiply")
        self.assertEqual(multiply(self.x, self.y), 50)
        self.assertEqual(multiply(0, 10), 0)

    def test_add_floats(self):
        print("Running test_add_floats")
        self.assertAlmostEqual(add(0.1, 0.2), 0.3) # Use assertAlmostEqual for floats

    def test_divide_by_zero(self):
        print("Running test_divide_by_zero")
        with self.assertRaises(ZeroDivisionError):
            # Assuming a divide function exists that raises this error
            # from my_math import divide
            # divide(10, 0)
            pass # Placeholder, as divide is not in my_math.py

if __name__ == '__main__':
    unittest.main(verbosity=2) # verbosity=2 shows more details
```

### Running Tests

-   **From the command line:**
    ```bash
    python -m unittest test_my_math.py
    # Or to discover all tests in the current directory and subdirectories:
    python -m unittest discover
    ```

-   **From within the test file:** If you include `if __name__ == '__main__': unittest.main()`, you can run the file directly.

## 2. `pytest` (Popular Third-Party Framework)

`pytest` is a widely used and highly recommended testing framework for Python. It's known for its simplicity, powerful features, and extensive plugin ecosystem. It requires installation:

```bash
pip install pytest
```

### Key Features of `pytest`

-   **Simple Test Discovery:** Automatically finds tests (files named `test_*.py` or `*_test.py`, functions/methods starting with `test_`).
-   **Plain Functions:** You can write tests as simple functions, no need for `TestCase` inheritance.
-   **Powerful Assertions:** Uses standard `assert` statements, making tests more readable.
-   **Fixtures:** A flexible way to set up and tear down test environments (more powerful than `setUp`/`tearDown`).
-   **Plugins:** A rich ecosystem for various testing needs (e.g., coverage, mocking, parallel execution).

### Example (`pytest` style):

Using the same `my_math.py`:

```python
# test_my_math_pytest.py
from my_math import add, subtract, multiply
import pytest

# Test functions (must start with test_)
def test_add():
    assert add(10, 5) == 15
    assert add(-1, 1) == 0
    assert add(-1, -1) == -2

def test_subtract():
    assert subtract(10, 5) == 5
    assert subtract(5, 10) == -5

def test_multiply():
    assert multiply(10, 5) == 50
    assert multiply(0, 10) == 0

# Test with floats
def test_add_floats():
    assert abs(add(0.1, 0.2) - 0.3) < 1e-6 # Direct assert with tolerance

# Test for expected exceptions
def test_divide_by_zero_raises_error():
    with pytest.raises(ZeroDivisionError):
        # Assuming a divide function exists that raises this error
        # from my_math import divide
        # divide(10, 0)
        pass # Placeholder

# Example of a fixture
@pytest.fixture
def sample_data():
    # Setup code
    data = {"a": 10, "b": 5}
    print("\nFixture setup: providing sample_data")
    yield data # This is where the test function runs
    # Teardown code
    print("Fixture teardown: cleaning up sample_data")

def test_add_with_fixture(sample_data):
    print("Running test_add_with_fixture")
    assert add(sample_data["a"], sample_data["b"]) == 15

# Parameterized testing with pytest.mark.parametrize
@pytest.mark.parametrize("input_a, input_b, expected", [
    (1, 2, 3),
    (-1, 1, 0),
    (0, 0, 0),
])
def test_add_parameterized(input_a, input_b, expected):
    assert add(input_a, input_b) == expected
```

### Running `pytest`

Simply run `pytest` in your terminal from the project root or the directory containing your tests:

```bash
pytest
```

-   `pytest -v`: Verbose output.
-   `pytest -k "add"`: Run tests whose names contain "add".
-   `pytest -m "slow"`: Run tests marked with `@pytest.mark.slow`.

## 3. Mocking

Mocking is essential for unit testing, especially when your code interacts with external systems (databases, APIs, file systems). A mock object simulates the behavior of a real object, allowing you to test your code in isolation without relying on the actual external dependency.

-   **`unittest.mock` (built-in):** Python's standard library for mocking.
-   **`pytest-mock` (pytest plugin):** Provides a convenient `mocker` fixture for `pytest`.

### Example with `unittest.mock`

Let's say we have a function that fetches data from an API:

```python
# my_service.py
import requests

def get_user_data(user_id):
    response = requests.get(f"https://api.example.com/users/{user_id}")
    response.raise_for_status() # Raise an exception for bad status codes
    return response.json()
```

Test file `test_my_service.py`:

```python
# test_my_service.py
import unittest
from unittest.mock import patch, Mock
from my_service import get_user_data

class TestMyService(unittest.TestCase):

    @patch('my_service.requests.get') # Patch requests.get in the context of my_service
    def test_get_user_data_success(self, mock_get):
        # Configure the mock object's return value
        mock_response = Mock()
        mock_response.status_code = 200
        mock_response.json.return_value = {"id": 1, "name": "Alice"}
        mock_get.return_value = mock_response

        data = get_user_data(1)

        mock_get.assert_called_once_with("https://api.example.com/users/1")
        self.assertEqual(data, {"id": 1, "name": "Alice"})

    @patch('my_service.requests.get')
    def test_get_user_data_http_error(self, mock_get):
        mock_response = Mock()
        mock_response.status_code = 404
        mock_response.raise_for_status.side_effect = requests.exceptions.HTTPError
        mock_get.return_value = mock_response

        with self.assertRaises(requests.exceptions.HTTPError):
            get_user_data(999)

if __name__ == '__main__':
    unittest.main()
```

## 4. Test Coverage

Test coverage measures the percentage of your source code that is executed when your tests run. Tools like `coverage.py` (often used with `pytest-cov` plugin for `pytest`) help you identify untested parts of your codebase.

```bash
pip install coverage pytest-cov

# Run tests with coverage (using pytest)
pytest --cov=.

# Generate an HTML report
coverage html
# Open htmlcov/index.html in your browser
```

## Best Practices for Testing

-   **F.I.R.S.T Principles:**
    -   **F**ast: Tests should run quickly.
    -   **I**solated: Tests should be independent of each other.
    -   **R**epeatable: Running tests multiple times should yield the same results.
    -   **S**elf-validating: Tests should automatically pass or fail without manual inspection.
    -   **T**imely: Write tests early, ideally before or alongside the code (TDD).
-   **Test One Thing:** Each test should focus on verifying a single piece of functionality.
-   **Arrange-Act-Assert (AAA):** A common pattern for structuring tests:
    -   **Arrange:** Set up the test data and environment.
    -   **Act:** Perform the action you want to test.
    -   **Assert:** Verify the outcome.
-   **Meaningful Names:** Give your test functions/methods descriptive names.
-   **Don't Test Python:** Don't write tests for built-in Python features or standard library functions; assume they work correctly.
-   **Use Fixtures/Setup:** Leverage `setUp`/`tearDown` (unittest) or fixtures (pytest) for common setup/teardown logic.
