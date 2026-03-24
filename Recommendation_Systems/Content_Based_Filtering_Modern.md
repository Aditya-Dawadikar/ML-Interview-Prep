# Modern Content-Based Filtering (Embedding-Based)

## 1. Overview

Modern Content-Based Filtering replaces manual feature engineering with **learned embeddings**.

> “Represent users and items in a shared vector space, where similarity = relevance”

Instead of:

* TF-IDF
* handcrafted features

We use:

* deep models to learn representations

---

## 2. Core Idea

Map both users and items into embeddings:

```text
User → embedding_u ∈ R^d  
Item → embedding_v ∈ R^d
```

Then:

[
score(u, i) = u \cdot v
]

Higher score → higher relevance

---

## 3. Item Embedding (Content Encoder)

Items are encoded using their raw content.

### Modalities:

#### Text (titles, descriptions)

* BERT / Transformer encoders

#### Images (thumbnails)

* CNN / Vision Transformers

#### Audio / Video

* specialized encoders

```text
Item → Encoder → embedding_v
```

👉 This replaces manual feature extraction

---

## 4. User Embedding

User representation is derived from history.

### 4.1 Simple Aggregation

[
u = \frac{1}{N} \sum v_i
]

### 4.2 Weighted Aggregation

[
u = \sum w_i \cdot v_i
]

Weights based on:

* recency
* watch time
* rating

---

### 4.3 Learned User Encoder (advanced)

```text
User history → sequence model → embedding_u
```

* Transformer / RNN
* captures temporal behavior

---

## 5. Training Strategies

### 5.1 Supervised (Interaction-based)

Train using:

* (user, item) pairs

Objective:

* maximize similarity for positives
* minimize for negatives

---

### 5.2 Contrastive Learning

For each user:

* positive → consumed item
* negatives → random / hard samples

Loss encourages:

```text
sim(u, positive) >> sim(u, negative)
```

---

## 6. Retrieval Pipeline

```text
Offline:
    Item → embedding → stored in vector DB

Online:
    User → embedding
    ↓
    ANN Search (FAISS)
    ↓
    Top-K candidates
```

---

## 7. Advantages

### 7.1 Handles Cold Start (Items)

* new items can be embedded instantly

### 7.2 Scalable

* vector search (ANN) is efficient

### 7.3 Multi-modal

* combines text, image, audio

### 7.4 Generalization

* learns semantic similarity, not just exact matches

---

## 8. Limitations

### 8.1 Limited Interaction Modeling

* dot product is simple → misses complex patterns

### 8.2 Requires Good Negatives

* training quality depends heavily on sampling

### 8.3 Overspecialization

* still biased toward similar items

---

## 9. Improvements

### 9.1 Better User Modeling

* sequence models (Transformers)

### 9.2 Hybrid Signals

* combine CF + content embeddings

### 9.3 Hard Negative Mining

* improves embedding quality

---

## 10. Relationship to Two-Tower Models

```text
Content-Based (Embedding)
        ↓
Two-Tower Model
```

Difference:

| Aspect       | Content-Based (Modern) | Two-Tower      |
| ------------ | ---------------------- | -------------- |
| Item encoder | content only           | content + ID   |
| User encoder | aggregation / simple   | learned model  |
| Training     | optional               | always trained |

👉 Two-tower is a **learned extension** of embedding-based CBF

---

## 11. When to Use

Use when:

* rich content is available
* cold start is critical
* semantic similarity matters

---

## 12. Summary

* Replace manual features with embeddings
* Represent users and items in same vector space
* Use similarity (dot / cosine) for ranking
* Scale using ANN

---

## One key intuition (don’t miss this)

```text
Classic CBF → manual similarity  
Modern CBF → learned similarity  
Two-Tower → optimized similarity for retrieval
```
