# Database Theory: Key Concepts & Mechanics

A foundational guide covering core database concepts, architectural mechanics, storage engine internals, data models, and system-level trade-offs in modern database management systems.

---

## Index

1. [What is a database, and why do we need one?](#what-is-a-database-and-why-do-we-need-one)
2. [What are the differences between Online Transaction Processing (OLTP) and Online Analytical Processing (OLAP)?](#what-are-the-differences-between-online-transaction-processing-oltp-and-online-analytical-processing-olap)
3. [What are the most common types of databases, and when should each be used?](#what-are-the-most-common-types-of-databases-and-when-should-each-be-used)
4. [What is the architecture of a modern Database Management System (DBMS)?](#what-is-the-architecture-of-a-modern-database-management-system-dbms)
5. [What is a database storage engine, and how do B+Tree and Log-Structured Merge-tree (LSM) architectures differ?](#what-is-a-database-storage-engine-and-how-do-btree-and-log-structured-merge-tree-lsm-architectures-differ)
6. [What are ACID guarantees, and how do database management systems implement and enforce them?](#what-are-acid-guarantees-and-how-do-database-management-systems-implement-and-enforce-them)
7. [What is the BASE consistency model, and how does it compare to ACID in distributed databases?](#what-is-the-base-consistency-model-and-how-does-it-compare-to-acid-in-distributed-databases)

---

## What is a database, and why do we need one?

A database is an organized, persistent collection of data managed by a Database Management System (DBMS) that provides efficient storage, querying, indexing, and transaction management.

Rather than relying on flat files, databases solve core concurrency, durability, and access problems:
- **Buffer Pool Management**: Efficiently caches disk pages in RAM to minimize expensive block I/O.
- **Durability & Crash Recovery**: Uses Write-Ahead Logging (WAL) and `fsync` to guarantee data survives sudden hardware or OS crashes.
- **Logarithmic Indexing**: Employs B+Trees or LSM-trees to provide fast lookups instead of full disk scans.
- **Concurrency Control**: Implements Multi-Version Concurrency Control (MVCC) or lock managers to prevent dirty reads, lost updates, and race conditions without manual application-level file locking.

---

## What are the differences between Online Transaction Processing (OLTP) and Online Analytical Processing (OLAP)?

OLTP and OLAP represent fundamentally distinct database workloads optimized for opposite operational patterns:
- **OLTP (Transactional)**: High-concurrency, low-latency workloads where short read-write transactions touch a small number of rows (e.g., e-commerce checkouts, bank transfers).
- **OLAP (Analytical)**: Low-concurrency, high-throughput aggregate workloads where complex queries scan millions of rows across wide datasets (e.g., business intelligence, reporting).

### Storage & Execution Mechanics
- **OLTP (Row-Oriented / N-ary Storage)**: Stores complete rows contiguously on disk pages (e.g., PostgreSQL, MySQL). Optimizes fast single-row lookups, insertions, and point updates via B+Tree indexes and row-level locks.
- **OLAP (Column-Oriented)**: Stores all values of a single column contiguously (e.g., ClickHouse, Snowflake, Parquet files). Achieves high compression ratios (run-length or dictionary encoding) and vectorized SIMD CPU execution, reading only the queried columns (e.g., `SUM(revenue)`) and skipping the rest.

### Architectural Trade-offs
- Running analytical queries directly on OLTP databases evicts hot pages from the buffer pool and causes lock contention on transactional tables.
- **Production Standard**: Extract transactional data via Change Data Capture (CDC, using Debezium or Kafka) into dedicated OLAP data warehouses. Modern architectures also leverage HTAP engines (e.g., TiDB, SingleStore) maintaining dual row and column replicas.

---

## What are the most common types of databases, and when should each be used?

Modern databases follow the **polyglot persistence** model, where different storage paradigms solve distinct access patterns:

1. **Relational Databases (RDBMS)**: (e.g., PostgreSQL, MySQL) Enforce structured tabular schemas with explicit foreign keys, complex SQL joins, and strict ACID guarantees.
   - *Best For*: Financial ledgers, ERPs, and domains where data integrity and relational constraints are non-negotiable.
2. **Document Databases**: (e.g., MongoDB, Couchbase) Store semi-structured data as hierarchical JSON/BSON documents with dynamic schemas, eliminating joins for self-contained records.
   - *Best For*: Content management, product catalogs, and user profiles with evolving schema structures.
3. **Key-Value Stores**: (e.g., Redis, Memcached, DynamoDB) Store arbitrary blobs indexed by unique string keys, operating with simple get/put/delete primitives at sub-millisecond latencies.
   - *Best For*: Session caching, rate limiting, and real-time leaderboards.
4. **Graph Databases**: (e.g., Neo4j, Amazon Neptune) Model data as nodes, edges, and properties using index-free adjacency (nodes point directly to memory neighbors), avoiding costly join operations.
   - *Best For*: Social graphs, fraud detection networks, and recommendation engines.
5. **Wide-Column / Column-Family Stores**: (e.g., Apache Cassandra, ScyllaDB) Store data in dynamic column families partitioned across a distributed hash ring, writing via LSM-trees.
   - *Best For*: High-ingest time-series metrics, IoT streams, and event logging.

---

## What is the architecture of a modern Database Management System (DBMS)?

A modern DBMS translates high-level declarative queries (SQL) into safe physical I/O operations through six core decoupled subsystems:

1. **Transport & Connection Manager**: Manages client TCP connections, thread pools, authentication, and wire protocols (e.g., PostgreSQL wire format).
2. **Parser & Lexer**: Converts raw SQL into an Abstract Syntax Tree (AST), verifying table identifiers, types, and permissions against the system catalog.
3. **Query Optimizer & Planner**: Uses Cost-Based Optimization (CBO) and table statistics (histograms, selectivity) to choose join orders, index scans, and predicate evaluations, outputting an optimal physical plan.
4. **Execution Engine**: Executes the plan using vectorized execution or the Volcano iterator model (`open()`, `next()`, `close()`), streaming rows between relational operators.
5. **Buffer Pool Manager**: Caches disk pages in RAM using replacement algorithms (LRU, Clock, 2Q) and manages dirty page flushing to reduce disk I/O.
6. **Transaction & Recovery Subsystem**: Coordinates ACID guarantees via Write-Ahead Logging (WAL/Redo log), Undo logging for MVCC, lock managers, and crash recovery using ARIES algorithms.

---

## What is a database storage engine, and how do B+Tree and Log-Structured Merge-tree (LSM) architectures differ?

A storage engine manages how data pages are physically organized, written, read, and indexed on disk. The two primary paradigms are B+Trees and LSM-trees:

- **B+Tree Storage Engines** (e.g., InnoDB in MySQL, WiredTiger in MongoDB):
  - *Data Layout*: Balanced $N$-ary search tree of fixed-size pages (typically 4KB–16KB). Leaf nodes contain records linked in a doubly-linked list for sequential range scans.
  - *I/O Pattern*: **Update-in-place** model. Updates modify existing disk pages directly, protected by a Write-Ahead Log.
  - *Characteristics*: Fast $O(\log N)$ point lookups, efficient range scans, and low read amplification, but causes random disk I/O and write amplification from page splits.
- **LSM-Tree Storage Engines** (e.g., RocksDB, LevelDB, Cassandra):
  - *Data Layout*: Incoming writes append sequentially to an in-memory sorted **MemTable** (backed by an on-disk WAL). When full, the MemTable flushes to disk as an immutable **SSTable** (Sorted String Table). Background compactions continuously merge overlapping SSTables across hierarchical levels.
  - *I/O Pattern*: **Append-only** model. Eliminates random disk writes by turning all updates and deletes (tombstones) into sequential writes.
  - *Characteristics*: High write throughput and low write latency, but introduces read amplification (searching MemTables, SSTables, Bloom filters) and compaction I/O overhead.

### Trade-off Summary
- **B+Trees**: Best for read-heavy OLTP workloads requiring low read amplification and predictable query latency.
- **LSM-Trees**: Best for write-heavy workloads (time-series, event ingestion, key-value stores) where sequential write speed and storage space efficiency matter most.

---

## What are ACID guarantees, and how do database management systems implement and enforce them?

ACID defines the foundational guarantees of a database transaction:

- **Atomicity (All-or-Nothing)**: The transaction executes completely or rolls back entirely.
  - *Implementation*: Managed via Undo logs and Write-Ahead Logging (WAL). If a transaction fails mid-flight, undo logs replay previous record states to revert all modifications.
- **Consistency (Invariant Preservation)**: The database transitions from one valid state to another, satisfying schema constraints (foreign keys, unique constraints, check predicates).
  - *Implementation*: Enforced by engine-level validation alongside atomicity and isolation rollbacks.
- **Isolation (Concurrent Non-Interference)**: Concurrent transactions execute without mutual interference.
  - *Implementation*: Managed via Multi-Version Concurrency Control (MVCC) and Two-Phase Locking (2PL). MVCC uses tuple version timestamps (`xmin`/`xmax`) so read transactions see a consistent snapshot without blocking write locks, preventing dirty reads and non-repeatable reads.
- **Durability (Survival Across Crashes)**: Committed changes survive power loss or system crashes.
  - *Implementation*: Enforced by flushing WAL records to disk with `fsync` before acknowledging commits. On startup after a crash, ARIES recovery replays the redo log.

---

## What is the BASE consistency model, and how does it compare to ACID in distributed databases?

BASE is a relaxed consistency model designed for distributed databases that prioritize high availability and horizontal scaling over immediate consistency (under CAP constraints):

- **Basically Available (BA)**: The system guarantees availability by routing requests to any responsive node, accepting local writes even during network splits rather than failing.
- **Soft State (S)**: Data values can drift or change over time without explicit user interaction because background replication continuously synchronizes replicas.
- **Eventual Consistency (E)**: If no new updates are made, all replicas will eventually converge. Enforced via asynchronous replication, Vector Clocks / Hybrid Logical Clocks (HLC), Conflict-Free Replicated Data Types (CRDTs), and anti-entropy repair (Read Repair, Merkle tree sync in Cassandra/Dynamo).

### ACID vs. BASE Trade-offs
- **ACID**: Enforces immediate linearizable consistency and data correctness (e.g., Spanner, CockroachDB, PostgreSQL), but incurs consensus latency and risks unavailability on minority partitions during network splits.
- **BASE**: Delivers high throughput and partition tolerance with local write latencies (e.g., Cassandra, DynamoDB), but shifts stale read and conflict handling to the application.
