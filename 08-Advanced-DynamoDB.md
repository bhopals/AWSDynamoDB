## Advanced DynamoDB Concepts

### Auto Scaling in DynamoDB

- Many times the READ/WRITE workloads on Ordinary Tables vary over the course of the day, or course of the week or month.
- Some tables see High Throughput during Morning, and low throughput towards the end of the day.
- Some tables see High Throughput during business hours and low throughput during non-business hours or over the weekends.
- We usually provisined the RCUs and WCUs based on Maximum Estimated Throughput that Table Might need.
  However, with such an approach, we end up wasting a lot of Production Capacity during the non-peak periods
  OR when the tables do not have high READ/WRITE Workloads.
- Manual Scaling Limits
  - No limit on Scaling up table capacity
  - Max SACLE DOWN limited to 4 times per Calendar Day (UTC Timezone)
  - Additional 1 SCALE DOWN if no SCALE DOWN in last 4 hours
  - Effectively 9 Scale Downs per day
    - (24/4 = 6 ... make it 5 so have more evenly hours) ==> 5 scale down till 20th hour in the interval of every 4 hours
    - Remaning Hours (24-20 == 4 hours, you could use daily limit which is 4)
    - Hence total Max SCALE Down (If Planned Effectively), can be 9(NINE) Times
- While setting up SCALING, we simply tell DynamoDB the Minimum and Maximum capacity units between which to SCALE the CAPACITY automatically
- MAXIMU CAPACITY <=== % Target Utilization (80%, 70%) ===> MINIMUM CAPACITY
- Provisioned Capacity V/S Consumed Capacity
- DynamoDB AUTO Scaling only kicks in when the actual workload stays elevated or depressed for a sustained period of several Minutes
- We can also APPLY some autoscaling ORDER or RULES or POLICY on GSI off the table (USUAGE)

### DynamoDB Accelerator(DAX)

- DynamoDB Responses are Single DIGIT Milliseconds
- For More Faster response time (Microseconds), we could use DynamoDB Service called DynamoDB Accelerator(DAX)
- DAX Brings down the DynamoDB response time from MILLISECONDS to MICROSECONDS
- DynamoDB Accelerator(DAX)
  - In-Memory Caching
  - Microsecond Latency
- DAX Sits between your APPLICATION and DynamoDB, and it acts as a PROXY for all the requests directed to it.
- If it is WRITE Operation, Data is returned to DAX as well as to DynamoDB
- For Write Operation, DAX uses WRITE-THROUGH Approach
  - It means data is first writted to DynamoDB, and then to DAX, and only if both of these WRITES are successful,
    the WRITE operation is considered to be successful
- If it is READ operation, and DAX has the DATA, which is called CACHE HIT, DAX will simply return the response without going to DynamoDB
- If it is READ operation, and DAX does not have the DATA, which is called CACHE MISS,
  the Data will be returned from DynamoDB and also updated in DAXs Primary or Master Node.
- Only ITEM Level Operations (DATA Related) can be processed through DAX, not TABLE Level Operations (GSI, LSI, Table Config. etc.)
- DAX is highly compatible with DynamoDB as well DynamoDB SDK
- BENEFITS of DAX
  - Cost Savings
  - Microsecond Latency
  - Helps Prevent Hot Partitions
  - Minimum Code Changes
- DAX is useful if our application is READ INTENSIVE or have Repeated READS of the Same DATA
- LIMITATIONS OF DAX
  - Only Eventual Consistency
  - Not for Write-Heavy Applications
- DAX Cluster
  - To use DAX, we use DAX Cluster
  - A DAX Cluster Consist of one or more NODES
  - We can use 10 Nodes per Cluster
  - Each Node is like an instance of DAX, with ONE MASTER/PRIMARY Node, and Remaining ones act as a READ REPLICAS/READER NODES
  - DAX Internally handles Load Balancing between those NODES.
- DAX Caches
  - Internally DAX has TWO Types of Caches
    - Item Cache
      - Stores the results of INDEX READS
      - In other words, it stores the results of getItem, and batchGetItem operations
      - Items in the ITEM CACHE have default TTL or time to live setting off about 5 MINUTES
      - The TTL can be specified while creating DAX CLUSTER
      - Also, if CACHE Gets Full, DynamoDB automatically removes older or less frequently accessed items from Item CACHE
    - Query Cache
      - Stores the results of QUERY or SCAN Operations
      - Default TTL 5 MINUTES
      - It is WORTH metioning here that Updates to the Item CACH or Corresponse DynamoDB Tables do not INVALIDATE the Query Cache
      - Hence the TTL value for the QUERY Cache should be choosen depending on how long your application can tolerate the INCONSISTENT READS.
- When we request strongly consistent READS, they will be served directly from DynamoDB, and will not be updated in DAX
- WRITE-Through Approach - Writes in DynamoDB as well as in DAX
- WRITE-Around Approach - Writes in DynamoDB Only (Skips/Bypass the DAX)

### DynamoDB Streams and DynamoDB Triggers with AWS Lambda

- DynamoDB Streams can be used to Trigger AWS Lambda Functions, and these Lambda functions in turn
  can automatically react to changes in our DynamoDB tables.

- DynamoDB Streams maintains a time ordered log of all the changes in the given DynamnoDB Table.
  This log stores all the time activity that took place in the last 24 HOURS Period.

- DynamoDB Streams

  - 24 HOURS Time-ordered Log, and can be used for:
    - Replication
    - Archival
    - Notifications on Data Change
    - Logging
  - The Use Cases can be numurous, and there are several ways to consume and process the data from the DynamoDB Streams

  - DynamoDB Streams ==> Kinesis Adapter + KCL(Kinesis Client Library) / DynamoDB Streams SDK / AWS LAmbda Triggers
    - Kinesis is a platform for processing high volume streaming data on AWS
    - Another way to work with DynamoDB Streams is using the DynamoDB Streams SDK
    - Much Simpler and Intuitive approach - AWS Lambda Triggers/Events
      - Events/Triggers Fires on the based on the activity within the DynamoDB Streams.
        Means when there is a changes to the ITEMS in the DynamoDB Table, if the DynamoDB Streams
        are enable for that table, these changes would be written to DynamoDB Stream. Once triggered, the
        lambda function then can run are called REACT to these Items changes.
        AWS Lambda Function is an AWS Service that allows you to run your code in Serverless Environment.
        In other words, Lambda is a Function as a Service(FaaS)

- To Enable Streams, Click on Oveview ==> MANAGE STREAM and select one of the OPTIONS given:
  - Keys only - only the key attributes of the modified item
  - New Image - the entire item, as it appears after it was modified
  - Old Image - the entire item, as it appears before it was modified
  - New and Old Image - both the new and old Image of the Item
- By choosing one of the option here, we tell DynamDB that whenever there is a change to any item in the table, what data should
  we return to the underlying stream?
- Once one of the options SELECTED, Clink on ENABLE Stream Button to Enable DynamoDB Streams

- To attach Trigger to the Stream, click on Triggers Section ==> Create Trigger ==> Create Lambda/Select Existing
  ==> Set Trigger Name ==> Select Batch Size ==> create trigger

### Time to Live (TTL) in DynamoDB

- Time to Live or TTL is a very Simple and Convenient Feature in DynamoDB,
- It allows us to tell the DynamoDB when to delet an item from the TABLE.
- TTL Attributes
  - Defines expiry timestamp of the table item (EPOCH or UNIX Timestamp)
  - Items marked for deletion on expiry
  - Expired items are removed from the table and indexes within about 48 HOURS
  - Application should use filter operations to exclude items marked for deletion
- Enable TTL

  - Create/Add an attribute in DynamDB Item with any name, for example - "expires"
  - It should be of number type, and should have timestamp value (Future time that we want as expiry/ttl value)
  - GO to Overview ==> Manage TTL ==> Add the name of the TTL Attribute "expires" ==> Run Preview (To Test it) ==> Save

- The TTL Feature can be very useful, especially with time series tables, such as, a table that stores Daily Temperature
- If we want expired data to be archived/moved into another table, we can use DynamoDB Triggers + Lambda Functions

### Global Tables in DynamoDB

- Global Tables are DynamoDB's built-in solution for Cross-region replication
- Why do we need Cross-Region Replication?

  - We might have uses spread across the world (multi regions), and if our tables are located in us-west-2, then the
    LATENCY (Response time experienced) by users in the diagonally opposite part of the Glove might be much HIGER than
    the People in Oregan are in the United State. So you may want to place a Replica of your DynamoDB Table in Europe,
    and this would simply improve the application Response Times for Users in Europe.
  - You could also achieve this using DynamoDB Streams, by replicating Data in Multiple Tables in other REGIONS;
  - However, DynamoDB now PROVIDES us with a BUILT-IN Solution to address this situation, and its called as the GLOBAL Tables.
  - So we no longer have to build and maintain a Custom Replication Solution.

- Global Tables are easy to setup, and they take away aby complexity that might be introduced by our OWN Custom Replication Solutions.
- Global Tables provide an autmatic multi master cross-region replication to AWS Regions across the Globe.
- When we create a Global Table, we simply specify the regions in which we want to have the table replicas.
  And DynamoDB does all the Heavy-Lifting in the background to ensure our records remain in Sync and Provide us the Desired Latency

- Global Tables (Conditions or Prerequisites)
  - Tables must be Empty across Regions (When Adding a new regions or setting it up initially)
  - Only ONE Replica per Region
  - Same Table Name and Keys Across Regions
  - Table also should have Streams Enabled with "New and Old Images"
  - DynamoDB also adds some attributes to the Global Tables (such as "aws:rep:"), so we should never alter any of these attributes
  - Near Real-Time Replication
  - Eventual Consistency for CROSS REGIONS READS
  - Strong Consistency for SAME REGIONS READS
  - DynamoDB uses "Last Writer Wins" to resolve any conflicts
  - We should use consistent Settings for Tables and Indexes across Regions
