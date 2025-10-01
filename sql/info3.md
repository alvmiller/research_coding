# SQL : Examples

## Common Table Expressions (CTE)
```sql
SELECT
columns
FROM table1
INNER JOIN (
SELECT columns
FROM table2
WHERE conditions
) t2
WHERE conditions;
```
the same as
```sql
WITH t2 AS (
SELECT columns
FROM table2
WHERE conditions
)
SELECT columns
FROM table1
INNER JOIN t2
WHERE conditions;
```

## Window Functions
```
You wanted to see the total amount of orders per day. You can use a SUM and a GROUP BY
```
```sql
SELECT order_date,
SUM(order_amount)
FROM orders
GROUP BY order_date;
```
```
This works, but using the SUM and GROUP BY in this way means
that the records you display must match the way that the data is calculated.
You show one row for each date, and you calculate the sum for that date.
```
```
What if you wanted to see all orders and the sum for each date?
Or, you wanted to see a running total: the sum of orders so far for that date?
You can do this with window functions.
A window function is a way to write an SQL function that lets you perform
a calculation on a range of rows that's different to how you display the rows.
```
```sql
SELECT id,
order_date,
order_amount,
SUM(order_amount) OVER (
PARTITION BY order_date
ORDER BY order_id
)
FROM orders;
```
```
The OVER clause means that the function is calculated over a range of rows,
and not the entire set of results.
The PARTITION BY clause refers to the column that's used to define the range of rows.
In this example, all records with the same order_date are used for calculating the sum.
The ORDER BY determines which records come first in the running total calculation.
In this example, it's by order_id.
This query will give you the results including the running total.
So, if you need to calculate data using a different range to what is being displayed,
you can use a window function. If it's something you were going to do in application code,
consider using a window function to take advantage of the database.
```

## Hierarchical Queries
```
A hierarchical query, or recursive query, is where you write a query that links one record
with other records in the same table.
This allows you to work with a table that stores different records where a record
is a parent of another record.
Some common examples are:
- employees and managers, where the manager is an employee
- product categories and subcategories, which can have many levels
Rather than having many tables for each level, you can store all of your data
in a single table and have a column that refers to the primary key of another record in the same table.
You can then write a query to show the data you need.
For example, you may have an employee table that has
a column called manager_id that refers to that employee's manager.
This allows many levels of employees and managers. Sarah has a manager_id of NULL
indicating she is at the head of the organization.

You can use SQL to write a recursive query or hierarchical query to show the entire organization chart.
```
```sql
WITH empdata AS (
(SELECT employee_id, first_name, manager_id, 1 AS level
FROM employee
WHERE employee_id = 1)
UNION ALL
(SELECT this.employee_id, this.first_name, this.manager_id, prior.level + 1
FROM empdata prior
INNER JOIN employee this ON this.manager_id = prior.employee_id)
)
SELECT e.employee_id, e.first_name, e.manager_id, e.level
FROM empdata e
ORDER BY e.level;
```
```
This query uses a Common Table Expression to get a list of employees and their managers.
Within the CTE, called empdata, we select the employee with an id of 1,
which is the top of the organization.
We then combine this with all of the other employees, by selecting from the same query
defined in the WITH clause, joining on the manager id and employee id.
This ability to refer to a CTE from within the same CTE is what makes the query a recursive query.
If you're using Oracle or SQL Server, this will work. If you're using MySQL or Postgres,
you'll need to add the word RECURSIVE after the WITH keyword to get it to work:
WITH RECURSIVE empdata AS ( …
You will then display a list of employees and their level in the organization.
You can also do some interesting things with this query, such as:
- adding the name of a person's manager
- indenting their name a number of spaces depending on a person's level, to make it look like a tree
So, a recursive or hierarchical query uses a range of advanced techniques
such as Common Table Expressions, Unions, and self-joins
to get the result you want in a specific type of database design.
```























