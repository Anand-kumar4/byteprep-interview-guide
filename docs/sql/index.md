---
title: SQL Interview Questions
description: A collection of commonly asked SQL interview questions with explanations and examples.
keywords: SQL, interview questions, joins, subquery, group by, having, duplicates
---

# SQL Interview Questions

## Q1. What is a LEFT ANTI JOIN? How is it different from LEFT JOIN?

A **LEFT ANTI JOIN** returns only the rows from the left table **that do not have a match** in the right table.

This is different from a LEFT JOIN, which returns **all rows** from the left table and matching rows from the right (NULL if no match).

### 🔍 Example:

```sql
SELECT *
FROM orders o
LEFT ANTI JOIN customers c
ON o.customer_id = c.id;
```

This query helps identify orphaned records — such as orders that are not linked to any customer.

---

## Q2. Write a SQL query to find the second highest salary from an `employees` table.

### 🔍 Example:

```sql
SELECT MAX(salary) AS second_highest
FROM employees
WHERE salary < (
  SELECT MAX(salary) FROM employees
);
```

This uses a subquery to exclude the highest salary and returns the next highest.

---

## Q3. What is the difference between `WHERE` and `HAVING` in SQL?

- `WHERE` filters rows **before** aggregation.
- `HAVING` filters **after** aggregation.

### 🔍 Example:

```sql
SELECT department, COUNT(*) AS emp_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

This query filters departments with more than 5 employees — `HAVING` is used after the `GROUP BY`.

---

## Q4. Write a query to find duplicate records based on `email` in a `users` table.

### 🔍 Example:

```sql
SELECT email, COUNT(*) AS count
FROM users
GROUP BY email
HAVING COUNT(*) > 1;
```

This helps identify duplicates that can be cleaned or deduplicated.
