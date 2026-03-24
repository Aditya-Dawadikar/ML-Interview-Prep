# Item-Wise Ranking (Pointwise Learning to Rank)

## 1. Overview

Pointwise ranking treats recommendation as a **regression or classification problem**.

> “Given (user, item), predict how likely the user will engage”

Each item is scored **independently**, then sorted.

---

## 2. Core Idea

```text
(user, item) → model → score
```

Then:

```text
rank items by score (descending)
```

---

## 3. Label Types

### 3.1 Explicit Feedback

* ratings (1–5)

### 3.2 Implicit Feedback (most common)

* click (1 / 0)
* watch / skip
* dwell time

---

## 4. Model Formulation

### Binary classification (CTR)

[
P(click | u, i) = \sigma(f(u, i))
]

### Regression

[
score = f(u, i)
]

Where:

* (f) = model (MLP, GBDT, etc.)

---

## 5. Features (Critical)

### User Features

* history embeddings
* activity stats
* preferences

### Item Features

* embeddings
* metadata
* popularity

### Context Features

* time of day
* device
* session info

---

## 6. Training Objective

### Binary Cross-Entropy

[
L = -[y \log(p) + (1-y)\log(1-p)]
]

* y = 1 (clicked), 0 (not clicked)

---

## 7. Pipeline

```text
Candidates (~1000 from retrieval)
        ↓
Score each item independently
        ↓
Sort by score
        ↓
Top-K recommendations
```

---

## 8. Advantages

### 8.1 Simple

* easy to implement and scale

### 8.2 Flexible

* supports rich features

### 8.3 Stable training

* no complex pair construction

---

## 9. Limitations

### 9.1 Ignores Relative Order

* doesn’t directly optimize ranking

Example:

* predicts 0.9, 0.8, 0.7 → good
* predicts 0.6, 0.59, 0.58 → same order, worse signal

---

### 9.2 Label Bias

* depends heavily on observed clicks

---

### 9.3 Position Bias

* top items get more clicks → skewed data

---

## 10. When It Works Well

Use when:

* large-scale systems
* strong feature engineering
* need stable training

👉 Most production systems still use pointwise as baseline

---

## 11. Comparison

| Method    | Optimization Target    |
| --------- | ---------------------- |
| Pointwise | absolute score         |
| Pairwise  | relative order (A > B) |
| Listwise  | full ranking           |

---

## 12. Practical Tricks

* downsample negatives
* calibrate probabilities
* use weighted loss (handle imbalance)
* combine with business rules

---

## 13. Connection to Full System

```text
Two-Tower → Retrieval
        ↓
Pointwise Model → Ranking
        ↓
Re-ranking (rules, diversity)
```

---

## 14. Summary

* Predict score per (user, item)
* Rank by sorting scores
* Simple, scalable, widely used
* But doesn’t directly optimize ranking quality

---

## One Key Intuition

```text
Pointwise → “how good is this item?”
Pairwise → “is A better than B?”
Listwise → “is the whole ranking optimal?”
```
