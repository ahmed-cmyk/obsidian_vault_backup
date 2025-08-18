# Queries with Aggregates

In addition to simple expressions, SQL supports **aggregate functions** that allow you to summarize or compute information across a group of rows. 
These are useful for answering questions like:
* *How many movies has Pixar produced?*
* *What is the highest-grossing Pixar film each year?*

# Aggregate Functions Over All Rows
You can use aggregate functions over the entire result set to produce a single summary value.

**Example:**

```sql
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
FROM mytable
WHERE constraint_expression;
```

* Without a GROUP BY, the aggregate function runs over *all rows* returned by the WHERE clause.
* Like other expressions, it’s good practice to give aggregate results a descriptive alias using AS.

⠀
## Common Aggregate Functions

| **Function** | **Description** |
|:-:|:-:|
| COUNT(*) / COUNT(column) | Counts the number of rows. If you specify a column, it only counts rows where that column is not NULL. |
| MIN(column) | Finds the smallest (minimum) value in the column. |
| MAX(column) | Finds the largest (maximum) value in the column. |
| AVG(column) | Calculates the average of the values in the column. |
| SUM(column) | Calculates the total (sum) of the values in the column. |

Refer to your database’s documentation (MySQL, PostgreSQL, SQLite, SQL Server, etc.) for details and additional functions.

# Aggregate Functions With Groups

You can also compute aggregates **for each group of rows**, rather than the entire dataset. This is done using the `GROUP BY` clause.

**Example:**

```sql
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
FROM mytable
WHERE constraint_expression
GROUP BY column;
```

* The GROUP BY clause groups rows that share the same value in the specified column.
* The aggregate function is then computed **within each group**.
* The result set will contain one row per unique group.

# Filtering Grouped Rows withHAVING
Our queries are getting fairly complex, but we’ve now introduced nearly all the important parts of a SELECT query. One thing you might notice is: if GROUP BY comes **after** WHERE (which filters individual rows), how do we filter the **grouped rows** themselves?
SQL solves this with the HAVING clause, which is designed specifically for filtering groups after aggregation.

**Example:**

```sql
SELECT group_by_column, AGG_FUNC(column_expression) AS aggregate_result_alias, …
FROM mytable
WHERE condition
GROUP BY column
HAVING group_condition;
```

* HAVING is written similarly to WHERE, but it applies to the grouped rows after aggregation.
* This is especially useful when working with large datasets where you want to filter out groups that don’t meet certain criteria.

> **NOTE:** If you’re not using GROUP BY, a simple WHERE clause is enough — HAVING is only necessary when working with grouped data.

---

#sql