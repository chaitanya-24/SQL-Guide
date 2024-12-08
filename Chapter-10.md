# **Chapter 10: Data Governance and Security**

---

Data governance and security are critical for protecting sensitive data, ensuring compliance, and maintaining trust. This chapter covers access control, encryption, and auditing techniques in SQL.

---

## **10.1 Access Control**

Access control ensures only authorized users can access or modify data.

---

### **10.1.1 GRANT and REVOKE**

These SQL commands manage permissions for database objects.

**Example**: Granting and revoking permissions.
```sql
-- Grant SELECT permission on Employees table to UserA
GRANT SELECT ON Employees TO UserA;

-- Revoke SELECT permission from UserA
REVOKE SELECT ON Employees FROM UserA;
```

---

### **10.1.2 Role-Based Access Control (RBAC)**

Roles group permissions, simplifying user management.

**Example**: Creating and assigning roles.
```sql
-- Create a role for read-only access
CREATE ROLE ReadOnly;

-- Grant permissions to the role
GRANT SELECT ON Employees TO ReadOnly;

-- Assign the role to a user
GRANT ReadOnly TO UserA;
```

---

### **10.1.3 Column-Level Security**

Restrict access to specific columns containing sensitive data.

**Example**: Masking sensitive columns.
```sql
-- Create a view to expose limited columns
CREATE VIEW EmployeePublic AS
SELECT EmployeeID, FirstName, LastName
FROM Employees;
```

---

## **10.2 Data Encryption**

Encryption secures data in transit and at rest.

---

### **10.2.1 Transparent Data Encryption (TDE)**

TDE encrypts entire databases at the storage level.

**Example**: Enabling TDE in SQL Server.
```sql
-- Create a master key
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'StrongPassword123';

-- Create a certificate for encryption
CREATE CERTIFICATE MyDBCert WITH SUBJECT = 'TDE Certificate';

-- Create a database encryption key
USE MyDatabase;
CREATE DATABASE ENCRYPTION KEY
WITH ALGORITHM = AES_256
ENCRYPTION BY SERVER CERTIFICATE MyDBCert;

-- Enable encryption
ALTER DATABASE MyDatabase SET ENCRYPTION ON;
```

---

### **10.2.2 Column-Level Encryption**

Encrypt sensitive columns like credit card numbers.

**Example**: Encrypting and decrypting a column.
```sql
-- Encrypt sensitive data
INSERT INTO Payments (CardNumber)
VALUES (ENCRYPTBYKEY(KEY_GUID('CardKey'), '4111111111111111'));

-- Decrypt data for use
SELECT CONVERT(VARCHAR, DECRYPTBYKEY(CardNumber)) AS CardNumber
FROM Payments;
```

---

### **10.2.3 Encryption Key Management**

Centralize key management to enhance security.

- **Example**: Use AWS Key Management Service (KMS) or Azure Key Vault for managing encryption keys.

---

## **10.3 Compliance and Auditing**

Regulatory compliance frameworks like GDPR and HIPAA require robust auditing and masking.

---

### **10.3.1 GDPR Considerations**

GDPR mandates protecting personal data and providing user controls.

**Example**: Anonymizing personal data.
```sql
UPDATE Customers
SET Name = NULL, Email = NULL
WHERE RequestDeletion = TRUE;
```

---

### **10.3.2 Data Masking**

Mask sensitive data to protect it during non-production use.

**Example**: Masking SSNs with SQL Server.
```sql
CREATE TABLE Customers (
    SSN VARCHAR(11) MASKED WITH (FUNCTION = 'partial(0,"XXX-XX-",4)')
);
```

---

### **10.3.3 Audit Logging**

Track database activity for security and compliance.

**Example**: Enable logging for SQL Server.
```sql
-- Create an audit
CREATE SERVER AUDIT MyAudit
TO FILE (FILEPATH = 'C:\AuditLogs\');

-- Enable the audit
ALTER SERVER AUDIT MyAudit WITH (STATE = ON);

-- Create a database audit specification
CREATE DATABASE AUDIT SPECIFICATION MyAuditSpec
FOR SERVER AUDIT MyAudit
ADD (SELECT ON Employees BY PUBLIC);

-- Enable the specification
ALTER DATABASE AUDIT SPECIFICATION MyAuditSpec WITH (STATE = ON);
```

---

## **10.4 Tools for Governance and Security**

| **Tool**                | **Purpose**                                | **Use Case**                              |
|-------------------------|--------------------------------------------|------------------------------------------|
| SQL Server Audit        | Tracks changes and access                 | Security auditing                        |
| Azure Key Vault         | Centralized key management                | Encrypting cloud-based SQL databases     |
| Oracle Data Masking     | Masks sensitive data                      | GDPR and HIPAA compliance                |
| IBM Guardium            | Monitors and secures databases            | Enterprise security and compliance       |

---
