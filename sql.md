# SQL Interview Preparation Notes

---

## Core Concepts to Remember

- **Primary Key**: Uniquely identifies a record in a table.
- **Foreign Key**: Links two tables together.
- **Normalization**: Reduces redundancy in the database.
- **Indexes**: Improve query performance but may slow down INSERT/UPDATE.
- **ACID Properties**: Atomicity, Consistency, Isolation, Durability.
- **Joins**: Combine data from multiple tables.
- **Window Functions**: Perform calculations across a set of table rows.
- **Transactions**: Ensure data integrity during multiple operations.

---

## 20 Important SQL Interview Questions with Answers

### 1. Difference between INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL OUTER JOIN
- **INNER JOIN**: Returns only matching rows.
- **LEFT JOIN**: Returns all rows from the left table + matching rows from the right.
- **RIGHT JOIN**: Returns all rows from the right table + matching rows from the left.
- **FULL OUTER JOIN**: Returns all rows from both tables, matched or unmatched.

### 2. Query to fetch second highest salary
```sql
SELECT MAX(salary) AS SecondHighest
FROM Employee
WHERE salary < (SELECT MAX(salary) FROM Employee);
```
Or using ROW_NUMBER():
```sql
SELECT salary 
FROM (
  SELECT salary, ROW_NUMBER() OVER (ORDER BY salary DESC) rn 
  FROM Employee
) t 
WHERE rn = 2;
```

### 3. Difference between WHERE and HAVING
- `WHERE` filters rows **before GROUP BY**.
- `HAVING` filters rows **after GROUP BY**.

### 4. Find employees with duplicate salaries
```sql
SELECT salary, COUNT(*) 
FROM Employee 
GROUP BY salary 
HAVING COUNT(*) > 1;
```

### 5. Query to fetch Nth highest salary
```sql
SELECT salary 
FROM (
  SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) as rnk
  FROM Employee
) t 
WHERE rnk = N;
```

### 6. Difference between UNION and UNION ALL
- `UNION` removes duplicates.
- `UNION ALL` keeps duplicates (faster).

### 7. Department with maximum employees
```sql
SELECT dept_id, COUNT(*) as emp_count
FROM Employee
GROUP BY dept_id
ORDER BY emp_count DESC
LIMIT 1;
```

### 8. Clustered vs Non-Clustered Index
- **Clustered**: Data is physically stored in index order (only one per table).
- **Non-Clustered**: Separate structure pointing to data (multiple allowed).

### 9. Employees not assigned to any project
```sql
SELECT e.emp_id, e.name
FROM Employee e
LEFT JOIN Project p ON e.emp_id = p.emp_id
WHERE p.emp_id IS NULL;
```

### 10. Normalization (1NF, 2NF, 3NF)
- **1NF**: Atomic values, no repeating groups.
- **2NF**: No partial dependency (all columns depend on PK).
- **3NF**: No transitive dependency.

### 11. Self Join Example
```sql
SELECT e1.name AS Emp, e2.name AS Manager
FROM Employee e1
JOIN Employee e2 ON e1.manager_id = e2.emp_id;
```

### 12. Find duplicate records
```sql
SELECT name, COUNT(*) 
FROM Employee 
GROUP BY name 
HAVING COUNT(*) > 1;
```

### 13. Window Function Example
```sql
SELECT emp_id, salary, 
       RANK() OVER (ORDER BY salary DESC) as rank
FROM Employee;
```

### 14. Transaction Example
```sql
BEGIN;
UPDATE Account SET balance = balance - 100 WHERE acc_id = 1;
UPDATE Account SET balance = balance + 100 WHERE acc_id = 2;
COMMIT;
```

### 15. Tips to Optimize SQL Queries
- Use indexes appropriately.
- Avoid `SELECT *`.
- Prefer EXISTS over IN for subqueries.
- Partition large tables.
- Use EXPLAIN plan to analyze queries.
- Denormalize for heavy read operations.

---

## Additional Join Questions

### 16. Find employees and their department names
```sql
SELECT e.name, d.dept_name
FROM Employee e
JOIN Department d ON e.dept_id = d.dept_id;
```

### 17. Employees who have not submitted projects
```sql
SELECT e.name
FROM Employee e
LEFT JOIN Project p ON e.emp_id = p.emp_id
WHERE p.emp_id IS NULL;
```

### 18. Find all pairs of employees who share the same manager
```sql
SELECT e1.name AS Employee1, e2.name AS Employee2, e1.manager_id
FROM Employee e1
JOIN Employee e2 ON e1.manager_id = e2.manager_id AND e1.emp_id < e2.emp_id;
```

### 19. Fetch departments with no employees
```sql
SELECT d.dept_name
FROM Department d
LEFT JOIN Employee e ON d.dept_id = e.dept_id
WHERE e.emp_id IS NULL;
```

### 20. Count employees per project using RIGHT JOIN
```sql
SELECT p.project_name, COUNT(e.emp_id) as emp_count
FROM Employee e
RIGHT JOIN Project p ON e.project_id = p.project_id
GROUP BY p.project_name;
```

---

## Quick Tips for Interviews
- Always explain **trade-offs** (indexes, joins, performance).  
- Know aggregate functions: `SUM, COUNT, AVG, MIN, MAX`.  
- Be clear on **JOIN types** (most frequently asked).  
- Practice **Top-N queries** and **duplicate detection**.  
- Revise **transactions & isolation levels**.  
- Review **CTEs, set operations (UNION, INTERSECT)** for advanced questions.

---

**End of SQL Interview Notes**

