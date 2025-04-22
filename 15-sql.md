# 15 - SQL

## SQL (Structured Query Language)

SQL is a domain-specific language used for managing and manipulating relational databases. It serves as the standard language for relational database management systems (RDBMS).

### Core Components of SQL

#### 1. Data Definition Language (DDL)

Commands that define and modify database structure:

- `CREATE`: Create databases, tables, indexes, views
- `ALTER`: Modify existing database objects
- `DROP`: Remove database objects
- `TRUNCATE`: Remove all records from a table

#### 2. Data Manipulation Language (DML)

Commands that manipulate data within tables:

- `SELECT`: Retrieve data from one or more tables
- `INSERT`: Add new records
- `UPDATE`: Modify existing records
- `DELETE`: Remove records

#### 3. Data Control Language (DCL)

Commands that control access to data:

- `GRANT`: Give privileges to users
- `REVOKE`: Remove privileges from users

#### 4. Transaction Control Language (TCL)

Commands that manage transactions:

- `COMMIT`: Save changes permanently
- `ROLLBACK`: Restore to the last commit point
- `SAVEPOINT`: Create points to roll back to

### Key SQL Concepts

#### Joins

Connect rows from multiple tables based on related columns:

- **INNER JOIN**: Returns records with matching values in both tables
- **LEFT JOIN**: Returns all records from the left table and matching records from the right
- **RIGHT JOIN**: Returns all records from the right table and matching records from the left
- **FULL JOIN**: Returns all records when there is a match in either table

#### Indexes

Special data structures that improve the speed of data retrieval operations:

- Make queries faster but can slow down write operations
- Primary keys are automatically indexed

#### Constraints

Rules enforced on data columns:

- **PRIMARY KEY**: Uniquely identifies each record
- **FOREIGN KEY**: Ensures referential integrity
- **UNIQUE**: Ensures all values in a column are different
- **CHECK**: Ensures values meet specified conditions
- **NOT NULL**: Ensures a column cannot have NULL value

#### Aggregate Functions

Operations that perform calculations on sets of values:

- `COUNT()`: Counts rows
- `SUM()`: Calculates total
- `AVG()`: Calculates average
- `MIN()`: Finds minimum value
- `MAX()`: Finds maximum value

## B+ Trees

B+ Trees are self-balancing tree data structures that maintain sorted data and allow for efficient insertion, deletion, and search operations. They are the most common implementation for indexes in database systems.

### Structure of B+ Trees

#### 1. Nodes

- **Root Node**: The top node of the tree
- **Internal Nodes**: Contain keys and pointers to child nodes
- **Leaf Nodes**: Store actual data or pointers to data

#### 2. Properties

- All leaf nodes are at the same level (balanced tree)
- Each node contains between m/2 and m children (where m is the order of the tree)
- Leaf nodes are linked together in a linked list (crucial for range queries)
- All keys are present in leaf nodes

### B+ Tree Operations

#### Search Operation

1. Start at the root node
2. For each level, find the appropriate subtree based on key comparison
3. Continue until reaching a leaf node
4. Scan the leaf node for the target key

#### Range Queries

1. Search for the lower bound key
2. Once found in a leaf node, traverse the linked list of leaf nodes
3. Continue until reaching the upper bound
4. Time complexity: O(log n + k) where k is the number of elements in the range

### Advantages in Database Systems

- **Shallow depth**: Most databases can find any row with 3-4 disk reads
- **Sequential access**: Leaf nodes link enables efficient range scans
- **Space utilization**: High branching factor minimizes wasted space
- **Self-balancing**: Maintains performance even with frequent changes

## ACID Properties

ACID is an acronym that represents a set of properties ensuring reliable processing of database transactions.

### The Four ACID Properties

#### 1. Atomicity

- Transactions are "all or nothing"
- If any part fails, the entire transaction fails (rollback)
- The database state remains unchanged if a transaction fails

**Example**: A bank transfer must either complete fully (debit one account and credit another) or not happen at all. Partial completion is not acceptable.

#### 2. Consistency

- Transactions only transition the database from one valid state to another
- All constraints, triggers, and rules must be satisfied
- Data integrity is preserved

**Example**: If a table has a constraint that account balances cannot be negative, any transaction resulting in a negative balance will be rejected.

#### 3. Isolation

- Concurrent transactions do not interfere with each other
- Results of a transaction are invisible to other transactions until completed
- Prevents "dirty reads," "non-repeatable reads," and "phantom reads"

**Example**: When two users update the same data simultaneously, isolation ensures one user's changes don't overwrite or interfere with the other's.

#### 4. Durability

- Once a transaction is committed, it remains so
- Changes survive system failures (power outages, crashes)
- Typically implemented using transaction logs

**Example**: After confirming a payment, the data is permanently stored even if the database crashes immediately afterward.

### ACID vs. BASE

Modern distributed systems sometimes use BASE (Basically Available, Soft state, Eventually consistent) as an alternative to ACID:

| ACID                 | BASE                        |
| -------------------- | --------------------------- |
| Strong consistency   | Eventual consistency        |
| High isolation       | Lower isolation             |
| Focus on reliability | Focus on availability       |
| Traditional RDBMS    | Often used in NoSQL systems |
