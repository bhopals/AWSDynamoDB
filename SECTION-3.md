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

### Basics of DynamoDB Indexes

### Local Secondary Indexes and Global Secondary Indexes

### Interacting with DynamoDB
