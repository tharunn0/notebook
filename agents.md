# Guidelines for Agents: Note Structuring & Authoring

This document defines the architectural standard for authoring, editing, and expanding technical notes across this repository. 

All agents and contributors must follow these guidelines to ensure content maintains **senior-level technical rigor** while remaining **immediately readable and scannable** ("decently easy to pick up").

---

## 1. Core Philosophy: High Signal, Zero Fluff

- **Depth over generalities**: Always explain *how* and *why* things work under the hood (memory layouts, protocols, network packet flow, concurrency primitives, data structures, disk I/O).
- **Structure over walls of text**: High technical density must be matched by high visual structure. Avoid academic run-on paragraphs that bury critical concepts in filler prose.
- **Concrete over abstract**: Name specific tools, algorithms, protocols, and production benchmarks (e.g., mention `Raft`, `WAL fsync`, `gRPC over HTTP/2`, `B+Trees`, rather than generic "consensus algorithms" or "data structures").
- **Table of Contents (Index) Required**: Every file must start with a clickable Index/Table of Contents linking to each question header using standard GitHub markdown anchors (e.g., `[Title](#anchor)`).
- **Logical Question Ordering (Never Blindly Append)**: When adding a new question, **do not simply append it to the end of the file**. Position it logically within the flow of concepts (foundations -> mechanisms -> protocols -> advanced patterns / resilience), and immediately update the top Index to include it.

---

## 2. Standard Answer Anatomy

Every technical question section must follow this 4-part framework:

### Part 1: The Executive Summary (1–2 Sentences)
- Immediately define what the concept is and the core problem it solves.
- Avoid throat-clearing openings like *"In modern computing, it is well known that..."* or *"Distributed systems are complex environments where..."*.
- Start directly with the definition and its purpose.

### Part 2: Mechanics & Under-the-Hood Operation
- Explain execution semantics, state machines, data flow, or hardware/OS interactions.
- Break multi-step or multi-component concepts into **bolded bullet lists** or **sub-headings** rather than compounding them into single run-on paragraphs.

### Part 3: Categorical Breakdown & Edge Cases
- Group related aspects into distinct buckets (e.g., *Communication & Memory*, *Failure Modes*, *Consistency Guarantees*).
- Use lists with bold lead-ins: `- **Dimension**: Explanation of mechanics`.
- Include mathematical formulas using standard LaTeX when relevant (e.g., quorum math $R + W > N$, availability formulas).

### Part 4: Production Trade-offs & Real-World Implementations
- Detail real-world trade-offs: latency vs. consistency, throughput vs. safety, operational simplicity vs. flexibility.
- Reference actual implementations (e.g., Envoy, PostgreSQL, Kafka, Resilience4j, TrueTime).
- State practical engineering rules of thumb (when to use X, when to avoid X).

---

## 3. Formatting Rules

| Rule | Bad Practice ❌ | Good Practice ✅ |
| :--- | :--- | :--- |
| **Sentence Length** | 60–80 word sentences with 5+ nested clauses. | 15–30 word sentences that make a single clear point. |
| **Paragraph Height** | Unbroken text blocks exceeding 6 lines. | Max 3–4 lines per paragraph, separated by thematic lists. |
| **Lists** | Naked bullet points with vague summaries. | Bolds with informative lead-in: `- **Open (Fail-Fast)**: ...` |
| **Equations** | Inlined ASCII math: `Avail = MTBF / (MTBF + MTTR)`. | LaTeX formatting: `$$\text{Availability} = \frac{\text{MTBF}}{\text{MTBF} + \text{MTTR}}$$` |
| **Naming** | "Some messaging systems or databases" | "Append-only commit logs like Apache Kafka or message brokers like RabbitMQ" |

---

## 4. Concrete Example: Before vs. After

### ❌ Anti-Pattern (Too Dense & Hard to Parse)
> In a monolithic system, inter-module communication is facilitated through in-process function invocations that leverage the host hardware's shared memory, CPU caches, and register sets. These calls pass pointers or stack values with virtually zero serialization cost at sub-microsecond latencies, synchronized via kernel or language runtime concurrency primitives like mutexes, condition variables, and read-write locks, while state mutations are governed by local ACID transactions enforced by a centralized database engine's write-ahead log and lock manager.

*Problem: Highly informative, but one single 85-word sentence creates cognitive fatigue and obscures key contrasts.*

### ✅ Target Pattern (Technical, Deep, and Scannable)
> The core divergence lies in execution semantics, communication overhead, and state synchronization:
> - **Communication & Memory**: Monoliths use in-process function calls that share memory, CPU caches, and registers at sub-microsecond latency with zero serialization cost, synchronized via mutexes and locks. Distributed systems cross network boundaries via RPCs (e.g., gRPC over HTTP/2) or event streams (e.g., Kafka), requiring explicit payload serialization (Protocol Buffers, JSON, Avro) across disjoint address spaces.
> - **Time & Transactions**: Monoliths rely on a single system clock and local ACID transactions enforced by a database write-ahead log (WAL). Distributed systems face clock drift and network jitter, requiring logical clocks (Lamport, Vector Clocks) and consensus algorithms (Raft, Paxos) for state transitions.

---

## 5. Agent Pre-Submission Checklist

Before finalizing any notes or answers, verify:

- [ ] Does the answer immediately provide the core definition without fluff?
- [ ] Are all critical protocols, algorithms, failure modes, and trade-offs explicitly mentioned?
- [ ] Is the answer broken down with clear headings, sub-headings, or bold lead-in bullets?
- [ ] Are there any unbroken paragraphs longer than 4 lines? (If yes, split or bulletize them).
- [ ] Are sentences kept under ~30 words?
- [ ] Are real-world tools, systems, or libraries named where appropriate?
- [ ] Is the question placed logically in the document rather than merely appended?
- [ ] Is the clickable Table of Contents (Index) at the top of the file updated with the new section link?
