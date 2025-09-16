# Pandas Data Cleaning and Preprocessing

## Handling Missing Data

Missing data is often represented as `NaN` (Not a Number).

### Checking for Missing Values

`isnull()` and `notnull()` are used to detect missing values.

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    'A': [1, 2, np.nan, 4],
    'B': [5, np.nan, np.nan, 8],
    'C': [9, 10, 11, 12]
})

print("Is null:\n", df.isnull())
print("Sum of nulls per column:\n", df.isnull().sum())
```

### Dropping Missing Values

`dropna()` removes rows or columns with missing values.

```python
# Drop rows with any NaN values
df_dropped_rows = df.dropna()
print("Dropped rows:\n", df_dropped_rows)

# Drop columns with any NaN values
df_dropped_cols = df.dropna(axis=1)
print("Dropped columns:\n", df_dropped_cols)

# Drop rows only if all values are NaN
df_dropped_all = df.dropna(how='all')
print("Dropped if all NaN:\n", df_dropped_all)
```

### Filling Missing Values

`fillna()` replaces missing values with a specified value or method.

```python
# Fill with a static value
df_filled_static = df.fillna(0)
print("Filled with 0:\n", df_filled_static)

# Fill with mean of the column
df_filled_mean = df.fillna(df.mean(numeric_only=True))
print("Filled with mean:\n", df_filled_mean)

# Forward fill (propagate last valid observation forward to next valid)
df_ffill = df.fillna(method='ffill')
print("Forward fill:\n", df_ffill)

# Backward fill (propagate next valid observation backward to next valid)
df_bfill = df.fillna(method='bfill')
print("Backward fill:\n", df_bfill)
```

## Handling Duplicates

### Checking for Duplicates

`duplicated()` returns a boolean Series indicating duplicate rows.

```python
df_dup = pd.DataFrame({
    'A': [1, 2, 2, 3],
    'B': ['x', 'y', 'y', 'z']
})

print("Duplicated rows:\n", df_dup.duplicated())
```

### Dropping Duplicates

`drop_duplicates()` removes duplicate rows.

```python
df_no_dup = df_dup.drop_duplicates()
print("After dropping duplicates:\n", df_no_dup)

# Keep first occurrence (default)
# df_no_dup_first = df_dup.drop_duplicates(keep='first')

# Keep last occurrence
# df_no_dup_last = df_dup.drop_duplicates(keep='last')

# Drop all duplicates
# df_no_dup_false = df_dup.drop_duplicates(keep=False)
```

## Data Type Conversion

`astype()` is used to convert a column to a different data type.

```python
df_types = pd.DataFrame({'A': [1, 2, 3], 'B': ['1.1', '2.2', '3.3']})
print("Original dtypes:\n", df_types.dtypes)

df_types['B'] = df_types['B'].astype(float)
print("New dtypes:\n", df_types.dtypes)
```
```