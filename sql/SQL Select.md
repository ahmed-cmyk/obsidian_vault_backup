# SQL Select

Retrieving all columns:

```sql
SELECT * 
FROM mytable;
```

Retrieving 2 columns:

```sql
SELECT column, another_column, …
FROM mytable;
```

## Constraints

To filter the results returned we need to use a `WHERE` clause in the query.

Syntax:

```sql
SELECT column, another_column, …
FROM mytable
WHERE condition
    AND/OR another_condition
    AND/OR …;
```

### Useful Operators

| Operator | Condition | SQL Example |
|:-:|---|---|
| =, !=, <, <=, >, >= | Standard numerical operators | col_name **!=** 4 |
| BETWEEN … AND … | Number is within range of two values (inclusive) | col_name **BETWEEN** 1.5 **AND** 10.5 |
| NOT BETWEEN … AND … | Number is not within range of two values (inclusive) | col_name **NOT BETWEEN** 1 **AND** 10 |
| IN (…) | Number exists in a list | col_name **IN** (2, 4, 6) |
| NOT IN (…) | Number does not exist in a list | col_name **NOT IN** (1, 3, 5) |

> **NOTE:** In addition to making the results more manageable to understand, writing clauses to constrain the set of rows returned also allows the query to run faster due to the reduction in unnecessary data being returned.

More operators:

| Operator | Condition | Example |
|:-:|---|---|
| = | Case sensitive exact string comparison (**notice the single equals**) | col_name **=** "abc" |
| != or <> | Case sensitive exact string inequality comparison | col_name **!=** "abcd" |
| LIKE | Case insensitive exact string comparison | col_name **LIKE** "ABC" |
| NOT LIKE | Case insensitive exact string inequality comparison | col_name **NOT LIKE** "ABCD" |
| % | Used anywhere in a string to match a sequence of zero or more characters (only with LIKE or NOT LIKE) | col_name **LIKE** "%AT%"<br>(matches "~AT~", "~AT~TIC", "C~AT~" or even "B~AT~S") |
| _ | Used anywhere in a string to match a single character (only with LIKE or NOT LIKE) | col_name **LIKE** "AN_"<br>(matches "~AN~D", but not "~AN~") |
| IN (…) | String exists in a list | col_name **IN** ("A", "B", "C") |
| NOT IN (…) | String does not exist in a list | col_name **NOT IN** ("D", "E", "F") |

> **NOTE:** All strings must be quoted so that the query parser can distinguish words in the string from SQL keywords.

> **NOTE:** We should note that while most database implementations are quite efficient when using these operators, full-text search is best left to dedicated libraries like [Apache Lucene](http://lucene.apache.org/) or [Sphinx](http://sphinxsearch.com/). These libraries are designed specifically to do full text search, and as a result are more efficient and can support a wider variety of search features including internationalization and advanced queries.

## Avoiding Duplicates

SQL provides a convenient way to discard rows that have a duplicate column value by using the `DISTINCT` keyword.

```sql
SELECT DISTINCT column, another_column, …
FROM mytable
WHERE condition(s);
```

## Ordering Results

SQL provides a way to sort your results by a given column in ascending or descending order using the **ORDER BY** clause.

```sql
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC;
```

## Limiting Results

```sql
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

---

#sql
