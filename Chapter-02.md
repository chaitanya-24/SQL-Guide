
# **Chapter 2: Basic SQL Operations**

---

## **2.1 Data Retrieval**

Retrieving data from a database is one of the most fundamental tasks. SQL provides the `SELECT` statement to query specific data.

---

### **2.1.1 SELECT Statement**

The `SELECT` statement fetches data from one or more tables. The simplest form:
```sql
SELECT column1, column2 FROM table_name;
```

**Example**: Retrieve names and hire dates from the `Employees` table.
```sql
SELECT FirstName, HireDate FROM Employees;
```

**Explanation**: This statement fetches two columns (`FirstName` and `HireDate`) from the `Employees` table.

---

### **2.1.2 Filtering Data (WHERE Clause)**

Use the `WHERE` clause to filter rows based on conditions.
```sql
SELECT * FROM Employees WHERE HireDate > '2022-01-01';
```

**Explanation**: Retrieves all employees hired after January 1, 2022.

**Comparison Operators**: `=`, `!=`, `<`, `>`, `<=`, `>=`.  
**Logical Operators**: `AND`, `OR`, `NOT`.  

**Example**: Combine conditions.
```sql
SELECT * FROM Employees 
WHERE Department = 'Sales' AND HireDate BETWEEN '2020-01-01' AND '2023-01-01';
```

---

### **2.1.3 Sorting Data (ORDER BY Clause)**

The `ORDER BY` clause sorts the results.
```sql
SELECT FirstName, HireDate 
FROM Employees 
ORDER BY HireDate DESC;
```

**Explanation**: Retrieves employees’ first names and hire dates, sorted by the latest hire date.

---

### **2.1.4 Limiting Results (LIMIT/TOP)**

Limit the number of returned rows with `LIMIT` (MySQL/PostgreSQL) or `TOP` (SQL Server).

**Example**: Return the first 5 rows.
```sql
SELECT * FROM Employees LIMIT 5;
-- OR for SQL Server
SELECT TOP 5 * FROM Employees;
```

---

## **2.2 Data Manipulation**

Data manipulation involves adding, updating, and deleting records in a database.

---

### **2.2.1 INSERT Statement**

Add rows to a table using the `INSERT` statement.
```sql
INSERT INTO Employees (EmployeeID, FirstName, LastName, HireDate) 
VALUES (1, 'John', 'Doe', '2023-01-15');
```

**Explanation**: Inserts a record for John Doe with a hire date of January 15, 2023.

**Bulk Insert**:
```sql
INSERT INTO Employees (EmployeeID, FirstName, LastName, HireDate) 
VALUES 
    (2, 'Alice', 'Smith', '2022-05-01'),
    (3, 'Bob', 'Johnson', '2021-11-20');
```

---

### **2.2.2 UPDATE Statement**

Update existing rows.
```sql
UPDATE Employees 
SET LastName = 'Taylor', Department = 'HR' 
WHERE EmployeeID = 1;
```

**Explanation**: Updates John Doe's last name and department.

---

### **2.2.3 DELETE Statement**

Remove rows from a table.
```sql
DELETE FROM Employees WHERE EmployeeID = 3;
```

**Explanation**: Deletes the record of the employee with `EmployeeID = 3`.

---

### **2.2.4 MERGE Statement (Upserts)**

The `MERGE` statement combines insert, update, and delete operations based on conditions. Not all databases support `MERGE`.

**Example**: Synchronize two tables (`Source` and `Target`).
```sql
MERGE INTO Target AS t
USING Source AS s
ON t.EmployeeID = s.EmployeeID
WHEN MATCHED THEN 
    UPDATE SET t.FirstName = s.FirstName, t.LastName = s.LastName
WHEN NOT MATCHED BY TARGET THEN 
    INSERT (EmployeeID, FirstName, LastName) 
    VALUES (s.EmployeeID, s.FirstName, s.LastName)
WHEN NOT MATCHED BY SOURCE THEN 
    DELETE;
```

**Explanation**:  
- Updates `Target` when IDs match.  
- Inserts new rows from `Source` into `Target`.  
- Deletes rows from `Target` not in `Source`.

---
