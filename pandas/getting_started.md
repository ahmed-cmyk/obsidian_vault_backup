# Getting Started with Pandas

## Installation

Pandas is a powerful data manipulation library for Python. It's typically installed via `pip`.

- **Prerequisites**: Ensure you have Python installed (preferably Python 3.7+).

- **Install Pandas**:
  ```bash
  pip install pandas numpy
  ```
  `numpy` is a common dependency for Pandas and is often installed alongside it.

- **Verification**: To check if Pandas is installed correctly, open a Python interpreter and try importing it:
  ```python
  import pandas as pd
  print(pd.__version__)
  ```
  This should print the installed version of Pandas.

## Core Data Structures

Pandas introduces two primary data structures: `Series` and `DataFrame`.

### Series

A `Series` is a one-dimensional labeled array capable of holding any data type (integers, strings, floating point numbers, Python objects, etc.). The row labels are called the index.

```python
import pandas as pd

s = pd.Series([1, 3, 5, 7, 9])
print(s)

# Series with custom index
s_indexed = pd.Series([10, 20, 30], index=['a', 'b', 'c'])
print(s_indexed)
```

### DataFrame

A `DataFrame` is a two-dimensional labeled data structure with columns of potentially different types. You can think of it like a spreadsheet or SQL table, or a dictionary of Series objects.

```python
import pandas as pd

data = {'Name': ['Alice', 'Bob', 'Charlie'],
        'Age': [25, 30, 35],
        'City': ['New York', 'Los Angeles', 'Chicago']}
df = pd.DataFrame(data)
print(df)

# Accessing columns
print(df['Name'])

# Accessing rows by index
print(df.loc[0])
```

## Basic Operations

### Reading Data

Pandas can read data from various file formats.

```python
# Assuming 'data.csv' exists with comma-separated values
# df_csv = pd.read_csv('data.csv')
# print(df_csv.head())

# Assuming 'data.xlsx' exists
# df_excel = pd.read_excel('data.xlsx')
# print(df_excel.head())
```

### Data Selection

Selecting data is a fundamental operation.

```python
import pandas as pd

data = {'col1': [1, 2, 3, 4],
        'col2': ['A', 'B', 'C', 'D']}
df = pd.DataFrame(data)

# Select a single column
print(df['col1'])

# Select multiple columns
print(df[['col1', 'col2']])

# Select rows based on condition
print(df[df['col1'] > 2])
```