# Pandas Grouping and Aggregation

## `groupby()`

The `groupby()` method is used for grouping data based on some criteria. It returns a `GroupBy` object, which can then be used to apply various aggregation functions.

```python
import pandas as pd

data = {'Category': ['A', 'B', 'A', 'B', 'A'],
        'Value': [10, 20, 15, 25, 12],
        'City': ['NY', 'LA', 'NY', 'LA', 'SF']}
df = pd.DataFrame(data)

print("Original DataFrame:\n", df)
```

### Aggregation Functions

Common aggregation functions include `sum()`, `mean()`, `count()`, `min()`, `max()`, `std()`, etc.

```python
# Group by 'Category' and calculate the sum of 'Value'
grouped_sum = df.groupby('Category')['Value'].sum()
print("\nGrouped Sum:\n", grouped_sum)

# Group by 'Category' and calculate the mean of 'Value'
grouped_mean = df.groupby('Category')['Value'].mean()
print("\nGrouped Mean:\n", grouped_mean)

# Group by multiple columns
grouped_multi = df.groupby(['Category', 'City'])['Value'].mean()
print("\nGrouped by Category and City:\n", grouped_multi)
```

### `agg()` Method

The `agg()` method allows you to apply multiple aggregation functions at once, or apply different functions to different columns.

```python
# Apply multiple functions to a single column
agg_single_col = df.groupby('Category')['Value'].agg(['sum', 'mean', 'count'])
print("\nAggregated (single column):\n", agg_single_col)

# Apply different functions to different columns
agg_multi_col = df.groupby('Category').agg(
    total_value=('Value', 'sum'),
    average_value=('Value', 'mean'),
    city_count=('City', 'count')
)
print("\nAggregated (multiple columns):\n", agg_multi_col)
```

## `transform()`

`transform()` returns a Series with the same index as the original DataFrame, allowing for broadcasting results back to the original DataFrame.

```python
# Normalize 'Value' within each 'Category'
df['Normalized_Value'] = df.groupby('Category')['Value'].transform(lambda x: (x - x.mean()) / x.std())
print("\nDataFrame with Normalized Value:\n", df)
```

## `apply()`

`apply()` is a more general method that can apply an arbitrary function to each group. It is more flexible but can be slower than `agg()` or `transform()` for simple aggregations.

```python
# Apply a custom function to each group
def custom_agg(group):
    return group['Value'].max() - group['Value'].min()

applied_result = df.groupby('Category').apply(custom_agg)
print("\nApplied Custom Function:\n", applied_result)
```
