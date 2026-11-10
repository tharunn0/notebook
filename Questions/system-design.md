# System Design — Question Bank (Fundamentals → Senior → Staff Backend)

Part A covers fundamentals and core concepts. Part B covers advanced distributed systems topics. Part C is classic design problems. Part D is senior+ and staff-level design, trade-offs, and leadership.

---

# Part A — Fundamentals & Core Concepts

## 1. Approach & Requirements

1. How do you structure a system design interview from requirements to deep dive?
2. What is the difference between functional and non-functional requirements? Give examples of each.
3. Which clarifying questions should you ask before designing anything?
4. How do you estimate QPS, storage, bandwidth, and memory from a user count (back-of-the-envelope)?
5. What latency numbers should every engineer know (L1 cache, RAM, SSD, network round trip within and across regions)?
6. How do you decide which part of the design to deep-dive into?
7. What is the difference between availability, reliability, durability, and resilience?
8. What do 99.9%, 99.99%, and 99.999% availability mean in downtime per year, and what does each cost to achieve?
9. What are SLIs, SLOs, SLAs, and error budgets?
10. How do serial and parallel component dependencies affect overall availability?

## 2. Scalability Basics

11. What is the difference between vertical and horizontal scaling, and when does vertical scaling stop making sense?
12. What does "stateless service" mean, and why does it simplify scaling?
13. Where should session state live in a horizontally scaled system?
14. What is the difference between latency and throughput, and how can improving one hurt the other?
15. What is Little's Law, and how do you use it to size thread pools or connection pools?
16. Why do p99 and p999 latencies matter more than averages?
17. What is tail-latency amplification in fan-out requests, and how do hedged requests help?
18. What is the difference between scaling reads and scaling writes?

## 3. Networking & Protocols

19. What happens, step by step, when you type a URL into a browser?
20. How does DNS resolution work, and how is DNS used for load balancing and failover?
21. What is the difference between TCP and UDP, and when would you choose UDP?
22. What are the differences between HTTP/1.1, HTTP/2, and HTTP/3 (QUIC)?
23. How does a TLS handshake work, and what does TLS 1.3 improve?
24. REST vs gRPC vs GraphQL: when would you choose each?
25. Polling vs long polling vs Server-Sent Events vs WebSockets: what are the trade-offs?
26. How do you scale WebSocket connections to millions of concurrent users?
27. What is a CDN, how does it work, and what content should you put behind one?
28. What are anycast and GeoDNS, and how do they route users to the nearest region?

## 4. Load Balancing & Proxies

29. What is the difference between an L4 and an L7 load balancer?
30. What load-balancing algorithms exist (round robin, least connections, weighted, consistent hashing, power of two choices)?
31. What is the difference between a forward proxy and a reverse proxy?
32. What does an API gateway do, and how is it different from a load balancer?
33. What are sticky sessions, and why are they usually a smell?
34. How do load balancers perform health checks, and what goes wrong with overly aggressive checks?
35. How do you avoid the load balancer itself being a single point of failure?
36. What is a service mesh (Envoy, Istio, Linkerd), and what problems does it solve?

## 5. Caching

37. Where can you cache in a system (client, CDN, edge, application, distributed cache, database)?
38. Explain cache-aside, read-through, write-through, write-behind, and write-around.
39. What eviction policies exist (LRU, LFU, FIFO, TTL), and when is each appropriate?
40. Why is cache invalidation hard, and what strategies exist?
41. What is a cache stampede (thundering herd), and how do you prevent it (locking, request coalescing, early refresh)?
42. What are cache penetration and cache avalanche, and how do you mitigate each?
43. How do you handle hot keys in a distributed cache?
44. How do you keep a cache consistent with the database under concurrent writes?
45. Redis vs Memcached: what are the differences?
46. When does adding a cache make a system worse?

## 6. Databases & Storage

47. SQL vs NoSQL: how do you choose for a given problem?
48. What are the main NoSQL categories (key-value, document, wide-column, graph), with examples of each?
49. How do indexes speed up reads, and what do they cost on writes?
50. What is the difference between B-tree and LSM-tree storage engines?
51. What are read replicas, and what consistency problems do they introduce?
52. What is database sharding, and how do you choose a shard key?
53. What are range-based, hash-based, and directory-based sharding, and their trade-offs?
54. How do you handle resharding as data grows?
55. What is denormalization, and when is it the right call?
56. How do you handle cross-shard queries and joins?
57. Object storage vs block storage vs file storage: when do you use each?
58. How would you store and serve large binary files (images, videos)?
59. What is a time-series database, and when do you need one?
60. When would you use a search engine like Elasticsearch, and how do you keep it in sync with the primary database?

## 7. Messaging & Asynchronous Processing

61. Why use a message queue, and what does it decouple?
62. What is the difference between a message queue (SQS, RabbitMQ) and a log-based stream (Kafka)?
63. What do at-most-once, at-least-once, and exactly-once delivery mean?
64. How do consumers achieve effective exactly-once processing (idempotency, deduplication, transactions)?
65. How does Kafka ensure ordering, and why only within a partition?
66. How do you choose the number of Kafka partitions and the partition key?
67. What are consumer groups, and how does rebalancing work?
68. What is a dead-letter queue, and how should poison messages be handled?
69. What is backpressure, and how do you implement it across producers and consumers?
70. What is the difference between pub/sub and point-to-point messaging?

## 8. Reliability & Fault Tolerance

71. What is a single point of failure, and how do you find them in a design?
72. How do timeouts, retries, and exponential backoff with jitter work together?
73. What is a retry storm, and how do retry budgets prevent it?
74. How does the circuit breaker pattern work (closed, open, half-open)?
75. What is the bulkhead pattern?
76. What is graceful degradation, and give an example?
77. What is load shedding, and how do you decide which requests to drop?
78. What is a health check vs a readiness check vs a liveness check?
79. Active-active vs active-passive: what are the trade-offs?
80. How do you design for disaster recovery (RPO and RTO, backups, multi-region)?

## 9. API Design

81. What makes a good REST API (resources, verbs, status codes, versioning)?
82. How do you version APIs without breaking clients?
83. What is idempotency, and how do idempotency keys work for POST requests?
84. Offset vs cursor pagination: what are the trade-offs?
85. How do you design rate limiting for a public API (per user, per IP, per endpoint)?
86. How should errors be returned so clients can handle them correctly?
87. How do you design APIs for long-running operations (async job + status polling, webhooks)?
88. How do you design reliable webhooks (signing, retries, ordering, replay)?

## 10. Security Basics

89. What is the difference between authentication and authorization?
90. How do OAuth 2.0 and OpenID Connect work, and what are the main flows?
91. Session cookies vs JWTs: what are the trade-offs, especially around revocation?
92. How do you store passwords securely?
93. What are RBAC and ABAC, and when do you use each?
94. How do you protect services from DDoS attacks?
95. How do you manage secrets in a distributed system?
96. What is mTLS, and what is zero-trust networking?

---

# Part B — Distributed Systems (Senior)

## 11. Consistency & Theory

97. What does the CAP theorem actually say, and what are common misinterpretations?
98. What does PACELC add to CAP?
99. Explain linearizability, sequential consistency, causal consistency, and eventual consistency.
100. What are read-your-writes, monotonic reads, and consistent prefix guarantees?
101. How do you provide read-your-writes consistency with async replicas?
102. What is the difference between strong and eventual consistency from a user's perspective?
103. What are the fallacies of distributed computing?
104. What is the FLP impossibility result, and what does it mean in practice?
105. What are the two generals and Byzantine generals problems?

## 12. Replication & Consensus

106. Single-leader vs multi-leader vs leaderless replication: what are the trade-offs?
107. How do quorum reads and writes (N, R, W) work, and when does R + W > N guarantee consistency?
108. What are sloppy quorums and hinted handoff?
109. How do read repair and anti-entropy with Merkle trees work?
110. How does Raft work at a high level (leader election, log replication, commit)?
111. How does Paxos differ from Raft?
112. When does a system need consensus, and when is it overkill?
113. How do systems like ZooKeeper and etcd provide coordination, leader election, and distributed locks?
114. Why are distributed locks dangerous, and what are fencing tokens?
115. What is split-brain, and how do quorums and fencing prevent it?

## 13. Partitioning & Data Distribution

116. How does consistent hashing work, and why are virtual nodes needed?
117. How does rendezvous (highest random weight) hashing compare with consistent hashing?
118. How do you rebalance partitions with minimal data movement?
119. What are hot partitions, and how do you mitigate them (key salting, splitting)?
120. How do secondary indexes work in a partitioned database (local vs global)?

## 14. Time, Ordering & Coordination

121. Why can't you rely on wall clocks in distributed systems?
122. What are Lamport clocks and vector clocks, and what does each capture?
123. What are hybrid logical clocks, and how does Spanner's TrueTime work?
124. How do you resolve conflicts with last-write-wins, vector clocks, or CRDTs?
125. What are CRDTs, and give examples (G-counter, OR-set)?
126. How do you generate globally unique IDs at scale (UUID, Snowflake, ULID, database sequences)?
127. How do you guarantee ordering of events across services?

## 15. Distributed Transactions & Data Integrity

128. How does two-phase commit work, and why is it rarely used across microservices?
129. What is the saga pattern, and what is the difference between choreography and orchestration?
130. How do you design compensating actions, and what happens when a compensation fails?
131. What is the transactional outbox pattern, and what problem does it solve?
132. What is the dual-write problem, and how does Change Data Capture help?
133. What are event sourcing and CQRS, and when are they worth the complexity?
134. How do you keep data consistent between a primary database, a cache, and a search index?

## 16. Microservices & Architecture Patterns

135. Monolith vs modular monolith vs microservices: how do you decide?
136. How do you define service boundaries (domain-driven design, bounded contexts)?
137. What is the "distributed monolith" anti-pattern?
138. Why should each service own its data, and how do you handle queries that span services?
139. How do services discover each other (client-side vs server-side discovery)?
140. What is the strangler fig pattern for migrating a monolith?
141. What are sidecars, and what belongs in a sidecar vs a shared library?
142. How do you handle schema evolution for events and APIs between services (Avro/Protobuf, schema registry)?
143. What is a cell-based architecture, and how does it limit blast radius?

## 17. Observability & Operations

144. What are the three pillars of observability, and how do they complement each other?
145. How does distributed tracing work, and how is context propagated?
146. What are the RED and USE methods for monitoring?
147. How do you design alerts that are actionable and avoid alert fatigue?
148. How do you deploy safely (blue-green, canary, rolling, feature flags)?
149. How do you roll back a deployment that included a database migration?
150. What is chaos engineering, and how do you start doing it safely?
151. How do you run a blameless postmortem?

---

# Part C — Classic Design Problems

For each problem, cover requirements, scale estimates, APIs, data model, high-level design, bottlenecks, and failure modes.

## 18. Core Problems

152. Design a URL shortener (TinyURL).
153. Design a rate limiter (token bucket, sliding window, distributed counters).
154. Design a distributed key-value store (Dynamo-style).
155. Design a distributed cache (Memcached/Redis cluster).
156. Design a unique ID generator (Snowflake).
157. Design a web crawler.
158. Design a notification system (push, email, SMS) with retries and user preferences.
159. Design a news feed (Twitter/Instagram) — fan-out on write vs fan-out on read.
160. Design a chat system (WhatsApp/Slack) with delivery receipts, ordering, and offline sync.
161. Design a typeahead or autocomplete system.
162. Design a search engine for a large document corpus.
163. Design a video streaming platform (YouTube/Netflix), including upload, transcoding, and adaptive bitrate.
164. Design a file storage and sync service (Dropbox/Google Drive).
165. Design an object storage system (S3).
166. Design a ride-sharing service (Uber) with real-time location matching.
167. Design a proximity service (Yelp, "nearby friends") using geohashes or quadtrees.
168. Design a payment system with idempotency, a ledger, and reconciliation.
169. Design a digital wallet with strong consistency and audit trails.
170. Design an e-commerce checkout and inventory system that prevents overselling.
171. Design a ticket booking system (Ticketmaster) under flash-sale traffic.
172. Design a hotel or airline reservation system.
173. Design a distributed job scheduler (cron at scale).
174. Design a distributed message queue (Kafka).
175. Design a metrics monitoring and alerting system (Prometheus/Datadog).
176. Design a logging pipeline that ingests terabytes per day.
177. Design a leaderboard for a game with millions of players.
178. Design an ad click aggregation system with real-time counts and exact billing.
179. Design a collaborative document editor (Google Docs) — OT vs CRDTs.
180. Design a stock exchange matching engine.
181. Design a feature flag service.
182. Design a recommendation system's serving layer.
183. Design a multi-tenant SaaS platform with tenant isolation.

---

# Part D — Senior+ & Staff

## 19. Advanced Trade-offs & Deep Dives

184. How would you design a system to serve users globally with low-latency reads and consistent writes?
185. Multi-region active-active: how do you handle conflicts, failover, and data residency laws?
186. How do you design for a 100x traffic spike that you know is coming (launches, sales)?
187. How do you design for an unexpected viral spike?
188. How do you migrate a live system to a new database with zero downtime (dual writes, backfill, shadow reads, cutover)?
189. How do you split a monolithic database into service-owned databases?
190. How do you design a system whose cost scales sub-linearly with traffic?
191. How do you decide between building and buying (managed services vs self-hosted)?
192. How do you handle the "celebrity problem" in feeds, followers, and hot keys?
193. How do you design idempotency across a whole workflow, not just one API?
194. How do you reason about metastable failures, where a system stays down after the trigger is gone?
195. How do you design admission control and prioritization when a system is overloaded?
196. How would you design for data correctness when bugs will happen (auditing, reconciliation, replayability)?
197. How do you design systems that are easy to debug in production?
198. How do you evaluate a design's blast radius, and how do you shrink it?
199. What is the cost of strong consistency in latency and availability, and how do you explain it to product teams?

## 20. Staff-Level Judgment & Leadership

200. How do you write a design document, and what makes one effective?
201. How do you review someone else's design, and which questions do you always ask?
202. How do you drive alignment on an architecture decision across several teams that disagree?
203. How do you decide when to pay down technical debt versus ship features?
204. How do you plan a multi-quarter migration and keep it from stalling?
205. How do you define platform standards (APIs, observability, reliability) without becoming a bottleneck?
206. How do you set SLOs for a new service, and who should own them?
207. How do you measure whether an architecture change actually succeeded?
208. Tell me about a design you got wrong. What did you learn, and what would you do differently?
209. How do you balance simplicity against future scalability ("you aren't gonna need it" vs over-engineering)?
210. How do you mentor engineers to get better at system design?
