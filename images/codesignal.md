# Introduction to SQL with an Online Shop

## Getting started with SQL for Online Shopping

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


## Learning SQL JOINS with Online Shopping 

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












