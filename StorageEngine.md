# Storage Engine

## Introduction:

The database engine has two major components:

- Storage Engine
- Query Processor

### Storage Engine:

The storage engine writes data to and retrieves data from stable media like disks, hard-drives etc.

#### Standard definition:

A storage engine is the underlying **software component**  that a database management system uses to create, read,
update and delete data from a database. Most database management systems include their own API that allows the user to
interact with their underlying engine without going through the user interface of the DBMS.

**Example for storage engine**: InnoDB, MyISAM

- In other words, The storage engine refers to the ***code that actually stores and retrieves data***
- The storage engine refers to the ***type of table, and the storage engine determines how the table is stored in the
  computer***
- Each technology uses different storage mechanisms, indexing techniques, locking levels, and ultimately provides a wide
  range of functions and capabilities;
- These different data storage technologies are logically transformed into the "storage engine layer" in the overall
  architecture of MySQL
- Storage engine, usually called "table type" (that is, you can specify the storage engine when creating a table, but
  you cannot specify a storage engine for a database)

#### Types:

1. Transactional - ability to perform rollback and commit in a transaction
2. Non - Transactional - manual code for changes as no impact of rollback or commit

- For MySQL 5.5 and later, the default storage engine is *InnoDB*. The default storage engine for MySQL prior to version
  5.5 was *MyISAM*.

---

## MySQL 8.0 Supported Storage Engines

- [`InnoDB`](https://dev.mysql.com/doc/refman/8.0/en/innodb-storage-engine.html): The default storage engine in MySQL
  8.0. `InnoDB` is a transaction-safe (ACID compliant) storage engine for MySQL that has commit, rollback, and
  crash-recovery capabilities to protect user data. `InnoDB` row-level locking (without escalation to coarser
  granularity locks) and Oracle-style consistent nonlocking reads increase multi-user concurrency and
  performance. `InnoDB` stores user data in clustered indexes to reduce I/O for common queries based on primary keys. To
  maintain data integrity, `InnoDB` also supports `FOREIGN KEY` referential-integrity constraints. For more information
  about `InnoDB`, see
- [`MyISAM`](https://dev.mysql.com/doc/refman/8.0/en/myisam-storage-engine.html): These tables have a small
  footprint. [Table-level locking](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_table_lock) limits the
  performance in read/write workloads, so it is often used in read-only or read-mostly workloads in Web and data
  warehousing configurations.
- [`Memory`](https://dev.mysql.com/doc/refman/8.0/en/memory-storage-engine.html): Stores all data in RAM, for fast
  access in environments that require quick lookups of non-critical data. This engine was formerly known as the `HEAP`
  engine. Its use cases are decreasing; `InnoDB` with its buffer pool memory area provides a general-purpose and durable
  way to keep most or all data in memory, and `NDBCLUSTER` provides fast key-value lookups for huge distributed data
  sets.
- [`CSV`](https://dev.mysql.com/doc/refman/8.0/en/csv-storage-engine.html): Its tables are really text files with
  comma-separated values. CSV tables let you import or dump data in CSV format, to exchange data with scripts and
  applications that read and write that same format. Because CSV tables are not indexed, you typically keep the data
  in `InnoDB` tables during normal operation, and only use CSV tables during the import or export stage.
- [`Archive`](https://dev.mysql.com/doc/refman/8.0/en/archive-storage-engine.html): These compact, unindexed tables are
  intended for storing and retrieving large amounts of seldom-referenced historical, archived, or security audit
  information.
- [`Blackhole`](https://dev.mysql.com/doc/refman/8.0/en/blackhole-storage-engine.html): The Blackhole storage engine
  accepts but does not store data, similar to the Unix `/dev/null` device. Queries always return an empty set. These
  tables can be used in replication configurations where DML statements are sent to replica servers, but the source
  server does not keep its own copy of the data.
- [`NDB`](https://dev.mysql.com/doc/refman/8.0/en/mysql-cluster.html) (also known
  as [`NDBCLUSTER`](https://dev.mysql.com/doc/refman/8.0/en/mysql-cluster.html)): This clustered database engine is
  particularly suited for applications that require the highest possible degree of uptime and availability.
- [`Merge`](https://dev.mysql.com/doc/refman/8.0/en/merge-storage-engine.html): Enables a MySQL DBA or developer to
  logically group a series of identical `MyISAM` tables and reference them as one object. Good for VLDB environments
  such as data warehousing.
- [`Example`](https://dev.mysql.com/doc/refman/8.0/en/example-storage-engine.html): This engine serves as an example in
  the MySQL source code that illustrates how to begin writing new storage engines. It is primarily of interest to
  developers. The storage engine is a “stub” that does nothing. You can create tables with this engine, but no data can
  be stored in them or retrieved from them.

---

#### General Terms:

##### **Transaction**

Transactions are atomic units of work that can be **committed** or **rolled back**. When a transaction makes multiple
changes to the database, either all the changes succeed when the transaction is committed, or all the changes are undone
when the transaction is rolled back.

Database transactions, as implemented by `InnoDB`, have properties that are collectively known by the acronym **ACID**,
for atomicity, consistency, isolation, and durability.

**Lock:**

The high-level notion of an object that controls access to a resource, such as a table, row, or internal data structure,
as part of a **locking** strategy. For intensive performance tuning, you might delve into the actual structures that
implement locks, such as **mutexes** and **latches**

##### **Lock mode**

A shared (S) **lock** allows a **transaction** to read a row. Multiple transactions can acquire an S lock on that same
row at the same time.

An exclusive (X) lock allows a transaction to update or delete a row. No other transaction can acquire any kind of lock
on that same row at the same time.

##### **shared lock:**

A kind of **lock** that allows other **transactions** to read the locked object, and to also acquire other shared locks
on it, but not to write to it. The opposite of **exclusive lock**.

##### **Exclusive lock**

A kind of **lock** that prevents any other **transaction** from locking the same row. Depending on the transaction **
isolation level**, this kind of lock might block other transactions from writing to the same row, or might also block
other transactions from reading the same row.

##### **ACID**

- An acronym standing for atomicity, consistency, isolation, and durability. These properties are all desirable in a
  database system, and are all closely tied to the notion of a **transaction**. The transactional features of `InnoDB`
  adhere to the ACID principles.

- Transactions are **atomic** units of work that can be **committed** or **rolled back**. When a transaction makes
  multiple changes to the database, either all the changes succeed when the transaction is committed, or all the changes
  are undone when the transaction is rolled back.

- The database remains in a consistent state at all times — after each commit or rollback, and while transactions are in
  progress. If related data is being updated across multiple tables, queries see either all old values or all new
  values, not a mix of old and new values.

- Transactions are protected (isolated) from each other while they are in progress; they cannot interfere with each
  other or see each other's uncommitted data. This isolation is achieved through the **locking** mechanism. Experienced
  users can adjust the **isolation level**, trading off less protection in favor of increased performance and **
  concurrency**, when they can be sure that the transactions really do not interfere with each other.

- The results of transactions are durable: once a commit operation succeeds, the changes made by that transaction are
  safe from power failures, system crashes, race conditions, or other potential dangers that many non-database
  applications are vulnerable to. Durability typically involves writing to disk storage, with a certain amount of
  redundancy to protect against power failures or software crashes during write operations. (In `InnoDB`, the **
  doublewrite buffer** assists with durability.

---

#### Table-Level Locking

MySQL uses [table-level locking](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_table_lock) for `MyISAM`
, `MEMORY`, and `MERGE` tables, permitting only one session to update those tables at a time. This locking level makes
these storage engines more suitable for read-only, read-mostly, or single-user applications.

These storage engines avoid [deadlocks](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_deadlock) by always
requesting all needed locks at once at the beginning of a query and always locking the tables in the same order. The
trade off is that this strategy reduces concurrency; other sessions that want to modify the table must wait until the
current data change statement finishes.

Advantages of table-level locking:

- Relatively little memory required (row locking requires memory per row or group of rows locked)
- Fast when used on a large part of the table because only a single lock is involved.
- Fast if you often do `GROUP BY` operations on a large part of the data or must scan the entire table frequently.

---

## InnoDB storage engine:

`InnoDB` is a general-purpose storage engine that balances high reliability and high performance. In MySQL 8.0, `InnoDB`
is the default MySQL storage engine. Unless you have configured a different default storage engine, issuing
a [`CREATE TABLE`](https://dev.mysql.com/doc/refman/8.0/en/create-table.html) statement without an `ENGINE` clause
creates an `InnoDB` table.

### Key advantages:

- Follows ACID model for transactions featuring commit, rollback and crash-recovery capabilities.
  - **ACID model:**


- **Row level locking mechanism** to increase multi-user concurrency and performance.
- `InnoDB` tables arrange your data on disk to optimize queries based on **primary keys**. Each `InnoDB` table has a
  primary key index called the **clustered index** that organizes the data to minimize I/O for primary key lookups.
- Supports foreign key constraints to maintain data integrity.

| Feature                                                      | Support |
| :----------------------------------------------------------- | :------ |
| **B-tree indexes**                                           | Yes     |
| **Backup/point-in-time recovery** (Implemented in the server, rather than in the storage engine.) | Yes     |
| **Cluster database support**                                 | No      |
| **Clustered indexes**                                        | Yes     |
| **Compressed data**                                          | Yes     |
| **Data caches**                                              | Yes     |
| **Encrypted data**                                           | Yes     |
| **Foreign key support**                                      | Yes     |
| **Full-text search indexes**                                 | Yes     |
| **Geospatial data type support**                             | Yes     |
| **Geospatial indexing support**                              | Yes     |
| **Hash indexes**                                             | No      |
| **Index caches**                                             | Yes     |
| **Locking granularity**                                      | Row     |
| **MVCC**                                                     | Yes     |
| **Replication support**                                      | Yes     |
| **Storage limits**                                           | 64TB    |
| **T-tree indexes**                                           | No      |
| **Transactions**                                             | Yes     |
| **Update statistics for data dictionary**                    | Yes     |

---

### Internal Data Structure used in InnoDB:

Innodb uses B-tree as the internal data-structure. B-Tree is a self-balancing tree data structure that keeps data sorted
and allows searches, sequential access, insertions, and deletions in logarithmic time

![img](https://cdn-images-1.medium.com/max/1600/0*5ZExOe0pf6SQRfBE.png)

### Pros & Cons

- B-trees usually grow wide and shallow, so for most queries very few nodes need to be traversed. The net result is high
  throughput, low latency reads.

- However, the need to maintain a well-ordered data structure with random writes usually leads to poor write
  performance. This is because random writes to the storage are more expensive than sequential writes. Also, a minor
  update to a row in a block requires a read-modify-write penalty of an entire block.

#### What is random writes ?

When you **write** two blocks that are next to each-other on disk, you have a sequential **write**. When you **write**
two blocks that are located far away from each other on disk, you have **random writes**.

---

> show engines;

> create table t1 (i int) engine=innodb;

> alter table t1 engine=innodb;