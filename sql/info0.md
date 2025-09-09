# SQL

## Find string with maximum/minimum length

- https://stackoverflow.com/questions/21885267/how-to-find-longest-string-in-the-table-column-data
- https://www.geeksforgeeks.org/sql/sql-query-to-find-shortest-longest-string-from-a-column-of-a-table/
- https://tableplus.com/blog/2019/09/select-rows-with-longest-string-value-sql.html

```sql
SELECT TOP 1 * FROM friends
WHERE
len(firstName) = 
(SELECT min(len(firstName)) FROM friends);

SELECT TOP 1 *  FROM friends
WHERE
len(firstName) = 
(SELECT min(len(firstName)) FROM friends)
ORDER BY firstname;

SELECT TOP 1 * FROMfriends
WHERE
len(firstName) = 
(SELECT max(len(firstName)) FROMfriends);

SELECT TOP 1* FROM friends
WHERE
len(firstName) = 
(SELECT max(len(firstName)) FROM friends) 
ORDER BY firstName;

SELECT *
FROM table_name
ORDER BY LENGTH(col_name) DESC
LIMIT 1;

SELECT *
FROM table_name
WHERE LENGTH(col_name) = (
	SELECT MAX(LENGTH(col_name))
	FROM table_name)
LIMIT 1;

SELECT *
FROM table_name
WHERE LENGTH(col_name) = (
	SELECT MAX(LENGTH(col_name))
	FROM table_name);

SELECT *
FROM departments
WHERE LENGTH(dept_name) = (
	SELECT MAX(LENGTH(dept_name))
	FROM departments);
```

## Some windows functions

- https://sqlite.org/windowfunctions.html
- https://datalemur.com/sql-tutorial/sql-aggregate-window-functions
- https://www.postgresql.org/docs/current/tutorial-window.html
- https://cloud.google.com/bigquery/docs/reference/standard-sql/window-function-calls
- https://medium.com/@manutej/mastering-sql-window-functions-guide-e6dc17eb1995
- https://www.datacamp.com/cheat-sheet/sql-window-functions-cheat-sheet
- https://www.geeksforgeeks.org/sql/window-functions-in-sql/
- https://mode.com/sql-tutorial/sql-window-functions

```sql
-- calculate the running total of SaleAmount for each row ordered by SaleDate
-- new column called RunningTotal
-- (RunningTotal.curr = SaleAmount.prev + SaleAmount.curr)
SELECT 
  SaleID, 
  Salesperson, 
  SaleAmount, 
  SaleDate, 
  SUM(SaleAmount) OVER (ORDER BY SaleDate) AS RunningTotal
FROM Sales;
```

## Having

- https://www.coginiti.co/tutorials/intermediate/sql-having/
- https://datalemur.com/sql-tutorial/sql-having
- https://www.w3schools.com/mysql/mysql_having.asp
- https://dbschema.com/blog/tutorials/sql-having-clause/?srsltid=AfmBOoq_yWUlPQEPPNU97dK5ABwKY7_2ioqlO7wd53jYYzDXJhkLLPqm
- https://www.datacamp.com/tutorial/group-by-having-clause-sql
- https://learn.microsoft.com/en-us/sql/t-sql/queries/select-having-transact-sql?view=sql-server-ver17
- https://hightouch.com/sql-dictionary/sql-having
- https://www.programiz.com/sql/having
- https://www.geeksforgeeks.org/sql/sql-having-clause-with-examples/
- https://www.w3schools.com/sql/sql_having.asp

```sql
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5
ORDER BY COUNT(CustomerID) DESC;
```

## Many to Many

- https://sqlmodel.tiangolo.com/tutorial/many-to-many/
- https://docs.vultr.com/using-many-to-many-sql-relationships-and-intermediate-tables
- https://www.geeksforgeeks.org/sql/relationships-in-sql-one-to-one-one-to-many-many-to-many/
- https://docs.fintechos.com/Studio/24.1/UserGuide/Content/EvolutiveDataCore/DataModelExplorer/nnEntityRelationships.htm
- https://five.co/blog/how-to-create-many-to-many-relationships-in-sql/

```sql
-- 1. One-to-One Relationship
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    username VARCHAR(50));
CREATE TABLE user_profiles (
    profile_id INT PRIMARY KEY,
    user_id INT UNIQUE,
    profile_data VARCHAR(255),
    FOREIGN KEY (user_id) REFERENCES users(user_id));

-- 2. One-to-Many Relationship
CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(50));
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(department_id));

-- 3. Many-to-Many Relationship
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(50));
CREATE TABLE courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(50));
CREATE TABLE student_courses (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(student_id),
    FOREIGN KEY (course_id) REFERENCES courses(course_id));

-- 4. Many-to-One Relationship
CREATE TABLE Teachers (
    teacher_id INT PRIMARY KEY,
    first_name VARCHAR(255),
    last_name VARCHAR(255)
);
CREATE TABLE Courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(255),
    teacher_id INT,
    FOREIGN KEY (teacher_id) REFERENCES Teachers(teacher_id)
);
```

## Anti Join

- https://mode.com/blog/anti-join-examples
- https://www.geeksforgeeks.org/difference-between-anti-join-and-semi-join/
- https://www.crunchydata.com/blog/rise-of-the-anti-join
- https://www.navicat.com/en/company/aboutus/blog/2785-the-sql-anti-join.html
- https://www.jooq.org/doc/latest/manual/sql-building/table-expressions/joined-tables/join-type-anti/
- https://www.geeksforgeeks.org/dbms/difference-between-anti-join-and-semi-join/
- https://medium.com/@ritusantra/anti-join-semi-join-in-sql-077582f67ea8
- https://sonra.io/sql-anti-patterns-outer-joins-anti-joins/

```sql
SELECT A.i FROM A
WHERE A.i NOT IN (SELECT B.i FROM B);
  
SELECT A.i FROM A
EXCEPT
SELECT B.i FROM B;

SELECT A.i FROM A
WHERE NOT EXISTS (SELECT B.i FROM B WHERE A.i = B.i);

SELECT A.i FROM A
LEFT JOIN B ON (A.i = B.i)
WHERE B.i IS NULL;
```

# SQL : COUNT

- https://www.w3schools.com/sql/sql_count.asp
- https://www.geeksforgeeks.org/sql/sql-count-function/
- https://www.datacamp.com/tutorial/count-sql-function
- https://www.programiz.com/sql/count
- https://www.ionos.com/digitalguide/server/configuration/sql-count/

```sql
SELECT COUNT(*)
FROM Products;

SELECT COUNT(ProductName)
FROM Products;

SELECT COUNT(DISTINCT Price)
FROM Products;
```








