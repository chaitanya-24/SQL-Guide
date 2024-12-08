# **Chapter 6: Performance Optimization**

---

Performance optimization ensures SQL queries run efficiently, minimizing resource use and execution time. This chapter covers indexing, query optimization, and database tuning strategies.

---

## **6.1 Indexing Strategies**

Indexes enhance query performance by speeding up data retrieval. However, they come with trade-offs, such as slower `INSERT`, `UPDATE`, and `DELETE` operations due to index maintenance.

---

### **6.1.1 B-Tree Indexes**

Default index type in most databases. Suitable for equality and range queries.

**Example**: Create an index on the `LastName` column.
```sql
CREATE INDEX idx_lastname ON Employees(LastName);
```

**Usage**: Efficient for queries like:
```sql
SELECT * 
FROM Employees
WHERE LastName = 'Smith';
```

---

### **6.1.2 Bitmap Indexes**

Used for columns with low cardinality (few distinct values). Common in data warehouses.

**Example**: Create a bitmap index on a `Gender` column.
```sql
CREATE BITMAP INDEX idx_gender ON Employees(Gender); -- Oracle
```

---

### **6.1.3 Clustered vs Non-Clustered Indexes**

- **Clustered**: Data stored in index order. Each table can have only one clustered index.
- **Non-Clustered**: Points to data stored separately.

**Example**: Create a clustered index on `EmployeeID`.
```sql
CREATE CLUSTERED INDEX idx_employeeid ON Employees(EmployeeID); -- SQL Server
```

---

### **6.1.4 Composite Indexes**

Indexes on multiple columns. Useful for queries filtering by multiple conditions.

**Example**: Create a composite index on `DepartmentID` and `HireDate`.
```sql
CREATE INDEX idx_dept_hiredate ON Employees(DepartmentID, HireDate);
```

**Usage**: Efficient for:
```sql
SELECT * 
FROM Employees
WHERE DepartmentID = 5 AND HireDate > '2023-01-01';
```

---

### **6.1.5 Partial Indexes**

Indexes only a subset of rows based on conditions.

**Example**: Index employees hired after 2020.
```sql
CREATE INDEX idx_recent_hires ON Employees(HireDate)
WHERE HireDate > '2020-01-01'; -- PostgreSQL
```

---

## **6.2 Query Optimization**

Query optimization reduces execution time by restructuring queries or analyzing execution plans.

---

### **6.2.1 Execution Plan Analysis**

Execution plans show how a query is executed, highlighting bottlenecks.

**Example**: Analyze a query plan.
```sql
EXPLAIN SELECT * FROM Employees WHERE DepartmentID = 5;
```

**Output**: Look for:
- **Index Scan**: Efficient.
- **Full Table Scan**: Avoid when possible.

---

### **6.2.2 Query Restructuring**

Improve queries by avoiding inefficiencies.

- **Avoid `SELECT *`**: Fetch only required columns.
```sql
SELECT FirstName, LastName FROM Employees WHERE DepartmentID = 5;
```

- **Use Joins Instead of Subqueries**:
```sql
-- Subquery
SELECT FirstName 
FROM Employees 
WHERE DepartmentID IN (SELECT DepartmentID FROM Departments WHERE Region = 'North');

-- Join
SELECT e.FirstName
FROM Employees e
JOIN Departments d ON e.DepartmentID = d.DepartmentID
WHERE d.Region = 'North';
```

---

### **6.2.3 Common Pitfalls to Avoid**

- **Using Functions on Indexed Columns**: Avoid applying functions in `WHERE` clauses.
```sql
-- Avoid
SELECT * FROM Employees WHERE YEAR(HireDate) = 2023;

-- Use
SELECT * FROM Employees WHERE HireDate BETWEEN '2023-01-01' AND '2023-12-31';
```

- **Inefficient Wildcards**:
```sql
-- Avoid leading wildcard
SELECT * FROM Employees WHERE LastName LIKE '%Smith';

-- Use
SELECT * FROM Employees WHERE LastName LIKE 'Smith%';
```

---

### **6.2.4 Using `EXPLAIN` or `EXPLAIN ANALYZE`**

Evaluate query performance and adjust indexes or structure.

**Example**: Analyze query costs in PostgreSQL.
```sql
EXPLAIN ANALYZE SELECT * FROM Employees WHERE DepartmentID = 5;
```

---

## **6.3 Database Tuning**

Database tuning improves overall system performance by optimizing schema, storage, and partitioning.

---

### **6.3.1 Partitioning**

Divide large tables into smaller, more manageable segments.

- **Range Partitioning**: Partition based on a range of values.
```sql
CREATE TABLE Employees_2023 PARTITION OF Employees 
FOR VALUES FROM ('2023-01-01') TO ('2023-12-31'); -- PostgreSQL
```

- **Hash Partitioning**: Partition by hashing a column.
```sql
CREATE TABLE Employees PARTITION BY HASH(DepartmentID); -- PostgreSQL
```

---

### **6.3.2 Sharding**

Horizontal partitioning across multiple servers for scalability.

**Example**: Split `Customers` table into shards based on `CustomerID`.
- Shard 1: `CustomerID % 2 = 0`
- Shard 2: `CustomerID % 2 = 1`

---

### **6.3.3 Denormalization Techniques**

Combine tables or pre-compute data to improve read performance.

- **Example**: Store department names in the `Employees` table to avoid joins.
```sql
-- Before Denormalization
SELECT e.FirstName, d.DepartmentName
FROM Employees e
JOIN Departments d ON e.DepartmentID = d.DepartmentID;

-- After Denormalization
SELECT FirstName, DepartmentName FROM Employees;
```

---
