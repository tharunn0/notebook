# Neural Network Training Mechanics: Key Concepts & Mechanics

A foundational guide covering how neural networks learn: backpropagation, gradient descent and its variants, loss functions, and the practical mechanics of avoiding over/underfitting.

---

## Index

1. [What is backpropagation, and how does the chain rule apply across layers?](#what-is-backpropagation-and-how-does-the-chain-rule-apply-across-layers)
2. [What is gradient descent, and how do variants like SGD, Momentum, and Adam differ?](#what-is-gradient-descent-and-how-do-variants-like-sgd-momentum-and-adam-differ)
3. [What is a loss function, and how does the choice differ for classification vs. regression?](#what-is-a-loss-function-and-how-does-the-choice-differ-for-classification-vs-regression)
4. [What are overfitting and underfitting, and how do regularization, dropout, and early stopping address them?](#what-are-overfitting-and-underfitting-and-how-do-regularization-dropout-and-early-stopping-address-them)
5. [What is the difference between a batch, an epoch, and an iteration?](#what-is-the-difference-between-a-batch-an-epoch-and-an-iteration)

---

## What is backpropagation, and how does the chain rule apply across layers?

Backpropagation is the reverse-mode automatic differentiation algorithm that computes the exact partial derivatives of a scalar loss function with respect to every weight and bias across a neural network.

It does not update weights on its own; it provides gradients to an optimizer (like SGD or Adam) to execute parameter updates.

### Execution Mechanics
1. **Forward Pass**: The network computes and caches intermediate activations $\mathbf{z}^{(l)}$ and $\mathbf{a}^{(l)}$ layer-by-layer, producing a final scalar loss $\mathcal{L}$.
2. **Backward Pass (Chain Rule)**: Traverses backward through the computational graph:
   $$\frac{\partial \mathcal{L}}{\partial \mathbf{W}^{(l)}} = \frac{\partial \mathcal{L}}{\partial \mathbf{a}^{(l)}} \cdot \frac{\partial \mathbf{a}^{(l)}}{\partial \mathbf{z}^{(l)}} \cdot \frac{\partial \mathbf{z}^{(l)}}{\partial \mathbf{W}^{(l)}}$$
   Each layer needs only its cached activations and the upstream gradient from the adjacent layer, scaling computational complexity to roughly one extra forward pass.

### Production Mitigations
- **Vanishing/Exploding Gradients**: Deep layers compounding small derivatives (Sigmoid/Tanh) decay gradients to zero. Mitigated with non-saturating activations (ReLU, GELU), residual connections (ResNet skip connections), and layer normalization (RMSNorm).
- **Exploding Updates**: Controlled using **gradient clipping** to enforce a maximum $L_2$ gradient norm ceiling.

---

## What is gradient descent, and how do variants like SGD, Momentum, and Adam differ?

Gradient descent updates parameters iteratively in the opposite direction of the gradient of the loss function, scaled by a learning rate $\eta$:
$$\theta_{t+1} = \theta_t - \eta \nabla_\theta \mathcal{L}(\theta_t)$$

### Optimizer Variants
- **Batch Gradient Descent**: Computes gradients across the entire training dataset per step; accurate but prohibitively slow and memory-intensive for large datasets.
- **Stochastic Gradient Descent (SGD)**: Computes gradients over a small mini-batch. Injects stochastic noise that helps escape saddle points and shallow local minima.
- **SGD with Momentum**: Maintains an exponentially decaying moving average of past gradients (velocity vector $\mathbf{v}_t = \beta \mathbf{v}_{t-1} + (1-\beta)\mathbf{g}_t$), dampening high-frequency oscillations across steep loss ravines and accelerating descent.
- **Adam (Adaptive Moment Estimation)**: Tracks both the first moment (mean gradient) and second moment (uncentered gradient variance) per parameter. Dynamically scales learning rates per parameter, converging rapidly with minimal manual tuning.
- **AdamW**: Standard for modern LLMs; decouples $L_2$ weight decay regularization from gradient moment updates to prevent regularization distortion.

### Production Trade-offs
- Adam/AdamW requires keeping two additional state buffers per weight in VRAM (doubling memory requirements).
- In massive LLM training, memory is saved using **8-bit Adam** or **Adafactor**. Always pair optimizers with learning rate warmups and cosine decay schedules.

---

## What is a loss function, and how does the choice differ for classification vs. regression?

A loss function $\mathcal{L}(y, \hat{y})$ quantifies the discrepancy between model predictions $\hat{y}$ and ground truth $y$, defining the optimization landscape for gradient descent.

### Regression Loss Functions (Continuous Targets)
- **Mean Squared Error (MSE / $L_2$)**:
  $$\text{MSE} = \frac{1}{N} \sum_{i=1}^N (y_i - \hat{y}_i)^2$$
  Penalizes larger errors quadratically; sensitive to outliers, with smooth gradients.
- **Mean Absolute Error (MAE / $L_1$)**: Penalizes errors linearly; robust to outliers, but has a non-smooth gradient at zero.
- **Huber Loss**: Smooth hybrid behaving like MSE for small errors and MAE for large outliers.

### Classification Loss Functions (Categorical Distributions)
- **Cross-Entropy Loss (Log Loss)**:
  $$\mathcal{L}_{\text{CE}} = -\sum_{i=1}^C y_i \log(\hat{y}_i)$$
  Measures divergence between predicted softmax probabilities and one-hot true labels. Keeps gradient signals strong even when predictions are confidently incorrect.
- **Focal Loss**: Down-weights easy, well-classified examples to prevent dominant background classes from overwhelming minority classes in imbalanced detection tasks.

---

## What are overfitting and underfitting, and how do regularization, dropout, and early stopping address them?

Overfitting and underfitting represent the fundamental bias-variance trade-off in machine learning:

- **Overfitting (High Variance)**: The model memorizes training noise rather than generalizable structure. Training error continues falling while validation error diverges upward.
- **Underfitting (High Bias)**: The model lacks representational capacity or training duration to capture underlying patterns. Both training and validation error remain high.

### Regularization Techniques
- **$L_2$ Regularization (Weight Decay)**: Adds a penalty proportional to the sum of squared weights ($\frac{1}{2} \lambda \|\mathbf{W}\|^2$), penalizing excessively large weights and encouraging smooth decision boundaries.
- **$L_1$ Regularization (Lasso)**: Adds a penalty proportional to absolute weights ($\lambda \|\mathbf{W}\|_1$), driving uninformative parameters to exactly zero to produce sparse representations.
- **Dropout**: Randomly zeroes out a percentage of neuron activations during training forward passes. Prevents co-adaptation of features and simulates training an ensemble of sub-networks. (Disabled during inference).
- **Early Stopping**: Continuously monitors validation loss and checkpoints weights, halting training when validation performance stagnates to prevent over-optimization.

---

## What is the difference between a batch, an epoch, and an iteration?

These three metrics define the execution cadence of neural network training:

- **Batch**: A discrete subset of training examples processed simultaneously in one forward and backward pass.
- **Iteration (Step)**: One full parameter update cycle (forward pass $\to$ loss computation $\to$ backward pass $\to$ optimizer step) over a single batch.
- **Epoch**: One complete pass through the entire training dataset.

$$\text{Iterations per Epoch} = \frac{\text{Total Dataset Size}}{\text{Batch Size}}$$

### Production Dynamics
- **Hardware Tiling**: Mini-batch sizes are configured in powers of 2 (e.g., 32, 64, 128) to maximize tensor core hardware utilization on GPUs/TPUs.
- **Linear Scaling Rule**: Larger batch sizes reduce gradient variance, requiring proportional increases in learning rate alongside warmup schedules.
- **Gradient Accumulation**: When accelerator VRAM cannot accommodate the desired effective batch size, systems compute and accumulate gradients across multiple smaller *micro-batches* before executing a single optimizer update step.
