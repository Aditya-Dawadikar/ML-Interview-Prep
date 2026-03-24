# Two-Tower Network (Dual Encoder) for Recommendation Systems

## 1. Overview

Two-Tower is a neural architecture used for **large-scale candidate retrieval**.

> “Learn embeddings for users and items such that relevant pairs are close in vector space”

It enables:

* fast retrieval (via ANN)
* scalable recommendations over millions of items

---

## 2. Core Idea

Two independent encoders:

```text
User Tower   → embedding_u ∈ R^d
Item Tower   → embedding_v ∈ R^d
```

Score:

[
score(u, i) = u \cdot v
]

Goal:

* maximize score for positive pairs
* minimize for negative pairs

---

## 3. Architecture

```text
User Features  ──▶ User Tower ──▶ embedding_u
                                      │
                                      ▼
                                  Dot Product
                                      ▲
                                      │
Item Features ──▶ Item Tower ──▶ embedding_v
```

---

## 4. Inputs

### 4.1 User Features

* user ID (embedding)
* past interactions (history)
* demographics
* context (time, device)

---

### 4.2 Item Features

* item ID (embedding)
* metadata (category, tags)
* content (text, image embeddings)

---

## 5. Towers

### 5.1 User Tower

* MLP / Transformer
* encodes user behavior

### 5.2 Item Tower

* MLP / content encoder
* encodes item semantics

👉 Towers do **not interact during encoding**
(interaction happens only via dot product)

---

## 6. Training Objective

### 6.1 Positive Pairs

* (user, clicked item)

### 6.2 Negative Pairs

* non-clicked items

---

### 6.3 Loss (example: sampled softmax)

[
L = -\log \frac{e^{u \cdot v^+}}{e^{u \cdot v^+} + \sum e^{u \cdot v^-}}
]

Encourages:

```text
score(u, positive) >> score(u, negatives)
```

---

## 7. Negative Sampling (Critical)

Types:

* random negatives (easy)
* in-batch negatives (efficient)
* hard negatives (high impact)

👉 Training quality heavily depends on this

---

## 8. Inference Pipeline

### 8.1 Offline

```text
All items → item tower → embeddings → store in vector DB (FAISS)
```

---

### 8.2 Online

```text
User request → user tower → embedding_u
             ↓
     ANN search (FAISS)
             ↓
     Top-K candidates (~100–1000)
```

---

## 9. Why It Scales

Naive approach:

```text
compare user with all items → O(N)
```

Two-tower:

```text
ANN search → sublinear (~log N)
```

👉 makes million-scale retrieval feasible

---

## 10. Advantages

### 10.1 Scalable Retrieval

* works with millions/billions of items

### 10.2 Precomputation

* item embeddings computed offline

### 10.3 Flexible Features

* supports multi-modal inputs

### 10.4 Cold Start (Items)

* content features allow new items to be embedded

---

## 11. Limitations

### 11.1 Weak Interaction Modeling

* only dot product → limited expressiveness

### 11.2 Depends on Negative Sampling

* bad negatives → poor embeddings

### 11.3 Not Final Ranking

* only retrieves candidates

---

## 12. Role in Full System

```text
Two-Tower → Candidate Generation
             ↓
        Ranking Model
             ↓
        Final Recommendations
```

---

## 13. Comparison

| Method                  | Role       | Strength   |
| ----------------------- | ---------- | ---------- |
| Collaborative Filtering | similarity | simple     |
| Content-Based           | features   | cold start |
| Two-Tower               | retrieval  | scalable   |
| Ranking Model           | ordering   | accuracy   |

---

## 14. Design Variations

### 14.1 Shared Embedding Space

* both towers map to same vector space

### 14.2 Multi-Task Learning

* optimize CTR, watch time, etc.

### 14.3 Deep User Models

* sequence-based (Transformer)

---

## 15. When to Use

Use when:

* item catalog is large
* latency is critical
* need fast candidate retrieval

---

## 16. Summary

* Two-Tower learns **user + item embeddings**
* Uses **dot product similarity**
* Enables **fast ANN-based retrieval**
* Forms the **first stage of recommender systems**

---

## One Key Intuition

```text
Matrix Factorization → Two-Tower → Retrieval System
```

* MF = shallow embeddings
* Two-tower = deep, feature-aware embeddings
