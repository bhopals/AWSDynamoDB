## Advanced DynamoDB

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
    - (24/4 = 6 .. make it 5 so have more evenly hours) ==> 5 scale down till 20th hour in the interval of every 4 hours
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

### Time to Live (TTL) in DynamoDB

### Global Tables in DynamoDB
