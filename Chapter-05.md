# **Chapter 5: Data Transformation**

---

Data transformation involves reshaping and preparing data for analysis or presentation. SQL provides robust tools for string manipulation, date/time handling, and type conversion.

---

## **5.1 String Manipulation**

Working with textual data often requires cleaning, formatting, and extracting substrings.

---

### **5.1.1 Concatenation**

Combine strings from multiple columns or literals.

**Example**: Concatenate first and last names into a full name.
```sql
SELECT FirstName || ' ' || LastName AS FullName -- PostgreSQL, SQLite
FROM Employees;

-- For MySQL or SQL Server
SELECT CONCAT(FirstName, ' ', LastName) AS FullName
FROM Employees;
```

---

### **5.1.2 Substring Extraction**

Extract specific parts of a string using `SUBSTRING`.

**Example**: Extract the first three characters of an employee's first name.
```sql
SELECT SUBSTRING(FirstName, 1, 3) AS ShortName -- MySQL, SQL Server
FROM Employees;

-- PostgreSQL
SELECT SUBSTR(FirstName, 1, 3) AS ShortName 
FROM Employees;
```

---

### **5.1.3 Pattern Matching**

Find strings matching specific patterns using `LIKE` or `REGEXP`.

**Example**: Find employees with names starting with 'A'.
```sql
SELECT * 
FROM Employees
WHERE FirstName LIKE 'A%';
```

**Example**: Use a regular expression to find names containing digits.
```sql
SELECT * 
FROM Employees
WHERE FirstName REGEXP '[0-9]'; -- MySQL
```

---

### **5.1.4 String Functions**

SQL provides various string functions to manipulate text.

- **UPPER()**: Converts text to uppercase.
- **LOWER()**: Converts text to lowercase.
- **TRIM()**: Removes leading/trailing spaces.
- **REPLACE()**: Replaces substrings.

**Example**: Standardize employee names by trimming spaces and converting to uppercase.
```sql
SELECT UPPER(TRIM(FirstName)) AS StandardizedName
FROM Employees;
```

---

## **5.2 Date and Time Functions**

Dates and times are essential in many datasets, requiring proper handling for calculations and formatting.

---

### **5.2.1 Date Formatting**

Display dates in custom formats.

**Example**: Format a hire date as `MM/DD/YYYY`.
```sql
-- MySQL
SELECT DATE_FORMAT(HireDate, '%m/%d/%Y') AS FormattedDate
FROM Employees;

-- PostgreSQL
SELECT TO_CHAR(HireDate, 'MM/DD/YYYY') AS FormattedDate
FROM Employees;
```

---

### **5.2.2 Date Arithmetic**

Perform calculations on dates.

**Example**: Find employees hired in the last 90 days.
```sql
SELECT * 
FROM Employees
WHERE HireDate >= CURDATE() - INTERVAL 90 DAY; -- MySQL

-- PostgreSQL
SELECT * 
FROM Employees
WHERE HireDate >= CURRENT_DATE - INTERVAL '90 days';
```

---

### **5.2.3 Extracting Date Components**

Extract year, month, day, etc., from a date.

**Example**: Extract the year of hire.
```sql
SELECT EXTRACT(YEAR FROM HireDate) AS HireYear -- PostgreSQL
FROM Employees;

-- MySQL/SQL Server
SELECT YEAR(HireDate) AS HireYear
FROM Employees;
```

---

### **5.2.4 Time Zone Handling**

Handle time zones in timestamp data.

**Example**: Convert a UTC timestamp to a specific time zone.
```sql
-- PostgreSQL
SELECT HireDate AT TIME ZONE 'UTC' AT TIME ZONE 'America/New_York' AS LocalTime
FROM Employees;
```

---

### **5.2.5 Date Comparison**

Compare dates directly.

**Example**: Find employees hired before 2020.
```sql
SELECT * 
FROM Employees
WHERE HireDate < '2020-01-01';
```

---

## **5.3 Type Conversion**

---

### **5.3.1 CAST()**

Explicitly convert data types.

**Example**: Convert a string to a date.
```sql
SELECT CAST('2023-12-01' AS DATE) AS ConvertedDate;
```

---

### **5.3.2 Converting Between Data Types**

Use `CAST()` or database-specific functions for type conversion.

**Example**: Convert salary to a string.
```sql
SELECT CAST(Salary AS CHAR) AS SalaryString -- MySQL
FROM Employees;

-- PostgreSQL
SELECT Salary::TEXT AS SalaryString 
FROM Employees;
```

---

### **5.3.3 Handling NULL Values**

Use `COALESCE()` or `ISNULL()` to handle `NULL` values.

**Example**: Replace `NULL` with a default value.
```sql
SELECT COALESCE(Department, 'Not Assigned') AS DepartmentName -- PostgreSQL, MySQL
FROM Employees;

-- SQL Server
SELECT ISNULL(Department, 'Not Assigned') AS DepartmentName 
FROM Employees;
```

---
