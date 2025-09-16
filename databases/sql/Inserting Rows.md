# Inserting Rows

The **schema** is what makes it possible for us to add new data to our database.

## What is a Schema?
* A database schema defines the **structure** of each table:
  * What columns exist
  * What data types each column can store
  * Any constraints (e.g., primary keys, defaults, NOT NULL)
* Tables are like 2D grids:
  * Columns represent **properties** of the entity.
  * Rows represent **instances** of that entity.

> **NOTE:** This fixed structure allows the database to remain **efficient** and **consistent**, even with millions or billions of rows.

### Example:

In a Movies table:
* Year column: must be an INTEGER
* Title column: must be a STRING

## Inserting New Data

To insert data into a table, we use the INSERT statement. 
**It specifies:** 
- which table to write to
- which columns to fill
- the values to insert

### Insert with All Columns

If you’re providing values for *every column* (in the correct order), you don’t need to specify column names.

```sql
INSERT INTO mytable
VALUES 
  (value1, value2, value3),
  (value4, value5, value6);
```

* Each set of parentheses represents one row.
* You can insert multiple rows at once by listing them sequentially.

⠀
### Insert with Specific Columns

If you have **incomplete data** or want to explicitly specify which columns to fill, you can list the columns. 
This is also more robust against schema changes (e.g., adding a new column later).

```sql
INSERT INTO mytable
  (column1, column2)
VALUES 
  (value1, value2),
  (value3, value4);
```

* The number of values **must match** the number of columns listed.
* Columns not listed must either have a default value or allow NULL.
⠀
## Why Specify Columns?

🎯 Makes your queries **forward-compatible**: If the table schema changes (e.g., a new column is added with a default value), your INSERT statements won’t break.
🎯 Improves **clarity and maintainability** by making it explicit which data maps to which column.

## Using Expressions inINSERT

You can also use mathematical or string **expressions** in the VALUES:
* Useful for formatting, conversions, or calculations on the fly.

### Example:

```sql
INSERT INTO boxoffice
  (movie_id, rating, sales_in_millions)
VALUES 
  (1, 9.9, 283742034 / 1000000);
```

Here, sales_in_millions is calculated directly in the INSERT.

## Summary Table

| **Pattern** | **When to Use** |
|:-:|:-:|
| INSERT INTO table VALUES (…) | When inserting full rows, with all columns filled in order |
| INSERT INTO table (col1, col2) VALUES (…) | When you have incomplete data or want clarity |
| Using expressions in VALUES | When calculations or formatting are needed |

---

#sql