# AWSDynamoDB

AWS Dynamo DB

## Introduction

- What is DynamoDB
  - A NoSQL Database Service
  - Fully Managed Cloud Database
  - Seamless On-Demand Scaling (Scaling up and Scaling Down)
  - Unlimited Concurrent Read/Write Operations
  - Single-digit Millisecond Latency
  - Sub-microsecond latency with DAX (DynamoDB Accelerator)

## Background Concepts (RDBMS, NoSQL, JSON, Javascript, NodeJS)

- Data Normalization

  - Why Normalization: Efficient Organize Data, Eliminate Redundant Data
  - Normalized Forms

    - 1NF - Eliminate Repeating Groups
      - RULES (unique record, and single value per column)
        - Each Table cell should contain a Single Value
        - Each Record should be unique
    - 2NF - Eliminate Redundant Data

      - RULES (Primary Key - Foreign Key)
        - Be in 1NF
        - Single Column Primary Key that does not functonally dependant on any subset of candidate key relation

    - 3NF - Eliminate Columns that are not dependent on Key
      - RULES
        - Be in 2NF
        - Has no Transitive Functional Dependencies
    - BCNF (Boyce-Codd Normal Form) -
    - 4NF -
    - 5NF -
    - 6NF -

## Resources
