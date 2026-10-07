# 🧠 Block 1.3 — Neural Network Architecture

![Neural Network Architecture](https://upload.wikimedia.org/wikipedia/commons/4/46/Colored_neural_network.svg)

## 1. Neural Network Architecture

A neural network is organized into **layers**, where each layer transforms information and passes it to the next layer.

Basic structure:

```text
Input Layer → Hidden Layer(s) → Output Layer
```

Example:

```text
Input
  ↓
Hidden Layer 1
  ↓
Hidden Layer 2
  ↓
Output
```

The network learns the parameters of these transformations during training.

---

# 2. Input Layer 🔥🔥🔥

The **input layer** receives the features of the input data.

For example, suppose we want to predict whether a student passes based on:

```text
Study Hours
Attendance
Previous Marks
```

Then:

```text
Input = [study_hours, attendance, previous_marks]
```

The input layer therefore has **3 input features**.

### Important

The input layer:

- receives the input data
- represents the input features
- usually does not contain trainable parameters by itself
- is generally not counted as a trainable computational layer

---

# 3. Hidden Layer 🔥🔥🔥

A **hidden layer** is an intermediate computational layer between the input and output.

```text
Input → Hidden Layer → Output
```

Hidden layers learn useful representations from the input.

Conceptually:

```text
Raw Input
    ↓
Intermediate Representation
    ↓
Higher-Level Representation
    ↓
Prediction
```

The values inside hidden layers are not directly observed in the original dataset, which is why they are called **hidden layers**.

---

# 4. Output Layer 🔥🔥🔥

The **output layer** produces the final prediction.

Its structure depends on the problem.

### Binary Classification

Example:

```text
Spam vs Not Spam
Cat vs Dog
Pass vs Fail
```

Often:

```text
Output Neuron
      ↓
   Sigmoid
      ↓
 Probability
```

Example:

\[
P(y=1)=0.87
\]

---

### Multiclass Classification

Example:

```text
Cat
Dog
Horse
```

Usually:

```text
3 Output Neurons
       ↓
    Softmax
       ↓
[0.1, 0.7, 0.2]
```

The probabilities sum to 1.

---

### Regression

Example:

```text
House Price Prediction
```

Often:

```text
Output Neuron
      ↓
Predicted Value
```

Example:

\[
\hat y = ₹52,00,000
\]

---

# 5. Single-Layer Network 🔥🔥🔥

A **single-layer network** generally has one trainable computational layer.

```text
Input → Output
```

Example:

```text
x₁ ──┐
x₂ ──┼──→ Output
x₃ ──┘
```

There is **no hidden layer**.

Mathematically:

\[
z = Wx+b
\]

\[
y=f(z)
\]

A single linear neuron can only create a **linear decision boundary**.

Therefore, a single-layer perceptron cannot solve problems such as **XOR**.

---

# 6. Multi-Layer Network 🔥🔥🔥

A multi-layer network contains one or more hidden layers.

```text
Input
  ↓
Hidden Layer 1
  ↓
Hidden Layer 2
  ↓
Output
```

Example:

```text
10 inputs
    ↓
64 neurons
    ↓
32 neurons
    ↓
3 outputs
```

Multiple layers allow the network to perform a sequence of transformations and learn increasingly complex representations.

---

# 7. MLP — Multi-Layer Perceptron 🔥🔥🔥

**MLP = Multi-Layer Perceptron**

An MLP is a **feed-forward neural network** primarily built using fully connected/dense layers, usually with nonlinear activation functions.

Typical structure:

```text
Input
  ↓
Dense + Activation
  ↓
Dense + Activation
  ↓
Output Layer
  ↓
Prediction
```

Example:

```text
10 inputs
    ↓
64 neurons
    ↓
32 neurons
    ↓
3 outputs
```

MLPs are commonly used for:

- tabular data
- classification
- regression
- feature-based prediction

---

# 8. Fully Connected / Dense Layer 🔥🔥🔥

![Fully Connected Neural Network](https://upload.wikimedia.org/wikipedia/commons/9/99/Neural_network_example.svg)

A **fully connected layer** is a layer where **every neuron in one layer is connected to every neuron in the next layer**.

Example:

```text
Input                 Hidden

 x₁ ─────────────────→ h₁
  ├──────────────────→ h₂
  └──────────────────→ h₃

 x₂ ─────────────────→ h₁
  ├──────────────────→ h₂
  └──────────────────→ h₃

 x₃ ─────────────────→ h₁
  ├──────────────────→ h₂
  └──────────────────→ h₃
```

Every input is connected to every hidden neuron.

### Dense Layer Equation

\[
z=Wx+b
\]

Then:

\[
a=f(z)
\]

where:

| Symbol | Meaning |
|---|---|
| \(x\) | Input vector |
| \(W\) | Weight matrix |
| \(b\) | Bias vector |
| \(z\) | Pre-activation |
| \(f\) | Activation function |
| \(a\) | Output/activation |

---

# 9. Number of Parameters 🔥🔥🔥

For a fully connected layer:

\[
\boxed{\text{Parameters}=n_{in}\times n_{out}+n_{out}}
\]

Why?

### Weights

\[
n_{in}\times n_{out}
\]

### Biases

\[
n_{out}
\]

Therefore:

\[
\boxed{n_{in}n_{out}+n_{out}}
\]

### Example

Suppose:

```text
Input = 10 neurons
Output = 64 neurons
```

Weights:

\[
10\times64=640
\]

Biases:

\[
64
\]

Total:

\[
640+64=\boxed{704}
\]

---

# 10. Network Width 🔥🔥

**Width = number of neurons/units in a layer.**

Example:

```text
Input
  ↓
128 neurons
  ↓
64 neurons
  ↓
32 neurons
  ↓
Output
```

The hidden-layer widths are:

```text
128
64
32
```

So we can say:

> The first hidden layer has a width of 128.

### Easy Memory

```text
WIDTH = neurons across a layer
```

---

# 11. Network Depth 🔥🔥

**Depth describes how many successive computational layers a network contains.**

Example:

```text
Input
  ↓
Layer 1
  ↓
Layer 2
  ↓
Layer 3
  ↓
Output
```

Conceptually:

\[
x
\rightarrow f_1(x)
\rightarrow f_2(f_1(x))
\rightarrow f_3(f_2(f_1(x)))
\]

Each layer transforms the representation produced by the previous layer.

### Important

Different sources sometimes use different conventions for whether the output layer is included when counting depth.

For practical understanding:

> **Depth = number of successive computational layers.**

---

# 12. Width vs Depth 🔥🔥🔥

| Concept | Meaning |
|---|---|
| **Width** | Number of neurons in a layer |
| **Depth** | Number of successive layers |
| Wider network | More neurons per layer |
| Deeper network | More layers |

Example:

```text
Input
  ↓
128 neurons   ← Width
  ↓
64 neurons    ← Width
  ↓
32 neurons    ← Width
  ↓
Output

3 hidden layers → Depth
```

---

# 13. Parameters 🔥🔥🔥

**Parameters are values learned by the neural network during training.**

The major trainable parameters are:

- Weights
- Biases

Example:

\[
z=w_1x_1+w_2x_2+b
\]

Here:

\[
w_1,\ w_2,\ b
\]

are trainable parameters.

During training:

```text
Input
  ↓
Forward Propagation
  ↓
Prediction
  ↓
Loss
  ↓
Backpropagation
  ↓
Update Weights & Biases
```

The network learns these values from the training data.

---

# 14. Hyperparameters 🔥🔥🔥

**Hyperparameters are configuration choices made by the engineer rather than learned directly by the model.**

Examples:

- Learning rate
- Batch size
- Number of epochs
- Number of hidden layers
- Number of neurons
- Optimizer
- Dropout rate
- Weight decay

Example:

```text
Learning rate = 0.001
Batch size = 32
Hidden layers = 3
Hidden units = [128, 64, 32]
Optimizer = Adam
```

These are configured before/during training.

---

# 15. Parameters vs Hyperparameters 🔥🔥🔥

| Parameters | Hyperparameters |
|---|---|
| Learned by the model | Configured by us |
| Weights | Learning rate |
| Biases | Batch size |
| Updated during training | Number of layers |
| Learned from data | Number of neurons |
| Model learns them | Optimizer choice |

### 🧠 Easy Memory Trick

> **Parameters = Learned**

> **Hyperparameters = Configured**

---

# 16. Complete Architecture Example 🔥🔥🔥

Consider:

```text
Input: 10
   ↓
Hidden Layer 1: 64
   ↓
Hidden Layer 2: 32
   ↓
Output: 3
```

Architecture:

```text
10 → 64 → 32 → 3
```

### Layer 1

\[
10\times64+64=704
\]

### Layer 2

\[
64\times32+32=2080
\]

### Output Layer

\[
32\times3+3=99
\]

### Total Parameters

\[
704+2080+99
\]

\[
\boxed{2883}
\]

So this network has:

\[
\boxed{2883\text{ trainable parameters}}
\]

---

# 17. Why Nonlinear Activation Matters 🔥🔥🔥

This connects directly with the previous topic.

Suppose we stack only linear transformations:

\[
W_3W_2W_1x
\]

The complete network can still be represented as a single linear transformation.

So:

```text
Linear
   ↓
Linear
   ↓
Linear
```

does **not** give the network the ability to model arbitrary nonlinear relationships.

Instead:

```text
Linear
  ↓
ReLU
  ↓
Linear
  ↓
ReLU
  ↓
Linear
```

allows the network to model nonlinear relationships.

### Key idea

> **Depth + nonlinear activation = powerful representation learning**

---

# 18. Hierarchical Representation Learning 🔥🔥🔥

One of the most important ideas in deep learning.

A deep network can learn representations progressively.

For an image, conceptually:

```text
Pixels
  ↓
Edges
  ↓
Shapes
  ↓
Parts
  ↓
Objects
  ↓
Class
```

For example:

```text
Pixels
   ↓
Edges
   ↓
Eyes / Nose / Ears
   ↓
Face
   ↓
Person
```

The exact representations aren't manually programmed.

The network **learns useful representations from data**.

---

# 19. Architecture as a Design 🔥🔥

A neural network architecture specifies things such as:

```text
What goes in?
      ↓
How many layers?
      ↓
How many neurons?
      ↓
What transformations?
      ↓
What activation functions?
      ↓
What comes out?
```

Example:

```text
Customer Features
       ↓
Dense(128)
       ↓
ReLU
       ↓
Dense(64)
       ↓
ReLU
       ↓
Dense(1)
       ↓
Sigmoid
       ↓
Churn Probability
```

This is the kind of architecture you will later implement using frameworks such as **PyTorch** and **TensorFlow/Keras**.

---

# 🎯 Common Interview Questions

### 1. What is a hidden layer?

A computational layer between the input and output layers that learns intermediate representations.

### 2. What is a fully connected layer?

A layer where every neuron is connected to every neuron in the previous layer.

### 3. What is the parameter formula for a dense layer?

\[
\boxed{n_{in}n_{out}+n_{out}}
\]

### 4. What is network width?

The number of neurons/units in a layer.

### 5. What is network depth?

The number of successive computational layers.

### 6. What are model parameters?

Learned values, primarily weights and biases.

### 7. What are hyperparameters?

Configuration choices such as learning rate, batch size, number of layers, etc.

### 8. What is an MLP?

A feed-forward neural network primarily composed of fully connected/dense layers, generally with nonlinear activations.

### 9. Why are nonlinear activations needed?

Without nonlinear activations, stacking linear layers is still equivalent to a linear transformation.

### 10. Can a single perceptron solve XOR?

**No.** XOR is not linearly separable.

---

# 🧠 Final Cheat Sheet

```text
                 NEURAL NETWORK
                       │
       ┌───────────────┼───────────────┐
       ↓               ↓               ↓
    Input           Hidden           Output
    Layer           Layers            Layer
                      │
                      ↓
              Learn Representations
                      │
                      ↓
                 Prediction
```

### Architecture

```text
Single Layer:

Input → Output
```

```text
Multi Layer:

Input → Hidden → Output
```

```text
Deep Network:

Input → H1 → H2 → H3 → ... → Output
```

### Core equations

\[
\boxed{z=Wx+b}
\]

\[
\boxed{a=f(z)}
\]

### Dense-layer parameters

\[
\boxed{\text{Parameters}=n_{in}n_{out}+n_{out}}
\]

### Remember 🧠

```text
Width       → Neurons per layer
Depth       → Number of layers
Parameters  → Learned
Hyperparams → Configured
Dense       → Every neuron connected
Activation  → Introduces nonlinearity
MLP         → Feed-forward + mainly dense layers
```

### 🔥 The Big Picture

```text
                NEURAL NETWORK
                      │
                      ↓
                Architecture
                      │
          ┌───────────┼───────────┐
          ↓           ↓           ↓
        Input       Hidden      Output
        Layer       Layers       Layer
                      │
                ┌─────┴─────┐
                ↓           ↓
              Width       Depth
                │
                ↓
          Dense Layers
                │
                ↓
        Weights + Biases
                │
                ↓
       Nonlinear Activation
                │
                ↓
    Complex Representation Learning
                │
                ↓
            Prediction
```

## ⚡ One-Minute Revision

> A neural network consists of **input, hidden, and output layers**.  
> A **single-layer network** has no hidden layer, while a **multi-layer network** has one or more hidden layers.  
> An **MLP** is a feed-forward network mainly composed of fully connected layers.  
> In a **fully connected layer**, every neuron connects to every neuron in the previous layer.  
> **Width** means neurons per layer, while **depth** means number of computational layers.  
> **Weights and biases are parameters** learned during training.  
> **Learning rate, batch size, layer count, and neuron count are hyperparameters** configured by us.  
> Nonlinear activation functions allow multiple layers to learn **complex nonlinear relationships**.

---

### 📌 Markdown image format to use in your notes