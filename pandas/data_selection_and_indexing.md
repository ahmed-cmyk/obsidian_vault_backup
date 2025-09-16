# Pandas Data Selection and Indexing

## Selecting Columns

### Single Column

Select a single column using bracket notation.

```python
import pandas as pd

data = {'Name': ['Alice', 'Bob', 'Charlie'],
        'Age': [25, 30, 35],
        'City': ['New York', 'Los Angeles', 'Chicago']}
df = pd.DataFrame(data)

print(df['Name'])
# Output:
# 0      Alice
# 1        Bob
# 2    Charlie
# Name: Name, dtype: object
```

### Multiple Columns

Select multiple columns by passing a list of column names.

```python
print(df[['Name', 'Age']])
# Output:
#       Name  Age
# 0    Alice   25
# 1      Bob   30
# 2  Charlie   35
```

## Selecting Rows

### Using `.loc[]` (Label-based Indexing)

Select rows by label (index name).

```python
# Select row with index 0
print(df.loc[0])
# Output:
# Name    Alice
# Age        25
# City New York
# Name: 0, dtype: object

# Select multiple rows by label
print(df.loc[[0, 2]])
# Output:
#       Name  Age      City
# 0    Alice   25  New York
# 2  Charlie   35   Chicago
```

### Using `.iloc[]` (Integer-location based Indexing)

Select rows by integer position.

```python
# Select row at integer position 0
print(df.iloc[0])
# Output:
# Name    Alice
# Age        25
# City New York
# Name: 0, dtype: object

# Select multiple rows by integer position
print(df.iloc[[0, 2]])
# Output:
#       Name  Age      City
# 0    Alice   25  New York
# 2  Charlie   35   Chicago
```

### Slicing Rows

Both `.loc[]` and `.iloc[]` support slicing.

```python
# .loc[] with label slicing (inclusive of end label)
print(df.loc[0:1])
# Output:
#     Name  Age      City
# 0  Alice   25  New York
# 1    Bob   30   Los Angeles

# .iloc[] with integer slicing (exclusive of end position)
print(df.iloc[0:2])
# Output:
#     Name  Age      City
# 0  Alice   25  New York
# 1    Bob   30   Los Angeles
```

## Conditional Selection

Select rows based on a condition applied to one or more columns.

```python
# Select rows where Age is greater than 28
print(df[df['Age'] > 28])
# Output:
#       Name  Age         City
# 1      Bob   30  Los Angeles
# 2  Charlie   35    Chicago

# Multiple conditions
print(df[(df['Age'] > 28) & (df['City'] == 'Chicago')])
# Output:
#       Name  Age     City
# 2  Charlie   35  Chicago
```