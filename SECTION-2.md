## Background Concepts (RDBMS, NoSQL, JSON, Javascript, NodeJS)

### Data Normalization

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
      * A transitive functional dependency is when changing a non-key column, might cause any of the other non-key columns to change

  * Achieving 3NF is usually considered good.

  - BCNF (Boyce-Codd Normal Form) -
    Even when a database is in 3rd Normal Form, still there would be anomalies resulted if it has more than one Candidate Key.
    Sometimes is BCNF is also referred as 3.5 Normal Form.

  - 4NF -
    If no database table instance contains two or more, independent and multivalued data describing the relevant entity, then it is in 4th Normal Form.

  - 5NF -
    A table is in 5th Normal Form only if it is in 4NF and it cannot be decomposed into any number of smaller tables without loss of data.

  - 6NF -
    6th Normal Form is not standardized, yet however, it is being discussed by database experts for some time. Hopefully, we would have a clear & standardized definition for 6th Normal Form in the near future…
