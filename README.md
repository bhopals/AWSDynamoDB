# AWSDynamoDB

AWS Dynamo DB

## Introduction

## Background Concepts (RDBMS, NoSQL, JSON, Javascript, NodeJS)

## Resources

## Keywords

- Transitive Functional Dependencies
- Unstructured Data
- Huge Volumes of Data
- High Frequency Data
- Big Data (Volume, Velocity, Variety)
- You can bring down the response times from Milliseconds to microseconds at any scale
- Which is quite a Stellar Performance in my opinion.
- DynamoDB - A Distributed System of Nodes and Clusters
- DynamoDB uses several techniques such as CONSISTENT HASHING to STORE/RETRIEVE data reliably
- The returned HASH from Hash Algo. uniquely identifies a place to store a Particular table item within DynamoDB Distributed System of Nodes or Clusters
- A Predictable and Consistent Performance at SCALE
- DynamnoDB does not reduce or deallocate a partition once it is assigned
- Simple Keys (PARTITION KEY) - Means Each one of the Partition Key can Occur only exactly once in the Table
- Composite Keys: (PARTITION KEY + SORT KEY) - Means a Partition Key can Occur Multiple Times in the Table, , as long as the combination of BOTH is UNIQUE across the Table
- The Partition Keys should be Choosen so our READS and WRITES are UNIFORMLY DISTIBUTED across the partition key values
- Segregate HOT and COLD data into a Separate Tables
- To Achieve ATOMICITY, we can use ATOMIC Counter/Views in Dynamo Table. Means if there are Simultaneous requests to increment a variable, all these requests will be applied in ORDER in which they wre received.
- Spread the data UNIFORMLY ACROSS PARTITIONS - READ/WRITES are Uniformaly Distributed across partitions
- Prevent Operation being Throttled
- Shard Aggregation using - Batch GET ITEM API
- Error Retries
- Exponential Backoff
- DynamoDB Adaptive Capacity
- Substaintial reduction in READ Operations
- Table Keys - Primary Partition Key, Primary Sort Key
- Local Secondary Index(LSI)/Global Secondary Index(GSI) - Partition Key, Sort Key
- One way to reduce no. of Items in a partition is to use Partition Sharding (Contestant Example)
