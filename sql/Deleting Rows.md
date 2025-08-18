# Deleting Rows

When you need to remove data from a table, you can use the DELETE statement. 
**It specifies:**
* which **table** to delete from
* which **rows** to delete, using a WHERE condition

## Basic `DELETE` Syntax

```sql
DELETE FROM mytable
WHERE condition;
```

* Deletes all rows that satisfy the WHERE condition.
* If you omit WHERE, **all rows in the table will be deleted**. This effectively clears the table.

## Deleting All Rows

If you intentionally want to remove all rows from a table:

```sql
DELETE FROM mytable;
```

Alternatively, many databases also support:

```sql
TRUNCATE TABLE mytable;
```

TRUNCATE is often faster and resets auto-increment counters, but cannot include a WHERE clause.

## Taking Extra Care

Like the UPDATE statement, DELETE is powerful but risky:
* Running it without a WHERE deletes the entire table contents.
* Running it with the wrong WHERE condition deletes the wrong rows.
* Changes are often irreversible unless you have backups or transactions enabled.

## Best Practice

* First, test the WHERE clause with a SELECT query:

```sql
SELECT *
FROM mytable
WHERE condition;
```

* Once you confirm the correct rows are selected, run the DELETE.
* Always double-check the query before executing, especially in production environments.

⠀
## Example

```sql
DELETE FROM employees
WHERE department = 'Sales' AND hired_date < '2015-01-01';
```

This deletes employees in the Sales department who were hired before 2015.

# Summary Table
| **Part** | **Purpose** |
|:-:|:-:|
| DELETE FROM mytable | Specify which table to delete from |
| WHERE condition | Specify which rows to delete |

---

#sql