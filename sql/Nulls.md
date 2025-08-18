# Nulls

In SQL, `NULL` represents **missing** or **unknown** data. It’s not the same as `0`, an empty string, or `FALSE`.

---

## Why minimize NULLs?

- Queries, constraints, and calculations require special handling when `NULL`s are present.
- Many functions behave differently with `NULL` values (e.g., `AVG`, `COUNT`, comparisons).
- Default values are often easier to work with in analysis and queries.

---

## Alternatives to NULLs

Instead of using `NULL`, you can sometimes use **appropriate default values**, such as:
- `0` for numeric columns
- `''` (empty string) for text columns
- A sentinel value (like `-1` or `'N/A'`) if meaningful

However:
> If default values would distort or misrepresent the data (e.g., skew averages, make missing data appear valid), it’s better to use `NULL`.

---

## When are NULLs unavoidable?

Sometimes you cannot avoid `NULL`s — for example:
- When performing `OUTER JOIN`s between tables with asymmetric data, unmatched rows will have `NULL` in the missing columns.
- When the data itself is genuinely unknown or incomplete.

---

## How to work with NULLs in queries

Since `NULL` is not equal to anything (not even another `NULL`), you cannot use `=` or `!=` to check for it.

Instead, use:
```sql
WHERE column IS NULL
```

or:
```sql
WHERE column IS NOT NULL
```

---

## Example Query

```sql
SELECT column, another_column, …
FROM mytable
WHERE column IS/IS NOT NULL
AND/OR another_condition
AND/OR …;
```

---

#sql