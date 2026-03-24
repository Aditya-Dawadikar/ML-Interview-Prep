# Pairwise Ranking (Learning to Rank)

## 1. Overview

Pairwise ranking focuses on **relative preference between items**.

> “Given two items, learn which one the user prefers”

Instead of predicting absolute scores:

* it learns **A > B**

---

## 2. Core Idea

Training data:

```text
(user, item_A, item_B)
```

Where:

* item_A = preferred (clicked / positive)
* item_B = not preferred (negative)

Goal:

```text
score(u, A) > score(u, B)
```

---

## 3. Model Formulation

Model outputs:

[
s_A = f(u, A), \quad s_B = f(u, B)
]

We optimize:

[
s_A - s_B
]

---

## 4. Loss Functions

### 4.1 Logistic Loss (RankNet style)

[
L = \log(1 + e^{-(s_A - s_B)})
]

* pushes (s_A > s_B)

---

### 4.2 Hinge Loss

[
L = \max(0, 1 - (s_A - s_B))
]

* enforces margin between A and B

---

### 4.3 BPR (Bayesian Personalized Ranking)

[
L = -\log \sigma(s_A - s_B)
]

👉 widely used in recommender systems

---

## 5. Data Construction

Critical step.

For each user:

* positive items (clicked)
* negative items (not clicked)

Create pairs:

```text
(clicked_item, non_clicked_item)
```

---

### Negative Sampling

Types:

* random negatives (easy)
* hard negatives (similar but not clicked)
* in-batch negatives

👉 this determines model quality

---

## 6. Pipeline

```text
Candidates (~1000)
      ↓
Generate pairs (pos, neg)
      ↓
Model learns ordering
      ↓
Final scores → ranking
```

---

## 7. Advantages

### 7.1 Directly Optimizes Ranking

* focuses on relative order

### 7.2 Better than Pointwise

* handles ambiguous scores better

### 7.3 Robust to Scale Issues

* doesn’t depend on absolute labels

---

## 8. Limitations

### 8.1 Pair Explosion

* O(n²) possible pairs

→ need sampling

---

### 8.2 Training Complexity

* more expensive than pointwise

---

### 8.3 Local Optimization

* optimizes pair relations, not full list

---

## 9. When to Use

Use when:

* ranking quality matters
* implicit feedback (click/no-click)
* relative preference is important

---

## 10. Comparison

| Method    | Learns         |
| --------- | -------------- |
| Pointwise | absolute score |
| Pairwise  | relative order |
| Listwise  | full ranking   |

---

## 11. Connection to Metrics

Pairwise aligns better with:

* AUC
* ranking consistency

But not directly with:

* NDCG (list-level)

---

## 12. Practical Tips

* sample hard negatives
* limit number of pairs per user
* mix with pointwise loss (hybrid training)

---

## 13. Role in System

```text
Two-Tower → Retrieval
        ↓
Pairwise Model → Better ordering
        ↓
Re-ranking
```

---

## 14. Summary

* learns **A > B** instead of scores
* improves ranking quality
* depends heavily on good negative sampling
* sits between pointwise and listwise

---

## One Key Intuition

```text
Pointwise → independent scoring  
Pairwise → relative comparison  
Listwise → global ordering
```
