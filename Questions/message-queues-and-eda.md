# Message Queues & Event-Driven Architecture: 50 Interview Questions

Five levels, from fundamentals to design rounds. Each question has a short note on what a strong answer must cover, for self-checking.

**Target:** answer 40+ at that standard, including most of Levels 4 and 5.

---

## Level 1: Fundamentals (1–10)

1. **What is a message queue, and why use one instead of direct service-to-service calls?**
   *Decoupling, async processing, absorbing load spikes, resilience when consumers are down.*

2. **What's the difference between a queue (point-to-point) and pub/sub?**
   *A queue delivers each message to one consumer; pub/sub delivers every message to every subscriber.*

3. **What is event-driven architecture, and how does it differ from request-response?**
   *Producers emit facts without knowing who reacts, which inverts the dependency.*

4. **What's the difference between an event, a command, and a message?**
   *An event is something that happened (past tense, many listeners); a command is a request to do something (one handler); a message is the transport envelope.*

5. **What are producers, consumers, and brokers?**
   *Who writes, who reads, and who stores and routes.*

6. **What does "acknowledgement" mean in messaging, and what happens if a consumer crashes before acking?**
   *The message gets redelivered.*

7. **What's the difference between push-based and pull-based consumption?**
   *RabbitMQ pushes, Kafka pulls; trade-offs in flow control and latency.*

8. **When would you *not* use a message queue?**
   *When you need an immediate synchronous answer, the flow is simple, or the added operational complexity isn't worth it.*

9. **What is a dead letter queue (DLQ)?**
   *Where poison messages go after max retries, so they don't block the queue.*

10. **Name the main brokers (Kafka, RabbitMQ, NATS, SQS) and one key difference between each.**
    *Log vs. queue model, persistence, routing flexibility, managed vs. self-hosted.*

---

## Level 2: Core Concepts (11–20)

11. **Explain at-most-once, at-least-once, and exactly-once delivery.**
    *When each one loses or duplicates messages, and which one is the realistic default.*

12. **Why is at-least-once the most common choice, and what does it force consumers to do?**
    *Consumers must be idempotent.*

13. **What is an idempotent consumer, and how do you implement one?**
    *Dedup keys, a processed-IDs table, upserts, or natural idempotency.*

14. **How does Kafka store data? Explain topics, partitions, and offsets.**
    *An append-only log; offsets are positions in it; the log is retained, not deleted on read.*

15. **What is a consumer group, and how does Kafka distribute partitions within one?**
    *One partition maps to at most one consumer in the group, so parallelism is capped by the partition count.*

16. **How does Kafka guarantee ordering, and what are its limits?**
    *Ordering holds only within a partition, so the message key decides ordering scope.*

17. **How do you choose a partition key?**
    *Ordering needs vs. even distribution; hot partitions.*

18. **Explain RabbitMQ exchanges: direct, topic, fanout, and headers.**
    *Each routes messages to queues by different rules.*

19. **What are NATS Core and NATS JetStream, and how do they differ?**
    *Core is fire-and-forget at-most-once; JetStream adds persistence, replay, and acks.*

20. **What's the difference between message retention in Kafka and in RabbitMQ?**
    *Kafka keeps messages by time or size; RabbitMQ deletes them once acked. This affects replay.*

---

## Level 3: Reliability & Operations (21–30)

21. **What happens during a Kafka consumer group rebalance, and why is it a problem?**
    *Processing pauses and messages may be duplicated; cooperative-sticky assignment and static membership help.*

22. **When should a consumer commit offsets, and what goes wrong if it commits too early or too late?**
    *Too early loses messages on a crash; too late causes duplicates.*

23. **Explain Kafka producer `acks=0`, `acks=1`, and `acks=all`, and how `min.insync.replicas` interacts with them.**
    *The durability vs. latency trade-off.*

24. **What is the ISR (in-sync replica set), and what happens when the leader dies?**
    *Leader election from the ISR; unclean leader election causes data loss.*

25. **How do you design retries without overwhelming a failing downstream system?**
    *Exponential backoff with jitter, retry topics with delays, and a max-attempt limit before the DLQ.*

26. **What is a poison message, and how do you detect and handle one?**
    *A message that fails every time; route it to the DLQ, alert, and keep the payload for inspection.*

27. **What is consumer lag, and how would you monitor and alert on it?**
    *The gap between the latest offset and the committed offset; Prometheus exporters, and alerts on lag growth rate rather than just absolute value.*

28. **What is backpressure, and how do you handle a consumer that can't keep up?**
    *Prefetch limits, scaling consumers up to the partition count, batching, rate limiting the producer.*

29. **What is Kafka log compaction, and when would you use it?**
    *It keeps the latest value per key; useful for state snapshots and changelog topics.*

30. **How do you write a graceful-shutdown Kafka or NATS consumer in Go?**
    *Use `context` cancellation and signal handling, finish in-flight messages, commit, then close. Tie this to the GoFR goroutine-leak fix.*

---

## Level 4: Advanced Patterns (31–40)

31. **What is the dual-write problem, and how does the transactional outbox pattern solve it?**
    *Writing to the DB and publishing an event aren't atomic; the outbox writes the event into a DB table in the same transaction, and a relay publishes it.*

32. **What is change data capture (CDC), and how does it compare to the outbox pattern?**
    *Debezium reads the database WAL; trade-offs in coupling, schema exposure, and operational complexity.*

33. **How does Kafka achieve "exactly-once semantics", and what are its real limits?**
    *Idempotent producers (producer ID + sequence numbers), transactions, and `read_committed` consumers. It only covers Kafka-to-Kafka flows; side effects in external systems still need idempotency.*

34. **What is the saga pattern? Compare choreography and orchestration.**
    *Distributed transactions as a chain of local steps with compensating actions; choreography uses events, orchestration uses a central coordinator. Trade-offs in visibility and coupling.*

35. **What is event sourcing, and what are its pitfalls?**
    *State is derived by replaying an event log; snapshots for speed; pitfalls include schema evolution, replay cost, and the difficulty of correcting past events.*

36. **What is CQRS, and why is it often paired with event sourcing?**
    *Separate write and read models; read models are built from events and are eventually consistent.*

37. **How do you evolve event schemas without breaking consumers?**
    *Protobuf or Avro with a schema registry; backward and forward compatibility rules; never reuse or renumber Protobuf fields; add optional fields only.*

38. **How do you handle out-of-order and duplicate events?**
    *Sequence numbers or entity versions, rejecting stale updates, event time vs. processing time, watermarks in stream processing.*

39. **How do you implement delayed or scheduled messages?**
    *RabbitMQ TTL with a dead-letter exchange or the delayed-message plugin; Kafka has no native delay, so use tiered retry topics or an external scheduler; JetStream's redelivery delay (NakWithDelay).*

40. **How do you process messages in parallel in Go while preserving per-key ordering?**
    *Hash the key to a fixed worker goroutine (one channel per worker), track per-partition commit positions, and only commit offsets that all workers have finished.*

---

## Level 5: System Design & Debugging (41–50)

41. **Design an async job scheduling system for GPU workloads (submit, queue, schedule, run, report status).**
    *Priority queues, job state machine, idempotent job IDs, retries with limits, status events back to the API, handling worker death via heartbeats or visibility timeouts.*

42. **Design a usage-metering pipeline for billing that must never lose or double-count events.**
    *At-least-once delivery plus dedup by event ID, aggregation windows, late-event handling, durable storage, and periodic reconciliation against the source of truth.*

43. **Design autoscaling for an inference service based on queue depth or consumer lag.**
    *Choice of metric, KEDA or custom HPA metrics, scale-to-zero and cold starts, cooldown windows to prevent flapping, upper bounds.*

44. **Add event-driven re-replication to a distributed object store (like Castor) when a storage node fails.**
    *Node-failure detection emits an event, repair workers copy under-replicated chunks idempotently, rate limiting to avoid repair storms, and handling the node coming back mid-repair.*

45. **Kafka, RabbitMQ, or NATS: pick one for (a) control-plane notifications and (b) high-volume telemetry, and justify.**
    *Latency vs. throughput, replay needs, routing complexity, operational cost.*

46. **How would you move events across regions or clouds?**
    *MirrorMaker 2 or cluster linking, offset translation, ordering and duplication across regions, active-passive vs. active-active, conflict handling.*

47. **Consumer lag is growing steadily in production. Walk through how you debug it.**
    *Check whether throughput dropped or volume rose; slow downstream calls; rebalance loops; hot partitions; GC pauses; partition count vs. consumer count; then the fix for each cause.*

48. **Customers were charged twice. The billing flow is event-driven. How do you investigate and fix it?**
    *Trace the duplicate through redelivery, retries, and rebalances; find the missing idempotency check; add dedup keys and idempotency at the payment call; plan remediation for affected customers.*

49. **How do you capacity-plan a Kafka cluster? How many partitions, how much disk?**
    *Target throughput divided by per-partition throughput, consumer parallelism, retention × ingest rate × replication factor for disk, headroom, and the cost of adding partitions later (key remapping).*

50. **How do you trace a single request across async, event-driven services?**
    *Propagate trace context in message headers (W3C traceparent via OpenTelemetry), span links vs. parent-child spans for async hops, correlation IDs in logs. Tie this to the OpenTelemetry contributions.*
