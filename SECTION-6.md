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

### Atomic Counters - Item Level Operations with AWS SDK

### Read Operations - Item Level Operations with AWS SDK

### Paginated Reads - Item Level Operations with AWS SDK
