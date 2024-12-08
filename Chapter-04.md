# **Chapter 4: Aggregation and Analytical Functions**
---

## **4.1 Aggregate Functions**

Aggregate functions summarize data, typically used with `GROUP BY`.

---

### **4.1.1 COUNT()**

Counts rows or non-`NULL` values in a column.

**Example**: Count the total number of employees.
```sql
SELECT COUNT(*) AS TotalEmployees FROM Employees;
```

**Example**: Count employees in each department.
```sql
SELECT DepartmentID, COUNT(*) AS EmployeeCount
FROM Employees
GROUP BY DepartmentID;
```

---

### **4.1.2 SUM()**

Calculates the total of numeric values.

**Example**: Total salaries in the company.
```sql
SELECT SUM(Salary) AS TotalSalaries FROM Employees;
```

**Example**: Total salaries by department.
```sql
SELECT DepartmentID, SUM(Salary) AS TotalSalaries
FROM Employees
GROUP BY DepartmentID;
```

---

### **4.1.3 AVG()**

Calculates the average of numeric values.

**Example**: Average employee salary.
```sql
SELECT AVG(Salary) AS AverageSalary FROM Employees;
```

---

### **4.1.4 MAX() and MIN()**

Find the highest and lowest values.

**Example**: Maximum and minimum salary in the company.
```sql
SELECT MAX(Salary) AS MaxSalary, MIN(Salary) AS MinSalary
FROM Employees;
```

---

### **4.1.5 GROUP BY**

Groups rows for aggregate calculations.

**Example**: Average salary by department.
```sql
SELECT DepartmentID, AVG(Salary) AS AverageSalary
FROM Employees
GROUP BY DepartmentID;
```

---

### **4.1.6 HAVING Clause**

Filters grouped data. Similar to `WHERE` but works on aggregate results.

**Example**: Departments with total salaries exceeding $500,000.
```sql
SELECT DepartmentID, SUM(Salary) AS TotalSalaries
FROM Employees
GROUP BY DepartmentID
HAVING SUM(Salary) > 500000;
```

---

## **4.2 Window Functions**

Window functions perform calculations across a set of rows related to the current row. They retain row-level details while adding aggregate-like computations.

---

### **4.2.1 PARTITION BY**

Divides rows into partitions for calculations.

**Example**: Rank employees by salary within each department.
```sql
SELECT EmployeeID, FirstName, DepartmentID, 
       RANK() OVER (PARTITION BY DepartmentID ORDER BY Salary DESC) AS Rank
FROM Employees;
```

---

### **4.2.2 OVER Clause**

Defines the window for calculations.

**Example**: Calculate running totals.
```sql
SELECT EmployeeID, Salary, 
       SUM(Salary) OVER (ORDER BY HireDate) AS RunningTotal
FROM Employees;
```

---

### **4.2.3 ROW_NUMBER()**

Assigns a unique sequential number to rows in each partition.

**Example**: Find the most recent hire in each department.
```sql
WITH RankedEmployees AS (
    SELECT EmployeeID, DepartmentID, HireDate, 
           ROW_NUMBER() OVER (PARTITION BY DepartmentID ORDER BY HireDate DESC) AS RowNum
    FROM Employees
)
SELECT EmployeeID, DepartmentID, HireDate
FROM RankedEmployees
WHERE RowNum = 1;
```

---

### **4.2.4 RANK() and DENSE_RANK()**

- `RANK()`: Adds gaps in rank for ties.
- `DENSE_RANK()`: No gaps in ranking.

**Example**: Rank employees by performance score.
```sql
SELECT EmployeeID, PerformanceScore, 
       RANK() OVER (ORDER BY PerformanceScore DESC) AS Rank,
       DENSE_RANK() OVER (ORDER BY PerformanceScore DESC) AS DenseRank
FROM Employees;
```

---

### **4.2.5 LAG() and LEAD()**

Access previous (`LAG()`) or next (`LEAD()`) row values.

**Example**: Compare an employee’s salary with their previous employee's salary.
```sql
SELECT EmployeeID, Salary, 
       LAG(Salary) OVER (ORDER BY HireDate) AS PreviousSalary
FROM Employees;
```

---

### **4.2.6 FIRST_VALUE() and LAST_VALUE()**

Retrieve the first or last value in a partition.

**Example**: First and last hire date in each department.
```sql
SELECT DepartmentID, 
       FIRST_VALUE(HireDate) OVER (PARTITION BY DepartmentID ORDER BY HireDate) AS FirstHireDate,
       LAST_VALUE(HireDate) OVER (PARTITION BY DepartmentID ORDER BY HireDate ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS LastHireDate
FROM Employees;
```

---

## **4.3 Advanced Analytical Techniques**

---

### **4.3.1 Cumulative Calculations**

Calculate cumulative sums or averages.

**Example**: Cumulative salaries ordered by hire date.
```sql
SELECT EmployeeID, Salary, 
       SUM(Salary) OVER (ORDER BY HireDate) AS CumulativeSalary
FROM Employees;
```

---

### **4.3.2 Moving Averages**

Calculate averages for a moving window of rows.

**Example**: 3-month moving average of sales.
```sql
SELECT SaleDate, SalesAmount, 
       AVG(SalesAmount) OVER (ORDER BY SaleDate ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS MovingAvg
FROM Sales;
```

---

### **4.3.3 Running Totals**

Cumulative totals within partitions.

**Example**: Running totals by department.
```sql
SELECT DepartmentID, EmployeeID, Salary, 
       SUM(Salary) OVER (PARTITION BY DepartmentID ORDER BY HireDate) AS RunningTotal
FROM Employees;
```

---

### **4.3.4 Percentile Calculations**

Rank rows based on percentile.

**Example**: Calculate percentiles for salaries.
```sql
SELECT EmployeeID, Salary, 
       PERCENT_RANK() OVER (ORDER BY Salary) AS Percentile
FROM Employees;
```

---
