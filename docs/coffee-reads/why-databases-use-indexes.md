# Why Databases Use Indexes (Explained With a Phone Book) ☕

> **4 min read** · Databases

Imagine you want to find "Sarah Mitchell" in a phone book.

You don't start at page 1 and read every single entry. You flip to the M section, scan down, find Mitchell, done. The phone book is *sorted*, so you can skip most of it.

That's an index.

---

## What a Database Does Without an Index

Say you have a `users` table with 1 million rows, and you run:

```sql
SELECT * FROM users WHERE email = 'sarah@example.com';
```

Without an index, the database does a **full table scan** — it reads every single row, checks the email column, and keeps the matches. For 1 million rows, that's 1 million reads.

This is called an O(n) operation — it gets slower in direct proportion to the size of the table.

---

## What an Index Does

An index creates a **separate, sorted data structure** that maps column values to row locations.

```
Index on `email` column:

  alice@example.com  →  Row 847
  bob@example.com    →  Row 12
  charlie@example.com → Row 3,401
  sarah@example.com  →  Row 201,847   ← found it
```

Because it's sorted, the database can use **binary search** — repeatedly halving the search space — to find the value in O(log n) time.

For 1 million rows:
- Full table scan: up to **1,000,000 reads**
- Index lookup: roughly **20 reads** (log₂ of 1,000,000 ≈ 20)

---

## The Real Structure: B-Trees

In practice, most databases use a data structure called a **B-Tree** (balanced tree) rather than a simple sorted list.

```
                [50]
               /    \
           [20]      [70]
          /    \    /    \
       [10] [30] [60]   [90]
```

Each node holds multiple values and has multiple children. The tree stays balanced, meaning every lookup takes roughly the same number of steps regardless of where the value is.

The important thing to know: B-Trees are exceptionally fast at:
- Finding an exact value (`WHERE email = ...`)
- Finding a range of values (`WHERE age BETWEEN 20 AND 30`)
- Sorting results (`ORDER BY created_at`)

---

## Why Not Index Everything?

If indexes make reads faster, why not put one on every column?

Because indexes **slow down writes**.

Every time you `INSERT`, `UPDATE`, or `DELETE` a row, the database has to update every index that touches that row. More indexes = more work on every write.

Indexes also **take up storage** — sometimes as much space as the table itself.

So you index strategically:

```sql
-- ✅ Good candidates for indexes
CREATE INDEX idx_email ON users(email);       -- Frequently searched
CREATE INDEX idx_created_at ON orders(created_at); -- Frequently sorted
CREATE INDEX idx_user_id ON posts(user_id);   -- Frequently joined on

-- ❌ Probably not worth indexing
-- Boolean columns (only 2 values — not selective enough)
-- Columns that are rarely queried
-- Tiny tables (full scan is fine)
```

---

## Composite Indexes

You can index multiple columns together:

```sql
CREATE INDEX idx_name_age ON users(last_name, age);
```

This helps queries like:
```sql
SELECT * FROM users WHERE last_name = 'Smith' AND age > 30;
```

Important rule: **the order matters**. A composite index on `(last_name, age)` helps queries filtering on `last_name`, but doesn't help queries filtering only on `age`. The leftmost column has to be used.

---

## Seeing It in Action

```sql
-- Check if your query is using an index
EXPLAIN SELECT * FROM users WHERE email = 'sarah@example.com';
```

Look for `index` or `ref` in the output. `ALL` means full table scan — consider adding an index.

---

## The Takeaway

> An index is a sorted, separate data structure that lets the database find rows in O(log n) instead of O(n). They dramatically speed up reads at the cost of slightly slower writes and extra storage. Index the columns you search, sort, and join on most frequently.

This comes up in almost every backend or database interview. Understanding *why* indexes work — not just *that* they exist — is what separates a solid answer from a great one.

---

## Want to Go Deeper?

- 📄 [SQL Fundamentals](../databases/sql-fundamentals.md) — CRUD, joins, transactions
- 📄 [SQL Queries Cheat Sheet](../cheat-sheets/sql-queries.md) — common query patterns
- 🌐 [Use the Index, Luke](https://use-the-index-luke.com/) — the best free resource on database indexing

---

*← [Back to Coffee Reads](./README.md)*
