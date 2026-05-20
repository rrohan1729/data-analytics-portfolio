# 📘 SQL Study Notes
**Database:** PostgreSQL | **Tool:** pgAdmin

---

## 📝 Daily Log
* [2026-02-01]: Setup GitHub Portfolio. Installed PostgreSQL.
* [2026-05-12]: Learned AS keyword — aliasing columns in SELECT
* [2026-05-15]: Learned BETWEEN, IN, LIKE, ILIKE — pattern matching and filtering in WHERE clause
* [2026-05-16]: Learned JOINs — INNER, LEFT, RIGHT, FULL OUTER, SELF JOIN + UNION vs UNION ALL
* [2026-05-17]: Learned Timestamps, EXTRACT, TO_CHAR, Mathematical Functions, String Functions and Operators

---

## 💻 SQL Notes

### SELECT
```sql
SELECT * FROM table;
SELECT column_1, column_2 FROM table;
```

### AS Keyword (Aliasing)
```sql
SELECT amount AS net_revenue
FROM payment;
-- AS applies at end — cannot use alias in WHERE clause
```

### BETWEEN
Used to filter results within a range (inclusive of both ends).

```sql
SELECT column
FROM table
WHERE column BETWEEN value1 AND value2;
```

```sql
SELECT amount
FROM payment
WHERE amount BETWEEN 5 AND 10;
```

> Works with numbers, dates, and text.
> Always inclusive — both boundary values are included in the result.

### IN
Used when you want to match against multiple specific values — a cleaner alternative to writing multiple OR conditions.

Without IN (messy):
```sql
SELECT amount
FROM payment
WHERE amount = 5 OR amount = 7 OR amount = 10;
```

With IN (clean):
```sql
SELECT amount
FROM payment
WHERE amount IN (5, 7, 10);
```

> Use IN when you have a list of exact values to match.
> Much more readable than chaining multiple OR conditions.

### LIKE
Used to search for a pattern within a string. Case-sensitive.

```sql
SELECT column
FROM table
WHERE column LIKE 'pattern';
```

| Wildcard | Meaning |
|----------|---------|
| `%` | Matches any sequence of characters (zero or more) |
| `_` | Matches exactly one single character |

```sql
-- Names ending with 'A'
WHERE name LIKE '%A'

-- Names starting with 'A'
WHERE name LIKE 'A%'

-- Names with any single character before 'at'
WHERE name LIKE '_at'
-- Matches: bat, cat, hat (but NOT flat)
```

### ILIKE
Same as LIKE, but case-insensitive. (PostgreSQL specific)

```sql
-- Matches 'Alice', 'alice', 'ALICE'
WHERE name ILIKE 'alice'

-- Matches any name starting with 'a' or 'A'
WHERE name ILIKE 'a%'
```

### Quick Reference — Filtering Operators

| Operator | Use Case | Case Sensitive? |
|----------|----------|----------------|
| `BETWEEN` | Range of values | Depends on data type |
| `IN` | List of exact values | Depends on data type |
| `LIKE` | Pattern matching | Yes |
| `ILIKE` | Pattern matching | No (PostgreSQL only) |

---

### INNER JOIN
Returns only the rows where there is a match in **both** tables.

```sql
SELECT a.column, b.column
FROM table_a a
INNER JOIN table_b b
ON a.common_column = b.common_column;
```

```sql
SELECT customer.customer_id, payment.amount
FROM customer
INNER JOIN payment
ON customer.customer_id = payment.customer_id;
```

> Only matching rows from both sides are returned.
> If no match exists, that row is excluded entirely.

---

### LEFT JOIN (LEFT OUTER JOIN)
Keeps **all rows from the LEFT table**. Brings only matching rows from the right table. Non-matching right-side columns show as NULL.

```sql
SELECT a.column, b.column
FROM table_a a
LEFT JOIN table_b b
ON a.common_column = b.common_column;
```

> FROM table = LEFT table (always kept fully).
> JOIN table = RIGHT table (only matching rows come in).
> Think of it as: "Left table is the main table. Right table is added only where it matches."

---

### RIGHT JOIN (RIGHT OUTER JOIN)
Keeps **all rows from the RIGHT table**. Brings only matching rows from the left table. Non-matching left-side columns show as NULL.

```sql
SELECT a.column, b.column
FROM table_a a
RIGHT JOIN table_b b
ON a.common_column = b.common_column;
```

> Think of it as: "Right table is the main table. Left table is added only where it matches."
> In practice, RIGHT JOIN is rarely used — most developers flip the tables and use LEFT JOIN instead.

---

### FULL OUTER JOIN
Keeps **all rows from both tables**. Shows NULL wherever there is no match on either side.

```sql
SELECT a.column, b.column
FROM table_a a
FULL OUTER JOIN table_b b
ON a.common_column = b.common_column;
```

> Think of it as: "Keep everything from both sides. Show NULL where no match exists."

---

### SELF JOIN
A table joined with **itself**. Used for hierarchical data where rows relate to other rows in the same table.

```sql
SELECT a.column, b.column
FROM table_name a
JOIN table_name b
ON a.related_column = b.primary_column;
```

**Common use cases:**
- Employee → Manager relationships
- Category → Subcategory
- Referral systems (user referred by another user)

> Think of it as: "Same table talking to itself using two different aliases."

---

### UNION
Combines results from two SELECT statements **vertically (stacks rows)**. Removes duplicates automatically.

```sql
SELECT column FROM table_a
UNION
SELECT column FROM table_b;
```

### UNION ALL
Same as UNION but **keeps duplicates**.

```sql
SELECT column FROM table_a
UNION ALL
SELECT column FROM table_b;
```

> Key distinction: JOIN combines columns horizontally. UNION combines rows vertically.

---

### Multi-Table JOINs
Joining 3 tables requires 2 JOIN statements. Each JOIN needs its own ON condition.

```sql
SELECT film.title, actor.first_name
FROM film
INNER JOIN film_actor fa
ON film.film_id = fa.film_id
INNER JOIN actor
ON fa.actor_id = actor.actor_id;
```

> SQL builds the result step-by-step internally — first JOIN creates a temporary result, then second JOIN works on that.
> You can only reference columns that actually exist in a table — always check before writing the ON condition.

---

### Table Aliases (Standard in Professional SQL)

```sql
SELECT c.customer_id, p.amount
FROM customer c
INNER JOIN payment p
ON c.customer_id = p.customer_id;
```

**Common aliases used in DVD Rental DB:**

| Alias | Table |
|-------|-------|
| `f` | film |
| `fa` | film_actor |
| `a` | actor |
| `c` | customer |
| `p` | payment |
| `r` | rental |
| `i` | inventory |

---

### Quick Reference — JOIN Types

| JOIN Type | What It Returns | NULL Appears? |
|-----------|----------------|---------------|
| `INNER JOIN` | Only matching rows from both tables | No |
| `LEFT JOIN` | All left rows + matching right rows | Yes (right side) |
| `RIGHT JOIN` | All right rows + matching left rows | Yes (left side) |
| `FULL OUTER JOIN` | All rows from both tables | Yes (both sides) |
| `SELF JOIN` | Rows from same table matched with itself | Possible |
| `UNION` | Stacked rows from both queries, no duplicates | No |
| `UNION ALL` | Stacked rows from both queries, with duplicates | No |

---

### Timestamps and Date/Time Functions
Used to work with date and time data in PostgreSQL.

```sql
-- Get current timestamp
SELECT NOW();

-- Get current date only
SELECT CURRENT_DATE;

-- Get current time only
SELECT CURRENT_TIME;
```

### EXTRACT
Used to pull out a specific part from a timestamp — like year, month, day, hour etc.

```sql
SELECT EXTRACT(YEAR FROM payment_date) AS year
FROM payment;

SELECT EXTRACT(MONTH FROM payment_date) AS month
FROM payment;

SELECT EXTRACT(DAY FROM payment_date) AS day
FROM payment;
```

> Common parts you can extract: YEAR, MONTH, DAY, HOUR, MINUTE, SECOND, DOW (day of week), QUARTER

### TO_CHAR
Used to convert a date or timestamp into a formatted string. Useful for displaying dates in a readable format.

```sql
SELECT TO_CHAR(payment_date, 'MM-DD-YYYY')
FROM payment;

SELECT TO_CHAR(payment_date, 'Month DD, YYYY')
FROM payment;
```

> TO_CHAR gives you full control over how dates appear in your output.

---

### Mathematical Functions and Operators

```sql
-- Basic arithmetic in SELECT
SELECT 10 + 5;   -- Addition
SELECT 10 - 5;   -- Subtraction
SELECT 10 * 5;   -- Multiplication
SELECT 10 / 5;   -- Division
SELECT 10 % 3;   -- Modulo (remainder)

-- Using on columns
SELECT ROUND(rental_rate, 2) FROM film;

SELECT rental_rate * 0.1 AS tax
FROM film;
```

| Function | What It Does |
|----------|-------------|
| `ROUND(x, d)` | Rounds x to d decimal places |
| `CEIL(x)` | Rounds up to nearest integer |
| `FLOOR(x)` | Rounds down to nearest integer |
| `ABS(x)` | Returns absolute (positive) value |

---

### String Functions and Operators

```sql
-- Combine two columns into one
SELECT first_name || ' ' || last_name AS full_name
FROM customer;

-- Convert to uppercase
SELECT UPPER(first_name) FROM customer;

-- Convert to lowercase
SELECT LOWER(first_name) FROM customer;

-- Get length of a string
SELECT LENGTH(first_name) FROM customer;

-- Remove spaces from both sides
SELECT TRIM('  hello  ');

-- Extract part of a string
SELECT LEFT(first_name, 3) FROM customer;
-- Returns first 3 characters
```

| Function | What It Does |
|----------|-------------|
| `\|\|` | Concatenates (joins) strings |
| `UPPER()` | Converts to uppercase |
| `LOWER()` | Converts to lowercase |
| `LENGTH()` | Returns number of characters |
| `TRIM()` | Removes leading/trailing spaces |
| `LEFT(x, n)` | Returns first n characters |
| `RIGHT(x, n)` | Returns last n characters   |
