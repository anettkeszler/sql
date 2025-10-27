# Introduction to SQL with an Online Shop

## 1. Getting started with SQL for Online Shopping

### Exploring Databases and Relational Data Structure

A **database** is a bench of data, where you can store, manage, and retrieve information.
A relational database stores data in tables, much like how an Excel file has different sheets for different sets of data.

**RDBMS**: relational database management systems, such as PostgreSQL, SQL Server, SQLite, and MySQL

#### SHOW TABLES:
```
SHOW TABLES;
``` 
executing this command returns a list of all tables in your current database. 

#### Semicolon and comments
- Semicolon (;):serves as the end of a statement, correct usage is crutial for command separation
- Comments: - single-line comments: -- this command lists all tables
            - multi-line comments: /* this is useful for longer explanations*/


#### DESCRIBE: 
```
DESCRIBE Products;

-- Describe the table structures
/* Output: 
+---------------+---------------+------+-----+---------+-------+
| Field         | Type          | Null | Key | Default | Extra |
+---------------+---------------+------+-----+---------+-------+
| product_id    | int           | NO   | PRI | NULL    |       |
| product_name  | varchar(255)  | NO   |     | NULL    |       |
| product_price | decimal(10,2) | NO   |     | NULL    |       |
| category_id   | int           | YES  | MUL | NULL    |       |
+---------------+---------------+------+-----+---------+-------+
*/
```

### Navigating the SQL SELECT Statement

SQL, unlike many programming languages, doesn't deal with logic or flow control; instead, it understands, manipulates, and retrieves data stored in databases in a structured manner.

#### Alias in SQL: Using AS Keyword
```
SELECT product_name AS "Product Name", product_price AS Price FROM Products;
```

Rules for Using Aliases:

- Use double quotes (") around the alias if it contains spaces or special characters.

- Aliases exist only for the duration of the query and do not alter the database schema.

### Application of WHERE clause

With the WHERE clause we'll be able to narrow down our data retrieval to only select the records that meet certain conditions.

```
SELECT * FROM Orders
WHERE YEAR(order_date) > 2020;
```
Note how we used the YEAR() function on order_date for this query, as order_date is a DATE type column.

### Mastering SELECT Statements with Logical Operators
Logical operators help us refine our SQL queries to get the data we need in a more efficient and specific way. 
- AND operator: returnes true if both listed conditions are true.
- OR operator: returnes true if either of the conditions listed is true. 
- IN
- BETWEEN
- NOT IN

```
Fetch orders from year 2021 or 2022, with order IDs between 100 and 400, where the order status was either Delivered or Canceled

SELECT * FROM orders WHERE (order_date BETWEEN '2021-01-01' AND '2022-12-31') 
   AND (order_id BETWEEN 100 AND 400)
   AND (order_status IN ("Delivered", "Canceled"));
```

### ORDER BY clause for sorting data

- ORDER BY is udes to sort the result set in ascending or descending order. The ORDER BY keyword sorts the records in ascending order by default. 
- When using the ORDER BY clause, it’s important to understand functional dependency in SQL. A functional dependency exists when one column uniquely determines another column in a table.
- If you use ORDER BY on a column that is not unique (e.g., product_name in the Products table), rows with duplicate values in that column might appear in a random or unpredictable order in the result set.
- If multiple products share the same product_name, the SQL engine may return these rows in a different order each time you run the query. This happens because there’s no explicit instruction on how to handle ties (rows with identical product_name values).
- To prevent non-deterministic sorting:
Add a secondary column to the ORDER BY clause. This ensures a consistent sort order for rows with duplicate values in the primary sort column.
Choose a secondary column that uniquely identifies rows, such as a primary key or another distinct attribute.

```
SELECT product_name, product_price 
FROM Products 
ORDER BY product_name ASC, product_id ASC;
```
- In this case: product_name is the primary sort column, 
product_id is the secondary sort column, ensuring consistent order for rows with the same product_name.


## 2. Learning SQL JOINS with Online Shopping 

- SQL JOINs are techniques to combine data from two or more tables based on a shared column between them. They help in extracting valuable information that might be distributed across multiple tables. 

When performing a JOIN between two tables, it’s essential to understand the relationship between the columns involved. 

We must ensure that the columns used in the ON clause of the JOIN define a clear relationship between the tables.

### Exploring INNER JOIN

- INNER JOIN in SQL is a clause that merges rows from two tables based on a shared column between them. 
 
 
### Diving Deeper into JOINS

- LEFT JOIN:  includes all rows from the left table, along with any matches from the right table. If there's no match, the output displays NULL for the right table's columns.

- RIGHT JOIN: ensures that every row from the right table is included in the output, with matched rows from the left table.
```
SELECT Orders.order_id, Orders.order_date, OrderItems.product_id
FROM Orders
LEFT JOIN / RIGHT JOIN OrderItems ON Orders.order_id = OrderItems.order_id;
```

**Key Difference:**

- LEFT JOIN includes all rows from Orders, showing NULL for OrderItems columns when there’s no match.
- RIGHT JOIN includes all rows from OrderItems, showing NULL for Orders columns when there’s no match.

### SQL FULL JOIN 

- FULL JOIN provides all records where there is a match in either the left table or the right one, essentially unifying the results of LEFT JOIN and RIGHT JOIN to offer a comprehensive view of your data.

```
SELECT Products.product_name, Products.product_price, OrderItems.extended_support
FROM Products
LEFT JOIN OrderItems ON Products.product_id = OrderItems.product_id
WHERE Products.category_id=1

UNION ALL

SELECT Products.product_name, Products.product_price, OrderItems.extended_support
FROM Products
RIGHT JOIN OrderItems ON Products.product_id = OrderItems.product_id
WHERE Products.product_id IS NULL AND Products.category_id=1;
```

## 3. Mastering SQL Functions and Clauses 

### Mastering the COUNT function

**COUNT** function in SQL is used to return the number of rows that match a specified condition. It can also be used without any condition to count all rows in a table. It is a simple yet powerful tool for performing quantitative analysis on data.

The **COUNT** function is commonly used in the following scenarios:

- Counting the total number of rows in a table.
- Counting the number of unique entries.
- Counting the number of entries that satisfy a particular condition.

```
SELECT COUNT(column_name) FROM table_name WHERE condition;

SELECT COUNT(*) FROM table_name;
```
COUNT(column_name): The **COUNT** function, which takes a column name as an argument.


Difference Between **COUNT(*) and COUNT(column_name)**:
- COUNT(*): Counts all rows in the table, including rows with NULL values in any column.
- COUNT(column_name): Counts only non-NULL values in the specified column.


**Example:** we want to count the number of orders that have been processed.
```
SELECT COUNT(*) 
FROM Orders 
WHERE order_status = 'Processed';

-- Output: 156, this query would count the total number of rows in the Orders table where the order_status is 'Processed'.
```
### DISTINCT 
It is a powerful keyword / clause which helps us remove duplicates and present a clean, unique list of values. 
```
SELECT DISTINCT column_name FROM table_name;
```

When using DISTINCT, it's important to remember that fetching unique values from large datasets can be time-consuming and slow down your queries. Therefore, always consider the performance implications and use DISTINCT only when necessary.

### Aggregate Functions in SQL
- **SUM** function is an aggregate operation in SQL, used to calculate the total sum of a numerical column in a database. Think of it as a mathematical operation that adds up all the numbers in a set.

```
SELECT SUM(expression) FROM table_name;
```
**SUM(expression)**: The SUM() function expects an argument to specify what to sum. The correct usage is SUM(expression), where the expression is typically a column name or a numerical value, such as SUM(column_name) to sum up values from a specific column.

```
SELECT Products.category_id, SUM(1) AS TotalItemsSold
FROM OrderItems
JOIN Products ON OrderItems.product_id = Products.product_id
GROUP BY Products.category_id;

-- Output:
-- | category_id | TotalItemsSold |
-- |-------------|----------------|
-- |           1 |            270 |
-- |           2 |             60 |
-- |           3 |            120 |
-- |           4 |             60 |
-- |           5 |             90 |
```

```SELECT Products.category_id, SUM(1) AS TotalItemsSold```: 

In this query, SUM(1) is used to count the number of items sold for each product category. This utilizes the SUM() function in a straightforward manner to aggregate the total count of items sold by category. This part of the query selects rows separately for each group according to the category_id from the Products table and calculates the total number of items sold by summing 1 for each sold item in the group.

```GROUP BY Products.category_id```: 

Finally, we use the GROUP BY clause to group the total items sold by product categories.

**Some tips for using SUM():**
- Bear in mind that SUM works with numerical data. Using it on non-numerical columns will result in errors.
- The AS keyword, as seen in the code, can make your output more readable by renaming the result of our SUM operation. Don’t forget to use it as necessary.


**Let's also see the difference between using SUM(1) and COUNT(*):**

- **SUM(1)**: Adds the value 1 for each row, effectively counting rows within each group when used with GROUP BY. It's a creative use of the SUM function.

- **COUNT(*)**: Directly counts all rows in the result set, including those with NULL values, and is typically more straightforward for counting rows.
In this lesson's context, both SUM(1) and COUNT(*) achieve the same result.

### GROUP BY clause

The **GROUP BY** clause is used in collaboration with aggregate functions such as COUNT, SUM etc., to group the result-set by one or more columns. 
```
SELECT column_name, aggregate_function(column_name) AS alias_name
FROM table_name
GROUP BY column_name;
```



```
-- TODO: Select all orders placed after the year 2021 with extended support and count the total number of such orders

-- group by order date and order by order date in descending order


SELECT YEAR(order_date) AS Year, COUNT(1) AS TotalNumberOfOrders 
FROM Orders JOIN OrderItems ON Orders.order_id=OrderItems.order_id
WHERE OrderItems.extended_support=1 AND YEAR(order_date) > 2021
GROUP BY YEAR(order_date);
```







