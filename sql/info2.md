# SQL : find the nth highest value in a column of a table

```sql
SELECT value_column
FROM (
    SELECT
        value_column,
        DENSE_RANK() OVER (ORDER BY value_column DESC) as rnk
    FROM your_table
) ranked_data
WHERE rnk = N;
```
```sql
SELECT value_column
FROM your_table
ORDER BY value_column DESC
LIMIT 1 OFFSET N - 1;
```
```sql
SELECT MIN(value_column)
FROM (
    SELECT DISTINCT TOP (N) value_column
    FROM your_table
    ORDER BY value_column DESC
) AS tmp;
```

# SQL : Some examples

- https://www.kdnuggets.com/2020/11/5-tricky-sql-queries-solved.html
- https://www.geeksforgeeks.org/sql/most-common-sql-queries-that-you-should-know/
- https://learnsql.com/blog/25-advanced-sql-query-examples/
- https://popsql.com/blog/complex-sql-queries
- https://bytescout.com/blog/20-important-sql-queries.html
- https://medium.com/@jhoughton/fun-with-mysql-database-queries-c87c46a32a48

## Binary Search Tree
```
We are given a table, which is a Binary Search Tree consisting of two columns Node and Parent.
We must write a query that returns the node type ordered by the value of nodes in ascending order.
There are 3 types.
- Root — if the node is a root
- Leaf — if the node is a leaf
- Inner — if the node is neither root nor leaf.
```
![img0](https://i.ibb.co/j6mMpB1/sura-sql-tricky-3.png)
```sql
SELECT CASE
    WHEN P IS NULL THEN CONCAT(N, ' Root')
    WHEN N IN (SELECT DISTINCT P from BST) THEN CONCAT(N, ' Inner')
    ELSE CONCAT(N, ' Leaf')
    END
FROM BST
ORDER BY N asc;
```

## Users who purchased products on multiple days
```
We are given a transaction table that consists of transaction_id, user_id, transaction_date, product_id, and quantity.
We need to query the number of users who purchased products on multiple days.
(Note that a given user can purchase multiple products on a single day).
```
![img0](https://i.ibb.co/4SHQhds/sura-sql-tricky-4.png)
```sql
SELECT COUNT(user_id)
FROM
(
SELECT user_id
 FROM orders
 GROUP BY user_id
 HAVING COUNT(DISTINCT DATE(date)) > 1
) t1
```

## Create
```sql
CREATE TABLE table_name(column1 datatype, column2 datatype, … columnN datatype);
```

## Insert
```sql
INSERT INTO table_name (column1, column2, … columnN) VALUES (value1, value2, … valueN);
```

## Delete
```sql
DELETE FROM employee WHERE [condition];
```

## Update
```sql
UPDATE table_name SET column1 = value1, column2 = value2, … WHERE [condition];
```

## Viewing Only Selected Records From a Table
```
COUNT(*): Counts all rows, including those with NULL values.
COUNT(1): Counts all rows as well, but does not evaluate any column's NULL values since 1 is a constant.
```
```sql
SELECT COUNT (1) FROM table_name;
```

## Explain
```sql
EXPLAIN QUERY PLAN SELECT * FROM EMPLOYEE;
```

# SQL : Differences between DELETE and TRUNCATE

```
The DELETE command is used to delete specified rows(one or more).
While Truncate this command is used to delete all the rows from a table.

It is a DML(Data Manipulation Language) command.
While Truncate it is a DDL(Data Definition Language) command.

There may be a WHERE clause in the DELETE command in order to filter the records.
While Truncate there may not be WHERE clause in the TRUNCATE command.

In the DELETE command, a tuple is locked before removing it.
While Truncate in this command, the data page is locked before removing the table data.

The DELETE statement removes rows one at a time and records an entry in the transaction log for each deleted row.
TRUNCATE TABLE removes the data by deallocating the data pages used to store the table data and records only the page
deallocations in the transaction log.

DELETE command is slower than TRUNCATE command.
While the TRUNCATE command is faster than the DELETE command.

To use Delete you need DELETE permission on the table.
To use Truncate on a table we need at least ALTER permission on the table.

The identity of the fewer column retains the identity after using DELETE Statement on the table.
Truncate Identity the column is reset to its seed value if the table contains an identity column.

The delete can be used with indexed views.
Truncate cannot be used with indexed views.

This command can also active trigger.
Truncate command does not active trigger.

DELETE statement occupies more transaction spaces than Truncate.
Truncate statement occupies less transaction spaces than DELETE.

Delete operations can be ROLLED back.
TRUNCATE cannot be Rolled back as it causes an implicit commit.

Delete doesn't DROP the whole table. It acquires a lock on table and starts deleting the rows.
TRUNCATE first drops the table & then re-create it, which is faster than deleting individual rows.

Conclusion
In summary, DELETE, TRUNCATE, and DROP TABLE are all essential SQL commands for managing data,
but they serve different purposes and have unique characteristics.
Understanding the differences between them is important for database performance optimization.
Use DELETE when we need to selectively remove rows,
TRUNCATE when you want to quickly clear all rows from a table, and DROP TABLE when you need to remove a table entirely.
```

```
Parameter	DELETE	DROP
Basic	It removes some or all the tuples from a table.	It removes the entire schema, table, domain, or constraints from the database.
Language	Data Manipulation Language command	Data Definition Language command.
Clause	WHERE clause is mainly used along with the DELETE command.	No clause is required along with the DROP command.
Rollback	Actions performed by DELETE can be rolled back as it uses a buffer.	Actions performed by DROP can’t be rolled back because it directly works on actual data.
Space	space occupied by the table in the memory is not freed even if you delete all the tuples of the table using DELETE	It frees the table space from memory
Main Issue	Shortage of memory	Memory fragmentation
Locality of reference	Excellent	Adequate
Flexibility	Fixed-size	Resizing is possible
```























