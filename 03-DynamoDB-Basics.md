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

- DynamoDB Must Have PRIMARY KEY (Partition/Hash Key (Mandatory) + Sort Key (Optional))

### DynamoDB Tables and Naming Conventions

DynamoDB Tables are TOP LEVEL ENTITIES, INDEPENDENT ENTITIES (Cannot have TWO Tables JOINED/CONNECTED), FLEXIBLE SCHEMA

RELATION DB - STRICT ACID - STRICT SCHEMA
DYNAMODB - FLEXIBLE SCHEMA

### Data Types in DynamoDB

- Scalar Types

  - Exactly one value
  - e.g. string, number, binary, boolean, and null
  - String - Non-Empty Value
  - String used as a Primary KEY - 2KB
  - String used as a Sort KEY - 1KB
  - Binary - Blobs of Binary Data (Compressed Text, Encrypted Data, Images)
  - Keys or Index attributes only support string, number, and binary scalar types

- Set Types

  - Multiple scalar values
  - e.g. string set, number set, and binary set
  - Unordered collection of Strings
  - No duplicates allowed
  - All values must be of same scalar type

- Document Types

  - Complex structure with nested attributes (JSON objects)
  - e.g. List and Map
  - Nested up to 32 Levels DEEP
  - Lists
    - Ordered Collection of values
    - Can have multiple data types
    - e.g. ["John", 128.88, "Apples"]
  - Maps
    - Unordered collection of Key-Value Pairs
    - Ideal for storing JSON Documents
    - e.g. { name: "john", age: 22, { address: : {....}}}

- When we create a Table or an Index such as Local or Global Secondary index, we must specify the data type for each of the KEYs.
- KEYs of any table can only be of Certain SCALAR Data Types - Number, String, Binary
- You cannot use Boolean, Null, Set, List, Map data types to Define the Primary Key or Any of the Indexes of your Table.
- Each Item (Row) is limited to 400KB Size
- String used as a Primary Key - 2KB
- String used as a Sort Key - 1KB

### DynamoDB Consistency model

- AWS REGIONS
  - ONE or More Availibilit Zones
    - ONE or More Facilities

DynamoDB Automatic Synchronous Replication

High Availibility

- 3 Copies of data within a REGION
- Act as Independent Failure Domains
- Near Real-Time Replication

DynamoDB Read Consistency

- Strong Consistency

  - The most up-to-date data
  - Must be requested EXPLICITY

- Eventual Consistency
  - May or may not reflect the latest copy of data
  - Default consistency for all operations
  - 50% Cheaper

### DynamoDB Capacity Units

- DynamoDB Tables

  - Top-level entities
  - No stric inter-table relationships
  - Mandatory primary keys
  - Control performance at the table level

- Throughput Capacity

  - Allows for predictable performance at scale
  - Used to control read/write throughput
  - Supports auto-scaling
  - Defined using RCUs and WCUs
  - Major factor in DynamoDB Pricing
  - 1 capacity unit = 1 request/sec

- RCUs

  - Read Capacity Units
  - 1 RCU = 1 strongly consistent table read/second
  - 1 RCU = 2 eventually consistent table read/second
  - In blocks of 4KB

- WCUs

  - Write Capacity Units
  - 1 WCU = 2 table write/second
  - In blocks of 1KB

- Example (RCUs and WCUs)

  - Average Item Size: 10KB
  - Provisioned Capacity: 10 RCUs and 10 WCUs
  - Read Throughtput with strong consistency = 4KB x 10 = 40KB/second
  - Read throughput = 2(4KB x 10) = 80KB/second
  - Write throughput = 1KB x 10 = 10KB/second
  - RCU's to read 10KB of data per second with strong consistency = 10KB/4KB = 2.5 => rounded up => 3 RCUs
  - RCU's to read 10KB of data per second = 3 RCUs x 0.5 = 1.5 RCUs
  - WCUs to write 10KB of data per second = 10KB/1KB = 10 WCUs
  - WCUs to write 1.5KB of data per second = 1.5KB/1KB = 1.5 => rounded up => 2 WCUs

- Burst Capacity

  - To provide for occassional bursts or spikes
  - 5 minutes or 300 seconds of unused read and write capacity
  - Can get consumed quickly
  - Must not be relied upon

- Scaling
  - Scaling UP: As and when needed
  - Scaling Down: Up to 4 times in a day
  - Affects Partition Behavior
  - 1 Partition supports up to 1000 WCUs or 3000 RCUs

### Basics of DynamoDB Partitions

- Store DynamoDB table data
- A Partition is a block of memory allocated by DynamoDB for storage
- A table can have multiple partitions
- Number of table partitions depend on its size and provisioned capacity
- Managed internally by DynamoDB
- 1 Partition = 10GB of data (Maximum)
- 1 Partition = 1000 WCUs or 3000 RCUs (Maximum)

- Partition Behavior - Example
  - Provisioned Capacity: 500 RCUs and 500 WCUs
    - Number of Partitions (P1) => (500 RCUs/3000 + 500 WCUs/1000) = 0.67 => rounded up => 1 Partitions
  - New Capacity: 1000 RCUs and 1000 WCUs
    - Number of Partitions (P1) => (1000 RCUs/3000 + 1000 WCUs/1000) = 1.33 => rounded up => 2 Partitions of 500 RCU and 500 WCU Each

Once allocated, it will not be deallocated automatically once we scale down, and that could result in lower thoughput capacity
as The Throughput capacity is equally distibuted among all the Partitions.

### Basics of DynamoDB Indexes

- Table Index

  - Mandatory Primary Key - Either Simple or Composite
  - Simple Primary Key => Only Partition or Hash key
  - Composite Primary Key => Partition Key + Sort or Range Key
  - Partition or Hash Key decides the target partition

- Hashing
  - Partition Key ==> Hashing Algorithm ==> P1, P2

Scan operations does not require Partition Key. However, DynamoDB require us to specify the Partition Key for all Query opertations.
Use of Scan Operation indicates an insufficient data modeling activity.

### Local Secondary Indexes and Global Secondary Indexes

- DynamoDB internally creates an Index based on the Primary Key
- Example Table = Dept (Partition key), Emp ID (Sort key), Name, Age, Location, Date of Joining
- Local Secondary Indexes
  - This type of Index will have the partition key same as our Primary Key
  - Can only be created at the Time of Table Creation
  - Can have up to 5 Local Secondary Indexes
  - The RCU and WCU will be shared across all of your Local Secondary Indexes that you create (Shared with the Base Capacity Unit of the Table)
  - Use Case - You want to retrieve Dept List sorted by Date of Joining. To do that
    create a Local Secondary Indexes with Dept (Partition key), Date of Joining (Sort key)
  - Perform Strong and Eventual Consistent READS
- Global Secondary Indexes
  - When your partition key is different from the primary key of the Table
  - Can have up to 5 Global Secondary Indexes
  - Global Secondary Indexes will have their own Throughput Capacities (Not shared with the Base Capacity Unit of the Table)
  - Can be created any time
  - CAN ONLY perform Eventual Consistency READS
  - You can have Duplicate Items in Global Secondary Indexes (unlike Primary key where it should be UNIQUE)
  - Use Case - You want to retrieve Locations sorted by Date of Joining. To do that
    create a Global Secondary Indexes with Locations (Partition key), Date of Joining (Sort key)

### Interacting with DynamoDB

- Different Ways to Work with DynamoDB
  - AWS Management Console
  - AWS CLI
  - AWS CDK
