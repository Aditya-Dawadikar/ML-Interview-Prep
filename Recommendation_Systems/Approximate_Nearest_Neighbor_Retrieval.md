# Approximate Nearest Neighbor (ANN) Retrieval

## 1. Overview

ANN is used to efficiently find the **most similar vectors** in large-scale systems.

> “Given a query vector, quickly find top-K closest vectors without scanning everything”

Used in:

* recommendation systems (two-tower retrieval)
* semantic search
* vector databases (FAISS, ScaNN)

---

## 2. Problem

Given:

```text
N items → embeddings (N can be millions/billions)
query → embedding_u
```

Goal:

> find top-K items with highest similarity

---

## 3. Exact vs Approximate Search

### Exact (Brute Force)

```text
compute similarity with all N items → O(N)
```

* accurate ✅
* too slow ❌

---

### Approximate (ANN)

```text
search subset of space → O(log N) or sublinear
```

* fast ✅
* slightly inaccurate ⚠️

👉 tradeoff: **speed vs recall**

---

## 4. Similarity Metrics

* Dot product (common in two-tower)
* Cosine similarity
* Euclidean distance

---

## 5. Core Idea

Instead of scanning all vectors:

> **organize vectors so similar ones are grouped together**

Then:

* search only relevant regions

---

## 6. Main Techniques

### 6.1 Tree-Based (Partitioning)

Split vector space into regions:

```text
space → clusters → sub-clusters
```

Search:

* traverse tree
* visit few nodes

Examples:

* KD-Tree (low dimension)
* Ball Tree

❌ doesn’t scale well for high dimensions

---

### 6.2 Hashing (LSH)

Hash similar vectors to same buckets:

```text
similar vectors → same hash bucket
```

Search:

* only inside bucket

✔ fast
⚠️ approximate

---

### 6.3 Graph-Based (Most Important)

Build graph:

* nodes = vectors
* edges = nearest neighbors

Search:

* start from entry node
* greedily move closer to query

Example:

* HNSW (Hierarchical Navigable Small World)

👉 most widely used in production

---

### 6.4 Quantization (Compression)

Compress vectors:

```text
original → compressed codes
```

Search in compressed space:

* faster
* memory efficient

Example:

* Product Quantization (PQ)

---

## 7. FAISS (Common Tool)

Library by Meta

Supports:

* flat (exact)
* IVF (inverted file index)
* HNSW
* PQ

---

## 8. ANN Pipeline in Recommender Systems

### Offline

```text
items → embeddings → ANN index (FAISS)
```

---

### Online

```text
user → embedding
      ↓
ANN search
      ↓
Top-K candidates (~100–1000)
```

---

## 9. Key Tradeoffs

### 9.1 Recall vs Latency

* higher recall → slower
* lower recall → faster

---

### 9.2 Memory vs Speed

* compressed vectors → less memory
* but slightly less accurate

---

### 9.3 Build Time vs Query Time

* complex index → slower build
* faster queries

---

## 10. Important Parameters

Examples (FAISS-like systems):

* number of clusters (IVF)
* search depth (nprobe)
* graph connectivity (HNSW)

👉 tuning these = performance gains

---

## 11. Advantages

* sublinear search time
* scalable to millions/billions
* works with embeddings

---

## 12. Limitations

* not exact results
* tuning required
* index rebuild needed for updates (sometimes)

---

## 13. When to Use

Use when:

* large embedding space (>100k items)
* real-time retrieval required
* dot product / cosine similarity used

---

## 14. Connection to Two-Tower

```text
Two-Tower → embeddings
        ↓
ANN → fast retrieval
```

👉 ANN is the **engine**, two-tower is the **model**

---

## 15. Summary

* ANN = fast similarity search in vector space
* trades accuracy for speed
* uses structures like:

  * graphs (HNSW)
  * clusters (IVF)
  * hashing (LSH)

---

## One Key Intuition

```text
Brute force → accurate but slow  
ANN → slightly approximate but scalable
```
