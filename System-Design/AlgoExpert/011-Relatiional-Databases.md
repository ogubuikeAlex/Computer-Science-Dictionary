What are relational databases?

A relational database is a database tht stores data in a rigid tabular form.

What is a record?
a record is a single entry ion a database usually made up of rows and columns
What is a schema?
A schema represents a document that highlights the acceptable requirements a record must meet in other to be added to a relational database.
What is a non relational database?
A non relational DB is one that does not store data in rigid tables. It is usually very flexible. An example is a document based database like mongodb that stores information in JSON format.
What is SQL?
Structured Query Language. SQL is a query language used by most relational databases for performing improved queries. most relational Dbs use SQl and are called SQL Databases while the others are called No-SQL Dbs.

Why will i use a relational Db over a No-SQL DB?
With a relational DB, its easier to get consistency in your data when compared to No SQL database
What is an ACID transaction?
A transaction that is ACID compliant.
- A => Atomicity; The operation that constitutes a transaction either all succeed or all fail. There is no in-between states.
- C => Consistency: the transaction cannot bring the Db to an invalid state. After the transaction is committed or rolled back, the rules for each record will still apply, and all future transactions will see the effect of the transaction immediately.
- I => Isolation:The execution of multiple transactions concurrently wil have the same effect as if they had been executed sequentially.
- D => Durability: Any committed transaction is written to non-volatile storage. it will not be undone by a crash, power loss or network partition.

- What is a database index?
An auxiliary data storage setup on a column to improve the efficiency of searching for results

- What is Eventual Consistency and What is String Consistency?

Eventual Consistency: A system that guarantees that they will eventually be coherence in the data returned. This means that they is a possibility of getting stale data,but eventually everything will reflect correctly.

Strong Consistency: 