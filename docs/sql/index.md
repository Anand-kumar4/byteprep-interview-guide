---
title: SQL Interview Questions
description: A collection of commonly asked SQL interview questions with explanations and examples.
keywords: SQL, interview questions, joins, subquery, group by, having, duplicates
---


> ℹ️ All queries use ANSI SQL unless a specific dialect (e.g., PostgreSQL, MySQL, SQL Server) is mentioned.

# SQL Interview Questions


**🟢 Difficulty:** Easy  **📂 Topic:** Joins

## Q1. What is a LEFT ANTI JOIN? How is it different from LEFT JOIN?

A **LEFT ANTI JOIN** returns only the rows from the left table **that do not have a match** in the right table.

This is different from a LEFT JOIN, which returns **all rows** from the left table and matching rows from the right (NULL if no match).

### 📋 Table Structure & Sample Data

Assume we have the following `orders` and `customers` tables:


**orders**

| id | customer_id | order_date |
|----|-------------|------------|
| 1  | 101         | 2023-01-10 |
| 2  | 102         | 2023-01-11 |
| 3  | NULL        | 2023-01-12 |



**customers**

| id  | name  |
|-----|-------|
| 101 | Alice |
| 102 | Bob   |


### 🔍 Example:

```sql
SELECT *
FROM orders o
LEFT ANTI JOIN customers c
ON o.customer_id = c.id;
```

This query helps identify orphaned records — such as orders that are not linked to any customer.

### 💡 Key Insight:
LEFT ANTI JOIN effectively filters out all rows from the left table that have matching rows in the right table, returning only unmatched rows.

### 🧾 Expected Output:


| id | customer_id | order_date |
|----|-------------|------------|
| 3  | NULL        | 2023-01-12 |


---

← Previous Question | Next Question →

**Tags**: #joins #anti-join

---


**🟡 Difficulty:** Medium  **📂 Topic:** Subqueries

## Q2. Write a SQL query to find the second highest salary from an `employees` table.

### 📋 Table Structure & Sample Data


Assume we have the following `employees` table:

**employees**

| id | name  | salary | department | hire_date  | manager_id |
|----|-------|--------|------------|------------|------------|
| 1  | Alice | 70000  | HR         | 2021-05-01 | NULL       |
| 2  | Bob   | 90000  | IT         | 2020-03-15 | 1          |
| 3  | Carol | 80000  | IT         | 2022-01-10 | 2          |
| 4  | Dave  | 60000  | HR         | 2023-04-20 | 1          |


### 🔍 Example:

```sql
SELECT MAX(salary) AS second_highest
FROM employees
WHERE salary < (
  SELECT MAX(salary) FROM employees
);
```

This uses a subquery to exclude the highest salary and returns the next highest.

### 💡 Key Insight:
This query demonstrates how to find the second highest value using a subquery that excludes the maximum.

### 🧾 Expected Output:


| second_highest |
|----------------|
| 80000          |


---

← Previous Question | Next Question →

**Tags**: #subquery #ranking

---


**🟢 Difficulty:** Easy  **📂 Topic:** Aggregation

## Q3. What is the difference between `WHERE` and `HAVING` in SQL?

- `WHERE` filters rows **before** aggregation.
- `HAVING` filters **after** aggregation.

### 📋 Table Structure & Sample Data


Assume we have the following `employees` table:

**employees**

| id | name    | salary | department | hire_date  |
|----|---------|--------|------------|------------|
| 1  | Alice   | 50000  | HR         | 2022-02-15 |
| 2  | Bob     | 70000  | IT         | 2021-06-12 |
| 3  | Charlie | 60000  | IT         | 2023-01-20 |
| 4  | Diana   | 55000  | Finance    | 2020-11-05 |


### 🔍 Example:

```sql
SELECT department, COUNT(*) AS emp_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

This query filters departments with more than 5 employees — `HAVING` is used after the `GROUP BY`.

### 💡 Key Insight:
Use WHERE to filter rows before aggregation, and HAVING to filter groups after aggregation.

### 🧾 Expected Output:


| department | emp_count |
|------------|-----------|
| (no rows)  |           |


---

← Previous Question | Next Question →

**Tags**: #aggregation #filtering

---


**🟢 Difficulty:** Easy  **📂 Topic:** Duplicates

## Q4. Write a query to find duplicate records based on `email` in a `users` table.

### 📋 Table Structure & Sample Data


Assume we have the following `users` table:

**users**

| id | name  | email           |
|----|-------|-----------------|
| 1  | Alice | alice@email.com |
| 2  | Bob   | bob@email.com   |
| 3  | Carol | alice@email.com |
| 4  | Dave  | dave@email.com  |
| 5  | Eve   | eve@email.com   |
| 6  | Frank | bob@email.com   |


### 🔍 Example:

```sql
SELECT email, COUNT(*) AS count
FROM users
GROUP BY email
HAVING COUNT(*) > 1;
```

This helps identify duplicates that can be cleaned or deduplicated.

### 💡 Key Insight:
Grouping by email and filtering with HAVING identifies duplicate email entries in the dataset.

### 🧾 Expected Output:


| email           | count |
|-----------------|-------|
| alice@email.com | 2     |
| bob@email.com   | 2     |


---

← Previous Question | Next Question →

**Tags**: #duplicates #group-by

---


**🟡 Difficulty:** Medium  **📂 Topic:** Grouping and Filtering

## Q5. How do you retrieve the latest (Top 1) record per group in SQL?

This is useful when you want to get the most recent order per customer, highest sale per region, etc.

### 📋 Table Structure & Sample Data


Assume we have the following `orders` table:

**orders**

| id | customer_id | order_date | amount |
|----|-------------|------------|--------|
| 1  | 101         | 2023-01-10 | 250    |
| 2  | 102         | 2023-01-11 | 100    |
| 3  | 101         | 2023-02-12 | 400    |
| 4  | 103         | 2023-02-15 | 150    |
| 5  | 102         | 2023-03-01 | 200    |


### 🔍 Example: Get the most recent order per customer from an `orders` table

```sql
SELECT o.*
FROM orders o
JOIN (
    SELECT customer_id, MAX(order_date) AS latest_order
    FROM orders
    GROUP BY customer_id
) latest
ON o.customer_id = latest.customer_id
AND o.order_date = latest.latest_order;
```

This query uses a **self-join** to fetch the latest order for each customer based on `order_date`.

### 💡 Key Insight:
A self-join on max date per group efficiently retrieves the latest record per group.

### 🧾 Expected Output:


| id | customer_id | order_date | amount |
|----|-------------|------------|--------|
| 3  | 101         | 2023-02-12 | 400    |
| 5  | 102         | 2023-03-01 | 200    |
| 4  | 103         | 2023-02-15 | 150    |


---

### ✅ Alternative: Use `ROW_NUMBER()` (if supported by your SQL dialect)

```sql
SELECT *
FROM (
  SELECT *,
         ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC) AS rn
  FROM orders
) ranked
WHERE rn = 1;
```

This approach assigns a row number to each order within a customer partition and selects only the top-ranked (most recent) row.

### 💡 Key Insight:
Using ROW_NUMBER() allows retrieval of the top record per group without self-joins.

---

← Previous Question | Next Question →

**Tags**: #window-functions #latest-record

---


**🟠 Difficulty:** Medium-High  **📂 Topic:** Window Functions

## Q6. Write a query to calculate the running (cumulative) total of sales per day.

This is commonly asked to test your understanding of **window functions** and date-wise aggregations.

### 📋 Table Structure & Sample Data


Assume we have the following `sales` table:

**sales**

| id | sale_date  | amount | region |
|----|------------|--------|--------|
| 1  | 2023-01-01 | 100    | North  |
| 2  | 2023-01-02 | 200    | North  |
| 3  | 2023-01-01 | 150    | South  |
| 4  | 2023-01-03 | 250    | North  |
| 5  | 2023-01-02 | 120    | South  |


### 🔍 Example: From a `sales` table with columns `sale_date` and `amount`

```sql
SELECT
  sale_date,
  amount,
  SUM(amount) OVER (ORDER BY sale_date) AS running_total
FROM sales;
```

> 📝 Note: This syntax works in PostgreSQL. For MySQL, refer to the dialect-specific version below.

This query calculates a cumulative sum of `amount` ordered by `sale_date`.

### 💡 Key Insight:
Window functions enable cumulative calculations over ordered data without collapsing rows.

### 🧾 Expected Output:


| sale_date  | amount | running_total |
|------------|--------|---------------|
| 2023-01-01 | 100    | 100           |
| 2023-01-01 | 150    | 250           |
| 2023-01-02 | 200    | 450           |
| 2023-01-02 | 120    | 570           |
| 2023-01-03 | 250    | 820           |


---

### ✅ Alternative: Partition by another field (e.g., region)

```sql
SELECT
  region,
  sale_date,
  amount,
  SUM(amount) OVER (PARTITION BY region ORDER BY sale_date) AS regional_running_total
FROM sales;
```

This gives a running total **per region** per day.

### 💡 Key Insight:
Partitioning window functions allows separate running totals within categories.

---

← Previous Question | Next Question →

**Tags**: #window-functions #cumulative-sum

---


**🟡 Difficulty:** Medium  **📂 Topic:** Self Joins

## Q7. Write a SQL query to find employees who earn more than their manager.

This question tests understanding of **self-joins** and hierarchical data.

Assume the `employees` table has the following columns:
- `id`: Employee ID
- `name`: Employee name
- `salary`: Employee salary
- `manager_id`: Manager’s employee ID

### 📋 Table Structure & Sample Data


Assume we have the following `employees` table:

**employees**

| id | name  | salary | manager_id |
|----|-------|--------|------------|
| 1  | Alice | 90000  | NULL       |
| 2  | Bob   | 85000  | 1          |
| 3  | Carol | 95000  | 2          |
| 4  | Dave  | 80000  | 1          |
| 5  | Eve   | 99000  | 2          |


### 🔍 Example:

```sql
SELECT e.name AS employee_name, e.salary AS employee_salary,
       m.name AS manager_name, m.salary AS manager_salary
FROM employees e
JOIN employees m
  ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

This query joins the `employees` table to itself:
- `e` represents employees
- `m` represents their managers

The filter `e.salary > m.salary` ensures we return only those employees earning more than their respective manager.

### 💡 Key Insight:
Self-joins allow comparing hierarchical relationships such as employee and manager salaries.

### 🧾 Expected Output:


| employee_name | employee_salary | manager_name | manager_salary |
|---------------|----------------|--------------|---------------|
| Carol         | 95000          | Bob          | 85000         |
| Eve           | 99000          | Bob          | 85000         |


---

### ✅ Alternative: Select only employee and manager IDs for a simplified result

```sql
SELECT e.id AS employee_id, m.id AS manager_id
FROM employees e
JOIN employees m
  ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

This version returns just the IDs for use in further processing.

### 💡 Key Insight:
Selecting IDs can simplify output when only identifiers are needed for further queries.

---

← Previous Question | Next Question →

**Tags**: #self-join #hierarchical-data

---


**🟢 Difficulty:** Easy  **📂 Topic:** Date Filtering

## Q8. Write a SQL query to find employees hired in the last 6 months.

This question tests your ability to work with **date functions** and **interval calculations**.

Assume the `employees` table has a column:
- `hire_date`: the date when the employee was hired.

### 📋 Table Structure & Sample Data


Assume we have the following `employees` table:

**employees**

| id | name  | hire_date  | department |
|----|-------|------------|------------|
| 1  | Alice | 2023-12-01 | HR         |
| 2  | Bob   | 2023-04-15 | IT         |
| 3  | Carol | 2022-10-10 | IT         |
| 4  | Dave  | 2023-09-05 | Finance    |
| 5  | Eve   | 2022-07-20 | HR         |


### 🔍 Example:

```sql
SELECT *
FROM employees
WHERE hire_date >= CURRENT_DATE - INTERVAL '6 months';
```

> 📝 Note: This syntax works in PostgreSQL. For MySQL, refer to the dialect-specific version below.

This query retrieves all employees whose `hire_date` is within the past 6 months from the current date.

### 💡 Key Insight:
Date arithmetic with INTERVAL helps filter recent records relative to the current date.

### 🧾 Expected Output:


| id | name  | hire_date  | department |
|----|-------|------------|------------|
| 1  | Alice | 2023-12-01 | HR         |
| 4  | Dave  | 2023-09-05 | Finance    |


---

### ✅ Alternative (for MySQL):

```sql
SELECT *
FROM employees
WHERE hire_date >= DATE_SUB(CURDATE(), INTERVAL 6 MONTH);
```

Use the appropriate syntax based on your SQL dialect.

### 💡 Key Insight:
MySQL uses DATE_SUB and CURDATE() for date interval calculations similar to PostgreSQL's INTERVAL.

---

← Previous Question | Next Question →

**Tags**: #date-functions #interval

---


**🟠 Difficulty:** Medium-High  **📂 Topic:** Running Totals

## Q9. Write a SQL query to calculate the running total of employee salaries.

This question checks your understanding of **window functions** and cumulative aggregations.


Assume the `employees` table has the following structure:

| id | name    | department | salary | hire_date  |
|----|---------|------------|--------|------------|
| 1  | Alice   | HR         | 50000  | 2022-02-15 |
| 2  | Bob     | IT         | 70000  | 2021-06-12 |
| 3  | Charlie | IT         | 60000  | 2023-01-20 |
| 4  | Diana   | Finance    | 55000  | 2020-11-05 |


### 🔍 Example: Calculate cumulative salary ordered by hire date

```sql
SELECT
  id,
  name,
  salary,
  hire_date,
  SUM(salary) OVER (ORDER BY hire_date) AS running_salary_total
FROM employees;
```

This query gives the running total of `salary` ordered by `hire_date` — useful for analyzing salary expense trends over time.

### 💡 Key Insight:
Window functions can calculate cumulative totals ordered by a specific column, like hire date.

### 🧾 Expected Output:


| id | name    | salary | hire_date  | running_salary_total |
|----|---------|--------|------------|---------------------|
| 4  | Diana   | 55000  | 2020-11-05 | 55000               |
| 2  | Bob     | 70000  | 2021-06-12 | 125000              |
| 1  | Alice   | 50000  | 2022-02-15 | 175000              |
| 3  | Charlie | 60000  | 2023-01-20 | 235000              |


---

### ✅ Alternative: Partition by department

```sql
SELECT
  id,
  name,
  department,
  salary,
  hire_date,
  SUM(salary) OVER (PARTITION BY department ORDER BY hire_date) AS dept_running_salary
FROM employees;
```

This version gives the cumulative salary **per department**, ordered by hiring date.

### 💡 Key Insight:
Partitioning by department calculates running totals separately for each department.

---

← Previous Question | Next Question →

**Tags**: #window-functions #cumulative-sum

---


**🟡 Difficulty:** Medium  **📂 Topic:** Pivoting

## Q10. Write a SQL query to pivot monthly sales data by product.

This question tests your understanding of **pivoting**, **aggregation**, and conditional expressions.

### 📋 Table Structure & Sample Data


Assume we have the following `sales` table:

**sales**

| id | product  | sale_month | amount |
|----|----------|------------|--------|
| 1  | Laptop   | Jan        | 1000   |
| 2  | Laptop   | Feb        | 1500   |
| 3  | Monitor  | Jan        | 700    |
| 4  | Monitor  | Feb        | 900    |
| 5  | Keyboard | Jan        | 300    |
| 6  | Keyboard | Feb        | 400    |


### 🔍 Example:

```sql
SELECT
  product,
  SUM(CASE WHEN sale_month = 'Jan' THEN amount ELSE 0 END) AS Jan,
  SUM(CASE WHEN sale_month = 'Feb' THEN amount ELSE 0 END) AS Feb
FROM sales
GROUP BY product;
```

This query pivots `sale_month` into separate columns and aggregates `amount` for each product.

### 💡 Key Insight:
Conditional aggregation enables pivoting data without special pivot functions.

### 🧾 Expected Output:


| product  | Jan  | Feb  |
|----------|------|------|
| Laptop   | 1000 | 1500 |
| Monitor  | 700  | 900  |
| Keyboard | 300  | 400  |


---

### ✅ Alternative (Using PIVOT — SQL Server / Oracle):

```sql
SELECT *
FROM (
  SELECT product, sale_month, amount
  FROM sales
) src
PIVOT (
  SUM(amount)
  FOR sale_month IN ([Jan], [Feb])
) AS p;
```

This approach uses built-in pivot functionality available in some SQL dialects.

### 💡 Key Insight:
Pivot syntax simplifies transforming row data into columns but is dialect-specific.

---

← Previous Question | Next Question →

**Tags**: #pivot #aggregation #conditional-aggregation
