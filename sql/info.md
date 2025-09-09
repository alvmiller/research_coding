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

# DBMS : Normal Forms

- https://www.freecodecamp.org/news/database-normalization-1nf-2nf-3nf-table-examples/
- https://www.datacamp.com/tutorial/normalization-in-sql
- https://popsql.com/blog/normalization-in-sql
- https://www.geeksforgeeks.org/dbms/normal-forms-in-dbms/
- https://www.sqlservercentral.com/articles/database-normalization-in-sql-with-examples
- https://www.digitalocean.com/community/tutorials/database-normalization
- https://www.simplilearn.com/tutorials/sql-tutorial/what-is-normalization-in-sql
- https://www.studytonight.com/dbms/database-normalization.php
- https://dotnetfullstackdev.medium.com/normalization-in-ms-sql-simple-terms-and-examples-fc85196972d7

> The main purpose of database normalization is to avoid complexities, eliminate duplicates, and organize data in a consistent way. In normalization, the data is divided into several tables linked together with relationships.
>
> A primary key is a column that uniquely identifies the rows of data in that table. It’s a unique identifier such as an employee ID, student ID, voter’s identification number (VIN), and so on.
> A foreign key is a field that relates to the primary key in another table.
> A composite key is just like a primary key, but instead of having a column, it has multiple columns.

> The First Normal Form – 1NF
> For a table to be in the first normal form, it must meet the following criteria:
> - a single cell must not hold more than one value (atomicity)
> - there must be a primary key for identification
> - no duplicated rows or columns
> - each column must have only one value for each row in the table
>
> The Second Normal Form – 2NF
> The 1NF only eliminates repeating groups, not redundancy. That’s why there is 2NF.
> A table is said to be in 2NF if it meets the following criteria:
> - it’s already in 1NF
> - has no partial dependency. That is, all non-key attributes are fully dependent on a primary key.
>
>   The Third Normal Form – 3NF
> When a table is in 2NF, it eliminates repeating groups and redundancy, but it does not eliminate transitive partial dependency.
> This means a non-prime attribute (an attribute that is not part of the candidate’s key) is dependent on another non-prime attribute. This is what the third normal form (3NF) eliminates.
> So, for a table to be in 3NF, it must:
> - be in 2NF
> - have no transitive partial dependency.

![Image!](https://media.datacamp.com/cms/google/mmc6lomyxhnjn1b5em5oikuklwbyfisolv07jhdghsn03o5xafsvi4rbstojma2qwfhomadukokwizktqxhrta-x5seq8wxm9wxjkemt0vovjy1m_x1kxpn_xvyvotpv0u5lt_6gqrastqeaow9boae.png "Image")
![Image!](https://www.sqlservercentral.com/wp-content/uploads/2020/02/img_5e365b819bde8.png "Image")
![Image!](https://www.sqlservercentral.com/wp-content/uploads/2020/02/img_5e37abd765696.png "Image")
![Image!](https://www.sqlservercentral.com/wp-content/uploads/2020/02/img_5e37ab33158c5.png "Image")
![Image!](https://www.simplilearn.com/ice9/free_resources_article_thumb/normalizationinsql_13.png "Image")
![Image!](https://www.simplilearn.com/ice9/free_resources_article_thumb/normalizationinsql_14.png "Image")

```sql
-- Unnormalized structure (not in 1NF)
CREATE TABLE Purchases (
    CustomerID INT,
    CustomerName VARCHAR(100),
    PurchasedProducts VARCHAR(255) -- Comma-separated values
);

-- Normalized 1NF structure
CREATE TABLE CustomerProducts (
    CustomerID INT,
    CustomerName VARCHAR(100),
    Product VARCHAR(100)
);

-- Sample data for CustomerProducts table (1NF)
INSERT INTO CustomerProducts (CustomerID, CustomerName, Product) VALUES
(101, 'John Doe', 'Laptop'),
(101, 'John Doe', 'Mouse'),
(102, 'Jane Smith', 'Tablet'),
(103, 'Alice Brown', 'Keyboard'),
(103, 'Alice Brown', 'Monitor'),
(103, 'Alice Brown', 'Pen');
```

```sql
-- Orders table after 2NF
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    Product VARCHAR(100)
);

-- Customers table after 2NF
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    CustomerName VARCHAR(100)
);

-- Example foreign key constraint
ALTER TABLE Orders
ADD FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID);

-- Sample data for Customers and Orders (2NF)
INSERT INTO Customers (CustomerID, CustomerName) VALUES
(101, 'John Doe'),
(102, 'Jane Smith');

INSERT INTO Orders (OrderID, CustomerID, Product) VALUES
(201, 101, 'Laptop'),
(202, 101, 'Mouse'),
(203, 102, 'Tablet');
```

```sql
-- Sample data for Products and Suppliers (3NF)
INSERT INTO Suppliers (SupplierID, SupplierName) VALUES
(401, 'HP'),
(402, 'Logitech'),
(403, 'Apple');

INSERT INTO Products (ProductID, ProductName, SupplierID) VALUES
(301, 'Laptop', 401),
(302, 'Mouse', 402),
(303, 'Tablet', 403);

INSERT INTO Orders (OrderID, CustomerID, ProductID) VALUES
(201, 101, 301),
(202, 101, 302),
(203, 102, 303);
```

```sql
-- Original table (not in BCNF)
CREATE TABLE StudentCoursesWithInstructor (
    StudentID INT,
    Course VARCHAR(100),
    Instructor VARCHAR(100)
);

-- Normalized BCNF tables

-- Table tracking which students are in which courses
CREATE TABLE StudentCourses (
    StudentID INT,
    Course VARCHAR(100),
    PRIMARY KEY (StudentID, Course)
);

-- Table mapping each course to an instructor
CREATE TABLE CourseInstructors (
    Course VARCHAR(100) PRIMARY KEY,
    Instructor VARCHAR(100)
);

-- Sample data for BCNF decomposition
INSERT INTO StudentCourses (StudentID, Course) VALUES
(1, 'Math'),
(2, 'Math'),
(3, 'History'),
(4, 'History');

INSERT INTO CourseInstructors (Course, Instructor) VALUES
('Math', 'Dr. Smith'),
('History', 'Dr. Jones');

-- --

-- Example: Creating separate tables for normalization
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    CustomerName VARCHAR(100)
);

CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    Product VARCHAR(100),
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);
```

![Image!](https://doimages.nyc3.cdn.digitaloceanspaces.com/Temp-Vinayak/Database%20Normalization.png "Image")
![Image!](https://doimages.nyc3.cdn.digitaloceanspaces.com/Temp-Vinayak/2nf%20to%203nf.png "Image")

# SQL : Examples

- https://www.datacamp.com/blog/top-sql-interview-questions-and-answers-for-beginners-and-intermediate-practitioners
- https://www.geeksforgeeks.org/sql/sql-interview-questions/
- https://www.interviewbit.com/sql-interview-questions/
- https://leetcode.com/studyplan/top-sql-50/
- https://datalemur.com/questions
- https://roadmap.sh/questions/sql-queries
- https://www.geeksforgeeks.org/sql/sql-exercises/

# SQL : CTE

- https://www.geeksforgeeks.org/sql/cte-in-sql/
- https://www.datacamp.com/tutorial/cte-sql

> In SQL, a Common Table Expression (CTE) is an essential tool for simplifying complex queries and making them more readable. By defining temporary result sets that can be referenced multiple times, a CTE in SQL allows developers to break down complicated logic into manageable parts.
> Uses of CTEs
> - Breaking down complex queries into smaller, reusable components.
> - Improving readability and modularity by separating the logic.
> - Enabling recursive operations for hierarchical data.

```sql
WITH cte_name AS (
    SELECT query
)
SELECT *
FROM cte_name;
```

# SQL : Indexes

- https://www.geeksforgeeks.org/sql/sql-indexes/
- https://www.atlassian.com/data/sql/how-indexing-works
- https://medium.com/@sofiasondh/what-is-index-in-sql-142c50983328
- https://www.datacamp.com/tutorial/sql-server-index
- https://www.linkedin.com/pulse/mastering-sql-indexing-beginners-guide-adegboyega-aare-qimmf
- https://www.zen8labs.com/insights/development/sql/advanced-types-of-indexing-strategies-in-sql/
- https://reliasoftware.com/blog/types-of-indexes-in-sql
- https://www.linkedin.com/pulse/all-types-indexes-sql-l%C3%A2m-quang-vinh-vkssc
- https://www.sqlservertutorial.net/sql-server-indexes/sql-server-create-index/
- https://www.edureka.co/blog/index-in-sql/
- https://www.boardinfinity.com/blog/index-in-sql/
- https://learn.microsoft.com/en-us/sql/relational-databases/sql-server-index-design-guide?view=sql-server-ver17
- https://www.dbload.com/articles/indexes-in-mssql-and-mysql.htm

> Indexes in SQL are special data structures that improve speed of data retrieval operations. They work like a quick lookup table for the database instead of scanning the entire table row by row, database can use the index to directly locate the required rows.
> They play a crucial role in improving performance and efficiency in databases as it:
> - Speed up queries (SELECT, JOIN, WHERE, ORDER BY).
> - Reduce disk I/O and improve efficiency in large tables.
> - Ensure data integrity with unique indexes.
> - Must be used wisely as too many indexes can slow down INSERT, UPDATE and DELETE.
> Note: Primary Key and Unique constraints automatically create indexes.

```sql
CREATE INDEX index ON TABLE column;
```

> There are two types of databases indexes:
> - Clustered
> - Non-clustered

![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/0*xNMAVekRn935Dwo_ "Image")
![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*lJDXsBsBYuvCosWwIBhYHQ.png "Image")
![Image!](https://api.reliasoftware.com/uploads/types_of_indexes_in_sql_af5fdf6445.webp "Image")
![Image!](https://api.reliasoftware.com/uploads/clustered_indexes_7a6199b694.webp "Image")
![Image!](https://api.reliasoftware.com/uploads/non_clustered_indexes_f6ae5210c2.webp "Image")
![Image!](https://api.reliasoftware.com/uploads/composite_indexes_27e20950e9.webp "Image")
![Image!](https://www.sqlservertutorial.net/wp-content/uploads/SQL-Server-nonclustered-index.png "Image")

# SQL : Duplicates

- https://www.atlassian.com/data/sql/how-to-find-duplicate-values-in-a-sql-table
- https://www.w3schools.com/sql/sql_distinct.asp

```sql
SELECT username, email, COUNT(*)
FROM users
GROUP BY username, email
HAVING COUNT(*) > 1
```

```sql
SELECT a.*
FROM users a
JOIN (SELECT username, email, COUNT(*)
FROM users 
GROUP BY username, email
HAVING count(*) > 1 ) b
ON a.username = b.username
AND a.email = b.email
ORDER BY a.email
```






