# Pandas Merging and Joining

## Merging DataFrames (`pd.merge()`)

`pd.merge()` is used to combine DataFrames based on common columns or indices. It is similar to SQL JOIN operations.

### Inner Merge (Default)

Returns only the rows where the merge key is present in both DataFrames.

```python
import pandas as pd

df1 = pd.DataFrame({'key': ['A', 'B', 'C', 'D'], 'value': [1, 2, 3, 5]})
df2 = pd.DataFrame({'key': ['B', 'D', 'E', 'F'], 'value': [6, 7, 8, 9]})

merged_inner = pd.merge(df1, df2, on='key')
print("Inner Merge:\n", merged_inner)
# Output:
#   key  value_x  value_y
# 0   B        2        6
# 1   D        5        7
```

### Outer Merge

Returns all rows when there is a match in one of the DataFrames. Fills `NaN` for missing values.

```python
merged_outer = pd.merge(df1, df2, on='key', how='outer')
print("\nOuter Merge:\n", merged_outer)
# Output:
#   key  value_x  value_y
# 0   A      1.0      NaN
# 1   B      2.0      6.0
# 2   C      3.0      NaN
# 3   D      5.0      7.0
# 4   E      NaN      8.0
# 5   F      NaN      9.0
```

### Left Merge

Returns all rows from the left DataFrame, and matching rows from the right DataFrame. Fills `NaN` for unmatched rows in the right DataFrame.

```python
merged_left = pd.merge(df1, df2, on='key', how='left')
print("\nLeft Merge:\n", merged_left)
# Output:
#   key  value_x  value_y
# 0   A        1      NaN
# 1   B        2      6.0
# 2   C        3      NaN
# 3   D        5      7.0
```

### Right Merge

Returns all rows from the right DataFrame, and matching rows from the left DataFrame. Fills `NaN` for unmatched rows in the left DataFrame.

```python
merged_right = pd.merge(df1, df2, on='key', how='right')
print("\nRight Merge:\n", merged_right)
# Output:
#   key  value_x  value_y
# 0   B      2.0        6
# 1   D      5.0        7
# 2   E      NaN        8
# 3   F      NaN        9
```

### Merging on Different Column Names

Use `left_on` and `right_on` if the merge keys have different names.

```python
df3 = pd.DataFrame({'lkey': ['A', 'B', 'C'], 'value': [1, 2, 3]})
df4 = pd.DataFrame({'rkey': ['A', 'B', 'D'], 'value': [4, 5, 6]})

merged_diff_keys = pd.merge(df3, df4, left_on='lkey', right_on='rkey')
print("\nMerge with different keys:\n", merged_diff_keys)
```

## Joining DataFrames (`.join()`)

`.join()` is a convenient method for combining DataFrames by index or by a key column. It defaults to a left join.

```python
df_left = pd.DataFrame({'A': ['A0', 'A1'],
                        'B': ['B0', 'B1']},
                       index=['K0', 'K1'])
df_right = pd.DataFrame({'C': ['C0', 'C1'],
                         'D': ['D0', 'D1']},
                        index=['K0', 'K2'])

joined_df = df_left.join(df_right)
print("\nJoined DataFrame (default left join):\n", joined_df)
# Output:
#      A    B    C    D
# K0  A0   B0   C0   D0
# K1  A1   B1  NaN  NaN

joined_outer = df_left.join(df_right, how='outer')
print("\nJoined DataFrame (outer join):\n", joined_outer)
# Output:
#      A    B    C    D
# K0  A0   B0   C0   D0
# K1  A1   B1  NaN  NaN
# K2  NaN  NaN   C1   D1
```