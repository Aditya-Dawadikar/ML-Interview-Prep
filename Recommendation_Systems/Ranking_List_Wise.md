# Listwise Ranking (Learning to Rank)

## 1. Overview

Listwise ranking optimizes the **entire ranked list at once**, not individual items or pairs.

> “Learn to produce the best possible ordering of items for a user”

Unlike:

* Pointwise → scores
* Pairwise → comparisons

Listwise → **full ranking quality**

---

## 2. Core Idea

Instead of:

```text
(u, item)
(u, item_A, item_B)
```

We use:

```text
(u, [item_1, item_2, ..., item_n])
```

Goal:

> predicted ranking ≈ ideal ranking

---

## 3. Why Listwise Matters

Real evaluation metrics are **list-based**:

* NDCG
* MAP
* Recall@K

👉 Pointwise/pairwise don’t directly optimize these
👉 Listwise does

---

## 4. Scoring

Model still outputs scores:

[
s_i = f(u, item_i)
]

But loss is computed over the **entire list**

---

## 5. Common Approaches

---

### 5.1 ListNet (Probability-based)

Convert scores into probability distribution:

[
P(i) = \frac{e^{s_i}}{\sum_j e^{s_j}}
]

Compare:

* predicted distribution
* ground truth distribution

Loss:

* cross-entropy between the two

---

### 5.2 ListMLE

Goal:

> maximize probability of correct ranking

Treat ranking as permutation:

[
P(\pi) = \prod_{i} \frac{e^{s_{\pi_i}}}{\sum_{j \ge i} e^{s_{\pi_j}}}
]

Loss:

* negative log likelihood of correct ordering

---

### 5.3 LambdaRank / LambdaMART (Most Important)

Used in real systems.

Key idea:

> directly optimize NDCG via gradients

Instead of explicit loss:

* compute **“lambda gradients”**
* prioritize swaps that improve NDCG

---

## 6. Metric: NDCG (Core)

Measures ranking quality with position importance:

[
DCG = \sum \frac{rel_i}{\log_2(i+1)}
]

[
NDCG = \frac{DCG}{IDCG}
]

* top positions matter more
* normalized between 0 and 1

---

## 7. Pipeline

```text
Candidates (~1000)
      ↓
Listwise Model
      ↓
Optimized Ranking
      ↓
Top-K output
```

---

## 8. Advantages

### 8.1 Aligns with Business Metrics

* directly optimizes ranking quality

### 8.2 Global Optimization

* considers full list context

### 8.3 Better Top-K Performance

* prioritizes important positions

---

## 9. Limitations

### 9.1 Complex Training

* harder than pointwise/pairwise

### 9.2 Expensive

* operates on full lists

### 9.3 Data Requirements

* needs good relevance labels / ordering

---

## 10. When to Use

Use when:

* ranking quality is critical
* optimizing top-K performance
* large-scale systems with mature pipelines

---

## 11. Comparison

| Method    | Optimizes           |
| --------- | ------------------- |
| Pointwise | score               |
| Pairwise  | A > B               |
| Listwise  | full ranking (best) |

---

## 12. Practical Reality

Most systems:

```text
Train:
    Pointwise + Pairwise (cheap)
Fine-tune:
    Listwise (expensive but precise)
```

---

## 13. Role in System

```text
Two-Tower → Retrieval
        ↓
Ranker (Pointwise / Pairwise)
        ↓
Listwise Optimization (final layer)
```

---

## 14. Summary

* optimizes **entire ranking list**
* aligns with metrics like NDCG
* more accurate but more complex
* used in advanced ranking systems

---

## One Key Intuition

```text
Pointwise → local score  
Pairwise → local order  
Listwise → global ranking quality
```
