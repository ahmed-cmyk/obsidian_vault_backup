# Self Joins

A **SELF JOIN** is when a table is joined with itself. It allows you to relate rows in a table to *other rows in the same table*.

# When to Use
✅ When rows in a table refer to other rows in the same table. ✅ Common in hierarchical data, relationships, or comparisons.

**Examples:**
* Employees and their managers
* Products and related products
* Friends in a social network

## Syntax

```sql
SELECT a.column, b.column
FROM table_name AS a
JOIN table_name AS b
  ON a.some_column = b.some_column;
```

* You must use **aliases** to distinguish between the two instances of the table.

⠀
## Example
### Table:employees
| **id** | **name** | **manager_id** |
|:-:|:-:|:-:|
| 1 | Alice | NULL |
| 2 | Bob | 1 |
| 3 | Carol | 1 |
| 4 | Dave | 2 |
We want to list each employee with their manager’s name.

### Query

```sql
SELECT e.name AS employee, m.name AS manager
FROM employees AS e
LEFT JOIN employees AS m
  ON e.manager_id = m.id;
```

### Result
| **employee** | **manager** |
|:-:|:-:|
| Alice | NULL |
| Bob | Alice |
| Carol | Alice |
| Dave | Bob |

## Best Practices

* Always alias the table (e, m, etc.) to avoid ambiguity.
* Use LEFT JOIN if some rows might not have a match (e.g., top-level managers with no manager).
* Make sure the join condition matches rows logically — usually a foreign key pointing back to the same table.

---

#sql