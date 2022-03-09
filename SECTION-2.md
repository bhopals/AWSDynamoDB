## Background Concepts (RDBMS, NoSQL, JSON, Javascript, NodeJS)

### Data Normalization

- Why Normalization: Efficient Organize Data, Eliminate Redundant Data
- Normalized Forms

  - 1NF - Eliminate Repeating Groups
    - RULES (unique record, and single value per column)
      - Each Table cell should contain a Single Value
      - Each Record should be unique
  - 2NF - Eliminate Redundant Data

    - RULES (Primary Key - Foreign Key)
      - Be in 1NF
      - Single Column Primary Key that does not functonally dependant on any subset of candidate key relation

  - 3NF - Eliminate Columns that are not dependent on Key
    - RULES
      - Be in 2NF
      - Has no Transitive Functional Dependencies
      * A transitive functional dependency is when changing a non-key column, might cause any of the other non-key columns to change

  * Achieving 3NF is usually considered good.

  - BCNF (Boyce-Codd Normal Form) -
    Even when a database is in 3rd Normal Form, still there would be anomalies resulted if it has more than one Candidate Key.
    Sometimes is BCNF is also referred as 3.5 Normal Form.

  - 4NF -
    If no database table instance contains two or more, independent and multivalued data describing the relevant entity, then it is in 4th Normal Form.

  - 5NF -
    A table is in 5th Normal Form only if it is in 4NF and it cannot be decomposed into any number of smaller tables without loss of data.

  - 6NF -
    6th Normal Form is not standardized, yet however, it is being discussed by database experts for some time. Hopefully, we would have a clear & standardized definition for 6th Normal Form in the near future…

### Baiscs of NoSQL Databases

- Why NoSQL

  - Relaton Databases are:
    - Not Suited for unstructured data
    - Not Suited for Big Data Applications
    - Big Data means a huge volumes of data or high frequency data

- Basics of NoSQL Databases

  - Not-Only SQL (Means it supports SQL Queries (apache hive, redshift) as well)
  - Non-relational in Nature
  - Support Unstructured Data
  - Well suited for Big Data (Volume, Velocity, Variety) application

- Data Model

  - SQL (RELATIONAL)

    - Strict Schema
    - Predefined set of Columns
    - Each Record has same number of columns

  - NoSQL
    - Felxible Schema
    - Unstructured/Semistructured Data
    - Different Records can have different numbers of columns

- ACID Behavior

  - A - Atomicity - ALL or NOTHING
  - C - Consistency - Once Committed, the data should be in consistent state
  - I - Isolation - CONCURRENT Transactions Execute Separately from Each other
  - D - Durability - Durability is the ability of the Database to recover from an unexpected system failure or an Outage.
    So the transactions are typically logged and in case there is an outage or a system failure, these transactions can be replayed and applied
    back to the database once the system is back up and running.

  - These properties CONTROL or DEFINE how relational databases process the READ/WRITE Transactions.
  - Relational Database implement these ACID Properties in a very very strict sense, and these properties work
    together to ensure a consistent and desired behaviour of the relational database transactions.

  - SQL

    - Enforces strict ACID Properties
    - Loss of Flexibility
    - Vastly hinders the database performance
    - Loss the ability to Scall Horizontally

  - NoSQL

    - Trades some ACID Properties
    - Felxible Data model

- Scaling

  - SQL

    - Vertical Scaling
    - You scale by adding more power i.e. faster hardware, CPU, RAM etc.

  - NoSQL
    - Horizontal Scaling
    - You scale by adding more machines, partition or low-cost hardware to increase the throughtput of your database
    - Infact, with DAX (Dynamo Accelerator) you can bring down the response times from Milliseconds to microseconds at any scale, which is quite a Stellar Performance in my opinion.

- Interaction API's
  - SQL
    - Uses SQL (Structured Query Language) for interaction
  - NoSQL
    - Uses Object-based API's for interaction (use Partition Key or Primary Keys)

### Types of NoSQL Databases

    - Columnar Database - Dataware housing and analytics - Apacha Casandra / HBase, Redshift
    - Key-Value Store - Optimized for Read/ Compute Heavy Applications/ Workloads - Redis, Couchbase, Memcached, DynamoDB
    - Graph Database - Good fit for data like GRAPH/TREE Structure (Node-Edge Relations) - Neo4J, GraphDB
    - Document Database - To Store Semistructured data as documents (JSON or XML Documents, Schemaless, Flexible) - MongoDB, DynamoDB, CouchBase

- DynamoDB supports API Operations in JSON(Javascript Object Notations) Format

### JSON Fundamentals

- JSON
  - Javascript Object Notation
  - Simple and Popular Format
  - Used to exchange data over the API's
  - JSON is now widely used for data-interchange over the internet and is replacing XML very very fast
  - use "https://jsonlint.com" to validate JSON Object
