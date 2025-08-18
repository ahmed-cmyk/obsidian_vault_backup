# Queries with Expressions

In SQL, you can use **expressions** to apply logic, calculations, or transformations to column values directly in your query.  

Expressions can include:
- Mathematical operations
- String functions
- Date/time functions
- Basic arithmetic

> **NOTE:** This allows you to compute and transform data *while querying*, which can save time and reduce the need for post-processing.

### ---

## Example: Query with an Expression

```sql
SELECT particle_speed / 2.0 AS half_particle_speed
FROM physics_data
WHERE ABS(particle_position) * 10.0 > 500;
```

Here:
* The SELECT divides particle_speed by 2.0 and gives it an alias half_particle_speed.
* The WHERE clause multiplies the absolute value of particle_position by 10.0 and compares it to 500.

## Supported Functions

Each database system (like PostgreSQL, MySQL, SQL Server, etc.) provides its own set of:
* Mathematical functions
* String manipulation functions
* Date/time functions

> **NOTE:** Check your database’s documentation for the full list of supported functions and syntax.

## Aliases for Readability

Since expressions can make queries harder to read, it’s a good practice to assign descriptive **aliases** using the AS keyword:
* Aliases can be applied to **expressions**, **columns**, and even **tables**.
* This improves clarity in the query output and simplifies complex queries.


## Examples

### Expression Alias

```sql
SELECT col_expression AS expr_description, …
FROM mytable;
```

### Column and Table Aliases

```sql
SELECT column AS better_column_name, …
FROM a_long_widgets_table_name AS mywidgets
INNER JOIN widget_sales
  ON mywidgets.id = widget_sales.widget_id;
```

In this example:
* column is given a clearer alias: better_column_name
* a_long_widgets_table_name is aliased to mywidgets for shorter references
* The join references mywidgets instead of the long table name

---

#sql