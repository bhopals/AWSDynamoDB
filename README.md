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
