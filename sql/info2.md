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






