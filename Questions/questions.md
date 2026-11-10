# Castor — Question Bank

Questions drawn from Castor and the topics around it: distributed storage, consensus, microservices, databases, networking, and operations.

---

## 1. Project Overview & Motivation

1. What problem does Castor solve, and who would use it instead of AWS S3, MinIO, or Ceph?
2. Why split the system into gateway, metadata, data, and auth tiers instead of building one binary?
3. What does "content-addressed, distributed, S3-compatible object storage" mean, word by word?
4. What are Castor's core architectural invariants, and why does each one matter?
5. Why are object versioning, erasure coding, IAM policies, and SSE listed as non-goals?
6. Walk through what happens end to end when a user runs `aws s3 cp file.mp4 s3://photos/`.
7. Which component is the single source of truth for storage state, and which for identity?
8. How is object storage different from block storage and file storage?
9. What is the role of the control plane versus the data plane in Castor?

## 2. Microservices Architecture

10. What are the responsibilities and boundaries of each service (`gateway-svc`, `metadata-svc`, `data-svc`, `auth-svc`)?
11. Why is the gateway stateless, and what does that make possible?
12. Why is `data-svc` described as agnostic to users, buckets, and auth?
13. How do services discover each other in Docker Compose and in Kubernetes?
14. What is a Backend-for-Frontend (BFF), and why does the gateway expose a separate BFF on `:9001`?
15. Why is the UI (`castor-ui`) deployed separately from the gateway binary?
16. How does Castor avoid distributed two-phase commit between the Auth DB and the Raft metadata store?
17. What are the trade-offs of using an in-process upload bridge instead of a network hop to the S3 port?
18. What failure modes appear when one microservice depends on another synchronously?
19. Where would you add circuit breakers, retries, or bulkheads in Castor?
20. What is the difference between orchestration and choreography, and which pattern do Castor's background workers follow?

## 3. S3 Protocol & Gateway

21. What is VersityGW, and why embed it rather than implement the S3 protocol from scratch?
22. How does AWS Signature Version 4 work, step by step (canonical request, string to sign, signing key)?
23. Why does SigV4 need the server to know the secret key, and how does the gateway get it?
24. What is the difference between path-style and virtual-hosted-style S3 addressing?
25. How do presigned URLs work, and why do they need no database storage?
26. What stops a presigned URL from being reused after it expires or tampered with?
27. How does `ListObjectsV2` implement `prefix`, `delimiter`, and folder simulation over a flat keyspace?
28. How do continuation tokens work, and what is `PrefixNext` upper-bound iteration?
29. How are HTTP `Range` requests mapped onto 4 MiB chunk boundaries?
30. When does the gateway return `200 OK` versus `206 Partial Content`?
31. What is an ETag in S3, and how is Castor's ETag computed for single-part and multipart objects?
32. What S3 error codes (`NoSuchKey`, `NoSuchBucket`, `BucketNotEmpty`) map to which HTTP statuses?
33. Why must S3 deletes be idempotent, and how does Castor guarantee that?
34. What does Last-Write-Wins mean for concurrent overwrites of the same key?
35. How does the gateway find the current Raft leader and reroute after a leader change?
36. How would you scale gateways horizontally behind an L4 or L7 load balancer?
37. How does the gateway apply backpressure when many large uploads arrive at once?

## 4. Consensus & Raft

38. What problem does a consensus algorithm solve?
39. Explain Raft's three roles (leader, follower, candidate) and how a node moves between them.
40. How does leader election work, including terms, votes, and randomized election timeouts?
41. How does log replication work, and when is an entry considered committed?
42. What is the Log Matching Property, and why does it matter for safety?
43. What is the Leader Completeness Property?
44. Why can a 3-node Raft cluster tolerate only one failure? How many failures can 5 nodes tolerate?
45. What happens during a network partition that isolates the leader from both followers?
46. What is split-brain, and how does Raft prevent two leaders from committing in the same term?
47. Why must writes go through the leader, and what are the options for serving reads?
48. What is the difference between leader-routed reads, ReadIndex reads, and lease-based reads?
49. How can a stale leader serve stale reads, and how is that prevented?
50. What is a Raft snapshot, and why is log compaction needed?
51. How does `hashicorp/raft` separate the log store, stable store, snapshot store, and FSM?
52. Why does Castor store the Raft log in BoltDB and the FSM state in BadgerDB?
53. What must an FSM `Apply` function guarantee (determinism, no side effects)?
54. Why must time-dependent values like `orphaned_at = now` be set by the proposer rather than inside `Apply`?
55. What happens if the leader crashes after committing an entry but before replying to the client?
56. How do you make `CommitManifest` safe to retry when the client doesn't know whether the first attempt committed?
57. Why does Castor keep a fixed 3-node membership, and what would dynamic membership change require?
58. What is joint consensus, and how does Raft handle membership changes?
59. How does Raft compare with Paxos, Multi-Paxos, and Zab?
60. How do heartbeat interval and election timeout trade off failover speed against false elections?
61. Why do background workers (GC, healing, scrubbing) run only on the leader?
62. What happens to an in-progress GC sweep when leadership moves to another node?
63. Why is the data-node heartbeat registry kept in memory and outside the Raft log?
64. What are the consequences when a new leader starts with an empty heartbeat registry?
65. What is a "linearizable write", and how does Raft provide one?

## 5. Metadata Store, BadgerDB & LSM Trees

66. What is an LSM tree, and how do memtables, SSTables, and compaction work?
67. How does an LSM tree compare with a B+Tree (BoltDB, Postgres) for read, write, and space amplification?
68. How does BadgerDB's key-value separation (WiscKey design) reduce write amplification?
69. What is the value log in BadgerDB, and why does it need its own garbage collection?
70. Why use prefixed keys like `bucket:`, `manifest:<bucket>:<key>`, and `chunk:<sha256>`?
71. How does key design enable efficient prefix scans for listing objects?
72. Why are multipart part numbers zero-padded to five digits?
73. What does `db.Update` guarantee, and what isolation level does BadgerDB provide?
74. What is MVCC, and how does BadgerDB use it for snapshot reads?
75. Why are metadata records encoded with protobuf instead of JSON?
76. What does each field of `ManifestRecord` and `ChunkLocationRecord` represent?
77. How does `CommitManifest` keep reference counts correct on overwrite?
78. What could go wrong if the reference-count update and the manifest write were not atomic?
79. How would the metadata store behave with billions of keys, and where would it break first?
80. How would you shard the metadata tier if one Raft group were no longer enough?

## 6. Storage Model: Content Addressing, Chunking & Deduplication

81. What is content-addressable storage, and what properties does it give you?
82. Why use SHA-256 for chunk identity, and how likely is a collision?
83. Why use a fixed 4 MiB chunk size? What changes with 1 MiB or 64 MiB chunks?
84. How does fixed-size chunking compare with content-defined chunking (Rabin fingerprinting, FastCDC)?
85. Why does inserting one byte at the start of a file break deduplication with fixed-size chunks?
86. How does inline deduplication work in the write path?
87. What is the difference between inline and post-process deduplication?
88. How is deduplication scoped: per user, per bucket, or cluster-wide? What are the privacy implications?
89. What is a deduplication side-channel attack, and does Castor's design expose one?
90. Why do raw chunk bytes never touch the Raft log or BadgerDB?
91. How are small objects (under 4 MiB) stored, and what overhead does that create?
92. Why shard chunk files into directories by a 2-character hex prefix?
93. How does reference counting work for chunks shared across many objects?

## 7. Durability & Crash Consistency

94. Why write to a staging file and then rename, instead of writing in place?
95. Why is `os.Rename` atomic on POSIX filesystems, and when is it not (across filesystems, on Windows)?
96. Why is `file.Sync()` needed before the rename?
97. Why is `dir.Sync()` on the parent directory needed after the rename?
98. What is a torn write, and how does the staging-and-rename pattern prevent it?
99. What does the OS page cache do, and how can it lose data on power failure?
100. Why does `data-svc` sweep `/data/staging/*.tmp` on startup?
101. What durability guarantee does a client get when it receives `200 OK` for a PUT?
102. What is bit rot, and how does a scrubber detect it?
103. Why does `ScrubChunk` verify the hash locally instead of streaming bytes to the leader?
104. Why persist a `scrub_cursor`, and what happens without one after a leader failover?

## 8. Replication, Quorums & Consistency Models

105. What do W=2 and R=3 mean in Castor, and how does this differ from Dynamo-style N/R/W quorums?
106. Why is W=2 out of 3 enough for durability, and what risk remains until the third replica is healed?
107. Why does Castor need strongly consistent metadata if chunks are immutable?
108. How does immutability of chunks simplify replication and caching?
109. What is the difference between linearizability, sequential consistency, and eventual consistency?
110. What consistency does a reader get immediately after a successful PUT?
111. How does capacity-weighted placement choose nodes, and how does it avoid hotspots?
112. How would consistent hashing or rendezvous hashing compare with Castor's placement strategy?
113. What happens if the gateway's cached node list is stale and includes a dead node?
114. What is read repair, and how is it triggered in Castor?
115. How does the active replica healer find under-replicated chunks and restore R=3?
116. Why copy chunks peer-to-peer between data nodes instead of through the gateway?
117. How would you add rack- or zone-awareness to placement?
118. When does erasure coding (e.g. Reed-Solomon 6+3) beat 3x replication, and why not at 3 nodes?

## 9. Failure Handling, Garbage Collection & Self-Healing

119. What happens when one storage node fails during a chunk write?
120. What happens when two storage nodes fail during a write, and what does the client see?
121. What happens when the client disconnects halfway through a 500 MB upload?
122. What happens to chunks written to disk when `CommitManifest` is never called?
123. Why is there a 24-hour quarantine before deleting orphaned chunks?
124. Describe the race between a deduplicating upload and GC, and how the quarantine prevents it.
125. Is the 24-hour window a correctness guarantee or a probabilistic one? How could you make it strict?
126. Why rate-limit GC and scrubbing with a token bucket?
127. How are data nodes marked DEGRADED or OFFLINE from heartbeats, and why after 15 seconds?
128. What happens when all replicas of a chunk are lost?
129. How would you run chaos tests (killing pods, partitioning networks, filling disks) against Castor?

## 10. Multipart Uploads

130. How does S3 multipart upload work (initiate, upload part, complete, abort)?
131. How are parts stored in metadata, and how are they assembled into one manifest on completion?
132. How is a multipart ETag computed, and why does it differ from a plain MD5 or SHA-256?
133. How are abandoned multipart uploads cleaned up?
134. Why can parts be uploaded in parallel and out of order?

## 11. Networking & Protocols

135. Why use gRPC for internal traffic and HTTP/REST for external traffic?
136. How does gRPC use HTTP/2 (multiplexing, streams, flow control, HPACK)?
137. What are unary, server-streaming, client-streaming, and bidirectional gRPC calls, and which does `PutChunk` use?
138. Why send a metadata frame first in `PutChunk` and then stream payload frames?
139. What are gRPC's default message size limits, and why stream 4 MiB chunks instead of sending one message?
140. How do gRPC deadlines and cancellation propagate across services?
141. How are gRPC status codes (`NOT_FOUND`, `INVALID_ARGUMENT`, `UNAVAILABLE`) mapped to HTTP errors?
142. What is protobuf's wire format, and how does it stay backward and forward compatible?
143. What is connection pooling, and how does the gateway reuse gRPC connections to data nodes?
144. Why use mTLS between internal services, and how would you add it?

## 12. Authentication, Authorization & Security

145. How does `auth-svc` store passwords, and why bcrypt over SHA-256?
146. How are S3 access key IDs and secrets generated, and what makes them cryptographically random?
147. Should S3 secret keys be stored in plaintext? What are the alternatives, given SigV4 needs the secret?
148. How do JWTs work (header, payload, signature), and what are their pitfalls?
149. Why store the JWT in a cookie, and which cookie flags (`HttpOnly`, `Secure`, `SameSite`) matter?
150. Why cache credentials in the gateway for 30 seconds, and what is the security cost when a key is revoked?
151. How is bucket ownership (`owner_id`) enforced on every request?
152. What is the difference between authentication and authorization in Castor?
153. Why are the default `admin/admin123` credentials dangerous in production, and how should bootstrap credentials be handled?

## 13. Relational Database (Auth DB)

154. Why use PostgreSQL or SQLite for identity rather than the Raft store?
155. What do ACID guarantees mean, and which matter most for credential management?
156. Why index `s3_credentials.user_id`, and what does `ON DELETE CASCADE` do?
157. How does connection pooling protect PostgreSQL under bursty S3 traffic?

## 14. Go Concurrency & Performance

158. What is `sync.Pool`, and how does it reduce GC pressure for 4 MiB buffers?
159. How does a concurrency semaphore bound memory usage on the gateway?
160. How would you fan out three parallel writes and return as soon as two succeed?
161. How do you cancel the slow third write without leaking goroutines?
162. How does `context.Context` propagate cancellation from a dropped HTTP client to gRPC streams?
163. How would you profile throughput bottlenecks in the gateway (pprof, tracing)?

## 15. Observability

164. What are the three pillars of observability, and how does Castor implement each?
165. How does OpenTelemetry trace context propagate across HTTP and gRPC boundaries?
166. Which Prometheus metrics would you use to alert on Raft health, quorum failures, and disk capacity?
167. What role does the OpenTelemetry Collector play between the services and Grafana Cloud?
168. How does structured logging with trace IDs help debug a failed upload across four services?

## 16. Deployment, Containers & Operations

169. How is the 7-node local cluster wired together in Docker Compose?
170. How would you deploy the metadata cluster on Kubernetes (StatefulSet, headless Service, PVCs)?
171. Why is a StatefulSet needed for Raft nodes but a Deployment fine for gateways?
172. What liveness and readiness probes should each service expose, and how do they differ?
173. How would you do a rolling upgrade of the Raft cluster without losing quorum?
174. How would you back up and restore the metadata store and chunk data consistently?

## 17. Testing & Quality

175. How do unit tests, integration tests, and end-to-end tests divide responsibilities in Castor?
176. How would you test Raft behavior under partitions deterministically?
177. What is Jepsen-style testing, and what would it check for Castor?
178. How do you test crash consistency of the staging-and-rename write path?
179. How would you test S3 compatibility against real SDKs (boto3, AWS SDK Go v2, rclone)?

## 18. Scalability & Capacity Planning

180. What is the theoretical maximum throughput of one gateway, and what limits it (CPU for SHA-256, NIC, memory)?
181. How would metadata write throughput limit the system with many small objects?
182. How does storage overhead change with 3x replication versus erasure coding at 1 PB?
183. How would you rebalance chunks when new data nodes join?
184. How would you support multiple regions or data centers?

## 19. Distributed Systems Theory

185. What does the CAP theorem say, and where does Castor sit for metadata versus data?
186. What is the FLP impossibility result, and how do practical systems like Raft work around it?
187. What are the Byzantine Generals problem and Byzantine fault tolerance, and why doesn't Castor need BFT?
188. Why can't you rely on wall-clock time in distributed systems? What are logical clocks and hybrid logical clocks?
189. What are the fallacies of distributed computing, and which ones does Castor's design account for?
190. What is idempotency, and why does it matter for retries across the network?

## 20. Design Trade-offs & What-Ifs

191. What would change if Castor supported object versioning?
192. What if a data node returns success but silently didn't fsync?
193. What if two gateways commit manifests for the same key at the same moment?
194. What if the metadata leader is reachable by the gateway but partitioned from its followers?
195. What if the disk on a data node fills up mid-write?
196. How would you add server-side encryption, and how does encryption interact with deduplication?
197. How would you add per-user storage quotas without breaking deduplication accounting?
198. If you rebuilt Castor today, what would you change first, and why?
199. How does Castor compare with MinIO, Ceph (RADOS), SeaweedFS, and GFS/HDFS?
200. Which single component is the riskiest to run in production, and how would you reduce that risk?
