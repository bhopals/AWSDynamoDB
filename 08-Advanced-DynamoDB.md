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

### DynamoDB Streams and DynamoDB Triggers with AWS Lambda

### Time to Live (TTL) in DynamoDB

### Global Tables in DynamoDB
