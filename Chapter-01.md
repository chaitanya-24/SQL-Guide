# **Comprehensive SQL Topics for Data Professionals**

## **Chapter 1: Foundational Concepts**

---

### **1.1 Database Fundamentals**

Databases are integral to modern data management, enabling the structured storage, retrieval, and manipulation of data. This section introduces the different types of databases and popular database management systems (DBMSs).

#### **1.1.1 Database Types**

**1. Relational Databases**  
Relational databases store data in tabular format with predefined schemas. Tables are interconnected using relationships (primary keys and foreign keys). SQL (Structured Query Language) is the standard language for querying relational databases.

- **Example Systems**: MySQL, PostgreSQL, Oracle DB, Microsoft SQL Server.

**Code Example**: Creating a table in a relational database.
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    HireDate DATE
);
```
**Explanation**: This SQL code creates a table named `Employees` with columns for employee ID, name, and hire date. The `PRIMARY KEY` ensures unique identification for each record.

---

**2. Non-Relational Databases**  
Non-relational databases (NoSQL) handle unstructured or semi-structured data. Types include key-value stores, document stores, wide-column stores, and graph databases.

- **Example Systems**: MongoDB (Document store), Cassandra (Wide-column store).

**Example**: MongoDB stores documents in JSON-like format:
```json
{
  "EmployeeID": 1,
  "FirstName": "Alice",
  "LastName": "Smith",
  "HireDate": "2023-05-01"
}
```

---

**3. Time-Series Databases**  
Designed to handle time-stamped data, these databases optimize for queries that involve time intervals.

- **Example Systems**: InfluxDB, TimescaleDB.

**Use Case**: Monitoring system metrics, stock price analysis.

---

**4. Graph Databases**  
Specialize in handling relationships using graph structures: nodes (entities) and edges (relationships).

- **Example Systems**: Neo4j, ArangoDB.

**Code Example**: Querying relationships in Neo4j using Cypher:
```cypher
MATCH (a:Person)-[:FRIEND_OF]->(b:Person)
RETURN a, b
```
**Explanation**: This query retrieves pairs of people who are friends in the graph database.

---

#### **1.1.2 RDBMS Systems**

An overview of popular RDBMS tools:

1. **MySQL**: Open-source, widely used for web applications.
2. **PostgreSQL**: Advanced open-source system with support for JSON and advanced indexing.
3. **Oracle Database**: Known for scalability and enterprise features.
4. **Microsoft SQL Server**: Integrated with Microsoft's ecosystem.
5. **SQLite**: Lightweight, serverless database suitable for small-scale applications.

**Code Example**: Basic SQL query across RDBMS systems.
```sql
SELECT * FROM Employees WHERE HireDate > '2020-01-01';
```

---

### **1.2 Database Design**

Effective database design ensures performance, maintainability, and scalability.

#### **1.2.1 Normalization Techniques**

Normalization is the process of organizing data to reduce redundancy and improve integrity.  
- **1NF**: Ensure atomicity (no repeating groups or arrays).  
- **2NF**: Ensure full dependency on the primary key.  
- **3NF**: Remove transitive dependencies.  
- **BCNF**: A stricter form of 3NF.

**Example**: Converting a non-normalized dataset into 3NF.

**Initial Table**:
| OrderID | CustomerName | ProductName  | ProductCategory |
|---------|--------------|--------------|-----------------|
| 1       | Alice        | Laptop       | Electronics     |
| 2       | Bob          | Smartphone   | Electronics     |

**Normalized Tables**:
1. Customers:  
   | CustomerID | CustomerName |
   |------------|--------------|
   | 1          | Alice        |
   | 2          | Bob          |

2. Products:  
   | ProductID | ProductName | ProductCategory |
   |-----------|-------------|-----------------|
   | 1         | Laptop      | Electronics     |
   | 2         | Smartphone  | Electronics     |

3. Orders:  
   | OrderID | CustomerID | ProductID |
   |---------|------------|-----------|
   | 1       | 1          | 1         |
   | 2       | 2          | 2         |

---

#### **1.2.2 Schema Design**

**Entity-Relationship Diagrams (ERD)**: Visual representation of database entities and their relationships.

**Example**: Designing an ERD for an e-commerce application:
- **Entities**: Customers, Orders, Products.
- **Relationships**: Customers place Orders, Orders contain Products.

---
