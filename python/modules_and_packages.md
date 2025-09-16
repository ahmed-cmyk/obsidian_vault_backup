# Python Modules and Packages

## Modules

A module is a file containing Python definitions and statements. The file name is the module name with the suffix `.py` appended.

### Creating a Module

Save the code in a file named `mymodule.py`:

```python
# mymodule.py
def greet(name):
    return f"Hello, {name}!"

person = {
    "name": "Alice",
    "age": 30
}
```

### Importing a Module

To use the functionality defined in a module, you use the `import` statement.

```python
import mymodule

print(mymodule.greet("Bob"))
print(mymodule.person["name"])
```

### Importing Specific Parts

You can choose to import only specific parts from a module using the `from` keyword.

```python
from mymodule import greet, person

print(greet("Charlie"))
print(person["age"])
```

### Renaming a Module

You can create an alias when you import a module.

```python
import mymodule as mx

print(mx.greet("David"))
```

## Packages

A package is a collection of modules in directories that gives a package hierarchy. A package must contain a special file called `__init__.py` (which can be empty) to tell Python that the directory is a package.

### Creating a Package

Consider the following directory structure:

```
my_package/
├── __init__.py
├── module_a.py
└── sub_package/
    ├── __init__.py
    └── module_b.py
```

`my_package/module_a.py`:
```python
# module_a.py
def func_a():
    return "Function A from my_package"
```

`my_package/sub_package/module_b.py`:
```python
# module_b.py
def func_b():
    return "Function B from sub_package"
```

### Importing from a Package

```python
# From module_a
from my_package import module_a
print(module_a.func_a())

# From module_b in sub_package
from my_package.sub_package import module_b
print(module_b.func_b())

# Direct import of functions
from my_package.module_a import func_a
print(func_a())
```