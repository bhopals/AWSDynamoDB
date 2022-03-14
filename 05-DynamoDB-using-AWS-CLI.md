## Working With DynamoDB using AWS CLI

### Installing AWS CLI

A Command Line tool that we can use to interact with any of the AWS Services

### Table Level Operations with AWS CLI

`aws dynamodb list-tables`
`aws dynamodb describe-table --table-name td_notes`

### Write Operations - Item level Operations with AWS CLI

`aws dynamodb put-item --table-name td_notes --item file://item.json`
`aws dynamodb batch-write-item --request-items file://item.json`

### Read Operations - Item level Operations with AWS CLI

`aws dynamodb get-item --table-name td_notes --key <partition-key>`
`aws dynamodb get-item --table-name td_notes --key-condition-expression <key-condition-expression>`
