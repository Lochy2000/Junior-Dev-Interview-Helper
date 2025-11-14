# SQL Queries Cheat Sheet

> Quick reference for common SQL queries and patterns

## 📋 Table of Contents

- [Basic Queries](#basic-queries)
- [Filtering & Conditions](#filtering--conditions)
- [Joins](#joins)
- [Aggregate Functions](#aggregate-functions)
- [Grouping & Having](#grouping--having)
- [Subqueries](#subqueries)
- [Constraints](#constraints)
- [Common Patterns](#common-patterns)

---

## Basic Queries

### SELECT

```sql
-- Select all columns
SELECT * FROM users;

-- Select specific columns
SELECT name, email FROM users;

-- Select with alias
SELECT name AS full_name, email AS contact_email
FROM users;

-- Select distinct values
SELECT DISTINCT city FROM users;

-- Limit results
SELECT * FROM users LIMIT 10;

-- Offset (skip first N rows)
SELECT * FROM users LIMIT 10 OFFSET 20;
```

### INSERT

```sql
-- Insert single row
INSERT INTO users (name, email, age)
VALUES ('Alice', 'alice@email.com', 25);

-- Insert multiple rows
INSERT INTO users (name, email, age)
VALUES
    ('Bob', 'bob@email.com', 30),
    ('Charlie', 'charlie@email.com', 22),
    ('David', 'david@email.com', 28);

-- Insert from another table
INSERT INTO archived_users (name, email)
SELECT name, email FROM users WHERE age > 65;
```

### UPDATE

```sql
-- Update single column
UPDATE users
SET age = 26
WHERE id = 1;

-- Update multiple columns
UPDATE users
SET email = 'new@email.com', age = 31
WHERE id = 2;

-- Update with calculation
UPDATE products
SET price = price * 1.10
WHERE category = 'electronics';

-- Update all rows (⚠️ dangerous!)
UPDATE users SET active = TRUE;
```

### DELETE

```sql
-- Delete specific rows
DELETE FROM users WHERE id = 5;

-- Delete with condition
DELETE FROM users WHERE age < 18;

-- Delete all rows (⚠️ dangerous!)
DELETE FROM users;

-- Truncate (faster, resets auto-increment)
TRUNCATE TABLE users;
```

---

## Filtering & Conditions

### WHERE Clause

```sql
-- Equality
SELECT * FROM users WHERE age = 25;
SELECT * FROM users WHERE city = 'NYC';

-- Comparison operators
SELECT * FROM users WHERE age > 25;
SELECT * FROM users WHERE age >= 18;
SELECT * FROM users WHERE age < 30;
SELECT * FROM users WHERE age <= 65;
SELECT * FROM users WHERE age != 25;

-- AND, OR, NOT
SELECT * FROM users
WHERE age > 18 AND city = 'NYC';

SELECT * FROM users
WHERE city = 'NYC' OR city = 'LA';

SELECT * FROM users
WHERE NOT city = 'NYC';

-- IN (multiple values)
SELECT * FROM users
WHERE city IN ('NYC', 'LA', 'Chicago');

SELECT * FROM users
WHERE age IN (25, 30, 35);

-- BETWEEN
SELECT * FROM users
WHERE age BETWEEN 18 AND 30;

-- LIKE (pattern matching)
SELECT * FROM users
WHERE email LIKE '%@gmail.com';      -- Ends with @gmail.com

SELECT * FROM users
WHERE name LIKE 'A%';                -- Starts with A

SELECT * FROM users
WHERE name LIKE '%son%';             -- Contains "son"

-- IS NULL / IS NOT NULL
SELECT * FROM users WHERE phone IS NULL;
SELECT * FROM users WHERE phone IS NOT NULL;
```

### ORDER BY

```sql
-- Ascending (default)
SELECT * FROM users ORDER BY age;
SELECT * FROM users ORDER BY age ASC;

-- Descending
SELECT * FROM users ORDER BY age DESC;

-- Multiple columns
SELECT * FROM users
ORDER BY city ASC, age DESC;

-- Order by alias
SELECT name, age * 12 AS age_in_months
FROM users
ORDER BY age_in_months DESC;
```

---

## Joins

### INNER JOIN

```sql
-- Returns rows with matches in both tables
SELECT
    employees.name,
    departments.name AS department
FROM employees
INNER JOIN departments
    ON employees.department_id = departments.id;

-- Short syntax
SELECT e.name, d.name AS department
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;
```

### LEFT JOIN

```sql
-- Returns all rows from left table, matches from right
SELECT
    users.name,
    orders.total
FROM users
LEFT JOIN orders ON users.id = orders.user_id;

-- Find users without orders
SELECT users.name
FROM users
LEFT JOIN orders ON users.id = orders.user_id
WHERE orders.id IS NULL;
```

### RIGHT JOIN

```sql
-- Returns all rows from right table, matches from left
SELECT
    orders.id,
    users.name
FROM orders
RIGHT JOIN users ON orders.user_id = users.id;
```

### FULL OUTER JOIN

```sql
-- Returns all rows from both tables (MySQL doesn't support, use UNION)

-- MySQL workaround
SELECT users.name, orders.total
FROM users
LEFT JOIN orders ON users.id = orders.user_id
UNION
SELECT users.name, orders.total
FROM users
RIGHT JOIN orders ON users.id = orders.user_id;
```

### Self JOIN

```sql
-- Join table to itself (e.g., employee-manager relationship)
SELECT
    e.name AS employee,
    m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;
```

### Multiple JOINS

```sql
SELECT
    orders.id,
    users.name,
    products.name AS product,
    orders.quantity
FROM orders
INNER JOIN users ON orders.user_id = users.id
INNER JOIN products ON orders.product_id = products.id;
```

---

## Aggregate Functions

```sql
-- COUNT
SELECT COUNT(*) FROM users;                  -- Count all rows
SELECT COUNT(phone) FROM users;              -- Count non-NULL values
SELECT COUNT(DISTINCT city) FROM users;      -- Count unique values

-- SUM
SELECT SUM(price) FROM products;
SELECT SUM(quantity * price) AS total FROM orders;

-- AVG
SELECT AVG(age) FROM users;
SELECT AVG(price) FROM products WHERE category = 'electronics';

-- MIN / MAX
SELECT MIN(age) FROM users;
SELECT MAX(price) FROM products;
SELECT MIN(created_at), MAX(created_at) FROM orders;

-- Multiple aggregates
SELECT
    COUNT(*) AS total_users,
    AVG(age) AS average_age,
    MIN(age) AS youngest,
    MAX(age) AS oldest
FROM users;
```

---

## Grouping & Having

### GROUP BY

```sql
-- Count users by city
SELECT city, COUNT(*) AS user_count
FROM users
GROUP BY city;

-- Average age by city
SELECT city, AVG(age) AS average_age
FROM users
GROUP BY city;

-- Sales by product
SELECT
    product_id,
    SUM(quantity) AS total_sold,
    SUM(quantity * price) AS total_revenue
FROM orders
GROUP BY product_id;

-- Multiple columns
SELECT
    city,
    gender,
    COUNT(*) AS count
FROM users
GROUP BY city, gender;
```

### HAVING

```sql
-- Filter groups (use HAVING, not WHERE, after GROUP BY)

-- Cities with more than 10 users
SELECT city, COUNT(*) AS user_count
FROM users
GROUP BY city
HAVING COUNT(*) > 10;

-- Products with average price > $50
SELECT
    category,
    AVG(price) AS avg_price
FROM products
GROUP BY category
HAVING AVG(price) > 50;

-- WHERE filters rows, HAVING filters groups
SELECT city, COUNT(*) AS user_count
FROM users
WHERE age > 18                    -- Filter rows first
GROUP BY city
HAVING COUNT(*) > 5;              -- Then filter groups
```

---

## Subqueries

### In SELECT

```sql
-- Add calculated column
SELECT
    name,
    age,
    (SELECT AVG(age) FROM users) AS average_age
FROM users;
```

### In WHERE

```sql
-- Users older than average
SELECT name, age
FROM users
WHERE age > (SELECT AVG(age) FROM users);

-- Users in cities with >100 residents
SELECT name FROM users
WHERE city IN (
    SELECT city FROM users
    GROUP BY city
    HAVING COUNT(*) > 100
);

-- EXISTS
SELECT name FROM users u
WHERE EXISTS (
    SELECT 1 FROM orders o
    WHERE o.user_id = u.id
);
```

### In FROM

```sql
-- Use subquery as table
SELECT city, average_age
FROM (
    SELECT city, AVG(age) AS average_age
    FROM users
    GROUP BY city
) AS city_stats
WHERE average_age > 30;
```

---

## Constraints

### CREATE TABLE with Constraints

```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    age INT CHECK (age >= 18),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    total DECIMAL(10, 2) NOT NULL,
    status ENUM('pending', 'shipped', 'delivered') DEFAULT 'pending',
    FOREIGN KEY (user_id) REFERENCES users(id)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);
```

### ALTER TABLE

```sql
-- Add column
ALTER TABLE users ADD phone VARCHAR(20);

-- Modify column
ALTER TABLE users MODIFY email VARCHAR(150);

-- Drop column
ALTER TABLE users DROP COLUMN phone;

-- Add constraint
ALTER TABLE users ADD UNIQUE (email);
ALTER TABLE users ADD CHECK (age >= 0);

-- Add foreign key
ALTER TABLE orders
ADD FOREIGN KEY (user_id) REFERENCES users(id);

-- Drop constraint
ALTER TABLE users DROP INDEX email;
```

---

## Common Patterns

### Pagination

```sql
-- Page 1 (rows 1-10)
SELECT * FROM users
ORDER BY id
LIMIT 10 OFFSET 0;

-- Page 2 (rows 11-20)
SELECT * FROM users
ORDER BY id
LIMIT 10 OFFSET 10;

-- General formula: OFFSET = (page - 1) * limit
```

### Top N Per Group

```sql
-- Top 3 highest paid employees per department
WITH RankedEmployees AS (
    SELECT
        name,
        department_id,
        salary,
        ROW_NUMBER() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rank
    FROM employees
)
SELECT name, department_id, salary
FROM RankedEmployees
WHERE rank <= 3;
```

### Find Duplicates

```sql
-- Find duplicate emails
SELECT email, COUNT(*) AS count
FROM users
GROUP BY email
HAVING COUNT(*) > 1;

-- Get duplicate rows
SELECT * FROM users
WHERE email IN (
    SELECT email FROM users
    GROUP BY email
    HAVING COUNT(*) > 1
);
```

### Date Queries

```sql
-- Today
SELECT * FROM orders
WHERE DATE(created_at) = CURDATE();

-- Last 7 days
SELECT * FROM orders
WHERE created_at >= DATE_SUB(NOW(), INTERVAL 7 DAY);

-- Last month
SELECT * FROM orders
WHERE MONTH(created_at) = MONTH(DATE_SUB(NOW(), INTERVAL 1 MONTH))
AND YEAR(created_at) = YEAR(DATE_SUB(NOW(), INTERVAL 1 MONTH));

-- Between dates
SELECT * FROM orders
WHERE created_at BETWEEN '2024-01-01' AND '2024-12-31';

-- Group by month
SELECT
    DATE_FORMAT(created_at, '%Y-%m') AS month,
    COUNT(*) AS order_count
FROM orders
GROUP BY month;
```

### Conditional Logic

```sql
-- CASE statement
SELECT
    name,
    age,
    CASE
        WHEN age < 18 THEN 'Minor'
        WHEN age < 65 THEN 'Adult'
        ELSE 'Senior'
    END AS age_group
FROM users;

-- IF statement (MySQL)
SELECT
    name,
    IF(age >= 18, 'Adult', 'Minor') AS status
FROM users;
```

### String Functions

```sql
-- Concatenate
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM users;

-- Uppercase/Lowercase
SELECT UPPER(name), LOWER(email) FROM users;

-- Substring
SELECT SUBSTRING(email, 1, 3) FROM users;  -- First 3 chars

-- Replace
SELECT REPLACE(phone, '-', '') FROM users;

-- Trim whitespace
SELECT TRIM(name) FROM users;
```

### Math Functions

```sql
-- Round
SELECT ROUND(price, 2) FROM products;

-- Ceiling/Floor
SELECT CEIL(price), FLOOR(price) FROM products;

-- Absolute value
SELECT ABS(balance) FROM accounts;

-- Power
SELECT POW(2, 3);  -- 8
```

---

## Transactions

```sql
-- Start transaction
START TRANSACTION;

-- Execute queries
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

-- Commit (save changes)
COMMIT;

-- Or rollback (undo changes)
ROLLBACK;
```

---

## Indexes

```sql
-- Create index
CREATE INDEX idx_email ON users(email);

-- Composite index
CREATE INDEX idx_city_age ON users(city, age);

-- Unique index
CREATE UNIQUE INDEX idx_username ON users(username);

-- Drop index
DROP INDEX idx_email ON users;

-- Show indexes
SHOW INDEX FROM users;
```

---

## Tips & Best Practices

```sql
-- Use EXPLAIN to analyze query performance
EXPLAIN SELECT * FROM users WHERE email = 'test@email.com';

-- Always use WHERE with UPDATE/DELETE
UPDATE users SET status = 'active' WHERE id = 1;  -- ✅
UPDATE users SET status = 'active';               -- ❌ Dangerous!

-- Use transactions for related changes
START TRANSACTION;
-- Multiple related queries
COMMIT;

-- Avoid SELECT *
SELECT id, name, email FROM users;  -- ✅ Better performance
SELECT * FROM users;                -- ❌ Wasteful
```

---

**Keywords**: SQL queries, SQL cheat sheet, SQL joins, SQL examples, database queries, SELECT statement, SQL aggregate functions, GROUP BY, SQL subqueries
