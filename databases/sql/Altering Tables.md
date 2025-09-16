# Altering Tables

As your data and requirements evolve, you may need to modify the structure of your tables. SQL provides the ALTER TABLE statement to update the table schema by adding, removing, or modifying columns and constraints.

## Adding Columns

You can add new columns to an existing table. 
**When adding a column:**
* Specify the column name, data type, optional constraints, and an optional default value.
* In some databases (like MySQL), you can also specify the position of the column (FIRST or AFTER another_column), though this is not part of the SQL standard.

**Syntax:**

```sql
ALTER TABLE mytable
ADD column DataType OptionalTableConstraint DEFAULT default_value;
```

* Existing rows will have the default value (or NULL if no default is specified) for the new column.
⠀
## Removing Columns

You can also drop (delete) columns from a table.
* Not all databases support this directly. For example, SQLite does **not** allow dropping columns.
* In databases without support, you may need to create a new table without the column and migrate the data.

**Syntax:**

```sql
ALTER TABLE mytable
DROP column_to_be_deleted;
```

## Renaming the Table
You can rename a table using the RENAME TO clause.

**Syntax:**

```sql
ALTER TABLE mytable
RENAME TO new_table_name;
```

## Other Changes

**Beyond adding, removing, and renaming, some databases support additional operations such as:**

* Modifying column types
* Renaming columns
* Adding or removing constraints
* Changing default values

> **NOTE:** These features can vary significantly between database systems, so always check your specific database’s documentation before making changes. See docs for: MySQL, PostgreSQL, SQLite, SQL Server.

---

#sql