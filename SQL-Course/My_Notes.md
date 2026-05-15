# 📘 SQL Study Notes
**Database:** PostgreSQL | **Tool:** pgAdmin

---

## 📝 Daily Log
* [2026-02-01]: Setup GitHub Portfolio. Installed PostgreSQL.
* [2026-05-12]: Learned AS keyword — aliasing columns in SELECT
* [2026-05-15]: Learned BETWEEN, IN, LIKE, ILIKE — pattern matching and filtering in WHERE clause

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

### Quick Reference

| Operator | Use Case | Case Sensitive? |
|----------|----------|----------------|
| `BETWEEN` | Range of values | Depends on data type |
| `IN` | List of exact values | Depends on data type |
| `LIKE` | Pattern matching | Yes |
| `ILIKE` | Pattern matching | No (PostgreSQL only) |
