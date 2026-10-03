# Artificial Intelligence & Neural Networks: Key Concepts & Mechanics

A foundational guide covering core AI concepts, biological and artificial neuron models, and the architectural mechanics of neural networks.

---

## Index

1. [What is AI?](#what-is-ai)
2. [What is a neuron?](#what-is-a-neuron)
3. [What is a neural network?](#what-is-a-neural-network)

---

## What is AI?

Artificial Intelligence (AI) is the field of computer science focused on building computational systems that perform tasks requiring human-like cognition—such as perception, reasoning, decision-making, and language understanding—by learning statistical patterns from data or executing knowledge-based rules.

### Core Paradigms
- **Machine Learning (Statistical AI)**: Models are parameterized mathematical functions. "Learning" involves minimizing a loss function over training data via gradient descent and backpropagation, iteratively updating weights.
- **Symbolic AI (Rule-Based)**: Encodes expert knowledge directly via logic trees, state machines, and search algorithms (e.g., A* pathfinding, classical chess engines) without requiring training datasets.
- **Modern Production Hybrid**: Statistical models handle perception and generative synthesis, while deterministic guardrails, search algorithms, and verification logic constrain output safety and correctness.

### Production Trade-offs
- **Generalization vs. Overfitting**: Over-parameterized models risk memorizing training data. Practitioners enforce generalization using regularization techniques (dropout, weight decay), held-out validation sets, and cross-validation.
- **Operational Metrics**: Systems are evaluated beyond benchmark accuracy—prioritizing p99 inference latency, memory footprint, robustness under distribution shifts, and failure mode containment.

---

## What is a neuron?

In artificial neural networks, a neuron (or unit) is the fundamental computational building block. It receives an input vector, computes a weighted sum plus a bias, and passes the result through a non-linear activation function to produce a scalar output.

Mathematically, a single neuron computes:
$$\text{output} = \sigma\left(\mathbf{w}^T \mathbf{x} + b\right) = \sigma\left(\sum_{i=1}^n w_i x_i + b\right)$$

Where:
- $\mathbf{x}$: Input feature vector.
- $\mathbf{w}$: Learned weight vector reflecting feature importance.
- $b$: Learned scalar bias shifting the activation threshold.
- $\sigma$: Non-linear activation function (e.g., ReLU, GELU, Sigmoid).

### Why Non-Linear Activation Matters
Without non-linear activations ($\sigma$), stacking multiple layers collapses algebraically into a single linear transformation ($\mathbf{W}_2(\mathbf{W}_1 \mathbf{x}) = \mathbf{W}_{net} \mathbf{x}$), making it impossible to learn complex non-linear decision boundaries.

### Activation Mechanics & Production Trade-offs
- **Sigmoid / Tanh**: Saturates at extreme values, causing vanishing gradients that stall backpropagation in deep networks.
- **ReLU ($\max(0, x)$)**: Solves vanishing gradients for positive inputs, but can suffer from "dying ReLU" if neurons become permanently inactive.
- **GELU / Swish**: Smooth, non-monotonic activations that provide continuous gradients; now the standard in modern Transformer architectures.

---

## What is a neural network?

A neural network is a directed computational graph of interconnected neurons organized into an input layer, one or more hidden layers, and an output layer, parameterized to approximate complex non-linear functions.

### Execution Mechanics (Forward & Backward Pass)
1. **Forward Propagation**:
   - Each layer computes a matrix multiplication of incoming activations with its weight matrix: $\mathbf{z}^{(l)} = \mathbf{W}^{(l)} \mathbf{a}^{(l-1)} + \mathbf{b}^{(l)}$.
   - Applies an element-wise activation function: $\mathbf{a}^{(l)} = \sigma(\mathbf{z}^{(l)})$.
   - The final output layer computes predictions (e.g., probability distributions via Softmax for classification, or continuous vectors for regression).
2. **Backpropagation**:
   - Computes prediction error using a loss function $\mathcal{L}$ (e.g., Cross-Entropy, MSE).
   - Traverses backward through the graph applying the multivariable calculus chain rule:
     $$\frac{\partial \mathcal{L}}{\partial \mathbf{W}^{(l)}} = \frac{\partial \mathcal{L}}{\partial \mathbf{z}^{(l)}} \cdot (\mathbf{a}^{(l-1)})^T$$
   - Optimizers (SGD with Momentum, AdamW) update weights using calculated gradient vectors across iterative mini-batches.

### Architectures & Production Challenges
- **Dense / Fully-Connected**: Base architecture; every neuron connects to every neuron in adjacent layers.
- **CNNs**: Use weight-sharing and convolutional filters to exploit spatial locality in image data.
- **Transformers**: Replace static recurrence with multi-head self-attention mechanisms to capture arbitrary long-range token relationships.
- **Production Engineering**: Deep stacks combat vanishing/exploding gradients using residual (skip) connections ($\mathbf{x} + F(\mathbf{x})$) and layer normalization (LayerNorm / RMSNorm).
