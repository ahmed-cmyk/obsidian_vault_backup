# UNION and UNION ALL

UNION and UNION ALL are used to **combine the result sets of two or more queries into a single result set**.

## When to Use
✅ When data you need is split across multiple tables or queries but has the same meaning and structure. ✅ When you want to stack rows from different sources.

## Rules
* Each SELECT query must return the same **number of columns**.
* The columns must have **compatible data types**.
* Column names in the final result come from the **first query**.

## Syntax

```sql
SELECT column1, column2, …
FROM table1
[WHERE condition]
UNION [ALL]
SELECT column1, column2, …
FROM table2
[WHERE condition];
```

# UNION vs UNION ALL
| **Feature** | UNION | UNION ALL |
|:-:|:-:|:-:|
| Duplicates? | Removes duplicates | Keeps duplicates |
| Performance | Slower (deduplicates rows) | Faster (no extra work) |
| Use Case | When only unique rows matter | When all rows are needed |

# Examples
### Tables

**students_2024**
| **id** | **name** |
|:-:|:-:|
| 1 | Alice |
| 2 | Bob |
**students_2025**
| **id** | **name** |
|:-:|:-:|
| 3 | Carol |
| 2 | Bob |

### Example with `UNION`

```sql
SELECT name
FROM students_2024
UNION
SELECT name
FROM students_2025;
```

**Result**
| **name** |
|:-:|
| Alice |
| Bob |
| Carol |

### Example with `UNION ALL`

```sql
SELECT name
FROM students_2024
UNION ALL
SELECT name
FROM students_2025;
```

**Result**
| **name** |
|:-:|
| Alice |
| Bob |
| Carol |
| Bob |

# Best Practices
* Use UNION ALL if you don’t care about duplicates — it’s faster.
* Always double-check that the columns are in the same order and have compatible data types.
* If you need distinct rows, use UNION.

---

#sql