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






















