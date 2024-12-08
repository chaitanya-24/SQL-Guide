# **Foundational Concepts**

---

## **1.1 Database Fundamentals**

Databases are organized collections of data that support efficient storage, retrieval, and management. They are used in applications ranging from simple inventory systems to complex data analytics pipelines.

---

### **Database Types**

#### **1. Relational Databases**
Relational databases store data in structured formats (tables) where rows represent records, and columns represent attributes. They use SQL for querying and ensure data consistency through ACID properties.

**Example: E-commerce**
- **Tables**: `Products`, `Customers`, `Orders`.
- A foreign key in the `Orders` table links to `Customers` to maintain relational integrity.

```sql
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(100)
);

CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    OrderDate DATE,
    CustomerID INT,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);
```

#### **2. Non-Relational Databases**
Non-relational databases (NoSQL) store unstructured or semi-structured data and are optimized for scalability and flexibility.

**Types**:
- **Document Stores**: MongoDB stores JSON-like documents.
- **Key-Value Stores**: Redis for caching.
- **Columnar Databases**: Cassandra for distributed datasets.

**Example: JSON in a Document Database**
```json
{
    "CustomerID": 1,
    "Name": "John Doe",
    "Orders": [
        {"OrderID": 101, "OrderDate": "2023-10-01"},
        {"OrderID": 102, "OrderDate": "2023-10-05"}
    ]
}
```

#### **3. Time-Series Databases**
Specialized for tracking changes over time, e.g., IoT or financial applications.

**Examples**: InfluxDB, TimescaleDB.
**Use Case**: Tracking server CPU usage.

```sql
CREATE TABLE CPU_Usage (
    Timestamp TIMESTAMP,
    ServerID INT,
    UsagePercent FLOAT
);
```

#### **4. Graph Databases**
Graph databases model data as nodes (entities) and edges (relationships). They are useful for social networks and recommendation engines.

**Examples**: Neo4j, Amazon Neptune.
**Use Case**: Modeling friendships.
```cypher
CREATE (Alice:Person {name: 'Alice'})-[:FRIEND]->(Bob:Person {name: 'Bob'});
```

---

### **RDBMS Systems**

Prominent relational database systems include:

#### **1. MySQL**
- **Features**: Open-source, widely used for web applications.
- **Example**: A basic CRUD operation.
```sql
INSERT INTO Customers (CustomerID, Name, Email) VALUES (1, 'John Doe', 'john@example.com');
SELECT * FROM Customers WHERE CustomerID = 1;
```

#### **2. PostgreSQL**
- **Features**: Advanced features like JSON support, full-text search.
- **Example**: Using JSON in PostgreSQL.
```sql
CREATE TABLE Products (
    ProductID INT,
    Details JSONB
);
INSERT INTO Products (ProductID, Details) VALUES 
(1, '{"name": "Laptop", "price": 1200}');
```

#### **3. Oracle**
- **Features**: Enterprise-grade, used in critical applications.
- **Example**: Using PL/SQL for stored procedures.
```sql
CREATE OR REPLACE PROCEDURE AddOrder (p_CustomerID INT, p_Amount FLOAT)
IS
BEGIN
    INSERT INTO Orders (CustomerID, Amount) VALUES (p_CustomerID, p_Amount);
END;
```

#### **4. Microsoft SQL Server**
- **Features**: Integration with Microsoft tools, T-SQL extensions.
- **Example**: Using a CTE.
```sql
WITH RecentOrders AS (
    SELECT TOP 10 * FROM Orders ORDER BY OrderDate DESC
)
SELECT * FROM RecentOrders;
```

#### **5. SQLite**
- **Features**: Lightweight, serverless database for local applications.
- **Example**: Creating a local database for a mobile app.
```sql
CREATE TABLE Notes (
    NoteID INT PRIMARY KEY,
    Content TEXT,
    CreatedAt TIMESTAMP
);
```

---

## **1.2 Database Design**

Database design ensures that data is stored efficiently and relationships between data are clearly defined.

---

### **Normalization Techniques**

Normalization organizes tables to reduce redundancy and improve data integrity.

#### **1. First Normal Form (1NF)**
- **Rule**: Ensure atomic values (no repeating groups).
  
**Example (Before)**:
| OrderID | Product 1  | Product 2 |
|---------|------------|-----------|
| 1       | Laptop     | Mouse     |

**Example (After)**:
| OrderID | Product    |
|---------|------------|
| 1       | Laptop     |
| 1       | Mouse      |

#### **2. Second Normal Form (2NF)**
- **Rule**: Eliminate partial dependencies.
  
**Example (Before)**:
| OrderID | ProductID | ProductName |
|---------|-----------|-------------|
| 1       | 101       | Laptop      |

**Example (After)**:
| ProductID | ProductName |
|-----------|-------------|
| 101       | Laptop      |

#### **3. Third Normal Form (3NF)**
- **Rule**: Eliminate transitive dependencies.
  
**Example (Before)**:
| ProductID | ProductName | SupplierCountry |
|-----------|-------------|-----------------|

**Example (After)**:
| SupplierID | SupplierCountry |
|------------|-----------------|
| ProductID  | SupplierID      |

#### **4. Boyce-Codd Normal Form (BCNF)**
- **Rule**: Ensure every determinant is a candidate key.
**Example**:
A functional dependency that violates 3NF is resolved in BCNF.

---

### **Schema Design**

Schema design involves representing database structure visually and logically.

#### **Entity-Relationship Diagrams (ERD)**
ERDs depict entities, attributes, and relationships between entities.

**Example**:
- **Entities**: Customers, Orders.
- **Relationships**: `Customers` places `Orders`.

---

### **Data Modeling**

#### **1. Conceptual Modeling**
High-level design showing the main entities and relationships.
- **Example**: `Customer` has `Order`.

#### **2. Logical Modeling**
Detailed design, specifying attributes and relationships.
- **Example**: `Customer (CustomerID, Name, Email)`.

#### **3. Physical Modeling**
Implementation design, specifying data types, indexes.
- **Example**:
```sql
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(100) UNIQUE
);
```

---
