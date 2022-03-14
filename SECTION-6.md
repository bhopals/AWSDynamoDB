## Working with DynamoDB using AWS SDK

### Working with DynamoDB using AWS SDK - Module Introduction

- `npm init`
- `npm install aws-sdk --save`

### Table Level Operationss with AWS SDK

- CRUD Operations (Create, Update, Read (List), Delete Table)

### Write Operations - Item Level Operations with AWS SDK

- AWS.DynamoDB
  - Low Level Programming Approach
  - Interaction with DynamoDB client Directly
- AWS.DynamoDB.DocumentClient
  - Document client class a Wrapper around AWS.DynamoDB
  - High Level Access to DynamoDB when working with Items
  - Refer - https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html
- UpdateExpression: `set #t=:t`
- ExpressionAttributeNames: `{ '#t' : 'title' }`
- ExpressionAttributeValues: `{ ':t' : 'Updated Titme' }`

### Conditional Writes - Item Level Operations with AWS SDK

- Conditional Writes
  - Are Idempotent
  - Returns `ConditionalCheckFailedException` if condition fails
  - Consumed Capacity not returned if Error
  - WCUs are still consumed even if the Update Failed
- Example
  - ConditionExpress: `#t <> :t`
  - ExpressionAttributeNames: `{ '#t' : 'timestamp' }`
  - ExpressionAttributeValues: `{ ':t' : 1 }`

### Atomic Counters - Item Level Operations with AWS SDK

- Increment/Decrement Atomically
- UpdateItem API
- Not Idempotent
- All requests are applied in order
- Means if there are Simultaneous requests to increment a variable, all these requests will be applied in ORDER in which they wre received.
- Not suitable for applications demanding high degree of ACCURACY
- Implementing an ATOMIC Counter is fairly easy, and we can do that Simply using an `UpdateExpression`
- Example (view is a Column/Attribute in Table to keep track on the current view counter)
  - UpdateExpression: `set #v = #v + :incr`
  - ExpressAttributeNames: `{ '#v': 'views'}`
  - ExpressionAttributeValues: `{ ':incr': 1}`

### Read Operations - Item Level Operations with AWS SDK

### Paginated Reads - Item Level Operations with AWS SDK
