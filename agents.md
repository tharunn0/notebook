# Guidelines for Agents: Note Structuring & Authoring

This document defines the architectural standard for authoring, editing, and expanding technical notes across this repository. 

All agents and contributors must follow these guidelines to ensure content maintains **senior-level technical depth and production rigor** while remaining **structured, scannable, and practical**.

---

## 1. Core Philosophy: Senior-Level Rigor, High Signal

- **Senior/Staff Engineer Depth**: Never settle for high-level summaries or textbook definitions. Always explain *how* and *why* things work under the hood (memory layouts, kernel sockets, wire serialization, execution runtimes, thread scheduling, disk I/O, and consensus mechanics).
- **Dissect Failure Modes & Edge Cases**: Don't just explain the happy path. Analyze what breaks under high load: thread pool saturation, socket backlog exhaustion, latency compounding across call graphs, distributed race conditions, split-brain, and thundering herds.
- **Concrete & Production-Grounded**: Name specific protocols, tools, algorithms, and benchmarks (e.g., `Raft`, `WAL fsync`, `gRPC over HTTP/2`, `BBR congestion control`, `Transactional Outbox with Debezium`, `Resilience4j`, rather than vague "messaging queues" or "caching layers").
- **Grounded Math Over Academic Fluff**: Avoid gratuitous mathematical derivations or abstract formulas unless they directly drive operational configurations (e.g., availability $\text{MTBF} / (\text{MTBF} + \text{MTTR})$, quorum math $R + W > N$, or randomized jitter formulas). Focus on real-world systems engineering over academic theory.
- **Index & Logical Question Ordering**: Every file must start with a clickable Table of Contents (Index) linking to each question header using standard GitHub markdown anchors. When adding a question, position it logically within the conceptual flow (foundations $\to$ mechanisms $\to$ protocols $\to$ resilience/patterns); **never blindly append to the end**.

---

## 2. Diagram & Visualization Policy

- **STRICTLY NO MERMAID DIAGRAMS**: Do not use Mermaid blocks (`mermaid`). They often fail to render consistently across markdown viewers, terminals, and CLI environments, and introduce rendering fragility.
- **ASCII Diagrams ONLY WHEN NECESSARY**: Use lightweight, clean ASCII diagrams only when illustrating complex spatial topologies, network/memory boundaries, state machines, or decision trees. If a concept is clearer in structured text or a table, omit the diagram.
- **ASCII Box & Flow Standards**: Keep ASCII diagrams compact, clean, and unambiguous:
  ```
  Client ──[ Synchronous Call ]──> [ Gateway ] ──> [ Microservice ]
                                        │
                             [ Async Event Stream ]
                                        ▼
                             [ Durable Commit Log ]
  ```

---

## 3. Standard Answer Anatomy

Every technical question section must adhere to this structured framework:

### Part 1: Executive Summary & Core Mechanics (The "What" and "Why")
- Immediately define the concept and state the fundamental problem it solves.
- Avoid conversational throat-clearing openings (e.g., *"In modern software development..."* or *"Distributed systems are complex environments where..."*).
- Highlight the underlying execution boundary (in-process vs. network, thread blocking vs. event-driven, memory registers vs. serialized payloads).

### Part 2: Detailed Architectural Mechanics & State Flow
- Detail internal operations: state transitions, socket lifecycles, concurrency locks, buffering, and persistence guarantees.
- Use a clean ASCII diagram if visual flow clarifies the state machine or network topology.
- Break multi-step mechanics into **bolded lead-in bullet points** or descriptive subheadings.

### Part 3: Deep Comparison Tables & Architectural Realities
- When comparing paradigms (e.g., Monolith vs. Distributed, Sync vs. Async, CP vs. AP), always provide a **markdown comparison table**.
- Contrast across concrete operational dimensions: Temporal Coupling, Availability Impact, Flow Control / Backpressure, Blast Radius, Debuggability / Tracing, and Consistency Semantics.

### Part 4: Production Decision Framework ("When to Choose Each")
- Provide an actionable, criteria-based decision guide.
- Include **concrete real-world enterprise scenarios** (e.g., payment authorizations, flash-sale order ingestion, search index projection, read-heavy user dashboards).
- Name the **essential defensive patterns required** to make each approach production-ready (e.g., Bulkheads, Deadline Budgets, Transactional Outbox + CDC, Idempotency Keys, Circuit Breakers, Dead-Letter Queues).

---

## 4. Formatting Rules

| Rule | Bad Practice ❌ | Good Practice ✅ |
| :--- | :--- | :--- |
| **Diagrams** | Rendering Mermaid charts (`mermaid`). | Clean ASCII diagrams (` ``` `) used only when necessary. |
| **Sentence Length** | 60–80 word sentences with 5+ nested clauses. | 15–30 word sentences that make a single clear point. |
| **Paragraph Height** | Unbroken text blocks exceeding 5 lines. | Max 3–4 lines per paragraph, broken up with structured lists. |
| **Lists** | Naked bullet points with vague summaries. | Bolds with informative lead-in: `- **OPEN (Fail-Fast)**: ...` |
| **Formulas** | Inlined ASCII math: `Avail = MTBF / (MTBF + MTTR)`. | LaTeX formatting: `$$\text{Availability} = \frac{\text{MTBF}}{\text{MTBF} + \text{MTTR}}$$` |
| **Naming** | "Some messaging systems or databases" | "Append-only commit logs like Apache Kafka or message brokers like RabbitMQ" |
| **Depth** | Stating "Circuit breakers prevent cascading failure." | Explaining that slow downstream calls saturate upstream worker threads and connection pools, causing cascading timeouts. |

---

## 5. Concrete Authoring Example: Before vs. After

### ❌ Anti-Pattern (Generic, Superficial, or Over-Academic)
> A circuit breaker is a design pattern used in modern software development to prevent failures from cascading. It has three states: Closed, Open, and Half-Open. When failures occur, it opens and stops traffic, then lets a little traffic through to see if it's healthy. It is important for microservices resilience.

*Problem: Superficial textbook answer. Lacks systems depth, ignores resource starvation mechanics (threads/sockets), misses windowing edge cases, and provides no production engineering context.*

### ✅ Target Senior Pattern (Technical, Structured, and Actionable)
> The **Circuit Breaker** pattern protects an upstream service from resource exhaustion when a downstream dependency degrades, while shielding the downstream service from traffic surges.
> 
> In distributed systems, **slow dependencies are far more dangerous than dead dependencies**. A crashed downstream service returns immediate TCP `RST` or `ECONNREFUSED` packets (sub-millisecond resolution). In contrast, a service experiencing high latency (e.g., GC pauses, unindexed database scans) leaves upstream caller sockets and threads blocked waiting on I/O. Under sustained traffic, this quickly causes **upstream thread pool exhaustion**, fills OS socket queues, and triggers cascading failure across the entire platform.
> 
> It operates as a finite state machine:
> - **`CLOSED` (Normal Flow)**: All calls pass through. Outcomes are recorded in a sliding time/count window. If failure or slow-call rates breach the threshold *and* minimum request volume is met, it trips to `OPEN`.
> - **`OPEN` (Fail-Fast)**: Calls short-circuit immediately without touching the network, returning a fallback or error instantly. Upstream threads are preserved. A sleep cooldown begins.
> - **`HALF-OPEN` (Canary Probe)**: A bounded batch of trial requests tests downstream health. Success resets to `CLOSED`; any failure immediately trips back to `OPEN`.

---

## 6. Agent Pre-Submission Checklist

Before finalizing any notes, answers, or documentation edits, verify:

- [ ] Does the answer immediately provide the core definition and problem context without fluff?
- [ ] Are underlying mechanics explained (memory, sockets, threads, serialization, storage, protocols)?
- [ ] Are real-world failure modes, edge cases, and resource contention scenarios addressed?
- [ ] Are diagrams formatted strictly in **ASCII** (zero Mermaid blocks)?
- [ ] Are ASCII diagrams used strictly when necessary, keeping them clean and minimal?
- [ ] Does it include a concrete **"When to Choose What"** production decision framework with real scenarios?
- [ ] Are complementary production patterns named (Bulkheads, Outbox, CDC, Idempotency, DLQs)?
- [ ] Is there an architectural comparison table where contrasting paradigms are discussed?
- [ ] Are there zero unbroken paragraphs longer than 4 lines?
- [ ] Are real tools, platforms, and protocols cited (e.g., Kafka, Envoy, gRPC, Redis, Postgres)?
- [ ] Is the question placed logically in the document rather than merely appended?
- [ ] Is the clickable Table of Contents (Index) at the top of the file updated with the exact title and anchor?
