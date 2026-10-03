# CS Notebook

A structured collection of notes covering core ideas and detailed explanations across distributed systems, database internals, operating systems, networking protocols, Go runtime mechanics, and AI/LLM architectures.

---

## 1. Why This Structure?

Technical notes often suffer from one of two extremes: they are either too shallow (skimming the surface with vague buzzwords) or too dense (unbroken walls of text that cause cognitive fatigue and bury key mechanisms).

This repository is structured around three core principles:
- **Instant Scannability & Fast Retrieval**: Each file features a top-level clickable **Index** linking directly to specific questions, allowing you to jump straight to the topic you need without endless scrolling.
- **Visual Breakdown over Walls of Text**: Concepts are decomposed into direct summaries, bold lead-in bullet points, and subheadings. This makes complex systems concepts decently easy to pick up and review while preserving full under-the-hood depth.
- **Modular Domain Organization**: Topics are separated by domain into dedicated directories and files, keeping related concepts grouped logically from foundational mechanics to advanced production trade-offs.

---

## 2. High-Level Repository Structure & Layout

```
.
├── README.md                          # Repository overview, structure rationale & domain directory
├── AGENTS.md                          # Authoring standard for notes (structure, indexing, formatting)
│
├── distributed-systems/               # Distributed computing, consensus & fault tolerance
│   └── distributed-systems.md         # Monolith vs. Distributed, CAP/PACELC, consistency & circuit breakers
│
├── databases/                         # Database internals, storage engines & transactions
│   └── db.md                          # OLTP vs. OLAP, 6-layer DBMS, B+Trees vs. LSM-trees, ACID & BASE
│
├── os/                                # Operating systems, networking & virtualization
│   ├── os.md                          # Kernel internals, process memory layout, virtual memory & deadlocks
│   ├── networking.md                  # OSI 7-layer, TCP/UDP transport deep dive, HTTP/1-3 & TLS 1.2/1.3
│   └── virt-containers.md             # Hypervisors, Linux namespaces, cgroups v2, OverlayFS & micro-VMs
│
├── go/                                # Go (Golang) runtime mechanics & concurrency
│   └── go.md                          # M:N scheduler, Goroutines, channels, GC internals & memory model
│
└── ai-llm/                            # Artificial intelligence, neural architectures & LLMs
    ├── ai.md                          # Neuron mechanics, non-linear activations & computational graphs
    ├── architectures.md               # CNNs, RNNs/LSTMs/GRUs, Attention mechanisms & Transformers
    ├── llm-fundamentals.md            # BPE tokenization, embeddings, LoRA, RLHF/DPO, context & RAG
    └── training-mechanics.md          # Backpropagation chain rule, AdamW, loss functions & training dynamics
```


