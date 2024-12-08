# **Chapter 8: Data Science and Machine Learning SQL**

---

SQL plays a critical role in data science and machine learning, particularly in data preparation, feature engineering, and statistical analysis. This chapter focuses on SQL techniques for handling these tasks.

---

## **8.1 Statistical Functions**

Statistical functions in SQL help summarize and analyze datasets.

---

### **8.1.1 Variance and Standard Deviation**

- **`VARIANCE()`**: Measures data spread.
- **`STDDEV()`**: Measures the standard deviation.

**Example**: Calculate variance and standard deviation of employee salaries.
```sql
SELECT 
    VARIANCE(Salary) AS SalaryVariance,
    STDDEV(Salary) AS SalaryStdDev
FROM Employees;
```

---

### **8.1.2 Correlation Calculations**

Correlation measures the linear relationship between two variables. SQL engines like PostgreSQL and Hive support correlation functions.

**Example**: Correlation between employee age and salary.
```sql
SELECT CORR(Age, Salary) AS AgeSalaryCorrelation
FROM Employees;
```

---

### **8.1.3 Regression Analysis in SQL**

Linear regression can be performed using SQL for basic predictive analytics.

**Example**: Find coefficients for a simple linear regression of `Salary` on `Experience`.
```sql
SELECT
    REGR_SLOPE(Salary, Experience) AS Slope,
    REGR_INTERCEPT(Salary, Experience) AS Intercept
FROM Employees;
```

---

## **8.2 Feature Engineering**

Feature engineering transforms raw data into meaningful features for machine learning models.

---

### **8.2.1 One-Hot Encoding**

Convert categorical variables into binary columns.

**Example**: One-hot encode a `JobRole` column.
```sql
SELECT 
    EmployeeID,
    CASE WHEN JobRole = 'Manager' THEN 1 ELSE 0 END AS IsManager,
    CASE WHEN JobRole = 'Developer' THEN 1 ELSE 0 END AS IsDeveloper,
    CASE WHEN JobRole = 'Analyst' THEN 1 ELSE 0 END AS IsAnalyst
FROM Employees;
```

---

### **8.2.2 Binning**

Group numerical values into discrete ranges.

**Example**: Bin salaries into low, medium, and high categories.
```sql
SELECT 
    EmployeeID,
    CASE 
        WHEN Salary < 40000 THEN 'Low'
        WHEN Salary BETWEEN 40000 AND 80000 THEN 'Medium'
        ELSE 'High'
    END AS SalaryRange
FROM Employees;
```

---

### **8.2.3 Handling Categorical Variables**

Convert text categories into numeric values.

**Example**: Assign numeric codes to departments.
```sql
SELECT 
    EmployeeID,
    CASE 
        WHEN Department = 'HR' THEN 1
        WHEN Department = 'Engineering' THEN 2
        WHEN Department = 'Marketing' THEN 3
    END AS DepartmentCode
FROM Employees;
```

---

### **8.2.4 Time-Based Feature Creation**

Extract meaningful time-based features for analysis.

**Example**: Calculate the tenure of employees.
```sql
SELECT 
    EmployeeID,
    DATEDIFF(CURRENT_DATE, HireDate) / 365.0 AS TenureInYears
FROM Employees;
```

---

## **8.3 Machine Learning Preparation**

Preparing datasets for machine learning involves sampling, splitting data, and preprocessing.

---

### **8.3.1 Data Sampling Techniques**

Select random subsets of data for analysis.

**Example**: Randomly sample 10% of the dataset.
```sql
SELECT *
FROM Employees
TABLESAMPLE SYSTEM (10); -- PostgreSQL
```

---

### **8.3.2 Train-Test Split Strategies**

Split data into training and testing sets.

**Example**: Split based on a date column.
```sql
SELECT * 
FROM Sales
WHERE SaleDate < '2023-01-01'; -- Training set

SELECT * 
FROM Sales
WHERE SaleDate >= '2023-01-01'; -- Testing set
```

---

### **8.3.3 Data Preprocessing in SQL**

Preprocessing ensures data quality and consistency.

- **Normalize Numeric Values**:
```sql
SELECT 
    (Salary - AVG(Salary) OVER ()) / STDDEV(Salary) OVER () AS NormalizedSalary
FROM Employees;
```

- **Handle Missing Values**:
```sql
SELECT 
    COALESCE(Salary, 50000) AS Salary -- Replace NULL with a default value
FROM Employees;
```

- **Remove Outliers**:
```sql
SELECT *
FROM Employees
WHERE Salary BETWEEN 
    (AVG(Salary) - 2 * STDDEV(Salary)) AND 
    (AVG(Salary) + 2 * STDDEV(Salary));
```

---
