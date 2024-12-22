## What are relational databases?
A relational database is a structured type of database that stores and manages data in tables with predefined schema. The tables are related to one another via foreign keys. It achieves by using a rigid row and column structure.

### What is a record?
a record is a single entry in a database usually made up of rows and columns

### What is a schema?
A schema represents a document that highlights the acceptable requirements a record must meet in other to be added to a relational database.

### What is a non relational database?
A non relational DB is one that is designed to handle a wide variety of data models, including document, key-value, column-family, and graph formats. It offers flexibility and scalability to accommodate unstructured  and semi-structured data. An example is a document based database like mongodb that stores information in JSON format.

### What is SQL?
Structured Query Language. SQL is a query language used by most relational databases for performing improved queries. most relational Dbs use SQl and are called SQL Databases while the others are called No-SQL Dbs.

### Why will i use a relational Db over a No-SQL DB?
 Choosing between a relational database (SQL) and a No-SQL database depends on the specific needs and requirements of your application. Here are some key factors to consider:

#### When to Use a Relational Database (SQL):

1. **Structured Data**:
   - If your data is highly structured and follows a predefined schema with relationships between tables, a relational database is a good fit.
   - Example: A customer order system where customers, orders, and products have well-defined relationships.

2. **ACID Compliance**:
   - Relational databases offer strong ACID (Atomicity, Consistency, Isolation, Durability) properties, ensuring reliable transactions.
   - Example: Financial transactions where consistency and integrity are crucial.

3. **Complex Queries**:
   - If you need to perform complex queries, joins, and aggregations, SQL databases provide powerful query languages (SQL) to handle these tasks.
   - Example: Reporting and analytics systems requiring intricate data retrieval.

4. **Data Integrity**:
   - Enforcing data integrity constraints such as foreign keys, unique constraints, and check constraints is easier in relational databases.
   - Example: Ensuring no duplicate entries and maintaining referential integrity in a database.

#### When to Use a No-SQL Database:

1. **Unstructured or Semi-Structured Data**:
   - No-SQL databases excel at handling unstructured or semi-structured data, which doesn't fit neatly into tables and columns.
   - Example: Storing JSON documents, blog posts, or multimedia content.

2. **Scalability**:
   - If your application requires horizontal scaling to handle large volumes of data and high traffic, No-SQL databases are designed for this.
   - Example: Social media platforms or IoT applications generating massive amounts of data.

3. **Flexible Schema**:
   - No-SQL databases offer a flexible schema, allowing you to store and manage evolving data structures without requiring schema changes.
   - Example: E-commerce platforms where product attributes may vary widely.

4. **High Throughput and Low Latency**:
   - For applications that need to process a large number of read and write operations with low latency, No-SQL databases can be more efficient.
   - Example: Real-time analytics or gaming applications requiring rapid data access

Generally:
 If you need strict consistency and complex querying capabilities, a relational database is a better choice. If you require scalability, flexibility, and high performance for diverse data types, a No-SQL database might be more suitable.

### What is an ACID transaction?
A transaction that is ACID compliant.
- A => Atomicity: The operation that constitutes a transaction either all succeed or all fail. There is no in-between states.
- C => Consistency: the transaction cannot bring the Db to an invalid state. After the transaction is committed or rolled back, the rules for each record will still apply, and all future transactions will see the effect of the transaction immediately.
- I => Isolation: The execution of multiple transactions concurrently wil have the same effect as if they had been executed sequentially.
- D => Durability: Any committed transaction is written to non-volatile storage. it will not be undone by a crash, power loss or network partition.

### What are the Base Properties Of No-SQL DBs?

### What is a database index?
An auxiliary data storage setup on a column to improve the efficiency of searching for results

A database index is a data structure that improves the speed of data retrieval operations on a database table at the cost of additional writes and storage space. Think of it like an index at the end of a book that allows you to quickly find specific information without having to read the entire book.

#### How it Works:

1. **Data Structure**: An index is typically built using data structures like B-trees or hash tables, which allow for fast searching, insertion, and deletion.
2. **Keys and Pointers**: The index stores a sorted list of key values and pointers to the actual data rows in the table. When a query is executed, the database uses the index to quickly locate the desired rows.

#### Types of Indexes:

1. **Primary Index**:
   - Created automatically when a primary key is defined.
   - Ensures that each value in the primary key column(s) is unique.

2. **Secondary Index**:
   - Created on non-primary key columns to speed up query performance.
   - Can be created on one or more columns to support different query patterns.

3. **Clustered Index**:
   - Determines the physical order of data in the table.
   - Only one clustered index can be created per table, as it defines how data is stored.

## Question: If this determines the physical state of the table, does it mean it is created automatically? if one is created automatically what happens when I create another one, does it move?

4. **Non-Clustered Index**:
   - Does not alter the physical order of data in the table.
   - Can have multiple non-clustered indexes on a table to support various queries

#### Benefits of Indexes:

1. **Faster Query Performance**: Indexes can significantly speed up data retrieval by reducing the number of rows scanned.
2. **Efficient Data Access**: Particularly beneficial for large tables where scanning all rows would be time-consuming.
3. **Support for Quick Search**: Helps in quickly locating rows based on indexed columns.

### Trade-offs:

1. **Additional Storage**: Indexes require extra storage space to maintain the index data structure.
2. **Slower Writes**: Insertions, updates, and deletions can be slower because the index needs to be updated along with the table data.

Indexes are a powerful tool to optimize database performance, especially for read-heavy operations, but should be used judiciously to balance the benefits with the trade-offs.

### What is Eventual Consistency and What is Strong Consistency?

Eventual Consistency: A system that guarantees that they will eventually be coherence in the data returned. This means that they is a possibility of getting stale data,but eventually everything will reflect correctly.

String Consistency: 

### Eventual Consistency

**Eventual Consistency** is a consistency model used in distributed systems. It guarantees that, given enough time without new updates, all replicas of a piece of data will become consistent. However, it does not guarantee immediate consistency. This model is often used in systems where high availability and partition tolerance are prioritized over strict consistency.

#### Characteristics:
- **Asynchronous Replication**: Data updates are propagated asynchronously across different nodes, which means there may be temporary discrepancies.
- **Availability Over Consistency**: Ensures high availability even in the presence of network partitions, but data may be temporarily out of sync.
- **Eventual Agreement**: All nodes will eventually agree on the data's final state, but there may be a delay.

#### Use Cases:
- **Social Media**: Likes, comments, and shares may take a moment to sync across all devices.
- **DNS Systems**: Updates to domain names propagate gradually across DNS servers.
- **Cassandra and DynamoDB**: Popular databases that use eventual consistency for certain operations.

### Strong Consistency

**Strong Consistency** guarantees that all read operations will return the most recent write. This means that once a write operation completes, any subsequent read will reflect that write. It provides a single, globally consistent view of data.

#### Characteristics:
- **Synchronous Replication**: Data updates are propagated synchronously, ensuring all replicas are immediately consistent.
- **Consistency Over Availability**: Prioritizes consistency, even if it means reduced availability during network partitions.
- **Immediate Agreement**: All nodes agree on the data's state immediately after a write operation.

#### Use Cases:
- **Financial Systems**: Banking transactions where it’s crucial to have the most up-to-date data.
- **Inventory Management**: Systems where real-time stock levels must be accurate.
- **Relational Databases**: Most traditional SQL databases aim to provide strong consistency.

### Summary

- **Eventual Consistency**: Ensures high availability and partition tolerance, with data becoming consistent over time.
- **Strong Consistency**: Guarantees immediate consistency across all nodes, prioritizing data accuracy over availability.

Each model has its own advantages and trade-offs, and the choice depends on the specific requirements of your application.