# Dropping Tables
In some situations, you may want to remove an entire table, including both its data and its schema. 
This is done with the `DROP TABLE` statement. 
Unlike `DELETE`, which removes rows but leaves the table structure intact, `DROP TABLE` removes the table **completely** from the database.

## Basic Syntax
### DROP TABLE IF EXISTS mytable;
* Removes the table mytable and all of its data and metadata.
* If the table does not exist:
  * Without IF EXISTS, the database throws an error.
  * With IF EXISTS, the statement silently does nothing if the table is missing.
⠀
## Important Considerations

* If any other table has a dependency on the table you’re dropping (such as a FOREIGN KEY relationship), you must either:
  * Remove or update the dependent rows in those tables first.
  * Or drop the dependent tables as well.
* Dropping a table is irreversible unless you have a backup. Use with caution, especially in production databases.
⠀
## Summary Table
| **Statement** | **Effect** |
|:-:|:-:|
| DELETE FROM mytable | Removes all rows but keeps the table and its schema. |
| DROP TABLE mytable | Removes both the table data and the table schema. |

---

#sql