## DynamoDB Data Modeling and Best Practices

- A Predictable and Consistent Performance at SCALE

### DynamoDB Architecture

- SAO (Service Oriented Architecture)

  - At high level, DynamoDB utilizes Service Oriented Architecture (SOA)
  - As like other AWS Services, it is designed to work as a Decouple Services that maintains a Service Contract with Other Services
  - Meaning it ensures a predefined and predictable level of performance at all times.

- API's (Application Programming Interface)
  - Data Transfer in DynamoDB happens for Simple API's like GET, PUT, DELETE
- SSDs (Solid State Drive)

  - Data Stored in High Performance SSDs (Solid State Drives)
  - Each Partition in DynamoDB provides for about 10GB of data (3000 RCUs and 1000 WCUs) and it is optimized to deliver a Predictable throughput

- In addition to GUARANTEE a consistent performance, DynamoDB stores at least three copies of your data
  in different facilities within the region.
- High Availibility

  - Automatic Synchronous Replication
  - A DynamoDB Partition - Synchronous Replication
  - 3 Copies of Data within a Region
  - Act as Independent Failure Domains
  - Near Real-time Replication

- HASHING

  - A Hash Function is typically a function that returns position within an array where a given element can be stored.
  - DynamoDB uses several techniques such as CONSISTENT HASHING to STORE/RETRIEVE data reliably
  - DynamoDB Stores data across the nodes, which are arranged in a form of a ring
  - Consistent Hashing Algorithm is used to decide where to store the particular piece of data
  - Hash = f(PartitionKey, size) - Hashing Algorithm
  - The returned HASH uniquely identifies a place to store a Particular table item within DynamoDB Distributed System of Nodes or Clusters.
  - The main purpose of hashing is to provide a consisten way to retrieve store items without any ambiguities
  - So the DynamoDB uses this technique of Consistent Hashing where both the Partition Key as well as the Nodes are HASHED.
  - And the same are used during Data Retrieval.
  - The Items with the SAME HASH value are stored as LINKED LIST
  - Hash = f(PartitionKey, <node/cluster-position>)

- DynamoDB Data Storage and Retrival DETAILS
  - Hash = f(Partition Key, Size)
  - DynamoDB arranges the cluster of nodes in a sort of a Circle. So each Node or Partition for table is Considered to be in a RING
  - For example, this Ring has 100 Positions, means DynamoDB can have 100 potential nodes or partitions in this table.
  - In reality, this number would, of course, be very large.
  - DynamoDB nodes are spread over this ring in a circular fashion.
  - Let's say we have FOUR nodes or partitions in our table located at position 17th, 53rd, 64th, and 80th (Within all 100 Positions)
  - Whenever there is an item to be stored, DynamoDB computes the hash using Partition key and the MAX size of the ring
  - The resulting hash will return a Number between 0 to 99 (For example we have 100 Nodes in a ring, ideally its more than that)
  - If the computed HASH has returned a NUMBER, lets say EIGHT(8), DynamoDB locates the position 8,
  - And then goes around the ring and find the next NODE which is 17th (in our case)
  - So the item is stored in the NODE 17th and also REPLICATED and COPIED to the next TWO nodes(53rd, and 64th)
  - Now, whenever we need to retrieve the item, DynamoDB will recalculate the hash, which would return EIGHT(8)
  - And then DynamoDB travel around the ring and return the data from the first available node.
  - NOW, if Node 17th Goes DOWN for some reason, then DynamoDB goes aroung the ring from Position 17th to further to find the
    next NODE, which is 53 and so on.
  - Another Benefits of Ring arrangement has to do with the addition of new HOSTS, so if a new node was added in Future,
    let's say at Position 35th, then only a subset of data between 17th and 35 would get affected.
  - The data can be copied easily between the affected nodes
  - And this does not effect the entire data in the table, just a small subset of it.
  - Since the total number of possible NODES in dynamoDB is fairly large, DynamoDB is able to calculate the HASH of
    the item consistently and locate the give item, even if the number of partitions change in future.
  - Moreover, since DynamoDB replicates the data to atleast three nodes, this also provides load balancing

### DynamoDB Partitions in Depth

- One DynamoDB Partition can deliver up to - 1000 WCUs, 3000 RCUs, and 10GB of storage (SSD)
- Formula for calculation no. of partitions based on THROUGHPUT requirement is = P(t) = `Round up[(RCUs/3000) + (WCUs/1000)]`
- Formula for calculation the STORAGE requirement is = P(s) = `Round up[(Storage required in GB / 10 GB)]`
- The Final Formula to calculate required no. of Partition based on BOTH THROUGHPUT and STORAGE will be = P = `max(P(t), P(s))`
- Example 1

  - 1000 RCUs, 500 WCUs, 50GB
  - P(t) = `Round up[(1000/3000) + (500/1000)]`
    P(t) = `Round up[0.67]`
    P(t) = 1
  - P(s) = `Round up[(50GB/10GB)]`
    P(s) = `Round up[50]`
    P(s) = 5
  - P = `max(1,5)`
    P = 5

  - RCUs per Partition = 1000 / 5 = 200 RCUs
  - WCUs per Partition = 500 / 5 = 100 WCUs
  - It will have 5 Partitions each of 10GB with 200 RCUs and 100 WCUs
  - Remember RCU and WCU capacity is shared across the partitions

- Example 2
  - 1000 RCUs, 500 WCUs, 500GB
  - It will have 50 Partitions each of 10GB with 20 RCUs and 10 WCUs (With above calculated Formaula - P(t), P(s), P)
- DynamnoDB does not reduce or deallocate a partition once it is assigned

### DynamoDB Efficient Key Design

- The KEY design in DynamoDB is one of the crucial aspects of ensuring efficient and cost effective performance from DynamoDB
- Choosing the table KEYs wisely ensure an efficient and cost effective performance from DynamoDB
- DynamoDB KEYS
  - Simple Keys: PARTITION KEY
    - Means Each one of the Partition Key can Occur only exactly once in the Table
  - Composite Keys: PARTITION KEY + SORT KEY
    - Means a Partition Key can Occur Multiple Times in the Table, as long as the combination of BOTH is UNIQUE across the Table
- An INDEX Attribute, which include partition and sort attributes, can only use SCALAR Types (string, number, or binary)
- No other Data types, such as OBJECT or ARRAY, can be used as an INDEX attribute

- Data Distribution

  - Ensure uniform distribution of data across partitions
  - Use as many unique values for partition key as possible

- Read/Write Patterns
  - RCUs and WCUs get equally distributed between the partitions
  - Prevent hot Partitions
  - Ensure UNIFORM Utilization of RCUs and WCUs
  - `Hash = f(Partition Key, Size)`
  - Only PARTITION key Influence the partition choice that Dynamo makes; However, SORT key does not.
  - Sort Key is only used to Influence the ORDERING of ITEMS within a Partition
  - Provisioned RCUs and WCUs are equally distributed between all the Partitions
  - The Partition Keys should be Choosen so our READS and WRITES are UNIFORMLY DISTIBUTED across the partition key values
  - For Example
    - If we have 10 PARTITIONS and 1000 RCUs Provisioned, then EACH Partition will receive about 100 RCUs
    - And if our Application request ITEMS from a Single Partition, most of the times (because of non-unique partition key),
      while other partitions remains idle, only about 100 RCUs which are allocated to that Particular Partition will be UTILIZED,
      and remaning 900 RCUs remains UNUTILIZED.
- Ideally the Partition key is selected in a way that our DATA of our Application is DISTRIBUTED UNIFORMALY ACROSS PARTITIONS.
  Then READ Operations will be performed uniformly across partitions means all 1000 RCUs could be utilized.
- This not only ensures efficient utilization of the provisioned capacity, and thereby efficient use of DOLLARS($$$$),
  But also ensure that our application performs at the DESIRED THROUGHPUT.

- Times Series Data

  - Segregate HOT and COLD data into a Separate Tables

- Scan Operations and Filters

  - Always Perfer QUERY Operations as it only search within the specified partition
  - Choose Keys and Indexes so as to Avoid SCAN (Full Table SCAN/All Partitions SCAN) Operations and Filters
  - We must Always specify the COMPLETE PARTITON Key
  - Filters are always applied after the READ operation has been Performed

- Secondary Indexes

  - Local Secondary Indexes(LSI)
    - Same Partition Key as Table Index
    - Use Table Throughput (Capacity Units)
    - Max Limit = 5
  - Global Secondary Indexes(GSI)
    - Different Partition than the Table Index
    - Use its own Throughput (Capacity Units)
    - Max Limit = 5

- When to Choose between LSI vs GSI

  - Local Secondary Indexes
    - When application needs same partition key as the Table
    - When you need to avoid additional Costs
    - When application needs strongly consistent Index Reads
  - Global Secondary Indexes
    - When application needs different or same partition key as the table
    - When application needs FINER THROUGHPUT CONTROL
    - When application only needs eventual consistent index reads

- Item Size
  - Max Size per Item = 400KB
  - Prefer SHORTER (Yet Intuitive) attribute names over long and descriptive once
- Index Attribute Size for string and binary data types

  - Max Size of simple Partition Key = 2 KB
  - Max Size of composite partition key = 1KB
  - Max Size of sort key = 1 KB

- SUMMARY (EFFICIENT KEY DESIGN)
- Partition Key should have many Unique values
- Uniform distribution of Read/Write operations across partitions
- Store Hot and Cold data in separate tables
- Consider all possible query patterns to eliminate use of scna operations and filters
- Choose SORT key depending on your application needs
- Use INDEXES based on when your application's query patterns
- Use Primary KEY or Local Secondary Indexes when Strong Consistency is desired
- Use Global Secondary Index for finer control over throughput or when your
  application needs to query using a different partition key
- Use shorter Attribute name (Per Item limit 400KB)

### Hot Keys or Hot Partitions

- BEST PRACTICE - Spread the data UNIFORMLY ACROSS PARTITIONS
- Our keys should be chosen in a way that READS/WRITES are uniformly distributed across the partition key values - As uniformly as possible.

- What happens when DynamoDB sees an non-uniform ACCESS PATTERNS?
- Two popular use cases here:

  - Time Series Data

    - Your application might sore data based on date and times
    - Not a good idea to create MONTH/DAY or Time Series as Partition key as we may end up with HOT Partitions.

    - To Avoid that,
      1. Either we create a separate table with Time Series data instead of storing YEAR/MONTH/DAY as partition KEY
      2. OR we can cache the HOT data using DynamoDB Accelerator (DAX), which is a caching service

  - Popular Datasets
    - Example - Voting for top 10 contestants in a compition. Usually TOP 3 tends to get More votes than others.
      - 10 contestants = C1, C2, C3, C4, C5, C6, C7, C8, C9, C10
      - 80% of the votes are divided in top 2-3 and rest 20% goes to remainig 7-8
      - This is a typical case of non-uniform load (If we make partition key with contestant name)
      - In case of NON-UNIFORM READ/WRITE, the table throughput will not be utilized well.
      - Resulting of that, there could be throttling of requests leading to incoming votes being rejected
      - So how do we ensure that we do not endup creating HOT Partitions with excessive throttling.
      - THE SOLUTION IS SOMETHING CALLED - SHARDING
      - Shard is nothing but another word for Partition.
      - Meaning we could split our partition keys into more partitions.
        - For example - C1 ==> C1_1, C1_2, C1_3, C1_4, C1_5, C1_6, C1_7, C1_8, C1_9, C1_10
        - WRITE Operations - We can WRITE to any of the partition key started with "C1\_"
        - READ Operations - We can use SHARD AGGREGATION
        - Shard is nothing but another word for Partition
        - So we simply use batched GET ITEM API Operation and the the 10 keys (C1_1...C1_10) to retrieve the result.
        - This way we are able to spread the load on HOT partitions into multiple partitions and prevent operations being throttled.

- These partitions that experience substantial amount of read/write operations as compared to other partitions are called HOT Partitions.
  And these keys can be called HOT Keys.

### DynamoDB Design Patterns

- DynamoDB does not have built-in support for implementing foreign key relationships between two tables.
- However, our application might demand such a need when where we want to implement such relationships
- Data Modeling

  - One-to-One

    - Means simple KEY-VALUES
    - Use Simple Keys (not Composite Keys) on Both Entities
    - One-to-One Relationship within a Table
      - To implement this, we simply create a Global Secondary Index (GSI)
      - Then it will have One-to-One Relationship between the Table PrimaryKey and the GSI
      - Means we can query ITEM either by Table Primary Key OR the GSI Partition Key
    - One-to-One Relationship between two Tables
      - When two tables uses the SAME Primary key.
        - Example, `student_id` as Primary Key in `Students` and `Grades` Tables (One-to-One relationship)
      - OR One tables Primary Key is used in GSI as Partition key to ensure 1-1 relationship.
        - Example, `student_id` as Primary Key in `Students` and GSI partition key in `Grades` Tables (One-to-One relationship)

  - One-to-Many

    - Use Simple Key on one Entity and Composite Key on the other
    - Example 1

      - Student Table - `student_id`(Partition Key) as Primary Key
      - Subjects Table - `student_id`(Partition Key) and `subject`(Sort Key) as Primary Key (One-to-Many relationship)
      - OR Subjects Table - `subject_id`(Partition Key) and GSI with `student_id`(Partition Key) `subject`(Sort Key) - (One-to-Many relationship)
      - OR Just Store Subjects in Student Table as SET ({Math, Physics}) - (One-to-Many relationship)

    - When to use Sort Keys/Composite Keys
      - Large Item Sizes
      - If Querying Multiple Items within a Partition Key is required
    - When to use Set Types
      - Small Item Sizes
      - If Querying Individual Item attribute in Sets is NOT needed (Students By SubjectID)

  - Many-to-Many

    - Composite Keys or Indexes on Both entities
    - The easiest way to Implement Many-to-Many relationship is using a Table and GSI with Partition and Sort Attributes SWITCHED
    - Example
      - Students Table
      - Partition Key and Sort Keys SWITCHED in created Primary Key and GSI
      - Create Primary Key on Students Table using `student_id`(Partition Key) and `subject_id`(Sort Key)
      - Create GSI on Students Table using `subject_id`(Partition Key) and `student_id`(Sort Key)

  - Hierarchical Data Structures
    - There are two ways to Model Hierarchical Data Structures
      - Table Items
        - Composite Keys
        - By Using Partition Key and Sort Key Combinations
      - JSON Documents
        - Store documents as JSON Documents (Map Data Type)

### Multi-value Sorts and Filters

- In DynamoDB we can only have two attributes in INDEX (Partition Key and Sort Key)
- The Sort key allows us to read the itmes in a certain fashion, either in Descending or Ascending Order
- BUT, what if we want to Sort the Items based on Multiple Attributes
- For example, we want to Sort the Address Table elements based on `country`, `state`, and `city`
  - cusomter=abc and country=USA (GSI - cusomter as Partition Key and country as SORT Key )
  - cusomter=abc and city=1234 (GSI - cusomter as Partition Key and city as SORT Key )
  - cusomter=abc and state=San Diago (GSI - cusomter as Partition Key and state as SORT Key)
  - the usual GSI will only let us use any one othe these (`country`, `state`, `city`) as a SORT Key
- To solve that, we can use Multi-Value Sorts
  - To do that we create sort key = `country_state_city` and the store the data in single column (country_state_city) with "|" separator
  - For Example, `US|San Diego|1234` in column name `country_state_city`
    - While Querying, we can use Partial Sorty Keys
      - `customer="john" and country_state_city BEGINS_WITH "US|San"`

### DynamoDB Limits

- Limits

  - Soft Limits
  - Hard Limits

- Capacity and Throughput Limits

  - 4 KB per RCU
  - 1 KB per WCU
  - 10 GB per partition
  - 3000 RCUs or 1000 WCUs per partition
  - Minimum 1 RCUs and 1 WCUs per table or index
  - Max 40,000 RCUs and WCUs each per table (US EAST Region)
  - Max 10,000 RCUs and WCUs each per table (Other Regions)
  - Max 80,000 RCUs and WCUs each per account (US EAST Region)
  - Max 40,000 RCUs and WCUs each per account (Other Regions)
  - Max limits are soft limits and can be increased on Request (By contacting AWS Support)
  - No limit on Scaling up table capacity
  - Max SACLE DOWN limited to 4 times per Calendar Day (UTC Timezone)
  - Additional 1 SCALE DOWN if no SCALE DOWN in last 4 hours
  - Effectively 9 Scale Downs per day
    - (24/4 = 6 .. make it 5 so have more evenly hours) ==> 5 scale down till 20th hour in the interval of every 4 hours
    - Remaning Hours (24-20 == 4 hours, you could use daily limit which is 4)
    - Hence total Max SCALE Down (If Planned Effectively), can be 9(NINE) Times
  - Max 256 tables per region (Soft Limit)

- Index and Attributes Limits

  - 5 Local Secondary Indexes Per Table
  - 5 Global Secondary Indexes Per Table
  - Max 20 user-specified project attribute across all secondary indexes of the Table
  - Max size of partition key = 2 KB
  - Max size of sort key - 1 KB
  - Max size of all items per partition key = 10 GB (Including all LSI's)
  - For Nested Attributes, max possible nesting is 32 Levels Deep

- API Limit

  - Max 10 Simultaneous requests for table-level Operations (CRUD Operations)
  - Max 100 Items (up to 16MB in size), returned per BatchGetItem request
  - Max 25 PutItem or DeleteItem requests(up to 16MB in size) per BatchWriteItem request
  - Max 1 MB data returned per query or scan request

- Paginated Queries/Reads
  - LastEvaluatedKey
  - ExclusiveStartKey

### Error Handling in DynamoDB

- HTTP 400

  - Error in our request
  - Authentication Failure
  - Missing required Parameters
  - Error will have Exception Code/Error Type, Error Message, and Stack Trace Details
  - Examples
    - Access Denied Exception Exception
    - Conditional Check Failed Exception
    - Item Collection Size Limit Exceeded Exception
    - Limit Exceeded Exception
    - Resource In Use Exception
    - Resource not found Exception
    - Validation Exception
    - Provisioned Throughput Exceeded Exception
      - Error Retries
      - Exponential Backoff

- HTTP 5XX
  - HTTP 500 => Service Side Error
  - HTTP 503 => Service Unavailable

### DynamoDB Best Practices

#### TABLE LEVEL BEST PARCTICES

- Uniform Data Access

  - DynamoDB divide the provision throughput equally between all the Partitions
  - Hence, in order to achieve maximum utilization of the provisioned RCUs and WCUs,
    we must DESIGN our table keys in such a way that the READ and WRITE loads are Uniform Across Partitions
  - Examples
    - Provisioned Capacity = 50 RCUs and 50 WCUs
    - No. of Partitions = 5
    - Capacity per Partition = 10 RCUs and 10 WCUs

- Hot Keys or Hot Partitions

  - Time Series Data
    - Segregate HOT and COLD data into Different Tables
    - Use DAX for Caching
  - Popular Datasets
    - Spreading the Partition Key into Multiple Partition Keys - Sharding

- Burst Capacity

  - 300 Seconds
  - For occassional Spikes
  - Not to be relied upon

- DynamoDB Adaptive Capacity

  - For Uniform Workloads
  - Works Automatically
  - No Guarantees
  - 5 to 30 Minutes of Delay

- Temporary Substantial Capacity Scaling
  - We MUST always remember that substantial increases in Provisioned Capacity almost always result in DynamoDB
    allocating additional Partitions, and when we subsequently (once our work is done) scale the capacity down,
    DynamoDB will not de-allocate previously allocated partitions. Since we SCALED Down the capacity each Partition
    will receive a very small chunk of CAPACITY UNITS, resulting in substantial degradation in performance

#### ITEM LEVEL BEST PARCTICES

- Use Shorte yet Intuitive Attribute Names

  - 400 KB Limit per Item (Attribute Names + Values)
  - Applies to Attribute Names and Values

- One-to-Many Relationship

  - When to use WHAT?
    - Sort Keys/Composite Keys
      - Large Item Sizes
      - If Querying Multiple Items within a Partition Key is required
    - Set Types
      - Small Item Sizes
      - If Querying Individual Item attribute in Sets is NOT needed (Students By SubjectID)

- Handling Large Item Attributes

  - Use Compression (GZIP)
  - Use S3 to store large attributes
  - Split Large Attrute across multiple items(item1_1, item1_2), and Retrieve using BatchGetItem API
    - Splitting Partition Keys across Multiple Items

- Best Practices for READ Operations

  - Avoid SCANS
  - Avoid Filters (As they are applied only after the QUERY and SCAN Operations)
  - Use Eventual Consistency
    - Any operation on GSIs are ALWAYS Eventual Consistent

- Best Practices when using Local Secondary Indexes (LSI)

  - Use LSIs Sparingly
    - Shares same Partition Space/Storage and Throughput Capacity (RCUs and WCUs)
  - Project Fewer Attributes
    - We have Maximum of 20 ITEMS per Index, so choose them Carefully
    - Projection Options:
      - KEYS_ONLY - Return the Index Partition and Sort Keys
      - INCLUDE - Retrun Specified Fields Only
      - ALL - Return entire Table Item
    - Also, it is important to project all the attributes that your application needs else if your READ Operation
      requests additional parameters which are not Projected in the INDEX, DynamoDB will have to fetch those attributes
      from the TABLE, and this is going to result in an additional read operation. This uses as more READ Capacity UNITS
      and it also introduces more Latency so spend good amount of time before deciding on which attributes to project in your INDEX.
      The Idea is to consider all the current and future possibilities of the READ operations that our application can possibly need to perform.
  - Use Sparse Indexes

    - An Index that has fewer items as compared to the items in the base table.
    - We know that Local Secondary Indexes(LSI) have the same partition key as of the Primary key of the TABLE.
      In other words, it is just the sort keys in LSI that is different. And not every item in the table is going to have
      the sort key attribute corresponding to the Local Secondary Indexes. So no. of ITEMS in the LSI could be less
      than the total number of items in the BASE Table. So In this case, the INDEX is called a SPARSE Index because it has fewer items
      than the main table.
    - Sparse Indexes are very useful for queries on attributes that don't appear in most table items.
    - Example (Sparse Index)
      - Tickets Table
        - `customer_id`, `ticket_id`, `ticket`, `is_open` ....
        - Primary Partition Key - `customer_id`
        - Primary Sort Key - `ticket_id`
      - Create a LSI with - `customer_id` as Partition Key, and `is_open` as SORT Key
      - `is_open` - will only have some status values if Ticket is `in_progress`, `open`, `on_hold`
      - `is_open` - Property/attribute will not exist if Ticket is RESOLVED/CLOSED
      - Means the created LSI will have fewer Items then the Base table Item

  - Watch for Expanding Item Collections
    - Have 10GB partition Limit
    - So if we have multiple LSI's and with collection of Items in it, we may end up running out of space soon
    - One way to reduce no. of Items in a partition is to use Partition Sharding (Contestant Example)

- Best Practices for Global Secondary Indexes (GSI)
  - Design for Uniform Workloads
  - Use Sparse Indexes
  - Project Fewer Attributes
  - Eventually Consistent Read Replicas
    - One TRICK is to create a Same GSI as Tables Primary Key
    - Advantages:
      - Easier to Divert some of the Load from the Main Table to the Index (GSI)
      - Also, we can control the throughtput separatly
      - And if you have multiple Application that read from this table, and you want to control throughput
        individually for each application. In this case One APP can use Table's Primary Key,
        and Other one could use GSI to perform the READ Operations

### Ways to Lower DynamoDB Costs

- DynamoDB Pricing
  - Provisioned throughput
  - Data Storage
  - Data Transfer
  - Global Tables
  - Backups
  - DAX(DynamoDB Accelerator)
  - DynamoDB Streams
  - Use Sparse indexes
  - Project Fewer Attributes
  - Design for Uniform Workloads
  - Use Compression
  - Avoid Scans and filters
  - Archive Old Data
  - Use Eventual Consistency
  - Choose Low-Cost Region
  - Use Auto-Scaling
  - Leverage Reserved Capacity
