# Database Fundamentals - SQL & Database Design

> Master database concepts, SQL queries, relationships, and best practices

## 📋 Table of Contents

- [Database Basics](#database-basics)
- [SQL Fundamentals](#sql-fundamentals)
- [CRUD Operations](#crud-operations)
- [SQL Joins](#sql-joins)
- [Database Relationships](#database-relationships)
- [Indexes & Performance](#indexes--performance)
- [Transactions](#transactions)
- [Interview Questions](#interview-questions)

---

## Database Basics

### What is a Database?

A **database** is an organized collection of structured data stored electronically. A **DBMS (Database Management System)** manages the database.

### Types of Databases

**Relational (SQL)**:
- PostgreSQL
- MySQL
- SQLite
- SQL Server

**NoSQL**:
- MongoDB (Document)
- Redis (Key-Value)
- Cassandra (Column)

### Key Concepts

- **Table**: Collection of related data (like Excel spreadsheet)
- **Row**: Single record in a table
- **Column**: Attribute/field in a table
- **Primary Key**: Unique identifier for each row
- **Foreign Key**: References primary key in another table

---

## SQL Fundamentals

### Basic Structure

```sql
-- Comments start with --

-- Database operations
CREATE DATABASE company;
USE company;
DROP DATABASE company;

-- Show databases
SHOW DATABASES;
```

### Creating Tables

```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL,
    age INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- View table structure
DESCRIBE users;
```

### Data Types

```sql
-- Numbers
INT              -- Integer
DECIMAL(10, 2)   -- Decimal with precision
FLOAT            -- Floating point

-- Text
CHAR(10)         -- Fixed length
VARCHAR(255)     -- Variable length
TEXT             -- Large text

-- Date/Time
DATE             -- YYYY-MM-DD
TIME             -- HH:MM:SS
DATETIME         -- YYYY-MM-DD HH:MM:SS
TIMESTAMP        -- Unix timestamp

-- Boolean
BOOLEAN          -- TRUE/FALSE
```

---

## CRUD Operations

### Create (INSERT)

```sql
-- Insert single row
INSERT INTO users (username, email, age)
VALUES ('alice', 'alice@email.com', 25);

-- Insert multiple rows
INSERT INTO users (username, email, age)
VALUES
    ('bob', 'bob@email.com', 30),
    ('charlie', 'charlie@email.com', 22);
```

### Read (SELECT)

```sql
-- Select all columns
SELECT * FROM users;

-- Select specific columns
SELECT username, email FROM users;

-- With WHERE clause
SELECT * FROM users WHERE age > 25;
SELECT * FROM users WHERE username = 'alice';

-- Multiple conditions
SELECT * FROM users WHERE age > 20 AND age < 30;
SELECT * FROM users WHERE age < 20 OR age > 60;

-- Pattern matching
SELECT * FROM users WHERE email LIKE '%@gmail.com';
SELECT * FROM users WHERE username LIKE 'a%';  -- Starts with 'a'

-- IN operator
SELECT * FROM users WHERE age IN (20, 25, 30);

-- Order results
SELECT * FROM users ORDER BY age ASC;   -- Ascending
SELECT * FROM users ORDER BY age DESC;  -- Descending

-- Limit results
SELECT * FROM users LIMIT 10;
SELECT * FROM users LIMIT 10 OFFSET 20;  -- Pagination

-- Count, min, max, avg, sum
SELECT COUNT(*) FROM users;
SELECT AVG(age) FROM users;
SELECT MIN(age), MAX(age) FROM users;
```

### Update (UPDATE)

```sql
-- Update single column
UPDATE users
SET age = 26
WHERE username = 'alice';

-- Update multiple columns
UPDATE users
SET email = 'newemail@email.com', age = 31
WHERE id = 2;

-- Update all rows (dangerous!)
UPDATE users SET active = TRUE;
```

### Delete (DELETE)

```sql
-- Delete specific rows
DELETE FROM users WHERE id = 5;
DELETE FROM users WHERE age < 18;

-- Delete all rows (dangerous!)
DELETE FROM users;

-- Drop entire table
DROP TABLE users;
```

---

## SQL Joins

Joins combine rows from two or more tables based on related columns.

### Sample Tables

```sql
CREATE TABLE departments (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(id)
);

INSERT INTO departments VALUES (1, 'Engineering'), (2, 'Sales'), (3, 'HR');
INSERT INTO employees VALUES
    (1, 'Alice', 1),
    (2, 'Bob', 1),
    (3, 'Charlie', 2),
    (4, 'David', NULL);
```

### INNER JOIN

Returns rows with matching values in both tables.

```sql
SELECT employees.name, departments.name AS department
FROM employees
INNER JOIN departments ON employees.department_id = departments.id;

/*
Result:
name    | department
--------|------------
Alice   | Engineering
Bob     | Engineering
Charlie | Sales
*/
```

### LEFT JOIN (LEFT OUTER JOIN)

Returns all rows from left table, matching rows from right (NULL if no match).

```sql
SELECT employees.name, departments.name AS department
FROM employees
LEFT JOIN departments ON employees.department_id = departments.id;

/*
Result:
name    | department
--------|------------
Alice   | Engineering
Bob     | Engineering
Charlie | Sales
David   | NULL
*/
```

### RIGHT JOIN (RIGHT OUTER JOIN)

Returns all rows from right table, matching rows from left.

```sql
SELECT employees.name, departments.name AS department
FROM employees
RIGHT JOIN departments ON employees.department_id = departments.id;

/*
Result:
name    | department
--------|------------
Alice   | Engineering
Bob     | Engineering
Charlie | Sales
NULL    | HR
*/
```

### FULL OUTER JOIN

Returns all rows from both tables (MySQL doesn't support, use UNION).

```sql
-- MySQL workaround
SELECT employees.name, departments.name AS department
FROM employees
LEFT JOIN departments ON employees.department_id = departments.id
UNION
SELECT employees.name, departments.name AS department
FROM employees
RIGHT JOIN departments ON employees.department_id = departments.id;
```

---

## Database Relationships

### One-to-One

One row in Table A relates to one row in Table B.

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    username VARCHAR(50)
);

CREATE TABLE profiles (
    id INT PRIMARY KEY,
    user_id INT UNIQUE,
    bio TEXT,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

### One-to-Many

One row in Table A relates to many rows in Table B.

```sql
CREATE TABLE authors (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

CREATE TABLE books (
    id INT PRIMARY KEY,
    title VARCHAR(100),
    author_id INT,
    FOREIGN KEY (author_id) REFERENCES authors(id)
);
```

### Many-to-Many

Many rows in Table A relate to many rows in Table B (uses junction table).

```sql
CREATE TABLE students (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

CREATE TABLE courses (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

-- Junction table
CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(id),
    FOREIGN KEY (course_id) REFERENCES courses(id)
);
```

---

## Indexes & Performance

### What are Indexes?

Indexes speed up data retrieval by creating a sorted reference to rows.

### Creating Indexes

```sql
-- Single column index
CREATE INDEX idx_email ON users(email);

-- Composite index
CREATE INDEX idx_name_age ON users(name, age);

-- Unique index
CREATE UNIQUE INDEX idx_username ON users(username);

-- Remove index
DROP INDEX idx_email ON users;
```

### When to Use Indexes

✅ **Use on**:
- Columns in WHERE clauses
- Columns used in JOINs
- Columns used in ORDER BY
- Primary keys (auto-indexed)

❌ **Avoid on**:
- Small tables
- Columns with low cardinality (few unique values)
- Frequently updated columns

---

## Transactions

Transactions ensure data integrity with ACID properties.

### ACID Properties

- **Atomicity**: All or nothing
- **Consistency**: Data remains valid
- **Isolation**: Transactions don't interfere
- **Durability**: Changes persist

### Transaction Example

```sql
START TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

-- If everything is OK
COMMIT;

-- If error occurred
ROLLBACK;
```

---

## Interview Questions

### 1. What is SQL?
**Answer**: Structured Query Language for managing relational databases. Used to create, read, update, delete data (CRUD).

### 2. What is a Primary Key?
**Answer**: Unique identifier for each row in a table. Cannot be NULL, must be unique. Tables typically have one primary key.

### 3. What is a Foreign Key?
**Answer**: Column that references a primary key in another table. Establishes relationships between tables.

### 4. Explain different types of JOINs
**Answer**:
- **INNER JOIN**: Only matching rows
- **LEFT JOIN**: All from left, matching from right
- **RIGHT JOIN**: All from right, matching from left
- **FULL JOIN**: All rows from both tables

### 5. What is normalization?
**Answer**: Process of organizing data to reduce redundancy. Divided into normal forms (1NF, 2NF, 3NF).

### 6. What is an index?
**Answer**: Data structure that improves query performance by creating sorted references. Trade-off: faster reads, slower writes.

### 7. What is a transaction?
**Answer**: Group of SQL statements executed as single unit. Ensures data consistency with ACID properties.

### 8. HAVING vs WHERE?
**Answer**:
- **WHERE**: Filters rows before grouping
- **HAVING**: Filters groups after GROUP BY

### 9. What is denormalization?
**Answer**: Intentionally adding redundancy to improve read performance. Trade-off with data integrity.

### 10. Explain ACID
**Answer**:
- **A**tomicity: All or nothing
- **C**onsistency: Valid state always
- **I**solation: Independent transactions
- **D**urability: Permanent changes

---

## Resources

- [SQL Tutorial - W3Schools](https://www.w3schools.com/sql/)
- [PostgreSQL Docs](https://www.postgresql.org/docs/)
- [MySQL Docs](https://dev.mysql.com/doc/)
- [SQL Bolt](https://sqlbolt.com/) - Interactive learning

---

**Keywords**: SQL tutorial, database design, SQL joins, database relationships, SQL queries, database normalization, indexes, transactions, CRUD operations, SQL interview questions
