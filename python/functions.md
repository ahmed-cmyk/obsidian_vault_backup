# Python Functions

## Defining a Function

A function is a block of code which only runs when it is called. You can pass data, known as parameters, into a function. A function can return data as a result.

```python
def greet(name):
    """This function greets the person passed in as a parameter."""
    print(f"Hello, {name}!")

greet("Alice")
```

## Arguments and Parameters

- **Parameters**: The names listed inside the function's parentheses.
- **Arguments**: The actual values passed to the function when it is called.

### Arbitrary Arguments (`*args`)

If you do not know how many arguments that will be passed into your function, add a `*` before the parameter name in the function definition. This way the function will receive a `tuple` of arguments.

```python
def my_function(*kids):
    print("The youngest child is " + kids[2])

my_function("Emil", "Tobias", "Linus")
```

### Keyword Arguments (`**kwargs`)

You can also send arguments with the `key = value` syntax. This way the order of the arguments does not matter.

```python
def my_function(child3, child2, child1):
    print("The youngest child is " + child3)

my_function(child1 = "Emil", child2 = "Tobias", child3 = "Linus")
```

### Arbitrary Keyword Arguments (`**kwargs`)

If you do not know how many keyword arguments that will be passed into your function, add two asterisk `**` before the parameter name in the function definition. This way the function will receive a `dictionary` of arguments.

```python
def my_function(**kid):
    print("His last name is " + kid["lname"])

my_function(fname = "Tobias", lname = "Refsnes")
```

## Return Values

To let a function return a value, use the `return` statement.

```python
def add(a, b):
    return a + b

result = add(5, 3)
print(result) # Output: 8
```

## Lambda Functions

A lambda function is a small anonymous function. It can take any number of arguments, but can only have one expression.

```python
x = lambda a, b : a * b
print(x(5, 6)) # Output: 30
```