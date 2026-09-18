# SQL Lessons Summary

## Lesson 1: GROUP BY y Agregaciones

### Count employees by department
```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department
ORDER BY department;
```

### Best Practices for GROUP BY y Agregaciones

1. **Always include all non-aggregated columns in GROUP BY**
   - Every column in SELECT that is NOT an aggregation function (COUNT, SUM, AVG, etc.) MUST appear in GROUP BY.

2. **Use HAVING to filter after grouping, WHERE to filter before**
   - WHERE is applied BEFORE GROUP BY (filters individual rows).
   - HAVING is applied AFTER GROUP BY (filters already formed groups).

3. **Favor WHERE over HAVING when possible**
   - Filtering with WHERE reduces rows before aggregation, improving performance.

4. **Use consistent aggregation expressions**
   - COUNT(*): counts all rows including NULLs.
   - COUNT(column): counts only non-null values.
   - COUNT(DISTINCT column): counts unique non-null values.
   - SUM(col) / AVG(col) ignore NULLs.

5. **Execution order vs. writing order**
   ```
   1. FROM / JOIN     → data is loaded
   2. WHERE           → rows are filtered
   3. GROUP BY        → groups are formed
   4. HAVING          → groups are filtered
   5. SELECT          → aggregations are calculated
   6. ORDER BY        → results are sorted
   7. LIMIT / OFFSET  → pagination
   ```

6. **Avoid heavy functions inside GROUP BY**
   - Pre-process with CTEs or subqueries.

7. **Aggregations over JOINs: beware of multiplicative effect**
   - A JOIN can duplicate rows before aggregation. Solution: aggregate first in a subquery or CTE, then JOIN.

8. **NULLs in GROUP BY**
   - NULL values are grouped together as one group.
   - Use COALESCE(col, 'N/A') to treat NULLs as visible category.

9. **Limit results with ORDER BY + LIMIT**
   - For top-N per group, use window functions (RANK() OVER (PARTITION BY ...)).

10. **Use column aliases for readability**
    - Name your aggregations: SUM(amount) AS total_amount.

---

## Lesson 2: CTEs (Common Table Expressions)

### Average salary by department using WITH
```sql
WITH department_avg AS (
    SELECT department, AVG(salary) AS avg_salary 
    FROM employees 
    GROUP BY department
)
SELECT department, avg_salary 
FROM department_avg 
ORDER BY department;
```

### Key Points about CTEs
- CTEs (Common Table Expressions) use the WITH clause
- They organize complex queries into readable named subqueries
- The CTE is defined once, then can be referenced multiple times in the main query
- They improve readability for intermediate calculations that will be reused
