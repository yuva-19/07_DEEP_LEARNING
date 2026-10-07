# 2.1 Forward Propagation

![Forward Propagation Neural Network](https://upload.wikimedia.org/wikipedia/commons/4/46/Colored_neural_network.svg)

## 🔥 Core Concept

**Forward propagation** is the process of passing input data through a neural network from the input layer to the output layer to produce a prediction.

```text
Input
  ↓
Weighted Sum
  ↓
Add Bias
  ↓
Activation
  ↓
Next Layer
  ↓
Repeat
  ↓
Final Prediction
```

---

# 1. Weighted Sum 🔥🔥🔥

Each neuron receives inputs and multiplies each input by its corresponding weight.

For example:

```text
x₁ ──× w₁──┐
x₂ ──× w₂──┼──→ Sum
x₃ ──× w₃──┘
```

The weighted sum is:

$$
w_1x_1 + w_2x_2 + w_3x_3
$$

In vector form:

$$
W^T x
$$

### Why weights?

Weights determine how strongly each input influences the neuron.

Example:

$$
z = 0.8x_1 + 0.1x_2
$$

Here, $x_1$ has a greater influence because its weight is $0.8$.

The neural network learns these weights during training.

---

# 2. Bias 🔥🔥🔥

After calculating the weighted sum, the neuron adds a **bias**.

$$
z = W^T x + b
$$

Bias is a **learnable parameter** that provides an offset to the weighted sum.

### Example

Suppose:

$$
W^T x = 1.6
$$

and:

$$
b = 0.4
$$

Then:

$$
z = 1.6 + 0.4
$$

$$
z = 2
$$

The value $z$ is called the **pre-activation**.

### 🧠 Remember

```text
Weighted Sum → combines input contributions
Bias         → shifts the result
```

Both **weights and biases are learned during training**.

---

# 3. Activation Function 🔥🔥🔥

The pre-activation $z$ is passed through an activation function.

$$
a = f(z)
$$

Therefore, a complete neuron performs:

$$
a = f(W^T x + b)
$$

Where:

| Symbol | Meaning |
|---|---|
| $W$ | Weights |
| $x$ | Input |
| $b$ | Bias |
| $z$ | Pre-activation |
| $f$ | Activation function |
| $a$ | Activation / output |

### Example: ReLU

$$
ReLU(z) = \max(0,z)
$$

If:

$$
z = 3
$$

then:

$$
ReLU(3) = 3
$$

If:

$$
z = -2
$$

then:

$$
ReLU(-2) = 0
$$

### Why activation?

Nonlinear activation functions allow neural networks to learn **nonlinear relationships**.

Without nonlinear activation functions, stacking multiple linear layers would still behave like one linear transformation.

---

# 4. Complete Neuron Computation 🔥🔥🔥

A neuron performs the following steps:

```text
Inputs
  ↓
Multiply by weights
  ↓
Weighted Sum
  ↓
Add Bias
  ↓
Pre-activation (z)
  ↓
Activation Function
  ↓
Activation (a)
```

Mathematically:

$$
z = W^T x + b
$$

Then:

$$
a = f(z)
$$

Therefore:

$$
a = f(W^T x + b)
$$

This is one of the most important equations in neural networks.

---

# 5. Layer-by-Layer Computation 🔥🔥🔥

A neural network contains multiple layers.

```text
Input
  ↓
Hidden Layer 1
  ↓
Hidden Layer 2
  ↓
Output
```

Each layer performs its own transformation.

## Layer 1

$$
z^{[1]} = W^{[1]}x + b^{[1]}
$$

$$
a^{[1]} = f^{[1]}(z^{[1]})
$$

The output of Layer 1 becomes the input to Layer 2.

## Layer 2

$$
z^{[2]} = W^{[2]}a^{[1]} + b^{[2]}
$$

$$
a^{[2]} = f^{[2]}(z^{[2]})
$$

This continues until the output layer.

### General form

$$
z^{[l]} = W^{[l]}a^{[l-1]} + b^{[l]}
$$

$$
a^{[l]} = f^{[l]}(z^{[l]})
$$

For the first layer:

$$
a^{[0]} = x
$$

### 🧠 Main idea

> **The output of one layer becomes the input to the next layer.**

---

# 6. Forward Pass 🔥🔥🔥

A **forward pass** is one complete execution of forward propagation through the network.

Example:

```text
Input x
  ↓
W₁x + b₁
  ↓
Activation
  ↓
a₁
  ↓
W₂a₁ + b₂
  ↓
Activation
  ↓
a₂
  ↓
Output Layer
  ↓
Prediction ŷ
```

The information flows **from input → output**.

---

# 7. Numerical Example 🔥🔥🔥

Consider one neuron.

### Given

$$
x_1 = 2
$$

$$
x_2 = 3
$$

$$
w_1 = 0.5
$$

$$
w_2 = 0.2
$$

$$
b = 0.4
$$

### Step 1 — Weighted Sum

$$
w_1x_1 + w_2x_2
$$

$$
= (0.5)(2) + (0.2)(3)
$$

$$
= 1 + 0.6
$$

$$
= 1.6
$$

### Step 2 — Add Bias

$$
z = 1.6 + 0.4
$$

$$
z = 2
$$

### Step 3 — Activation

Using ReLU:

$$
a = ReLU(2)
$$

$$
a = 2
$$

Therefore:

```text
Inputs
  ↓
Weighted Sum = 1.6
  ↓
Add Bias = 2
  ↓
ReLU
  ↓
Output = 2
```

---

# 8. Computational Graph 🔥🔥🔥

A **computational graph** represents a mathematical computation as a sequence of connected operations.

Simple neuron:

```text
x₁ ──×w₁──┐
           │
x₂ ──×w₂──┼──→ Σ ──→ +b ──→ z ──→ Activation ──→ a
           │
x₃ ──×w₃──┘
```

For a neural network:

```text
Input
  ↓
Matrix Multiplication
  ↓
Add Bias
  ↓
Activation
  ↓
Matrix Multiplication
  ↓
Add Bias
  ↓
Activation
  ↓
Output
```

### Why is it important?

Computational graphs become especially important during **backpropagation**.

They allow the framework to track the operations involved in producing the output and calculate gradients efficiently.

---

# 9. Forward Propagation vs Backpropagation 🔥🔥🔥

These are two major parts of neural-network training.

### Forward Propagation

Information moves:

```text
Input → Output
```

It produces the prediction.

### Backpropagation

Gradient information moves backward:

```text
Output → Previous Layers
```

It calculates gradients that are used to update the parameters.

```text
             FORWARD
Input ───────────────────→ Prediction
                            ↓
                           Loss
                            ↓
             BACKWARD
Input ←─────────────────── Gradients
```

---

# 10. Forward Propagation During Training 🔥🔥🔥

A simplified training iteration:

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
Gradients
  ↓
Parameter Update
```

### Important distinction

> **Forward propagation produces the prediction.**

> **Backpropagation calculates gradients used to improve the model.**

---

# 11. Forward Propagation During Inference

Forward propagation is also used after training.

### Training

```text
Input
  ↓
Forward Pass
  ↓
Prediction
  ↓
Loss
  ↓
Backpropagation
  ↓
Parameter Update
```

### Inference

```text
New Input
  ↓
Forward Pass
  ↓
Prediction
```

During inference, the model does **not update its parameters**.

---

# 12. Important Equations 🔥🔥🔥

### Single neuron

$$
z = W^T x + b
$$

### Activation

$$
a = f(z)
$$

### Complete neuron

$$
a = f(W^T x + b)
$$

### Layer $l$

$$
z^{[l]} = W^{[l]}a^{[l-1]} + b^{[l]}
$$

$$
a^{[l]} = f^{[l]}(z^{[l]})
$$

with:

$$
a^{[0]} = x
$$

---

# 13. Common Mistakes ⚠️

### ❌ Weighted sum is the final output

Not necessarily.

The weighted sum plus bias gives:

$$
z = Wx+b
$$

Then the activation gives:

$$
a=f(z)
$$

---

### ❌ Bias is the same as a weight

No.

Both are trainable parameters, but bias acts as an independent offset.

---

### ❌ Forward propagation changes the weights

No.

Forward propagation **uses the current weights and biases** to calculate the prediction.

Parameter updates happen after gradient computation and optimization.

---

### ❌ Forward propagation happens only during training

No.

It also happens during **inference** when the trained model makes predictions.

---

# 🎯 Interview Questions

### What is forward propagation?

The process of passing input data through a neural network from input to output to generate a prediction.

### What is the basic neuron equation?

$$
z = Wx+b
$$

followed by:

$$
a=f(z)
$$

### What is pre-activation?

The value before applying the activation function:

$$
z = Wx+b
$$

### What is activation?

The output after applying the activation function:

$$
a=f(z)
$$

### What happens layer by layer?

The output of one layer becomes the input to the next layer.

### What is a forward pass?

One complete execution of forward propagation through the network.

### Why are computational graphs important?

They represent the sequence of operations and are used to efficiently calculate gradients during backpropagation.

---

# 🧠 Final Cheat Sheet

```text
              FORWARD PROPAGATION

Input x
  ↓
Weighted Sum
  ↓
z = Wx + b
  ↓
Activation
  ↓
a = f(z)
  ↓
Next Layer
  ↓
Repeat
  ↓
Final Prediction ŷ
```

### Remember

```text
Weighted Sum → combines inputs using weights

Bias → adds a learnable offset

Activation → transforms z and introduces nonlinearity

Forward Pass → complete input-to-output computation

Layer-by-layer → output of one layer becomes input to next

Computational Graph → represents the sequence of operations
```

## ⚡ One-Minute Revision

> **Forward propagation** takes an input and passes it through each layer of a neural network to produce a prediction.
>
> Each layer performs:
>
> $$z = Wx+b$$
>
> followed by:
>
> $$a=f(z)$$
>
> The output activation becomes the input to the next layer. This continues until the final layer produces the prediction.
>
> The entire sequence of operations can be represented using a **computational graph**, which becomes important for **backpropagation**.

---

# 📌 Block Status

### ✅ Covered

- Weighted sum
- Bias
- Activation
- Layer-by-layer computation
- Forward pass
- Computational graph

### ⏳ Remaining

**None — 2.1 Forward Propagation is complete.** ✅

### ➡️ Next Logical Topic

**2.2 Loss Functions** — measuring how far the model's prediction is from the actual target.