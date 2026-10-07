# 🧠 Deep Learning — Block 2.3 Notes
# Output Layers

> **Core idea:** The type of prediction determines the output layer, activation function, and commonly used loss function.

---

# 1. Output Layer — Big Picture

A neural network's final layer produces the model's prediction.

The correct output design depends on the task:

```text
Prediction Task
      ↓
Output Layer
      ↓
Activation
      ↓
Prediction
      ↓
Loss Function
```

There are four important cases:

| Task | What are we predicting? | Typical output |
|---|---|---|
| **Regression** | Continuous number | Linear output |
| **Binary classification** | One of 2 classes | 1 output + Sigmoid |
| **Multiclass classification** | Exactly 1 of many classes | K outputs + Softmax |
| **Multi-label classification** | Multiple classes can be true | K outputs + independent Sigmoids |

---

# 2. 🟢 Regression Output

## Definition

**Regression** predicts a continuous numerical value.

Examples:

```text
House price → ₹72.5 lakh
Temperature → 31.6°C
Motor speed → 742.5 RPM
Battery voltage → 48.7 V
```

The output isn't a class. It is a number.

---

## Output Architecture

For a single continuous target:

```text
Input
  ↓
Hidden Layers
  ↓
Output Neuron
  ↓
Continuous value
```

The final neuron computes:

$$
z = Wx+b
$$

For typical unconstrained regression:

$$
\boxed{\hat{y}=z}
$$

This is called a **linear output** or **no output activation**.

---

## Why no activation?

We usually don't want to restrict the possible output range.

The model may need to predict:

```text
-20
0
15.7
742.5
10000
```

depending on the problem.

---

## Common Regression Losses

### Mean Squared Error

$$
\boxed{
MSE=\frac{1}{n}\sum_{i=1}^{n}(y_i-\hat{y}_i)^2
}
$$

Other common choices include:

- MAE
- Huber loss

So a common combination is:

```text
Regression
    ↓
Linear output
    ↓
MSE / MAE / Huber
```

---

# 3. 🔵 Binary Classification

## Definition

Binary classification predicts between **two possible classes**.

Examples:

```text
Spam / Not spam
Fraud / Not fraud
Pass / Fail
Disease / No disease
Cat / Not cat
```

Represent the classes as:

```text
0 → Class 0
1 → Class 1
```

---

# 4. Binary Classification Output Layer

Binary classification commonly uses **one output neuron**.

```text
Input
  ↓
Hidden Layers
  ↓
One output neuron
  ↓
Sigmoid
  ↓
Probability
```

The neuron first produces a logit:

$$
z=Wx+b
$$

Then sigmoid is applied:

$$
\boxed{\hat{y}=\sigma(z)}
$$

where:

$$
\boxed{
\sigma(z)=\frac{1}{1+e^{-z}}
}
$$

---

# 5. Sigmoid

Sigmoid converts any real-valued input into a value between 0 and 1.

```text
z
↓
Sigmoid
↓
0 to 1
```

Examples:

$$
\sigma(2)\approx0.881
$$

$$
\sigma(-2)\approx0.119
$$

So the output can be interpreted as the model's probability-like estimate for class 1.

---

# 6. Binary Prediction

Suppose:

$$
\hat{y}=0.82
$$

Using a threshold of 0.5:

```text
0.82 ≥ 0.5
```

Therefore:

```text
Predicted class = 1
```

If:

$$
\hat{y}=0.23
$$

then:

```text
0.23 < 0.5
```

so:

```text
Predicted class = 0
```

### Important

The threshold doesn't have to be 0.5 in every application. It can be adjusted depending on the desired precision/recall trade-off.

---

# 7. Binary Cross-Entropy

A common loss for binary classification is **Binary Cross-Entropy (BCE)**:

$$
\boxed{
L=-[y\log(\hat{y})+(1-y)\log(1-\hat{y})]
}
$$

The basic idea:

> Give a low loss when the model assigns high probability to the correct class, and a high loss when it is confidently wrong.

Example:

```text
Actual y = 1

Prediction = 0.95
→ Low loss

Prediction = 0.05
→ High loss
```

---

# 8. PyTorch Connection — BCEWithLogitsLoss 🔥

In PyTorch, you'll commonly encounter:

```python
nn.BCEWithLogitsLoss()
```

instead of manually doing:

```text
Sigmoid
   ↓
BCELoss
```

`BCEWithLogitsLoss` combines the sigmoid operation and BCE in a numerically stable implementation.

Therefore during training:

```text
Model
  ↓
Raw logits
  ↓
BCEWithLogitsLoss
  ↓
Loss
```

This is an important practical detail for implementation.

---

# 9. 🟣 Multiclass Classification

## Definition

Multiclass classification means:

> **One input belongs to exactly one class out of multiple possible classes.**

Example:

```text
Cat
Dog
Horse
```

One image can have only one final class.

Other examples:

- Digit recognition → 0–9
- Disease type → A/B/C
- Animal classification → cat/dog/horse

---

# 10. Multiclass Output Layer

For $K$ classes, we typically use **K output neurons**.

For 3 classes:

```text
             ┌→ Cat
Hidden ──────┼→ Dog
Layers       └→ Horse
                 ↓
               Softmax
```

The network first produces **logits**:

$$
z_1,z_2,\dots,z_K
$$

---

# 11. Softmax 🔥🔥🔥

Softmax converts the logits into a probability distribution:

$$
\boxed{
P(y=i)=
\frac{e^{z_i}}
{\sum_{j=1}^{K}e^{z_j}}
}
$$

Properties:

$$
0\le P(y=i)\le1
$$

and:

$$
\boxed{\sum_iP(y=i)=1}
$$

Therefore, all class probabilities add up to 1.

---

# 12. Softmax Example

Suppose the network produces:

```text
Cat   → 2.0
Dog   → 1.0
Horse → 0.1
```

These are **logits**, not probabilities.

Softmax converts them into values approximately like:

```text
Cat   → 0.66
Dog   → 0.24
Horse → 0.10
```

The predicted class is the one with the highest probability:

```text
Prediction → Cat
```

---

# 13. Multiclass Output Size

For $K$ classes:

$$
\boxed{\text{Number of output neurons}=K}
$$

Examples:

| Number of classes | Output neurons |
|---:|---:|
| 3 | 3 |
| 5 | 5 |
| 10 | 10 |
| 100 | 100 |

---

# 14. Multiclass Cross-Entropy

A common loss is **Cross-Entropy Loss**:

$$
\boxed{
L=-\sum_{i=1}^{K}y_i\log(\hat{y}_i)
}
$$

where:

- $y_i$ = true class indicator
- $\hat{y}_i$ = predicted probability

If Cat is the correct class:

```text
Cat   → 1
Dog   → 0
Horse → 0
```

The model should assign a high probability to Cat.

---

# 15. PyTorch CrossEntropyLoss 🔥

In PyTorch, you commonly use:

```python
nn.CrossEntropyLoss()
```

For training, you normally provide **raw logits** rather than manually applying Softmax first.

The conceptual flow is:

```text
Model
  ↓
Raw logits
  ↓
CrossEntropyLoss
  ↓
Loss
```

This is important when implementing classification networks.

---

# 16. 🟠 Multi-label Classification

Now we have a different situation.

## Definition

Multi-label classification means:

> **One input can belong to multiple classes simultaneously.**

Example:

An image contains:

```text
Dog ✓
Car ✓
Tree ✓
Person ✓
```

Multiple labels can be correct at the same time.

---

# 17. Multi-label Output Layer

Suppose there are four possible labels:

```text
Dog
Car
Tree
Person
```

We use **4 output neurons**.

Each output gets its own sigmoid:

```text
                 ┌→ Sigmoid → Dog
Hidden Layers ───┼→ Sigmoid → Car
                 ├→ Sigmoid → Tree
                 └→ Sigmoid → Person
```

Mathematically:

$$
\hat{y}_i=\sigma(z_i)
$$

Each label receives an independent probability.

---

# 18. Multi-label Example

Suppose the model outputs:

```text
Dog     → 0.91
Car     → 0.83
Tree    → 0.12
Person  → 0.76
```

Using an example threshold of 0.5:

```text
Dog     → YES
Car     → YES
Tree    → NO
Person  → YES
```

Final prediction:

```text
Dog + Car + Person
```

Multiple labels are allowed.

---

# 19. Multi-label Loss

A common choice is Binary Cross-Entropy independently across the labels:

$$
\boxed{
L=
-\sum_i
[
y_i\log(\hat{y}_i)
+
(1-y_i)\log(1-\hat{y}_i)
]
}
$$

In PyTorch, a common implementation is:

```python
nn.BCEWithLogitsLoss()
```

Again, the model normally provides **logits**, and the loss handles the sigmoid internally.

---

# 20. 🔥 Multiclass vs Multi-label

This is the **most important distinction** in this block.

## Multiclass

> Exactly **one** class is correct.

Example:

```text
What animal is this?

Cat
Dog
Horse
```

Output:

```text
Cat = 0.70
Dog = 0.20
Horse = 0.10
```

The probabilities sum to 1.

### → Softmax

---

## Multi-label

> **Multiple** classes can be correct.

Example:

```text
What objects are present?

Dog = 0.90
Car = 0.80
Tree = 0.20
Person = 0.75
```

Multiple outputs can simultaneously be high.

### → Independent Sigmoids

---

# 21. Why Softmax Doesn't Work for Multi-label

Softmax forces:

$$
\sum_iP_i=1
$$

Therefore, the outputs are competing with each other.

That's appropriate when classes are mutually exclusive.

But in multi-label classification:

```text
Dog = 0.90
Car = 0.80
Person = 0.75
```

can all be valid simultaneously.

Independent sigmoid outputs allow this.

---

# 22. 🔥 Complete Comparison

| Task | Meaning | Output neurons | Typical output activation | Common loss |
|---|---|---:|---|---|
| **Regression** | Continuous number | Usually 1 | Linear / none | MSE / MAE / Huber |
| **Binary** | One of 2 classes | 1 | Sigmoid | BCE |
| **Multiclass** | Exactly 1 of K classes | K | Softmax | Cross-Entropy |
| **Multi-label** | Multiple of K labels | K | Independent Sigmoids | BCE |

---

# 23. 🧠 Decision Flow

Use this mental model:

```text
              What are you predicting?
                       │
          ┌────────────┴────────────┐
          ↓                         ↓
    Continuous value            Classes
          │                         │
          ↓                 Can multiple classes
      Regression                be true?
          │                    /        \
          ↓                   NO        YES
    Linear output              │          │
                              ↓          ↓
                         Multiclass   Multi-label
                              │          │
                           Softmax    Sigmoid
```

Binary classification is the special two-class case:

```text
Binary
  ↓
1 output
  ↓
Sigmoid
```

---

# 24. 🔥 Activation + Loss Cheat Sheet

### Regression

$$
\boxed{\hat y=z}
$$

Common loss:

$$
\boxed{MSE}
$$

---

### Binary

$$
\boxed{\hat y=\sigma(z)}
$$

Common loss:

$$
\boxed{BCE}
$$

or in PyTorch:

```python
nn.BCEWithLogitsLoss()
```

---

### Multiclass

Conceptually:

$$
\boxed{\hat y=Softmax(z)}
$$

Common loss:

```python
nn.CrossEntropyLoss()
```

During training, pass logits directly.

---

### Multi-label

Conceptually:

$$
\boxed{\hat y_i=\sigma(z_i)}
$$

Common PyTorch loss:

```python
nn.BCEWithLogitsLoss()
```

---

# 25. 🖼️ Visual Summary

```text
                    NEURAL NETWORK
                          │
                          ↓
                    Output Layer
                          │
        ┌─────────────────┼─────────────────┐
        ↓                 ↓                 ↓
   Regression       Classification       Multi-label
        │                 │
   Linear output    ┌──────┴──────┐
                    ↓             ↓
                 Binary       Multiclass
                    ↓             ↓
                 Sigmoid       Softmax
```

---

# 26. 🏭 Industry Perspective

When designing a neural network, the output layer isn't arbitrary.

The problem determines:

$$
\boxed{
\text{Task}
\rightarrow
\text{Output}
\rightarrow
\text{Activation}
\rightarrow
\text{Loss}
}
$$

For example:

```text
Predict motor temperature
        ↓
Regression
        ↓
Linear output
        ↓
MSE / Huber
```

versus:

```text
Detect whether motor has a fault
        ↓
Binary classification
        ↓
Sigmoid
        ↓
BCE
```

versus:

```text
Identify fault type
        ↓
Multiclass classification
        ↓
Softmax
        ↓
Cross-Entropy
```

versus:

```text
Detect all fault categories present
        ↓
Multi-label classification
        ↓
Independent Sigmoids
        ↓
BCE
```

---

# ⚠️ Common Mistakes

### ❌ Mistake 1

Using Softmax for multi-label classification.

**Correct:**

```text
Multiclass → Softmax
Multi-label → Independent Sigmoids
```

---

### ❌ Mistake 2

Using multiple output neurons for binary classification unnecessarily.

Typical binary setup:

```text
1 output neuron
+
Sigmoid
```

---

### ❌ Mistake 3

Applying Softmax before `CrossEntropyLoss` in standard PyTorch training.

Usually:

```python
model → logits → CrossEntropyLoss
```

not:

```python
model → Softmax → CrossEntropyLoss
```

---

### ❌ Mistake 4

Applying Sigmoid before `BCEWithLogitsLoss`.

Usually:

```python
model → logits → BCEWithLogitsLoss
```

The loss handles the sigmoid internally.

---

# 🎯 Interview Questions

### Q1. What output activation is commonly used for regression?

**Linear / no output activation.**

### Q2. Binary classification?

**One output + sigmoid.**

### Q3. Multiclass classification?

**One output per class + softmax conceptually.**

### Q4. Multi-label classification?

**One output per label + independent sigmoid outputs.**

### Q5. Why not Softmax for multi-label?

Because Softmax forces the outputs to form one probability distribution whose values sum to 1, while multiple labels can independently be true.

### Q6. What is the difference between multiclass and multi-label?

```text
Multiclass → one correct class
Multi-label → multiple correct classes possible
```

---

# 🚀 Final Cheat Sheet

| Problem | Output | Activation | Common PyTorch loss |
|---|---|---|---|
| Regression | 1 value | None / Linear | `MSELoss` |
| Binary | 1 logit | Sigmoid conceptually | `BCEWithLogitsLoss` |
| Multiclass | K logits | Softmax conceptually | `CrossEntropyLoss` |
| Multi-label | K logits | Sigmoid per output conceptually | `BCEWithLogitsLoss` |

### 🧠 Remember this forever:

> **One continuous value → Regression → Linear**

> **One of two → Binary → Sigmoid**

> **One of many → Multiclass → Softmax**

> **Many can be true → Multi-label → Independent Sigmoids**

---