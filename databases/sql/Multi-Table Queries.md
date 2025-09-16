# Multi-Table Queries

# Using Joins

Tables that share information about a single entity need to have a *primary key* that identifies that entity *uniquely* across the database. One common primary key type is an auto-incrementing integer (because it is space-efficient), but it can also be a string, a UUID, or a hashed value — as long as it is unique.
Using the **JOIN** clause in a query, we can combine row data across two or more tables using this unique key.

### Selecting Columns from Joined Tables

When you use a JOIN in a query, the result is a single combined table that includes columns from all the tables involved in the join. This means you’re not limited to selecting only columns from the table specified after the FROM keyword — you can also select any columns from the joined tables.
**NOTE:** Always qualify columns with their table name (or alias) if the same column name exists in multiple tables.

# Types of Joins
### 🔷 Inner Join
The **INNER JOIN** combines rows from two tables where the join condition is satisfied — i.e., where there’s a match in both tables. 
Rows that don’t have a match in either table are excluded.

```sql
SELECT m.column, b.column
FROM movies m
INNER JOIN boxoffice b
  ON m.id = b.movie_id;
```

**NOTE:** You might see INNER JOIN written simply as JOIN. They are equivalent.

### 🔷 Left Join
A **LEFT JOIN** (or **LEFT OUTER JOIN**) returns all rows from the **left table**, and matching rows from the **right table**. If there’s no match in the right table, the result will have NULLs for columns from the right table.

```sql
SELECT m.column, b.column
FROM movies m
LEFT JOIN boxoffice b
  ON m.id = b.movie_id;
```

Use LEFT JOIN when you want to keep *all rows* from the left table, even if there’s no corresponding data in the right table.

### 🔷 Right Join
A **RIGHT JOIN** (or **RIGHT OUTER JOIN**) is the opposite of LEFT JOIN. It returns all rows from the **right table**, and matching rows from the **left table**. If there’s no match in the left table, the result will have NULLs for columns from the left table.

```sql
SELECT m.column, b.column
FROM movies m
RIGHT JOIN boxoffice b
  ON m.id = b.movie_id;
```

Use RIGHT JOIN when you want to keep *all rows* from the right table, even if unmatched.

### 🔷 Full Join
A **FULL JOIN** (or **FULL OUTER JOIN**) returns all rows from **both tables**. If there’s no match in one of the tables, the result will have NULLs for the missing side.

```sql
SELECT m.column, b.column
FROM movies m
FULL JOIN boxoffice b
  ON m.id = b.movie_id;
```

Use FULL JOIN when you need to see *all possible rows* from both tables, matched where possible.

# Summary Table of Joins
| **Join Type** | **Includes unmatched rows from…** |
|:-:|:-:|
| **INNER JOIN** | Neither table (only matched rows) |
| **LEFT JOIN** | Left table |
| **RIGHT JOIN** | Right table |
| **FULL JOIN** | Both tables |

💡 **Tip:** You can remember it like this:
* INNER = Intersection
* LEFT = Left + matches
* RIGHT = Right + matches
* FULL = Union (everything)

---

#sql