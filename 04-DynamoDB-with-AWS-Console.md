## Working With DynamoDB with AWS Console

### Table-Level Operations with AWS Console

- Create A Table

### Item-Level Operations with AWS Console

- Global Secondary Indexes - Partition Key other that Table's Partition/Primary Key
- Local Secondary Indexes - Partition Key same as Table's Primary Key'

- FILTER

  - Specifying FILTER conditions on the Table will not affect the RCU/WCY consumed by the particular READ Operation
  - Filter is just convenient Feature

- SCAN
  - Scan Operations can provide data across partitions and just like Query Operations, we can also specify FILTER Expression as well.
  - Scan Operations perform searches across the table, partitions, they can use up the RCUs very FAST, and
    that is the reason we should avoid using scan operations as far as possible and prefer the query operations instead.
  - This is the reason why Well-thought-out data modeling is very crucial when working with DynamoDB.
  - We should choose Table Keys and Indexes very carefully considering all the different possibilities of query operations
    that our Application might need to perform now as well as in future.

## Additional Features in DynamoDB Console

- All Types of Metrics (Error, Query, Success, Latency, Request by GSI or LSI)
- Scaling Activities
- Provisioned Capacity
- ALARMS
- Global Table (Mutli Region Table)
- Backups
- Triggers (Connect DynamoDB Streams to Lambda Function)
- Access Control
- Tags
