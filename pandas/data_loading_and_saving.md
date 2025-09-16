# Pandas Data Loading and Saving

## Reading Data

Pandas provides functions to read data from various file formats into a DataFrame.

### CSV Files

`read_csv()` is used to read comma-separated values (CSV) files.

```python
import pandas as pd

# Read from a local CSV file
df_csv = pd.read_csv('data.csv')
print("DataFrame from CSV:\n", df_csv.head())

# Read from a URL
# url = "https://raw.githubusercontent.com/pandas-dev/pandas/main/doc/data/titanic.csv"
# df_url = pd.read_csv(url)
# print("DataFrame from URL:\n", df_url.head())
```

### Excel Files

`read_excel()` is used to read Excel files (`.xls`, `.xlsx`).

```python
# df_excel = pd.read_excel('data.xlsx', sheet_name='Sheet1')
# print("DataFrame from Excel:\n", df_excel.head())
```

### JSON Files

`read_json()` is used to read JSON (JavaScript Object Notation) files.

```python
# df_json = pd.read_json('data.json')
# print("DataFrame from JSON:\n", df_json.head())
```

### SQL Databases

`read_sql_query()` or `read_sql_table()` can be used to read data directly from SQL databases.

```python
# from sqlalchemy import create_engine
# engine = create_engine('sqlite:///my_database.db')
# df_sql = pd.read_sql_table('my_table', engine)
# print("DataFrame from SQL Table:\n", df_sql.head())

# df_sql_query = pd.read_sql_query('SELECT * FROM my_table', engine)
# print("DataFrame from SQL Query:\n", df_sql_query.head())
```

## Saving Data

Pandas DataFrames can be easily saved to various file formats.

### To CSV

`to_csv()` saves a DataFrame to a CSV file.

```python
# Assuming df is an existing DataFrame
# df.to_csv('output.csv', index=False) # index=False prevents writing the DataFrame index as a column
```

### To Excel

`to_excel()` saves a DataFrame to an Excel file.

```python
# df.to_excel('output.xlsx', index=False, sheet_name='Sheet1')
```

### To JSON

`to_json()` saves a DataFrame to a JSON file.

```python
# df.to_json('output.json', orient='records') # orient can be 'records', 'columns', 'index', etc.
```

### To SQL

`to_sql()` writes records stored in a DataFrame to a SQL database.

```python
# df.to_sql('new_table', engine, if_exists='replace', index=False)
# # if_exists can be 'fail', 'replace', 'append'
```