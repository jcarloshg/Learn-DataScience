# Hola

## Missing

### SQL - Triangle Validation

Use `CASE WHEN` to determine if three given lengths can form a valid triangle.

A valid triangle satisfies the condition that the sum of any two sides is greater than the third side.

```sql
SELECT
    x,
    y,
    z,
    CASE
        WHEN x + y > z
         AND x + z > y
         AND y + z > x THEN 'Yes'
        ELSE 'No'
    END AS triangle
FROM triangle;
```
