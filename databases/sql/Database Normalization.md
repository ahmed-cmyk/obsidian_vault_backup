# Database Normalization

In the real world, entity data is broken down into pieces and stored across multiple orthological tables using a process known as **normalization**.

## Database Normalization

Database normalization is useful because it minimizes duplicate data in any single table, and allows for data in the database to grow independently of each other (ie. Types of car engines can grow independent of each type of car). As a trade-off, queries get slightly more complex since they have to be able to find data from different parts of the database, and performance issues can arise when working with many large tables.

---

#sql