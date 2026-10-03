# Neural Network Architectures: Key Concepts & Mechanics

A foundational guide covering the major neural network architectures—convolutional, recurrent, and attention-based—and the structural biases that make each suited to its domain.

---

## Index

1. [What is a Convolutional Neural Network (CNN), and why does it suit image data?](#what-is-a-convolutional-neural-network-cnn-and-why-does-it-suit-image-data)
2. [What is a Recurrent Neural Network (RNN), and what problem do LSTMs/GRUs solve for it?](#what-is-a-recurrent-neural-network-rnn-and-what-problem-do-lstmsgselected-solve-for-it)
3. [What is the attention mechanism, and how does self-attention differ from RNN-style sequence modeling?](#what-is-the-attention-mechanism-and-how-does-self-attention-differ-from-rnn-style-sequence-modeling)
4. [What is a Transformer architecture, and what are its core components?](#what-is-a-transformer-architecture-and-what-are-its-core-components)

---

## What is a Convolutional Neural Network (CNN), and why does it suit image data?

A Convolutional Neural Network (CNN) is a neural architecture designed for spatial grid data (like images) that replaces dense matrix multiplications with small, shared weight filters (kernels) that slide across the input.

It exploits two foundational visual priors: **spatial locality** (pixels close together are correlated) and **translation equivariance** (a feature detector like an edge or texture should activate regardless of where it appears in the frame).

### Core Mechanics
- **Convolution Operations**: Applies small parameterized kernels (e.g., $3 \times 3$ or $5 \times 5$) via sliding dot products across the input, producing 2D activation **feature maps**.
- **Receptive Field Expansion**: Stacking multiple convolutional layers expands the effective receptive field: shallow layers extract edges and corners; deeper layers compose them into complex shapes and object classes.
- **Pooling (Subsampling)**: Max or average pooling reduces spatial dimensions, cutting parameter counts and providing local translation invariance.
- **Weight Sharing**: Because the same filter kernel is applied across the entire image plane, CNNs require orders of magnitude fewer parameters than fully connected layers, drastically reducing sample complexity.

### Production Trade-offs
- **Modern Refinements**: Residual connections (ResNet) skip layers to prevent vanishing gradients in deep networks; depthwise-separable convolutions (MobileNet) factor standard convolutions to run efficiently on mobile/edge devices.
- **Limitations**: The strict locality prior limits capturing long-range global dependencies. Vision Transformers (ViTs) replace local convolutions with patch-based self-attention, outperforming CNNs when massive pre-training datasets are available.

---

## What is a Recurrent Neural Network (RNN), and what problem do LSTMs/GRUs solve for it?

A Recurrent Neural Network (RNN) processes sequential data step-by-step by maintaining an internal hidden state vector $h_t$ passed sequentially across time steps to model temporal context.

### Vanilla RNN Mechanics & Limitations
At step $t$, a vanilla RNN computes:
$$h_t = \sigma(W_h h_{t-1} + W_x x_t + b)$$

- **Backpropagation Through Time (BPTT)**: Unrolls the recurrence across sequence length $T$. Gradients are propagated backward through repeated multiplications by the same transition matrix $W_h$.
- **The Exploding / Vanishing Gradient Trap**: Repeated matrix multiplications cause gradients to either decay exponentially toward zero or explode toward infinity, restricting vanilla RNNs to short dependencies ($\le 10$–$15$ steps).

### How LSTMs and GRUs Solve Vanishing Gradients
- **LSTM (Long Short-Term Memory)**: Introduces a dedicated linear **cell state** $C_t$ alongside the hidden state $h_t$, governed by three multiplicative gates:
  - *Forget Gate*: Learns what percentage of past cell state to discard ($f_t = \sigma(\dots)$).
  - *Input Gate*: Regulates how much new candidate information to write to the cell state ($i_t = \sigma(\dots)$).
  - *Output Gate*: Decides what portion of the cell state to expose in the hidden state ($o_t = \sigma(\dots)$).
  - *Gradient Highway*: Because the cell state update is largely additive ($C_t = f_t \odot C_{t-1} + i_t \odot \tilde{C}_t$), gradients flow backward across long time horizons with minimal decay.
- **GRU (Gated Recurrent Unit)**: Simplifies the LSTM into two gates (Reset and Update) without a separate cell state, matching LSTM performance with fewer parameters and lower compute costs.
- **Modern Relevance**: Largely superseded by Transformers for bulk sequence tasks, but still used for streaming low-latency edge inference and real-time audio signal processing.

---

## What is the attention mechanism, and how does self-attention differ from RNN-style sequence modeling?

The attention mechanism computes dynamic, content-based relevance scores between all token representations in a sequence, replacing the single bottlenecked hidden state of an RNN with direct, single-hop access across all positions.

### Scaled Dot-Product Self-Attention
Each token representation is projected into Query ($Q$), Key ($K$), and Value ($V$) matrices via learned linear transformations:
$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

1. **Compatibility Scoring**: Computes the dot product between every Query vector and all Key vectors ($QK^T$).
2. **Scaling Factor ($\sqrt{d_k}$)**: Prevents dot products from growing excessively large in high dimensions, preventing softmax gradients from vanishing.
3. **Softmax Normalization**: Converts raw scores into probability distributions summing to 1.
4. **Value Aggregation**: Produces the output token representation as a weighted sum of all Value vectors.

### Self-Attention vs. Recurrent Models
- **Parallelization**: Self-attention processes all sequence tokens simultaneously via matrix multiplications, whereas RNNs must process tokens sequentially ($O(T)$ sequential steps).
- **Constant Path Length**: Any two tokens interact in exactly $O(1)$ operations regardless of distance, eliminating vanishing gradient bottlenecks across long sequences.
- **Quadratic Complexity ($O(T^2)$)**: Calculating attention across all token pairs scales quadratically with sequence length $T$. In production, this is optimized using exact tiled SRAM kernels (**FlashAttention**) or sub-quadratic approximations (sliding-window attention).

---

## What is a Transformer architecture, and what are its core components?

The Transformer is a non-recurrent neural architecture built entirely on multi-head self-attention and position-wise feed-forward networks, processing entire sequences in parallel.

### Core Architectural Components
1. **Multi-Head Attention (MHA)**: Runs $h$ independent attention projections in parallel subspaces, allowing tokens to simultaneously capture distinct relational contexts (e.g., syntax vs. semantic coreference).
2. **Positional Encoding**: Because attention operations are permutation-invariant, positional signals must be injected. Modern LLMs use relative encoding schemes like **Rotary Position Embeddings (RoPE)**.
3. **Position-Wise Feed-Forward Network (FFN)**: A two-layer MLP applied identically and independently to every token position, providing the bulk of the model's static memory capacity.
4. **Residual Connections & Normalization**: Each sub-layer is wrapped in skip connections ($\mathbf{x} + \text{SubLayer}(\mathbf{x})$) and normalized (RMSNorm or LayerNorm) to stabilize deep gradient flows.

### Encoder-Decoder vs. Decoder-Only
- **Encoder-Decoder (Original Transformer, T5)**: Bidirectional encoder feeds into an autoregressive cross-attending decoder; standard for translation and seq2seq tasks.
- **Decoder-Only (GPT, Llama, Claude)**: A single stack of causally-masked self-attention and FFN blocks trained via autoregressive next-token prediction. Dominates modern LLMs due to optimal pre-training compute scaling.

### Production Inference Regimes
- **Prefill Phase (Prompt Ingestion)**: Compute-bound; processes all prompt tokens in parallel to generate the initial KV cache.
- **Decode Phase (Token Generation)**: Memory-bandwidth bound; generates tokens sequentially, retrieving past Key and Value vectors from the **KV Cache**. Optimizations focus on KV cache compression, quantization (INT4/FP8), and speculative decoding.
