### 28 Interview questions from beginner to senior 

#### Beginner (0-2 years of experience)

**Question1: (Basic SQL data types and simple SELECT query)**

Write a SQL query that retrieves the `first_name`, `last_name`, and `email` columns from a table named `users`, where the `email` domain is “example.com”. Assume that `email` is a `VARCHAR` type.

```
SELECT first_name, last_name, email
FROM users
WHERE email LIKE "%example.com";
```

**Explanation:** The `LIKE` operator is used with a wildcard (`%`) to match any characters before “@example.com”.

**Question2: SQL JOIN and relationship**

Write a SQL query to retrieve the `order_id` and `order_date` from an `orders` table and the `product_name` from a `products` table for all orders. Assume that the `orders` table has a `product_id` foreign key that references the `product_id` in the `products` table.

```
SELECT o.order_id, o.order_date, p.product_name
FROM orders o
INNER JOIN products p ON o.product_id=p.product_id;
```

**Question3: Basic data manipulation**

Write a SQL query to update the `salary` column in the `employees` table, increasing it by 10% for all employees who work in the “Sales” department. Assume the `department` column is of type `VARCHAR`.

```
UPDATE employees
SET salary = (salary * 1.1)
WHERE department = "Sales";
```

#### Intermediate (2-5 years of exprience)

**Question4: Complex SQL queries and subqueries**

Write a SQL query to find the top 3 customers with the highest total `order_amount` from the `orders` table. Assume that each order is linked to a customer via a `customer_id` column, and the `order_amount` is a numeric column.

```
SELECT customer_id, SUM(order_amount) AS total_amount
FROM orders
GROUP BY customer_id
ORDER BY total_amount DESC
LIMIT 3;
```

**Question5: Subqueries and data integrity**


Write a SQL query to find all employees in the `employees` table whose `salary` is greater than the average salary in their department. Assume that the table has `employee_id`, `department_id`, and `salary` columns.

```
SELECT employee_id, department_id, salary 
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE department_id=employees.department_id);
```

**Explanation**: This query uses a subquery to calculate the average salary within each department. The main query then selects employees whose salary exceeds the average salary of their respective department. The use of correlated subqueries (where the subquery references a column from the outer query) is a powerful technique for comparing data within grouped contexts.





