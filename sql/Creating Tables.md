# Creating Tables

When you have new entities or relationships to store in your database, you can create a new table using the CREATE TABLEstatement.

## Basic Syntax

```sql
CREATE TABLE IF NOT EXISTS mytable (
    column DataType TableConstraint DEFAULT default_value,
    another_column DataType TableConstraint DEFAULT default_value,
    …
);
```

* Defines the **table schema**, which specifies:
  * Column names
  * Data types
  * Optional constraints on each column
  * Optional default values
* If a table with the same name already exists, most databases will throw an error. To prevent this and skip creation if the table exists, use IF NOT EXISTS.
⠀
## Table Data Types

Different databases support slightly different data types, but most include numeric, string, date, and binary types.

| **Data Type** | **Description** |
|:-:|:-:|
| INTEGER, BOOLEAN | Stores whole numbers. In some databases, BOOLEAN is stored as 0 or 1. |
| FLOAT, DOUBLE, REAL | Stores decimal or fractional numbers. Choose based on precision needs. |
| CHARACTER(n), VARCHAR(n), TEXT | Stores text strings. CHARACTER and VARCHAR are limited to n characters and may be more efficient. TEXT has no fixed limit but may be slower to query. |
| DATE, DATETIME | Stores dates and/or timestamps. Useful for time-series or event data. Can be tricky to handle with time zones. |
| BLOB | Stores binary data, such as images or files. Usually opaque to the database; metadata is required to interpret it later. |

## Table Constraints

Constraints limit the kind of data that can be inserted into columns. These help maintain data integrity and enforce business rules.

| **Constraint** | **Description** |
|:-:|:-:|
| PRIMARY KEY | Ensures the column has unique values that identify rows uniquely. |
| AUTOINCREMENT | Automatically generates and increments the value for each inserted row (common for primary keys). Not supported in all databases. |
| UNIQUE | Ensures all values in the column are unique, but does not make it a primary key. |
| NOT NULL | Ensures that the column cannot have NULL values. |
| CHECK (expression) | Validates values against a custom condition (e.g., positive numbers only). |
| FOREIGN KEY | Ensures that values in this column exist as values in another table’s column, maintaining referential integrity. |

### Example of a `FOREIGN` KEY

If you have an Employees table and a Payroll table:
* Payroll.employee_id can be a FOREIGN KEY referencing Employees.id.
* This ensures that every row in Payroll corresponds to an existing employee.

⠀
# Example Table
Below is an example schema for a Movies table:

```sql
CREATE TABLE movies (
    id INTEGER PRIMARY KEY,
    title TEXT,
    director TEXT,
    year INTEGER,
    length_minutes INTEGER
);
```

This table:
* Defines five columns
* Marks id as the PRIMARY KEY
* Uses appropriate data types (TEXT, INTEGER)

---

#sql