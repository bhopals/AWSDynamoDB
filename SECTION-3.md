## DynamoDB Basics

Relation Databases are largely inefficient when we need to process predominantly/commonly/largely unstructured
data with high volume and high frequency

### Overview of AWS Dynamo

DynamoDB is SERVERLESS (no need to manage server - Fully Managed) CLOUD (service over the Cloud) NoSQL Database that is
FAST (High Throughput with very low latency - Response time is milliseconds to microsecods (if DAX)),
FLEXIBLE (variety of data Unstructured and Semi structured Data),
COST EFFECTIVE (Pay what you USE and for DATA, NOT for Infrastructure),
HIGHLY SCALABLE,
FAULT TOLERANT (Automatically Replicate data to multiple availability zones, Also supports Cross Region Replication),
SECURE (Fine grained access control)

BIG DATA - HIGH VVV (High Volume, High Variety (Unstructured and Semi structured Data),
High Velocity (Huge No. of Concurrent Read Write Operations))

### Terminology Comparison with SQL

SQL / DynamoDB

- Tables / Tables
- Rows / Items
- Columns / Attributes
- Primary Keys - Multicolumn and Optional / Primary Keys - Mandatory, Minimum One and Maximum Two Attributes (Partition Key (Mandatory) and Sort Key (Optional))
- Indexes / Local Secondary Indexes (Share the Partition Key with Partition Key of Primary KEY of Table but have different SORT KEY)
- Views / Global Secondary Indexes (Partition Key is different than Partion key of Primary KEY of Table and have different SORT KEY)

- DynamoDB Must Have PRIMARY KEY (Partition Key (Mandatory) + Sort Key (Optional))

### DynamoDB Tables and Naming Conventions

### Data Types in DynamoDB

### DynamoDB Consistency model

### DynamoDB Capacity Units

### Basics of DynamoDB Partitions

### Basics of DynamoDB Indexes

### Local Secondary Indexes and Global Secondary Indexes

### Interacting with DynamoDB
