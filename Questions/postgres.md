# PostgreSQL & RDBMS — Question Bank (Mid → Senior → Staff Backend)

---

## 1. Relational Fundamentals

1. What is the relational model, and what are relations, tuples, and attributes?
2. What is the difference between a primary key, a candidate key, a surrogate key, and a natural key?
3. When would you choose a natural key over a surrogate key, and what goes wrong with each?
4. What are foreign keys, and what do `ON DELETE CASCADE`, `SET NULL`, `RESTRICT`, and `NO ACTION` do?
5. What is the difference between `RESTRICT` and `NO ACTION` when the constraint is deferrable?
6. How does `NULL` behave in comparisons, `UNIQUE` constraints, aggregates, and `NOT IN`?
7. Why can `NOT IN (subquery)` return no rows when the subquery contains a `NULL`?
8. What are the differences between `CHAR`, `VARCHAR(n)`, and `TEXT` in Postgres, and is there a performance difference?
9. When should you use `NUMERIC` instead of `FLOAT`/`DOUBLE PRECISION`?
10. Why is `TIMESTAMPTZ` almost always preferred over `TIMESTAMP`, and what does it actually store?
11. UUID vs `BIGSERIAL` vs `IDENTITY` primary keys: what are the trade-offs, including index locality and UUIDv7?
12. What is the difference between `SERIAL` and `GENERATED ... AS IDENTITY`?

## 2. Data Modeling & Normalization

13. Explain 1NF, 2NF, 3NF, and BCNF with an example of a violation for each.
14. When is denormalization justified, and how do you keep denormalized data consistent?
15. How do you model one-to-many, many-to-many, and polymorphic associations?
16. What are the trade-offs of an Entity-Attribute-Value (EAV) design versus `JSONB` columns?
17. How would you model soft deletes, and how do they interact with unique constraints?
18. How would you model a multi-tenant schema: shared tables with `tenant_id`, schema-per-tenant, or database-per-tenant?
19. How would you model hierarchical data (adjacency list, nested sets, materialized path, `ltree`, closure table)?
20. How would you model time-varying data and keep a full audit history (temporal tables, history triggers)?
21. How do you enforce "no overlapping bookings for the same room" in the database?
22. What are `CHECK` constraints and domains, and when should business rules live in the database?

## 3. SQL — Querying

23. What is the logical order of evaluation of a `SELECT` (FROM, WHERE, GROUP BY, HAVING, SELECT, ORDER BY, LIMIT)?
24. Explain the difference between `INNER`, `LEFT`, `RIGHT`, `FULL OUTER`, `CROSS`, and `LATERAL` joins.
25. What is the difference between `WHERE` and `HAVING`?
26. What is the difference between `EXISTS`, `IN`, and a `JOIN` for semi-joins, and which does the planner prefer?
27. How do you write an anti-join correctly, and why is `NOT EXISTS` usually safer than `NOT IN`?
28. What are window functions, and how do `ROW_NUMBER`, `RANK`, `DENSE_RANK`, `LAG`, and `LEAD` differ?
29. What is the difference between `ROWS` and `RANGE` frames in a window function?
30. How do you get the top N rows per group efficiently?
31. What are CTEs, and when are they materialized versus inlined (Postgres 12+ `MATERIALIZED` / `NOT MATERIALIZED`)?
32. How do recursive CTEs work, and how do you prevent infinite loops?
33. What does `DISTINCT ON` do, and how does it differ from `GROUP BY`?
34. What does `UPSERT` (`INSERT ... ON CONFLICT`) do, and what are its race-condition guarantees?
35. What is `MERGE` (Postgres 15+), and how does it compare with `ON CONFLICT`?
36. What does `RETURNING` do, and why is it useful in write-heavy code?
37. How do `GROUPING SETS`, `ROLLUP`, and `CUBE` work?
38. Why is `OFFSET` pagination slow on large tables, and how does keyset (seek) pagination fix it?
39. How do you implement stable keyset pagination when the sort column is not unique?
40. What is the difference between `UNION` and `UNION ALL`, and why does it matter for performance?

## 4. Indexing

41. How does a B-tree index work, and why is it the default?
42. When does Postgres use an index scan, an index-only scan, a bitmap index scan, or a sequential scan?
43. What is a covering index, and what does the `INCLUDE` clause do?
44. Why does column order matter in a composite index? Explain the leftmost-prefix rule.
45. What is a partial index, and give a real use case.
46. What is an expression (functional) index, and why doesn't `WHERE lower(email) = ...` use a plain index on `email`?
47. When would you use Hash, GIN, GiST, SP-GiST, and BRIN indexes?
48. How do GIN indexes work for `JSONB`, arrays, and full-text search, and what is `fastupdate`?
49. When is BRIN the right choice, and why does it depend on physical correlation?
50. What is index selectivity and cardinality, and when does an index stop helping?
51. Why might Postgres ignore an index you created?
52. How do `LIKE 'abc%'`, `LIKE '%abc'`, and collations affect index use? What is `text_pattern_ops` and `pg_trgm`?
53. What is the cost of having too many indexes on a write-heavy table?
54. What is index bloat, and how do you detect and fix it?
55. How does `CREATE INDEX CONCURRENTLY` work, what can make it fail, and what does an invalid index mean?
56. What are HOT (Heap-Only Tuple) updates, and how do indexes and `fillfactor` affect them?
57. Why does a visibility map matter for index-only scans?
58. How do you find unused or duplicate indexes in a production database?

## 5. Query Planning & Optimization

59. How does the Postgres planner choose a plan (cost model, statistics, `seq_page_cost`, `random_page_cost`)?
60. How do you read `EXPLAIN (ANALYZE, BUFFERS)` output, and what do actual vs estimated rows tell you?
61. What are nested loop, hash join, and merge join, and when is each chosen?
62. What causes bad row estimates, and how do extended statistics (`CREATE STATISTICS`) help?
63. What do `ANALYZE`, `default_statistics_target`, and `pg_stats` do?
64. Why does a query get slower when a parameter value is very common (skew)? What are generic vs custom plans?
65. What is the N+1 query problem, and how do you detect it from the database side?
66. How do you tune `work_mem`, and what happens when a sort or hash spills to disk?
67. What does `JIT` compilation do in Postgres, and when can it hurt?
68. How does parallel query work, and what prevents a query from running in parallel?
69. How do you use `pg_stat_statements` to find the most expensive queries?
70. How would you approach a production query that suddenly went from 50 ms to 10 s?
71. Why can `SELECT COUNT(*)` be slow on a large table, and what are the alternatives?
72. How do `ORDER BY ... LIMIT` queries interact with indexes, and when do they go badly wrong?

## 6. Transactions & ACID

73. Explain each ACID property and how Postgres implements it.
74. What are dirty reads, non-repeatable reads, phantom reads, lost updates, and write skew?
75. What are the four SQL isolation levels, and which ones does Postgres actually implement?
76. Why does Postgres `READ UNCOMMITTED` behave like `READ COMMITTED`?
77. How does `REPEATABLE READ` in Postgres differ from the SQL standard (snapshot isolation)?
78. How does `SERIALIZABLE` work in Postgres (SSI), and what are serialization failures?
79. Give an example of write skew that snapshot isolation allows but `SERIALIZABLE` prevents.
80. How must the application handle `40001` serialization failures and `40P01` deadlocks?
81. How do you prevent lost updates: `SELECT ... FOR UPDATE`, optimistic locking with a version column, or atomic updates?
82. What are savepoints, and how are they used for partial rollback?
83. What happens to a long-running transaction's snapshot, and why are long transactions dangerous?
84. What does `idle in transaction` mean, and what does `idle_in_transaction_session_timeout` protect against?
85. What is a deferrable constraint, and when do you need `DEFERRABLE INITIALLY DEFERRED`?

## 7. MVCC & Internals

86. How does MVCC work in Postgres, and what are `xmin` and `xmax`?
87. Why does an `UPDATE` in Postgres create a new tuple version, and what are the consequences?
88. How does Postgres decide whether a tuple is visible to a transaction (snapshots, commit log / `pg_xact`)?
89. What are dead tuples, and how do they cause table bloat?
90. How does Postgres's MVCC compare with MySQL InnoDB's undo-log approach?
91. What is transaction ID wraparound, and why can it force a database shutdown?
92. What is freezing, and what do `vacuum_freeze_min_age` and `autovacuum_freeze_max_age` control?
93. What are the heap, pages (8 KB), tuples, line pointers, and TOAST?
94. How does TOAST store large values, and what compression options exist?
95. What is the shared buffer pool, and how does it interact with the OS page cache (double buffering)?
96. Describe Postgres's process model (postmaster, backends, background writer, checkpointer, WAL writer, autovacuum launcher).
97. Why does Postgres use one process per connection, and what are the consequences?

## 8. VACUUM & Maintenance

98. What does `VACUUM` do, and how does it differ from `VACUUM FULL`?
99. How does autovacuum decide when to run (`autovacuum_vacuum_scale_factor`, thresholds)?
100. Why do autovacuum defaults fail for very large tables, and how do you tune per-table settings?
101. What blocks vacuum from cleaning dead tuples (long transactions, replication slots, prepared transactions, `hot_standby_feedback`)?
102. How do you remove bloat without long locks (`pg_repack`, `pg_squeeze`)?
103. What does `REINDEX CONCURRENTLY` do and when would you need it?
104. How do you monitor vacuum progress and table bloat?

## 9. Locking & Concurrency

105. What table-level lock modes exist, and which ones conflict (e.g. `ACCESS EXCLUSIVE` vs `ROW EXCLUSIVE`)?
106. What row-level locks exist (`FOR UPDATE`, `FOR NO KEY UPDATE`, `FOR SHARE`, `FOR KEY SHARE`)?
107. Why does `FOR NO KEY UPDATE` matter for foreign key contention?
108. How does Postgres detect deadlocks, and how do you design code to avoid them?
109. How does `SELECT ... FOR UPDATE SKIP LOCKED` let you build a job queue in Postgres?
110. What are advisory locks, and when would you use session vs transaction advisory locks?
111. Why can a simple `ALTER TABLE` take down production, and what is the lock queue problem?
112. What does `lock_timeout` do, and why should every migration set it?
113. How do you inspect who is blocking whom (`pg_locks`, `pg_blocking_pids`)?
114. How do you implement a distributed lock or leader election with Postgres, and what are the limits?

## 10. WAL, Durability & Recovery

115. What is the Write-Ahead Log, and why does it guarantee durability?
116. What happens during a checkpoint, and how do `checkpoint_timeout` and `max_wal_size` affect performance?
117. What are full-page writes, and why are they needed?
118. What does `synchronous_commit = off` trade away, and when is it acceptable?
119. How does crash recovery work after a power failure?
120. What is the difference between logical and physical backups (`pg_dump` vs `pg_basebackup`)?
121. How does point-in-time recovery (PITR) work with base backups and WAL archiving?
122. How would you design a backup strategy with defined RPO and RTO, and how do you verify backups?

## 11. Replication & High Availability

123. How does streaming replication work, and what is the difference between async and synchronous replication?
124. What do `synchronous_standby_names` and quorum-based synchronous commit (`ANY 1 (...)`) do?
125. What is replication lag, how do you measure it, and how does it cause read-your-writes bugs?
126. How do you route reads to replicas safely in an application?
127. What are replication slots, and how can an abandoned slot fill the disk?
128. What causes query cancellations on hot standbys, and what do `hot_standby_feedback` and `max_standby_streaming_delay` trade off?
129. How does logical replication differ from physical replication, and what are its limitations (DDL, sequences)?
130. What is Change Data Capture (CDC), and how do tools like Debezium use logical decoding?
131. How does automated failover work with Patroni (or similar), and what role does a DCS (etcd/Consul) play?
132. What is split-brain in a database cluster, and how is fencing done?
133. What happens to in-flight transactions during a failover, and what must the application do?
134. How would you do a major version upgrade with minimal downtime (`pg_upgrade`, logical replication)?

## 12. Scaling & Partitioning

135. How does declarative partitioning work (range, list, hash), and when does it help or hurt?
136. What is partition pruning, and when does it happen at plan time vs execution time?
137. How do you manage indexes, unique constraints, and foreign keys across partitions?
138. How would you retire old data efficiently using partitions (detach/drop vs `DELETE`)?
139. Vertical scaling vs read replicas vs sharding: when do you reach for each?
140. How would you shard a Postgres database, and what do Citus and application-level sharding offer?
141. How do you choose a shard key, and what makes cross-shard queries and transactions hard?
142. How would you migrate a 2 TB table to a new schema with zero downtime?

## 13. Connections & Application Integration

143. Why are Postgres connections expensive, and how many is too many?
144. How does PgBouncer work, and what are session, transaction, and statement pooling modes?
145. What breaks in transaction pooling mode (session-level `SET`, prepared statements, advisory locks, `LISTEN`)?
146. How do you size an application-side connection pool?
147. What are prepared statements, and how do they protect against SQL injection?
148. What are the trade-offs of ORMs, and which performance pitfalls do they cause most often?
149. How do you handle retries safely for transient errors (idempotency keys, retryable SQLSTATEs)?
150. How do you implement the transactional outbox pattern with Postgres?
151. What are `LISTEN/NOTIFY` and its limitations for event delivery?
152. How do you set `statement_timeout` per role or per request, and why should you?

## 14. Schema Migrations & Operations

153. Which DDL operations are safe on large live tables, and which rewrite the table?
154. How do you add a `NOT NULL` column with a default to a huge table safely (and what changed in Postgres 11)?
155. How do you add a foreign key or `CHECK` constraint without blocking writes (`NOT VALID` + `VALIDATE CONSTRAINT`)?
156. How do you rename a column or change a column type with zero downtime (expand–contract)?
157. How do you safely backfill millions of rows without overloading the database?
158. How do you roll back a bad migration in production?

## 15. JSONB & Advanced Features

159. What is the difference between `JSON` and `JSONB`?
160. Which operators (`->`, `->>`, `@>`, `?`, `jsonb_path_query`) does `JSONB` support, and which can use a GIN index?
161. What is the difference between `jsonb_ops` and `jsonb_path_ops` GIN operator classes?
162. When does storing data in `JSONB` become an anti-pattern?
163. How does full-text search work (`tsvector`, `tsquery`, ranking), and when do you need Elasticsearch instead?
164. What are generated columns, and when are they useful?
165. What are materialized views, and how does `REFRESH MATERIALIZED VIEW CONCURRENTLY` work?
166. What are triggers, and what are their performance and maintainability costs?
167. What is the difference between a function and a procedure, and when should logic live in PL/pgSQL?
168. What are exclusion constraints, and how do range types work with them?
169. Which extensions are commonly used in production (`pg_stat_statements`, `pg_trgm`, `postgis`, `pgvector`, `pg_partman`)?
170. How does `pgvector` work, and what are IVFFlat vs HNSW indexes?

## 16. Security

171. How do roles, users, and groups work in Postgres, and what is the principle of least privilege here?
172. What is Row-Level Security (RLS), and how would you use it for multi-tenant isolation?
173. What are the pitfalls of RLS (performance, `BYPASSRLS`, security-definer functions, leaky views)?
174. How does `pg_hba.conf` control authentication, and why prefer `scram-sha-256` over `md5`?
175. How do you encrypt data at rest and in transit, and what is `pgcrypto` for?
176. How do you manage and rotate database credentials securely?
177. How can `SECURITY DEFINER` functions become a privilege-escalation vector?

## 17. Monitoring & Troubleshooting

178. Which system views matter most (`pg_stat_activity`, `pg_stat_user_tables`, `pg_stat_user_indexes`, `pg_statio_*`)?
179. What is the cache hit ratio, and when is it a misleading metric?
180. Which metrics would you alert on for a production Postgres cluster?
181. The database CPU is at 100% — how do you diagnose it step by step?
182. Disk usage is growing fast — what are the likely causes and how do you confirm each?
183. Connections are exhausted — how do you find the cause and recover?
184. How do you safely kill a runaway query (`pg_cancel_backend` vs `pg_terminate_backend`)?
185. How do you use `auto_explain` and `log_min_duration_statement` to catch slow queries?
186. What key `postgresql.conf` settings would you tune on a new server (`shared_buffers`, `effective_cache_size`, `work_mem`, `maintenance_work_mem`, `max_connections`)?

## 18. Storage Engines & Database Theory

187. How do B-tree-based storage engines compare with LSM-tree engines (RocksDB) for reads, writes, and amplification?
188. What is the difference between row-oriented and column-oriented storage, and when do you use each?
189. OLTP vs OLAP: why shouldn't you run heavy analytics on your primary database?
190. How does two-phase commit work, and what are `PREPARE TRANSACTION`'s risks in Postgres?
191. How do distributed SQL databases (CockroachDB, Spanner, Yugabyte) provide serializable transactions across nodes?
192. What does the CAP theorem mean for a primary/replica Postgres setup?
193. Postgres vs MySQL: what are the key differences in MVCC, replication, indexing, and features?
194. When would you choose a NoSQL store (DynamoDB, Cassandra, MongoDB) over Postgres, and when is that a mistake?

## 19. System Design with Postgres (Senior/Staff Scenarios)

195. Design a reliable job queue on Postgres. How does it behave at 10k jobs per second?
196. Design an idempotent payments ledger with double-entry bookkeeping and strict consistency.
197. Design an inventory system that prevents overselling under heavy concurrent checkout.
198. Design a rate limiter or counter that handles hot rows without lock contention.
199. Design a multi-tenant SaaS database that supports 10,000 tenants with a few very large ones.
200. Design an audit log that is append-only, tamper-evident, and queryable.
201. Design a time-series store for metrics on Postgres. When would you move to TimescaleDB or ClickHouse?
202. Design a migration path from a single primary to a sharded setup without downtime.
203. How would you support global users with low-latency reads while keeping writes consistent?
204. How would you design cache invalidation between Postgres and Redis to avoid stale reads?
205. Your largest table has 5 billion rows and queries are degrading — what is your plan?
206. How would you set and defend database standards (migrations, indexing, review, SLOs) for an engineering org?
