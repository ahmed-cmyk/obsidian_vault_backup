# Updating Rows

In addition to inserting new data, a common task is to **update existing data**. This is done using the UPDATE statement.

Like INSERT, you must:
* specify which **table** to update
* specify which **columns** to modify
* ensure values match the **data type** of the columns
* define which **rows** to update (using WHERE)

⠀
## Basic `UPDATE` Syntax

```sql
UPDATE mytable
SET column = value_or_expr,
    other_column = another_value_or_expr,
    …
WHERE condition;
```

* Lists one or more column = value pairs.
* Updates all rows that satisfy the WHERE condition.
* If you omit WHERE, all rows in the table will be updated — use caution.
⠀
## How It Works
* SQL identifies the rows matching the WHERE condition.
* For each matched row, the specified columns are updated with the new values or expressions.
⠀
## Taking Care with Updates

Updating data is powerful but also risky. 
**Common mistakes include:**
* Forgetting the WHERE clause, which updates every row.
* Writing an incorrect WHERE condition, which updates the wrong rows.
* Assigning incompatible data types, which results in errors or truncated values.

⠀
## Best Practice

* Write and test the WHERE condition first in a SELECT query to confirm it selects the intended rows:

```sql
SELECT *
FROM mytable
WHERE condition;
```

* Once verified, write the UPDATE statement with the same condition.

⠀
## Example

```sql
UPDATE employees
SET salary = salary * 1.1,
    title = 'Senior Engineer'
WHERE department = 'Engineering' AND experience_years > 5;
```

This increases salary by 10% and updates the title for experienced engineers only.

## Summary Table
| **Part** | **Purpose** |
|:-:|:-:|
| UPDATE mytable | Specify which table to modify |
| SET col = value | Define new values for one or more columns |
| WHERE condition | Specify which rows to update |

---

#sql