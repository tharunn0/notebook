# Large Language Model Fundamentals: Key Concepts & Mechanics

A foundational guide covering the concepts specific to large language models: tokenization, embeddings, the pretraining/fine-tuning/alignment pipeline, and inference-time behavior.

---

## Index

1. [What is tokenization, and how do subword schemes like BPE work?](#what-is-tokenization-and-how-do-subword-schemes-like-bpe-work)
2. [What is an embedding, and how do embedding spaces capture semantic similarity?](#what-is-an-embedding-and-how-do-embedding-spaces-capture-semantic-similarity)
3. [What is the difference between pretraining and fine-tuning?](#what-is-the-difference-between-pretraining-and-fine-tuning)
4. [What is RLHF, and why is it used to align LLMs?](#what-is-rlhf-and-why-is-it-used-to-align-llms)
5. [What is a context window, and what limits it?](#what-is-a-context-window-and-what-limits-it)
6. [What are temperature, top-k, and top-p sampling in text generation?](#what-are-temperature-top-k-and-top-p-sampling-in-text-generation)
7. [What is hallucination in LLMs, and why does it occur?](#what-is-hallucination-in-llms-and-why-does-it-occur)
8. [What is Retrieval-Augmented Generation (RAG), and how does it mitigate hallucination and knowledge cutoff?](#what-is-retrieval-augmented-generation-rag-and-how-does-it-mitigate-hallucination-and-knowledge-cutoff)
9. [What is in-context learning, and how does it differ from fine-tuning?](#what-is-in-context-learning-and-how-does-it-differ-from-fine-tuning)

---

## What is tokenization, and how do subword schemes like BPE work?

Tokenization converts raw text strings into a sequence of discrete integer IDs from a fixed vocabulary, transforming characters into tensors consumable by neural networks.

### Tokenization Schemes
- **Word-Level**: Splits on whitespace/punctuation. Produces massive vocabularies and fails on out-of-vocabulary (OOV) words.
- **Character-Level**: Treats individual characters as tokens. Eliminates OOV words, but creates excessively long token sequences, exacerbating the quadratic cost of self-attention.
- **Subword Schemes (Byte-Pair Encoding / BPE)**: The industry standard. Starts with individual bytes or characters and iteratively merges the most frequent adjacent token pairs into new tokens until reaching a target vocabulary size (typically 32k–100k+ tokens).

### Byte-Level BPE Mechanics
- Frequent words become single tokens (`"the"`).
- Rare words decompose into known subwords (`"un" + "pre" + "dictable"`).
- Fallback to raw bytes ensures **zero OOV tokens**—every arbitrary Unicode string or code snippet is representable.

### Production Caveats
- Context windows and API billing are measured in tokens, not characters.
- Non-English languages and code often tokenize into more tokens per semantic unit due to training corpus imbalances, inflating latency and cost.

---

## What is an embedding, and how do embedding spaces capture semantic similarity?

An embedding maps a discrete categorical token into a continuous, high-dimensional vector space ($\mathbb{R}^d$, typically 768 to 4096 dimensions) where geometric proximity reflects semantic similarity.

### Under the Hood
- **Lookup Table**: In a Transformer, the embedding layer is a learned weight matrix of shape $(\text{vocab\_size}, d_{\text{model}})$. Token ID $i$ retrieves row $i$.
- **Distributional Hypothesis**: Words appearing in similar contexts are pulled toward neighboring vector coordinates via gradient descent.
- **Geometric Similarity**: Proximity is quantified via **Cosine Similarity**:
  $$\text{Cosine Similarity}(\mathbf{u}, \mathbf{v}) = \frac{\mathbf{u} \cdot \mathbf{v}}{\|\mathbf{u}\| \|\mathbf{v}\|}$$
- Classic linear semantic properties emerge naturally (e.g., $\mathbf{v}_{\text{king}} - \mathbf{v}_{\text{man}} + \mathbf{v}_{\text{woman}} \approx \mathbf{v}_{\text{queen}}$).

### Production Use in RAG
Standalone embedding models (trained with contrastive loss) project entire passages into vector databases. Note: Cosine similarity measures topical relatedness, not factual verification—topically similar chunks may contradict the prompt without reranking.

---

## What is the difference between pretraining and fine-tuning?

Pretraining and fine-tuning represent the foundational phases of modern LLM development, trading massive compute for domain specialization.

| Dimension | Pretraining | Fine-Tuning |
| :--- | :--- | :--- |
| **Objective** | Self-supervised next-token prediction over raw text. | Task-specific loss on curated input-output pairs. |
| **Data Scale** | Trillions of tokens (web, books, code). | Thousands to low millions of curated examples. |
| **Compute Cost** | Millions of dollars; thousands of GPUs across months. | Modest; single GPU to small cluster across hours/days. |
| **Outcome** | Base model with general world knowledge and syntax. | Aligned model following instructions, persona, or schema. |

### Adaptation Methods
- **Full Fine-Tuning**: Updates all model weights; high compute and storage cost per task.
- **Parameter-Efficient Fine-Tuning (PEFT / LoRA)**: Freezes base weights and injects low-rank trainable matrices ($\Delta W = B \cdot A$, where $r \ll d$). Drastically cuts VRAM requirements and enables swappable adapters.
- **When to Use**: Use fine-tuning for persistent style, formatting, or tone shifts. Use RAG/prompting for factual knowledge injection to avoid catastrophic forgetting.

---

## What is RLHF, and why is it used to align LLMs?

Reinforcement Learning from Human Feedback (RLHF) aligns a pretrained base model with human preferences—optimizing for helpfulness, accuracy, and safety beyond raw next-token plausibility.

### The RLHF Pipeline
1. **Preference Data Collection**: Human annotators rank multiple candidate model responses for the same prompt.
2. **Reward Model (RM)**: Trains a neural network to score prompt-response pairs, acting as a differentiable proxy for human approval.
3. **Policy Optimization (PPO)**: Tunes the LLM using Reinforcement Learning (Proximal Policy Optimization) to maximize the RM score, constrained by a **KL-divergence penalty** to prevent policy drift.
4. **Direct Preference Optimization (DPO)**: A modern alternative that bypasses training an explicit reward model, optimizing weights directly on preference pairs via an implicit mathematical objective.

### Production Realities
Next-token prediction alone produces models that ramble or mimic internet toxicity. RLHF bridges the gap to create interactive assistants, but risks **reward hacking** (sycophancy, excessive verbosity, or confident hallucinations that superficially please reward models).

---

## What is a context window, and what limits it?

A context window is the maximum number of tokens an LLM can attend to and condition upon in a single forward pass (including prompt, injected context, and generated output).

### Architectural Bottlenecks
- **Quadratic Compute ($O(N^2)$)**: Full self-attention calculates compatibility scores across all token pairs, quadrupling compute when doubling sequence length.
- **Memory: The KV Cache**: During autoregressive decoding, Key and Value vectors for all past tokens must be preserved in GPU VRAM to avoid recomputation. At long contexts, the KV cache can exceed the memory footprint of the model weights.
- **Positional Extrapolation**: Models struggle to extrapolate beyond pre-trained positions without continuous position embeddings like **RoPE** with position interpolation.

### Production Engineering
- **"Lost in the Middle"**: Models retrieve information placed at the absolute start or end of long context windows more reliably than content buried in the center.
- **RAG vs. Long Context**: Long contexts carry high latency and token cost. RAG retrieves only the most relevant passages, keeping inference fast and cost-efficient.

---

## What are temperature, top-k, and top-p sampling in text generation?

Temperature, top-$k$, and top-$p$ are inference-time decoding hyperparameters that reshape the predicted probability distribution over the vocabulary without altering model weights.

### Decoding Parameters
- **Temperature ($T$)**: Rescales raw logits before the softmax operation ($z_i / T$):
  - $T \to 0$: Sharpens the distribution, approaching **Greedy Decoding** (deterministic, picking the top token).
  - $T > 1$: Flattens the distribution, increasing output variety and creativity at the cost of coherence.
- **Top-$k$ Sampling**: Truncates the pool to the $k$ most probable tokens, zeroing out the rest. Prevents wild hallucinations but uses an inflexible cutoff regardless of distribution shape.
- **Top-$p$ (Nucleus) Sampling**: Dynamically selects the smallest set of tokens whose cumulative probability exceeds threshold $p$ (e.g., $p = 0.9$). Adapts candidates dynamically based on model confidence.

### Engineering Best Practices
- **Deterministic Tasks (Code, JSON extraction, Math)**: Set $T = 0$ or low temperature ($T \le 0.2$) with narrow top-$p$.
- **Creative Tasks (Brainstorming, Copywriting)**: Use $T = 0.7$–$0.9$ with top-$p = 0.9$–$0.95$.
- Pinned random seeds do not guarantee exact determinism across differing GPU hardware or batching configurations.

---

## What is hallucination in LLMs, and why does it occur?

Hallucination occurs when an LLM generates fluent, confident assertions that are factually false, ungrounded, or completely fabricated.

### Root Causes
- **Objective Mismatch**: Next-token prediction optimizes for statistical *plausibility*, not empirical *truth*. The model has no intrinsic mechanism to verify facts against reality.
- **Knowledge Cutoffs & Gaps**: When prompted on unrepresented topics or events past the training cutoff, the model interpolates plausible completions rather than signaling uncertainty.
- **Error Compounding**: During autoregressive decoding, once an incorrect token is generated, it becomes part of the conditioned context for all subsequent steps, cascading into elaborated false narratives.

### Mitigation Strategies
- **Grounding with RAG**: Restrict the model to answer exclusively using retrieved reference documents.
- **Prompt Engineering**: Explicitly instruct the model to state *"I don't know"* when confidence is low.
- **Decoding Controls**: Lower temperature to suppress high-variance tail tokens.
- **Post-hoc Verification**: Implement secondary model review passes or citation verification.

---

## What is Retrieval-Augmented Generation (RAG), and how does it mitigate hallucination and knowledge cutoff?

Retrieval-Augmented Generation (RAG) grounds language model responses in verifiable external knowledge by dynamically retrieving relevant document passages at query time.

### The 3-Stage Pipeline
1. **Ingestion & Indexing**: Documents are chunked, converted to dense vector embeddings, and indexed in an Approximate Nearest Neighbor (ANN) vector database.
2. **Retrieval & Reranking**: The user query is embedded; the vector DB returns top-$k$ candidates (via cosine similarity). A **cross-encoder reranker** re-scores these candidates for fine-grained semantic relevance.
3. **Generation**: The top retrieved chunks are injected into the prompt context with instructions to answer based strictly on the provided evidence.

### Benefits & Production Challenges
- **Dynamic Updates**: Knowledge bases update immediately without expensive model retraining, solving knowledge cutoff.
- **Verifiable Citations**: Enables traceable footnote citations back to source documents.
- **Engineering Hurdles**: Chunk size tuning (small chunks lose context; large chunks dilute relevance), retrieval latency, and the risk that irrelevant chunks mislead the model.

---

## What is in-context learning, and how does it differ from fine-tuning?

In-context learning is the emergent capability of an LLM to perform new tasks conditioned purely on examples and instructions provided within the prompt context—with **zero gradient updates to model weights**.

### Prompting Modes
- **Zero-Shot**: Instructions only, with no demonstration examples.
- **One-Shot**: Single demonstration example followed by the prompt.
- **Few-Shot**: Multiple input-output demonstrations establishing patterns, output schemas, or reasoning steps (Chain-of-Thought).

### In-Context Learning vs. Fine-Tuning
- **Infrastructure**: In-context learning requires zero training pipelines or GPU training clusters; it runs entirely within inference.
- **Durability**: In-context learning is strictly transient—context disappears once the request completes, consuming context tokens on every call.
- **Best Practice Strategy**:
  1. Prototype immediately with **In-Context Learning**.
  2. Add **RAG** when the model lacks factual knowledge.
  3. Escalate to **Fine-Tuning (LoRA)** only when a durable style, schema, or behavioral transformation is needed across high-volume production traffic.
