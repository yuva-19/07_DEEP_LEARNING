# 📚 Deep Learning — 1.2 Neural Network Foundations
## Complete Revision Notes

---

# 1. Biological Neuron 🔥🔥

A biological neuron receives signals, processes them and transmits a signal.

Simplified structure:

```text
Dendrites → Cell Body → Axon → Output
   ↑
Input signals
```

### Core idea

> **Multiple inputs → processing → output**

Artificial neural networks are inspired by this basic idea, but an artificial neuron is a mathematical model—not an exact simulation of a biological neuron.

---

# 2. Artificial Neuron 🔥🔥🔥

An artificial neuron receives inputs, applies weights, adds a bias, and produces an output.

```text
x₁ ──× w₁ ──┐
x₂ ──× w₂ ──┤
x₃ ──× w₃ ──┼──→ Σ + b ──→ f(.) ──→ y
             │
```

Mathematically:

\[
z=w_1x_1+w_2x_2+\cdots+w_nx_n+b
\]

Vector form:

\[
\boxed{z=\mathbf{w}^T\mathbf{x}+b}
\]

After an activation function:

\[
\boxed{y=f(z)}
\]

Therefore:

\[
\boxed{y=f(\mathbf{w}^T\mathbf{x}+b)}
\]

This is one of the most important equations in Deep Learning.

---

# 3. Inputs 🔥🔥🔥

Inputs represent the information provided to the neuron.

Example:

```text
x₁ = Hours studied
x₂ = Attendance
x₃ = Previous marks
```

In real DL systems, inputs can be:

- Pixel values
- Sensor readings
- Numerical features
- Embedding values
- Time-series values
- Outputs from previous neurons

### Important

Inputs come from the **data**.

They are generally **not learned parameters**.

---

# 4. Weights 🔥🔥🔥

A **weight** determines how strongly an input contributes to the neuron's calculation.

\[
z=w_1x_1+w_2x_2+\cdots+w_nx_n+b
\]

For example:

\[
w_1=5,\qquad w_2=0.2
\]

Assuming comparable input scales, \(x_1\) has a stronger influence on the weighted sum.

### Intuition

```text
Large positive weight → stronger positive contribution
Large negative weight → stronger negative contribution
Weight near zero      → weaker contribution
```

⚠️ "Weight = importance" is useful intuition, but technically weights are **learned parameters controlling the transformation of inputs**.

---

# 5. Bias 🔥🔥🔥

Bias is an additional learnable parameter:

\[
\boxed{z=\mathbf{w}^T\mathbf{x}+b}
\]

Its major role is to provide an **offset**.

Without bias:

\[
z=\mathbf{w}^T\mathbf{x}
\]

The decision boundary is constrained to pass through the origin.

With bias:

\[
z=\mathbf{w}^T\mathbf{x}+b
\]

the boundary can shift.

### Intuition

Compare:

\[
y=mx+c
\]

- \(m\) → affects orientation/slope
- \(c\) → shifts the line

Similarly:

- **Weights** → influence boundary orientation
- **Bias** → shifts the boundary

---

# 6. Linear Combination 🔥🔥🔥

The weighted sum plus bias is called a **linear combination**:

\[
\boxed{z=\sum_{i=1}^{n}w_ix_i+b}
\]

Example:

\[
x_1=2,\quad x_2=3
\]

\[
w_1=0.5,\quad w_2=2,\quad b=1
\]

Then:

\[
z=(0.5)(2)+(2)(3)+1
\]

\[
z=8
\]

So the neuron first calculates:

\[
\boxed{z=8}
\]

and then applies an activation function if required.

---

# 7. Parameters 🔥🔥🔥

**Parameters are values learned by the model during training.**

For:

\[
z=w_1x_1+w_2x_2+b
\]

the parameters are:

\[
\boxed{w_1,w_2,b}
\]

For \(n\) inputs:

\[
\boxed{\text{Parameters}=n+1}
\]

because:

- \(n\) weights
- 1 bias

### Example

A neuron has 100 inputs:

\[
100\text{ weights}+1\text{ bias}
\]

\[
\boxed{101\text{ parameters}}
\]

---

# 8. Inputs vs Parameters 🔥🔥🔥

| | Inputs | Parameters |
|---|---|---|
| Example | \(x_1,x_2,x_3\) | \(w_1,w_2,w_3,b\) |
| Source | Data | Model |
| Learned? | ❌ | ✅ |
| Updated during training? | Usually no | ✅ |
| Purpose | Provide information | Control transformation |

### Remember

> **Data provides the inputs; training learns the parameters.**

---

# 9. Perceptron 🔥🔥🔥

A **perceptron** is one of the earliest and simplest artificial neural-network models.

It performs:

\[
z=\mathbf{w}^T\mathbf{x}+b
\]

followed by a threshold/step function:

\[
\hat y=
\begin{cases}
1 & z\geq0\\
0 & z<0
\end{cases}
\]

Therefore:

```text
Inputs
   ↓
Weighted sum + bias
   ↓
Step function
   ↓
0 or 1
```

### Purpose

The basic perceptron performs **binary classification**.

Example:

```text
0 → Normal
1 → Fault
```

---

# 10. Perceptron Learning 🔥🔥

The perceptron learns its weights and bias from training data.

Conceptually:

```text
Training data
      ↓
Prediction
      ↓
Compare with target
      ↓
Calculate error
      ↓
Update parameters
      ↓
Repeat
```

Classical perceptron update:

\[
\boxed{
w_i\leftarrow w_i+\eta(y-\hat y)x_i
}
\]

\[
\boxed{
b\leftarrow b+\eta(y-\hat y)
}
\]

where:

- \(y\) = true label
- \(\hat y\) = prediction
- \(\eta\) = learning rate

### Important

Don't confuse this with modern neural-network training.

Modern deep networks primarily use:

\[
\boxed{\text{Gradient Descent + Backpropagation}}
\]

which you'll study later.

---

# 11. Output 🔥🔥🔥

For a basic binary perceptron:

\[
\hat y\in\{0,1\}
\]

The meaning depends on the problem.

Example:

| Output | Meaning |
|---:|---|
| 0 | Normal |
| 1 | Fault |

or:

| Output | Meaning |
|---:|---|
| 0 | Cat |
| 1 | Dog |

The numerical labels themselves don't inherently mean anything—the problem defines their meaning.

---

# 12. Decision Boundary 🔥🔥🔥

The perceptron separates the input space into different prediction regions.

The decision boundary occurs when:

\[
z=0
\]

Therefore:

\[
\boxed{\mathbf{w}^T\mathbf{x}+b=0}
\]

For two inputs:

\[
w_1x_1+w_2x_2+b=0
\]

This produces a **line**.

For three inputs:

\[
w_1x_1+w_2x_2+w_3x_3+b=0
\]

This produces a **plane**.

For higher dimensions:

\[
\boxed{\text{Hyperplane}}
\]

---

# 13. Role of Weights and Bias in the Decision Boundary 🔥🔥🔥

For:

\[
w_1x_1+w_2x_2+b=0
\]

### Weights

Control the **orientation** of the decision boundary.

### Bias

Controls the **position/offset** of the boundary.

Therefore:

> **Weights determine how the boundary is oriented; bias allows it to shift.**

---

# 14. Linear Separability 🔥🔥🔥

A dataset is **linearly separable** when a single linear decision boundary can separate its classes.

Example:

```text
● ● ●
● ●

──────────────

○ ○
○ ○ ○
```

A straight line can separate the classes.

Therefore, a perceptron can theoretically learn such a classification boundary.

---

# 15. XOR Limitation 🔥🔥🔥

XOR:

| \(x_1\) | \(x_2\) | Output |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

The classes cannot be separated using a single straight line.

Therefore:

\[
\boxed{\text{Single perceptron cannot solve XOR}}
\]

### Why?

Because XOR is **not linearly separable**.

This limitation motivated the use of **multiple neurons and layers**.

---

# 16. From Perceptron to Deep Neural Networks 🔥🔥🔥

The conceptual progression is:

```text
Biological neuron
       ↓
Artificial neuron
       ↓
Perceptron
       ↓
Multiple neurons
       ↓
Multiple layers
       ↓
Nonlinear transformations
       ↓
Complex decision boundaries
       ↓
Deep Neural Network
```

A single perceptron:

\[
\boxed{\text{Linear boundary}}
\]

Multiple layers with nonlinear activations:

\[
\boxed{\text{Complex nonlinear boundaries}}
\]

This is one of the most important conceptual bridges in Deep Learning.

---

# 🎯 Important Formulas

### Neuron

\[
\boxed{z=\mathbf{w}^T\mathbf{x}+b}
\]

### Expanded form

\[
\boxed{z=\sum_{i=1}^{n}w_ix_i+b}
\]

### Activated output

\[
\boxed{y=f(z)}
\]

### Complete neuron

\[
\boxed{y=f(\mathbf{w}^T\mathbf{x}+b)}
\]

### Perceptron

\[
\boxed{
\hat y=
\begin{cases}
1 & \mathbf{w}^T\mathbf{x}+b\geq0\\
0 & \mathbf{w}^T\mathbf{x}+b<0
\end{cases}}
\]

### Decision boundary

\[
\boxed{\mathbf{w}^T\mathbf{x}+b=0}
\]

### Parameters

For \(n\) inputs:

\[
\boxed{n+1}
\]

---

# ⚠️ Common Mistakes

### ❌ Weight and input are the same thing

No.

\[
x=\text{input/data}
\]

\[
w=\text{learned parameter}
\]

### ❌ Bias is an input feature

No.

Bias is a **learnable parameter** added to the weighted sum.

### ❌ Perceptron can solve every binary classification problem

No.

A single perceptron can only learn a **linear decision boundary**.

### ❌ XOR can be solved by one perceptron

No.

\[
\boxed{\text{XOR is not linearly separable}}
\]

### ❌ Deep learning starts with backpropagation

Conceptually, first understand:

\[
\text{Neuron → Perceptron → Layers → Activation → Loss → Gradient → Backpropagation}
\]

---

# ⚡ Final Cheat Sheet

```text
BIOLOGICAL NEURON
        ↓
ARTIFICIAL NEURON
        ↓
z = wᵀx + b
        ↓
ACTIVATION
        ↓
OUTPUT
```

### Remember:

- **Input** → information from data
- **Weight** → learned contribution of an input
- **Bias** → learned offset
- **Parameter** → learned weight/bias
- **Linear combination** → \(w^Tx+b\)
- **Perceptron** → simple binary classifier
- **Decision boundary** → \(w^Tx+b=0\)
- **Single perceptron** → linear boundary
- **XOR** → not linearly separable
- **Multiple layers + nonlinear activation** → complex decision boundaries

### ⭐ One-line mental model

> **A neural network learns parameters that transform inputs into useful representations and ultimately predictions.**