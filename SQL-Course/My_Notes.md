# 📘 SQL Study Notes
**Database:** PostgreSQL | **Tool:** pgAdmin

---

## 📝 Daily Log

* [2026-02-01]: Setup GitHub Portfolio. Installed PostgreSQL.
* [2026-05-12]: Learned AS keyword — aliasing columns in SELECT

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
