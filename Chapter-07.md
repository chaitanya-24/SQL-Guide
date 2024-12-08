# **Chapter 7: Advanced SQL Features**

---

Advanced SQL features allow you to write complex, reusable, and efficient queries. These include Common Table Expressions (CTEs), stored procedures, and triggers.

---

## **7.1 Common Table Expressions (CTEs)**

A Common Table Expression (CTE) provides a temporary result set that can be referenced within a query. It enhances readability and reusability.

---

### **7.1.1 Basic CTEs**

**Example**: Use a CTE to calculate average salaries by department and filter departments where the average exceeds $50,000.
```sql
WITH AvgSalaries AS (
    SELECT DepartmentID, AVG(Salary) AS AvgSalary
    FROM Employees
    GROUP BY DepartmentID
)
SELECT DepartmentID, AvgSalary
FROM AvgSalaries
WHERE AvgSalary > 50000;
```

---

### **7.1.2 Recursive CTEs**

Recursive CTEs are used for hierarchical data like organizational charts or directory structures.

**Example**: Find all subordinates of a specific employee in an organization.
```sql
WITH RECURSIVE Subordinates AS (
    SELECT EmployeeID, ManagerID
    FROM Employees
    WHERE EmployeeID = 1 -- Start with a specific manager
    UNION ALL
    SELECT e.EmployeeID, e.ManagerID
    FROM Employees e
    INNER JOIN Subordinates s ON e.ManagerID = s.EmployeeID
)
SELECT EmployeeID
FROM Subordinates;
```

---

### **7.1.3 Multiple CTEs**

You can define multiple CTEs and use them in the main query.

**Example**: Calculate average and maximum salaries by department using two CTEs.
```sql
WITH AvgSalaries AS (
    SELECT DepartmentID, AVG(Salary) AS AvgSalary
    FROM Employees
    GROUP BY DepartmentID
), MaxSalaries AS (
    SELECT DepartmentID, MAX(Salary) AS MaxSalary
    FROM Employees
    GROUP BY DepartmentID
)
SELECT a.DepartmentID, a.AvgSalary, m.MaxSalary
FROM AvgSalaries a
JOIN MaxSalaries m ON a.DepartmentID = m.DepartmentID;
```

---

### **7.1.4 Replacing Subqueries with CTEs**

CTEs improve the readability of nested subqueries.

**Example**: Use a CTE to simplify a query that calculates department revenue.
```sql
WITH DepartmentRevenue AS (
    SELECT DepartmentID, SUM(SalesAmount) AS TotalRevenue
    FROM Sales
    GROUP BY DepartmentID
)
SELECT *
FROM DepartmentRevenue
WHERE TotalRevenue > 100000;
```

---

## **7.2 Stored Procedures**

Stored procedures encapsulate SQL logic for reuse and modularity. They improve performance by reducing network traffic and centralizing logic.

---

### **7.2.1 Creating Procedures**

**Example**: Create a procedure to insert a new employee.
```sql
CREATE PROCEDURE AddEmployee (
    IN FirstName VARCHAR(50),
    IN LastName VARCHAR(50),
    IN DepartmentID INT,
    IN Salary DECIMAL(10, 2)
)
BEGIN
    INSERT INTO Employees (FirstName, LastName, DepartmentID, Salary)
    VALUES (FirstName, LastName, DepartmentID, Salary);
END;
```

---

### **7.2.2 Input and Output Parameters**

Stored procedures can accept inputs and return outputs.

**Example**: Procedure to calculate total salary for a department.
```sql
CREATE PROCEDURE GetDepartmentSalary (
    IN DeptID INT,
    OUT TotalSalary DECIMAL(10, 2)
)
BEGIN
    SELECT SUM(Salary) INTO TotalSalary
    FROM Employees
    WHERE DepartmentID = DeptID;
END;
```

**Usage**:
```sql
CALL GetDepartmentSalary(5, @TotalSalary);
SELECT @TotalSalary;
```

---

### **7.2.3 Error Handling**

Handle errors gracefully within procedures.

**Example**: Procedure with error handling.
```sql
CREATE PROCEDURE SafeInsert (
    IN EmployeeName VARCHAR(50),
    IN DepartmentID INT
)
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Insertion failed.';
    END;

    START TRANSACTION;
    INSERT INTO Employees (FirstName, DepartmentID) VALUES (EmployeeName, DepartmentID);
    COMMIT;
END;
```

---

### **7.2.4 Transaction Management**

Stored procedures support transaction control.

**Example**: Procedure to transfer funds between accounts.
```sql
CREATE PROCEDURE TransferFunds (
    IN SourceAccount INT,
    IN TargetAccount INT,
    IN Amount DECIMAL(10, 2)
)
BEGIN
    START TRANSACTION;
    UPDATE Accounts SET Balance = Balance - Amount WHERE AccountID = SourceAccount;
    UPDATE Accounts SET Balance = Balance + Amount WHERE AccountID = TargetAccount;
    COMMIT;
END;
```

---

## **7.3 Triggers**

Triggers automatically execute predefined actions in response to specific events, such as `INSERT`, `UPDATE`, or `DELETE`.

---

### **7.3.1 BEFORE Triggers**

Execute before a specified event.

**Example**: Validate salary before insertion.
```sql
CREATE TRIGGER ValidateSalary
BEFORE INSERT ON Employees
FOR EACH ROW
BEGIN
    IF NEW.Salary < 0 THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Salary must be non-negative.';
    END IF;
END;
```

---

### **7.3.2 AFTER Triggers**

Execute after a specified event.

**Example**: Log changes to employee salaries.
```sql
CREATE TRIGGER LogSalaryChange
AFTER UPDATE ON Employees
FOR EACH ROW
BEGIN
    INSERT INTO SalaryLog (EmployeeID, OldSalary, NewSalary, ChangeDate)
    VALUES (OLD.EmployeeID, OLD.Salary, NEW.Salary, NOW());
END;
```

---

### **7.3.3 Trigger Use Cases**

Triggers are used for:
- Auditing changes.
- Enforcing business rules.
- Maintaining derived tables.

**Example**: Automatically update stock levels in an inventory system.

---

### **7.3.4 Trigger Limitations**

- Performance overhead for frequent events.
- Difficult to debug and maintain.
- Avoid complex logic within triggers.

---
