# Order of Execution of a SQL Query

Understanding this order is crucial: since SQL executes each part sequentially, knowing what’s available at each stage prevents common mistakes (like referencing aliases too early) and helps you write more effective queries.

# Complete SELECT Query Structure

```sql
SELECT DISTINCT column, AGG_FUNC(column_or_expression), …
FROM mytable
    JOIN another_table
      ON mytable.column = another_table.column
WHERE constraint_expression
GROUP BY column
HAVING constraint_expression
ORDER BY column ASC/DESC
LIMIT count OFFSET count;
```

A query begins by identifying the **source data**, then progressively filtering, grouping, and transforming it into a result set that is meaningful and efficient to process.

# Query Execution Order
Here is the logical order in which SQL executes each clause of a SELECT query:

### 1. FROM and JOIN
* The query starts with the FROM clause and any JOINs.
* This defines the **working dataset**, including any subqueries.
* The database may create temporary tables for intermediate results, containing all rows and columns from the specified tables.

### 2. WHERE
* Next, WHERE applies **row-level filtering** to the working set.
* Only rows that satisfy the WHERE conditions remain.
* At this stage:
  * Only columns from the tables in FROM are available.
  * You **cannot** reference SELECT aliases yet because they haven’t been computed.

### 3. GROUP BY
* The remaining rows are grouped according to unique values in the specified column(s).
* Each group represents one row in the grouped result.
* This is typically used alongside **aggregate functions**.
⠀
### 4. HAVING
* After grouping, HAVING filters **groups**, not individual rows.
* It discards groups that don’t satisfy the HAVING condition.
* Like WHERE, HAVING cannot reference SELECT aliases in most databases.

### 5. SELECT
* Only now does SQL compute the expressions and columns specified in the SELECT clause.
* At this point, you can include:
  * Raw columns
  * Aggregated values
  * Expressions and calculations
  * Aliases (defined here) can now be referenced in later clauses like ORDER BY.

### 6. DISTINCT
* SQL removes duplicate rows if DISTINCT is specified.
* Duplicates are determined based on the columns returned in the SELECT clause.

### 7. ORDER BY
* The result set is sorted according to the specified columns or expressions.
* Since SELECT has already been computed, aliases can be used here safely.
* Default sort order is ascending (ASC) unless DESC is specified.
⠀
### 8. LIMIT / OFFSET
* Finally, SQL applies LIMIT and OFFSET to restrict the number of rows returned.
* Rows outside the specified range are discarded.

---

#sql