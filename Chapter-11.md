# **Chapter 11: NoSQL and NewSQL Interactions**

---

The evolution of data storage needs has led to NoSQL and NewSQL databases, enabling flexibility, scalability, and performance improvements for various use cases. SQL often interacts with NoSQL systems, bridging structured and unstructured data models.

---

## **11.1 SQL with NoSQL**

SQL integration with NoSQL databases allows querying semi-structured or unstructured data like JSON, XML, and graph-based data.

---

### **11.1.1 JSON Querying**

JSON is a common format in NoSQL databases like MongoDB, PostgreSQL, and Couchbase.

**Example**: Querying a JSON column in PostgreSQL.
```sql
-- Sample table with JSON column
CREATE TABLE Orders (
    OrderID SERIAL PRIMARY KEY,
    OrderDetails JSONB
);

-- Insert JSON data
INSERT INTO Orders (OrderDetails)
VALUES ('{"Product": "Laptop", "Price": 1200, "Quantity": 2}');

-- Query JSON fields
SELECT 
    OrderDetails->>'Product' AS Product,
    OrderDetails->>'Price' AS Price
FROM Orders;
```

---

### **11.1.2 Document Databases**

Document databases store semi-structured data. MongoDB, for instance, uses JSON-like BSON documents.

**Example**: Using SQL-like syntax in MongoDB with `mongo-sql`.
```sql
-- Equivalent SQL: SELECT * FROM Orders WHERE Price > 1000;
db.Orders.find({"Price": {$gt: 1000}});
```

---

### **11.1.3 Hybrid Data Models**

Databases like Cosmos DB support SQL for querying diverse data models.

**Example**: Query hierarchical data in Azure Cosmos DB.
```sql
SELECT c.ProductName, c.Manufacturer
FROM c
WHERE c.Category = "Electronics";
```

---

## **11.2 Polyglot Persistence**

Polyglot persistence involves using different types of databases for different aspects of an application, based on their strengths.

---

### **11.2.1 Choosing the Right Database**

Choose databases based on specific needs:
- **Relational**: Structured data requiring complex joins.
- **Document**: Flexible schemas for semi-structured data.
- **Graph**: Relationship-heavy datasets like social networks.
- **Time-Series**: Metrics and log data.

---

### **11.2.2 Multi-Model Databases**

Multi-model databases support multiple paradigms, like ArangoDB and Cosmos DB.

**Example**: Use SQL and graph queries in the same database.
```sql
-- Relational query
SELECT * FROM Orders WHERE OrderID = 1;

-- Graph query
FOR v, e, p IN 1..2 OUTBOUND "Customers/123" GRAPH "CustomerGraph"
RETURN p;
```

---

### **11.2.3 Data Synchronization Strategies**

Synchronizing SQL and NoSQL databases ensures consistency in hybrid systems.

**Techniques**:
- **Event-Driven**: Use event queues (e.g., Kafka) for updates.
- **ETL Pipelines**: Extract, transform, and load data periodically.
- **Change Data Capture (CDC)**: Automatically propagate updates.

**Example**: Use Debezium for real-time synchronization.
```text
- Capture changes from a MySQL database.
- Stream the changes into MongoDB for querying.
```

---

## **11.3 SQL Extensions in NoSQL**

Modern NoSQL systems often add SQL-like capabilities for ease of use.

| **NoSQL Database** | **SQL Feature**                 | **Example Query**                                            |
|--------------------|---------------------------------|-------------------------------------------------------------|
| MongoDB            | Aggregation framework          | `db.collection.aggregate([...])`                           |
| Cassandra          | CQL (Cassandra Query Language) | `SELECT * FROM users WHERE id = '123';`                    |
| Couchbase          | N1QL (SQL for JSON)            | `SELECT * FROM bucket WHERE age > 30;`                     |
| DynamoDB           | PartiQL (SQL-compatible query) | `SELECT * FROM Orders WHERE Status = 'Pending';`           |

---

## **11.4 Case Study: SQL-NoSQL Integration**

### **Scenario**: E-Commerce Application

**Requirements**:
1. Store structured customer and order data.
2. Maintain product catalog in a flexible schema.
3. Analyze relationships between products for recommendations.

**Solution**:
- **Relational Database (PostgreSQL)**: For structured customer and order data.
- **Document Database (MongoDB)**: For the product catalog with varying attributes.
- **Graph Database (Neo4j)**: For product recommendation algorithms.

---

### **Integration Example**:

- Use SQL for transactional data:
```sql
SELECT CustomerName, SUM(OrderTotal) AS TotalSpent
FROM Orders
WHERE OrderDate >= '2023-01-01'
GROUP BY CustomerName;
```

- Use MongoDB for product catalog:
```javascript
db.Products.find({"Category": "Electronics"});
```

- Use Neo4j for recommendations:
```cypher
MATCH (p:Product)-[:BOUGHT_WITH]->(rec:Product)
WHERE p.name = "Laptop"
RETURN rec.name, rec.popularity
ORDER BY rec.popularity DESC;
```

---
