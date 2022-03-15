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
  - The resulting hash will return a Number between 0 to 99
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

### DynamoDB Efficient Key Design

### Hot Keys or Hot Partitions

### DynamoDB Design Patterns

### Multi-value Sorts and Filters

### DynamoDB Limits

### Error Handling in DynamoDB

### DynamoDB Best Practices

### Ways to Lower DynamoDB Costs
