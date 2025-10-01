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

## Ranking Results
```
You can calculate the rank of a record based on one of its values
by using the RANK function as a window function.
For example, this query calculates the rank of an order based on its order amount.

This will show all orders, and the RANK column will show 1 for the highest order amount,
2 for the second highest, and so on. It shows the same values regardless
of how you want to order the overall result.
```
```sql
SELECT
order_id,
order_date,
customer_id
order_amount,
RANK() OVER (ORDER BY order_amount DESC)
FROM orders;
```

## Difference in Rows
```
Another example is using a window function called LAG
to find the difference of a value between two rows.
Without window functions, this would be difficult, but fortunately,
we can use LAG as a window function to find this out.
Let's say we want to display the total of orders for each month,
and then show the difference of this total compared to the previous month.

Using the LAG function lets you refer to the previous row,
which is done using the ORDER BY clause within the OVER clause.
```
```sql
SELECT
order_month,
order_total,
order_total - LAG(order_total) OVER (ORDER BY order_month) AS diff_last_month
FROM orders;
```

## Conditional Sum
```
You can use the SUM function to add up number values. It's often used to add everything in a column.
However, it can also be used to add numbers based on certain criteria.
Let's say we wanted to show the total amount of orders in a month but separated based on paid status.

You can use SUM along with CASE to create this kind of result where you only SUM values
that meet certain criteria.
```
```sql
SELECT
o.order_month,
SUM(
CASE WHEN p.status = 'Paid' THEN o.order_amount ELSE 0 END
) AS total_amount_paid,
SUM(
CASE WHEN p.status = 'Pending' THEN o.order_amount ELSE 0 END
) AS total_amount_pending,
SUM(
CASE WHEN p.status = 'Default' THEN o.order_amount ELSE 0 END
) AS total_amount_default
FROM orders o
INNER JOIN payment_status p ON o.payment_status_id = p.id;
```

# SQL : Examples

## Ranking Rows Based on a Specific Ordering Criteria
```
Sometimes we need to create a SQL query to show a ranking of rows based on a specific order criteria.
In this example query, we will show a list of all employees ordered by salary (highest salary first).
The report will include the position of each employee in the ranking.

In the above query, we use the function RANK().
It is a window function that returns each row’s position in the result set,
based on the order defined in the OVER clause  (1 for the highest salary,
2 for the second-highest, and so on). We need to use an ORDER BY ranking clause
at the end of the query to indicate the order on which the result set will be shown.
```
```sql
SELECT 
  employee_id,
  last_name,
  first_name,
  salary,
  RANK() OVER (ORDER BY salary DESC) as ranking
FROM employee
ORDER BY ranking
```

## List The First 5 Rows of a Result Set
```
The next SQL query creates a report with the employee data for the top 5 salaries in the company.
This kind of report must be ordered based on a given criteria;
in our example, the order criteria will again be salary DESC:

The WITH clause in the previous query creates a CTE called employee_ranking,
which is a kind of virtual table that’s consumed in the main query.
The subquery in the CTE uses the function RANK() to obtain the position of each row in the ranking.
The clause OVER (ORDER BY salary DESC) indicates how the RANK() value must be calculated.
The RANK() function for the row with the highest salary will return 1, and so on.
Finally, in the WHERE of the main query we ask for those rows with a ranking value smaller or equal than 5.
This lets us obtain only the top 5 rows by ranking value.
Again, we use an ORDER BY clause to show the result set, which is ordered by rank ascending.
```
```sql
WITH employee_ranking AS (
  SELECT
    employee_id,
    last_name,
    first_name,
    salary,
    RANK() OVER (ORDER BY salary DESC) as ranking
  FROM employee
)
SELECT
  employee_id,
  last_name,
  first_name,
  salary
FROM employee_ranking
WHERE ranking <= 5
ORDER BY ranking
```

## List The Second Highest Row of a Result Set
```
Let’s suppose we’d like to obtain the data of the employee with the second highest salary in the company.
We can apply a similar approach to our previous query:

The WHERE condition ranking = 2 is used to filter the rows with the salary in position 2.
Note that we can have more than one employee in position 2 if they have the same salary.
At this point, it is important to understand the behavior of the RANK() function
as well as other available functions like ROW_NUMBER() and DENSE_RANK().
```
```sql
WITH employee_ranking AS (
  SELECT
    employee_id,
    last_name,
    first_name,
    salary,
    RANK() OVER (ORDER BY salary DESC) as ranking
  FROM employee
)
SELECT
  employee_id,
  last_name,
  first_name,
  salary
FROM employee_ranking
WHERE ranking = 2
```

## List All Combinations of Rows from Two Tables
```sql
SELECT
  grain.product_name,
  box_size.description,
  grain.price_per_pound * box_size.box_weight
FROM product
CROSS JOIN box_sizes
```

## List the Last 25% Rows in a Result Set
```
As with the previous query, in this example we will use NTILE(4)
to divide the result set into 4 subsets; each subset will have 25% of the total result set.
Using the NTILE() function, we will generate a column called ntile with the values 1, 2, 3, and 4:

The WHERE ntile = 4 condition filters only the rows in the last quarter of the report.
The last clause ORDER BY salary orders the result set to be returned by the query,
while OVER (ORDER BY salary) orders the rows before dividing them into 4 subsets using NTILE(4).
```
```sql
WITH employee_ranking AS (
  SELECT
    employee_id,
    last_name,
    first_name,
    salary,
    NTILE(4) OVER (ORDER BY salary) as ntile
  FROM employee
)
SELECT
  employee_id,
  last_name,
  first_name,
  salary
FROM employee_ranking
WHERE ntile = 4
ORDER BY salary
```

## Join a Table to Itself
```sql
SELECT 
  e1.first_name ||' '|| e1.last_name AS manager_name,
  e2.first_name ||' '|| e2.last_name AS employee_name
FROM employee e1
JOIN employee e2
ON e1.employee_id = e2.manager_id
```

## Show All Rows with an Above-Average Value
```sql
SELECT
  first_name,
  last_name,
  salary
FROM employee 
WHERE salary > ( SELECT AVG(salary) FROM employee )
```

## Employees with Salaries Higher Than Their Departmental Average
```sql
SELECT
  first_name,
  last_name,
  salary
FROM employee e1
WHERE salary >
    (SELECT AVG(salary)
     FROM employee e2
     WHERE e1.departmet_id = e2.department_id)
```

## Obtain All Rows Where a Value Is in a Subquery Result
```sql
SELECT 
  first_name,
  last_name
FROM employee e1
WHERE department_id IN (
   SELECT department_id
   FROM department
   WHERE manager_name=‘John Smith’)
```

## Find Duplicate Rows in SQL
```sql
SELECT 
  employee_id,
  last_name,
  first_name,
  dept_id,
  manager_id,
  salary
FROM employee
GROUP BY   
  employee_id,
  last_name,
  first_name,
  dept_id,
  manager_id,
  salary
HAVING COUNT(*) > 1
```

## Count Duplicate Rows
```sql
SELECT 
  employee_id,
  last_name,
  first_name,
  dept_id,
  manager_id,
  salary,
  COUNT(*) AS number_of_rows
FROM employee
GROUP BY
  employee_id,
  last_name,
  first_name,
  dept_id,
  manager_id,
  salary
HAVING COUNT(*) > 1
```

## Find Common Records Between Tables
```sql
SELECT
  last_name,
  first_name
FROM employee
INTERSECT
SELECT
  last_name,
  first_name
FROM employee_2020_jan
```

## Grouping Data with ROLLUP
```
The GROUP BY clause in SQL is used to aggregate rows in groups
and apply functions to all the rows in the group, returning a single result value.
For example, if we want to obtain a report with the total salary amount per department
and expertise level, we can do the following query:
```
```sql
SELECT 
  dept_id,
  expertise,
  SUM(salary) total_salary
FROM    employee
GROUP BY dept_id, expertise
```
```
The GROUP BY has the optional clause ROLLUP, which allows it to include additional groupings in one query.
Adding the ROLLUP clause to our example could give us the total sum of salaries for each department
(no matter what expertise level the employee has)
and the total sum of salaries for the whole table
(no matter the employee’s department and expertise level). The modified query is:

The rows in the result set with a NULL are the extra rows added by the ROLLUP clause.
A NULL value in the column expertise means a group of rows for a specific value
of dept_id but without a specific expertise value. In other words,
it is the total amount of salaries for each dept_id. In the same way,
the last row of the result having a NULL for columns dept_id and expertise means
the grand total for all departments in the company.
```
```sql
SELECT
  dept_id,
  expertise,
  SUM(salary) total_salary
FROM employee
GROUP BY ROLLUP (dept_id, expertise)
```

## Conditional Summation
```sql
SELECT
  SUM (CASE
    WHEN dept_id IN ('SALES','HUMAN RESOURCES')
    THEN salary
    ELSE 0 END) AS total_salary_sales_and_hr,
  SUM (CASE
    WHEN dept_id IN ('IT','SUPPORT')
    THEN salary
    ELSE 0 END) AS total_salary_it_and_support
FROM employee
```

## Group Rows by a Range
```sql
SELECT
  CASE
    WHEN salary <= 750000 THEN 'low'
    WHEN salary > 750000 AND salary <= 100000 THEN 'medium'
    WHEN salary > 100000 THEN 'high'
  END AS salary_category,
  COUNT(*) AS number_of_employees
FROM    employee
GROUP BY
  CASE
    WHEN salary <= 750000 THEN 'low'
    WHEN salary > 750000 AND salary <= 100000 THEN 'medium'
    WHEN salary > 100000 THEN 'high'
END
```

## Compute a Running Total
```
When you have a table that stores any daily metric, such as a sales table
with the columns day and daily_amount, you can calculate the running total as the cumulative sum
of all previous daily_amount values. SQL provides a window function called SUM() to do just that.
In the following query, we’ll calculate the cumulative sales for each day:

The SUM() function uses the OVER() clause to define the order of the rows;
all rows previous to the current day are included in the SUM().
The first two columns day and daily_amount are values taken directly from the table sales.
The column running_total is calculated by the expression:
SUM (daily_amount) OVER (order by day)
You can clearly see how the running_total is the accumulated sum of the previous daily_amounts.
```
```sql
SELECT
  day,
  daily_amount,
  SUM (daily_amount) OVER (ORDER BY day) AS running_total
FROM sales
```

## TBD
```
TBD

TBD
```
```sql
TBD
```

## TBD
```
TBD

TBD
```
```sql
TBD
```

## TBD
```
TBD

TBD
```
```sql
TBD
```

## TBD
```
TBD

TBD
```
```sql
TBD
```

## TBD
```
TBD

TBD
```
```sql
TBD
```

## TBD
```
TBD

TBD
```
```sql
TBD
```

## TBD
```
TBD

TBD
```
```sql
TBD
```

## TBD
```
TBD

TBD
```
```sql
TBD
```

## TBD
```
TBD

TBD
```
```sql
TBD
```

## TBD
```
TBD

TBD
```
```sql
TBD
```

## TBD
```
TBD

TBD
```
```sql
TBD
```

## TBD
```
TBD

TBD
```
```sql
TBD
```

## TBD
```
TBD

TBD
```
```sql
TBD
```



















