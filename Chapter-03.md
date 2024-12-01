# **Chapter 3: Advanced Querying Techniques**

---

## **3.1 Joins**

Joins allow you to retrieve data from multiple tables based on relationships between them. They are a cornerstone of relational database querying.

### **3.1.1 INNER JOIN**

Returns rows with matching values in both tables.

**Example**: Retrieve employee names and their corresponding department names.
```sql
SELECT e.FirstName, e.LastName, d.DepartmentName
FROM Employees e
INNER JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```

**Explanation**: Combines `Employees` and `Departments` tables based on `DepartmentID`.

---

### **3.1.2 LEFT JOIN**

Returns all rows from the left table and matching rows from the right table. Unmatched rows in the right table will return `NULL`.

**Example**: Retrieve all employees and their department names, including employees not assigned to a department.
```sql
SELECT e.FirstName, e.LastName, d.DepartmentName
FROM Employees e
LEFT JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```

---

### **3.1.3 RIGHT JOIN**

Returns all rows from the right table and matching rows from the left table. Unmatched rows in the left table will return `NULL`.

**Example**: Retrieve all departments and their employees, including departments with no employees.
```sql
SELECT d.DepartmentName, e.FirstName, e.LastName
FROM Departments d
RIGHT JOIN Employees e ON e.DepartmentID = d.DepartmentID;
```

---

### **3.1.4 FULL OUTER JOIN**

Returns all rows when there is a match in either table. Rows without a match will include `NULL`.

**Example**: Retrieve all employees and departments, showing relationships where they exist.
```sql
SELECT e.FirstName, e.LastName, d.DepartmentName
FROM Employees e
FULL OUTER JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```

---

### **3.1.5 CROSS JOIN**

Combines all rows from both tables (Cartesian product).

**Example**: Match all employees with all departments.
```sql
SELECT e.FirstName, d.DepartmentName
FROM Employees e
CROSS JOIN Departments d;
```

---

### **3.1.6 SELF JOIN**

A table joins itself to find relationships within its rows.

**Example**: Retrieve managers and their direct reports.
```sql
SELECT e1.FirstName AS Employee, e2.FirstName AS Manager
FROM Employees e1
INNER JOIN Employees e2 ON e1.ManagerID = e2.EmployeeID;
```

---

## **3.2 Subqueries**

Subqueries are queries nested within another query.

### **3.2.1 Nested Subqueries**

Subqueries that provide intermediate results for the main query.

**Example**: Retrieve employees with salaries above the company average.
```sql
SELECT FirstName, LastName
FROM Employees
WHERE Salary > (SELECT AVG(Salary) FROM Employees);
```

---

### **3.2.2 Correlated Subqueries**

Subqueries that reference columns from the outer query.

**Example**: Retrieve employees with salaries higher than their department's average.
```sql
SELECT FirstName, LastName
FROM Employees e1
WHERE Salary > (
    SELECT AVG(Salary)
    FROM Employees e2
    WHERE e1.DepartmentID = e2.DepartmentID
);
```

---

### **3.2.3 Scalar Subqueries**

Return a single value.

**Example**: Retrieve the highest-paid employee’s name.
```sql
SELECT FirstName, LastName
FROM Employees
WHERE Salary = (SELECT MAX(Salary) FROM Employees);
```

---

### **3.2.4 Table Subqueries**

Subqueries that return a table.

**Example**: Use a subquery to create a derived table.
```sql
SELECT DepartmentID, AVG(Salary) AS AvgSalary
FROM (
    SELECT DepartmentID, Salary
    FROM Employees
) AS Sub
GROUP BY DepartmentID;
```

---

## **3.3 Set Operations**

Set operations combine results of two or more queries.

### **3.3.1 UNION**

Combines results, removing duplicates.

**Example**: Retrieve all unique job titles from two tables.
```sql
SELECT JobTitle FROM Employees
UNION
SELECT JobTitle FROM Contractors;
```

---

### **3.3.2 UNION ALL**

Combines results without removing duplicates.

**Example**: Retrieve all job titles, including duplicates.
```sql
SELECT JobTitle FROM Employees
UNION ALL
SELECT JobTitle FROM Contractors;
```

---

### **3.3.3 INTERSECT**

Returns only rows common to both queries.

**Example**: Retrieve job titles that exist in both tables.
```sql
SELECT JobTitle FROM Employees
INTERSECT
SELECT JobTitle FROM Contractors;
```

---

### **3.3.4 EXCEPT (MINUS)**

Returns rows from the first query that are not in the second.

**Example**: Retrieve job titles in `Employees` but not in `Contractors`.
```sql
SELECT JobTitle FROM Employees
EXCEPT
SELECT JobTitle FROM Contractors;
```

---
